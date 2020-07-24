[ProGuard](http://www.saikoa.com/proguard) and it's Android pendant [DexGuard](http://www.saikoa.com/dexguard) are optimizers and obfuscators for Java and Android applications. You can combine these tools with your libGDX application to make it harder for 3rd parties to decompile your app, reduce your apps size and even increase the runtime speed by ahead-of-time optimizations like inlining.

The following configuration file will make your libGDX app work with ProGuard:

```
# To enable ProGuard in your project, edit project.properties
# to define the proguard.config property as described in that file.
#
# Add project specific ProGuard rules here.
# By default, the flags in this file are appended to flags specified
# in ${sdk.dir}/tools/proguard/proguard-android.txt
# You can edit the include path and order by changing the ProGuard
# include property in project.properties.
#
# For more details, see
#   http://developer.android.com/guide/developing/tools/proguard.html

# Add any project specific keep options here:

# If your project uses WebView with JS, uncomment the following
# and specify the fully qualified class name to the JavaScript interface
# class:
#-keepclassmembers class fqcn.of.javascript.interface.for.webview {
#   public *;
#}

-verbose

-dontwarn android.support.**
-dontwarn com.badlogic.gdx.backends.android.AndroidFragmentApplication
-dontwarn com.badlogic.gdx.utils.GdxBuild
-dontwarn com.badlogic.gdx.physics.box2d.utils.Box2DBuild
-dontwarn com.badlogic.gdx.jnigen.BuildTarget*

-keepclassmembers class com.badlogic.gdx.backends.android.AndroidInput* {
   <init>(com.badlogic.gdx.Application, android.content.Context, java.lang.Object, com.badlogic.gdx.backends.android.AndroidApplicationConfiguration);
}

-keepclassmembers class com.badlogic.gdx.physics.box2d.World {
   boolean contactFilter(long, long);
   void    beginContact(long);
   void    endContact(long);
   void    preSolve(long, long);
   void    postSolve(long, long);
   boolean reportFixture(long);
   float   reportRayFixture(long, float, float, float, float, float);
}
```

Note that you will also have to keep any classes that you access via reflection yourself! Please refer to the ProGuard/DexGuard documentation for more details.

To apply ProGuard/DexGuard to your Android project, you can use the following `build.gradle` file (the one in the `android/` folder of your project, not the root `build.gradle`)

Note that in gradle plugin version 0.14+ the property **runProguard** has been changed to **minifyEnabled**.

```groovy
android {
   buildToolsVersion "19.0.3"
   compileSdkVersion 19

   signingConfigs {
       mySignature {
           storeFile     file('/home/eric/.android/debug.keystore')
           storePassword 'android'
           keyAlias      'androiddebugkey'
           keyPassword   'android'
       }
   }

   defaultConfig {
       signingConfig signingConfigs.mySignature
   }

   sourceSets {
       main {
           manifest.srcFile 'AndroidManifest.xml'
           java.srcDirs = ['src']
           resources.srcDirs = ['src']
           aidl.srcDirs = ['src']
           renderscript.srcDirs = ['src']
           res.srcDirs = ['res']
           assets.srcDirs = ['assets']
       }

       instrumentTest.setRoot('tests')
   }

   buildTypes {
       release {
           runProguard true
// use minifyEnabled instead of runProguard if your gradle plugin version is 0.14+
           proguardFile getDefaultProguardFile('proguard-android-optimize.txt')
           proguardFile 'proguard-project.txt'
       }
   }
}

// needed to add JNI shared libraries to APK when compiling on CLI
tasks.withType(com.android.build.gradle.tasks.PackageApplication) { pkgTask ->
   pkgTask.jniFolders = new HashSet<File>()
   pkgTask.jniFolders.add(new File(projectDir, 'libs'))
}

// called every time gradle gets executed, takes the native dependencies of
// the natives configuration, and extracts them to the proper libs/ folders
// so they get packed with the APK.
task copyAndroidNatives() { 
   file("libs/armeabi/").mkdirs();
   file("libs/armeabi-v7a/").mkdirs();
   file("libs/x86/").mkdirs();

   configurations.natives.files.each { jar ->
       def outputDir = null
       if(jar.name.endsWith("natives-armeabi-v7a.jar")) outputDir = file("libs/armeabi-v7a")
       if(jar.name.endsWith("natives-armeabi.jar")) outputDir = file("libs/armeabi")
       if(jar.name.endsWith("natives-x86.jar")) outputDir = file("libs/x86")
       if(outputDir != null) {
           copy {
               from zipTree(jar)
               into outputDir
               include "*.so"
           }
       }
   }
}

task run(type: Exec) {
   def adb = "$System.env.ANDROID_HOME/platform-tools/adb"
   commandLine "$adb", 'shell', 'am', 'start', '-n', 'com.dozingcatsoftware.bouncy.android/com.dozingcatsoftware.bouncy.android.AndroidLauncher'
}

// sets up the Android Eclipse project, using the old Ant based build.
eclipse {
   // need to specify Java source sets explicitely, SpringSource Gradle Eclipse plugin
   // ignores any nodes added in classpath.file.withXml
   sourceSets {
       main {
           java.srcDirs "src", 'gen'
       }
   }

   jdt {
       sourceCompatibility = 1.6
       targetCompatibility = 1.6
   }

   classpath {
       plusConfigurations += project.configurations.compile        
       containers 'com.android.ide.eclipse.adt.ANDROID_FRAMEWORK', 'com.android.ide.eclipse.adt.LIBRARIES'       
   }

   project {
       name = appName + "-android"
       natures 'com.android.ide.eclipse.adt.AndroidNature'
       buildCommands.clear();
       buildCommand "com.android.ide.eclipse.adt.ResourceManagerBuilder"
       buildCommand "com.android.ide.eclipse.adt.PreCompilerBuilder"
       buildCommand "org.eclipse.jdt.core.javabuilder"
       buildCommand "com.android.ide.eclipse.adt.ApkBuilder"
   }
}

// sets up the Android Idea project, using the old Ant based build.
idea {
   module {
       sourceDirs += file("src");
       scopes = [ COMPILE: [plus:[project.configurations.compile]]]        

       iml {
           withXml {
               def node = it.asNode()
               def builder = NodeBuilder.newInstance();
               builder.current = node;
               builder.component(name: "FacetManager") {
                   facet(type: "android", name: "Android") {
                       configuration {
                           option(name: "UPDATE_PROPERTY_FILES", value:"true")
                       }
                   }
               }
           }
       }
   }
}
```