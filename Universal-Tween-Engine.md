# Information

libGDX has its own Tween engine, more info here: https://github.com/libgdx/libgdx/wiki/Interpolation

# Universal Tween Engine

Since switching over to the gradle based build system some users have been having a hard time adding the Universal Tween Engine to their newly created projects. This article aims to make this process as painless as possible.

> **Note:** this article assumes that you've already created a project using gdx-setup. If you haven't created your project yet follow the instructions here before continuing:

> [Project Setup Gradle](https://github.com/libgdx/libgdx/wiki/Project-Setup-Gradle)

## Gradle

Open up the build.gradle file located in the root of your project directory in a text editor or your IDE. Make sure `mavenCentral()` is in `buildscript.repositories`

1. Locate the section marked **project(":core")**
2. In the **dependencies** section add the following line:
  ```groovy
  compile "org.mini2Dx:universal-tween-engine:6.3.3"
  ```
3. Refresh dependencies
  ```
  gradlew --refresh-dependencies
  ```

## FileTree Dependency Method (Easiest)

This method is the easiest to get up and running but if you end up with multiple projects that make use of the Universal Tween Engine you might want to consider [Using a Local Maven Repository](#using-your-local-maven-repository)

---

1. Download the Universal Tween Engine from here: [Universal Tween Engine](https://code.google.com/p/java-universal-tween-engine/downloads/list)
2. In the root of your project directory create a new directory called 'libs'
3. Extract the Universal Tween Engine zip file to the directory that you just created
4. Open up the build.gradle file located in the root of your project directory in a text editor or your IDE
  * Locate the section marked **project(":core")**
  * In the **dependencies** section add the following line:
  ```groovy
  compile fileTree(dir: '../libs', include: '*.jar')
  ```
  * the **project(":core")** section of your build.gradle file should now look like this:

  ```groovy
  project(":core") {
     ...

      dependencies {
          ...
          compile fileTree(dir: '../libs', include: '*.jar')
      }
  }
  ```
5. **Required For Android**: Locate the section marked **project(":android")**
  * In the **dependencies** section add the following line:
  ```groovy
  compile fileTree(dir: '../libs', include: '*.jar')
  ```
  * the **project(":android")** section of your build.gradle file should now look like this:
  ```groovy
  project(":android") {
     ...

      dependencies {
          ...
          compile fileTree(dir: '../libs', include: '*.jar')
      }
  }
  ```
6. **Required for HTML Projects**
  * Add the following line to **GdxDefinition.gwt.xml** and **GdxDefinitionSuperdev.gwt.xml**
    * they are located in &lt;my-project-dir&gt;/html/src/your/package/name
  ```xml
<inherits name='aurelienribon.tweenengine'/>
  ```

7. And most importantly you now need to refresh your project dependencies through your IDE or by running the following command from your project directory:

  ```
  gradlew --refresh-dependencies
  ```

## Using Your Local Maven Repository

1. If you don't have Maven installed follow the instructions here: [Maven in Five Minutes]( http://maven.apache.org/guides/getting-started/maven-in-five-minutes.html)
2. Download the Universal Tween Engine from here: [Universal Tween Engine](https://code.google.com/p/java-universal-tween-engine/downloads/list)
3. Extract the universal tween engine zip file to some location on your system
4. Open a command prompt to the location that you extracted the zip file
5. Run the following commands to add the universal tween engine jars to your local maven repo:

  ```
mvn install:install-file -Dfile=tween-engine-api.jar -DgroupId=aurelienribon -DartifactId=tweenengine -Dversion=6.3.3 -Dpackaging=jar
  ```
  ```
mvn install:install-file -Dfile=tween-engine-api-sources.jar -DgroupId=aurelienribon -DartifactId=tweenengine -Dversion=6.3.3 -Dpackaging=jar -Dclassifier=sources
  ```
6. Open up the build.gradle file located in the root of your project directory in a text editor or your IDE
  * Locate the section marked **project(":core")**
  * In the **dependencies** section add the following lines:
  ```groovy
  compile "aurelienribon:tweenengine:6.3.3"
  compile "aurelienribon:tweenengine:6.3.3:sources"
  ```
  * the **project(":core")** section of your build.gradle file should now look like this:

  ```groovy
  project(":core") {
     ...

      dependencies {
          ...
          compile "aurelienribon:tweenengine:6.3.3"
          compile "aurelienribon:tweenengine:6.3.3:sources"
      }
  }
  ```

7. Make sure gradle is configured to read from your local repo. 

```groovy
allprojects {
   ...
    repositories {
        mavenLocal();
        mavenCentral()
        maven { url "https://oss.sonatype.org/content/repositories/snapshots/" }
        maven { url "https://oss.sonatype.org/content/repositories/releases/" }
    }
}
```

8. **Required for HTML Projects**
  * Add the following line to **GdxDefinition.gwt.xml** and **GdxDefinitionSuperdev.gwt.xml**
    * they are located in &lt;my-project-dir&gt;/html/src/your/package/name
  ```xml
<inherits name='aurelienribon.tweenengine'/>
  ```

9. And most importantly you now need to refresh your project dependencies through your IDE or by running the following command from your project directory:

  ```
  gradlew --refresh-dependencies
  ```