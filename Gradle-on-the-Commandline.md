This article will show you how you can run your application from the command line and package it for distribution for the different platforms!
## Contents
* [**Prerequisites**](#setting-up-android_home) 
  * [Setting the ANDROID_HOME variable](#setting-up-android_home)
* [**Running**](#running-the-project)
  * [Running on Desktop](#running-the-desktop-project)
  * [Running on Android](#running-the-android-project)
  * [Running on iOS](#running-the-ios-project)
  * [Running HTML](#running-the-html-project)
* [**Packaging**](#packaging-the-project) 
  * [Packaging Desktop](#packaging-for-the-desktop)
  * [Packaging Android](#packaging-for-android)
  * [Packaging iOS](#packaging-for-ios)
  * [Packaging HTML](#packaging-for-the-web) 
* [**Debugging/Problems**](#debugging-and-common-problems) 
  * [Gradle tasks failing](#gradle-tasks-are-failing)
  * [Common problems](#common-problems)
  * [Debugging projects](#debugging-projects)
* [**Tweaking**](#tweaking)

***

## Setting up ANDROID_HOME
The ANDROID_HOME environment variable needs to be pointing to a valid android SDK before you do any command line wizardry.

Windows: `set ANDROID_HOME=C:/Path/To/Your/Android/Sdk`

Linux, Mac OS X: `export ANDROID_HOME=/Path/To/Your/Android/Sdk`

Alternatively you can create a file called "local.properties" with the following content: `sdk.dir /Path/To/Your/Android/Sdk`

## Running the project
Gradle let's you easily run a project from the commandline. Just use the gradlew command specify your target platform and the run command for that platform.

[**Desktop**](#running-the-desktop-project) - [**Android**](#running-the-android-project) - [**iOS**](#running-the-ios-project) - [**HTML**](#running-the-html-project) 

### Running the desktop project
`gradlew desktop:run`

This compiles your core and desktop project, and runs the desktop starter. The working directory is the project's assets folder!

### Running the Android project
`gradlew android:installDebug android:run`

This task will create a debug APK of your application, install it on the first connected emulator or device and start the main activity. The process is split into two tasks because the Android Gradle plugin lets you create multiple flavors of your app (e.g. debug as above, release, ...). You can find more information on the [Android Gradle Plugin site](http://tools.android.com/tech-docs/new-build-system/user-guide).

### Running the iOS project
Before you run on an iOS simulator or device, make sure you have [activated your free RoboVM key](https://github.com/libgdx/libgdx/wiki/Setting-up-your-Development-Environment-(Eclipse,-Intellij-IDEA,-NetBeans)#setup-for-ios-development). If you run on a device, you need to [provision](http://docs.robovm.com/getting-started/provisioning.html) it to be able to deploy to it!

`gradlew ios:launchIPhoneSimulator`

`gradlew ios:launchIPadSimulator`

`gradlew ios:launchIOSDevice`

The first two commands will launch your app on an iPhone or iPad simualtor, the last command will launch your ios project on a connected iDevice, provided it is provisioned. Please refer to Apple's docs on how to provision a device. Note that the first time you run your iOS project, the compilation will take a long time. Compilation time will decrease significantly on subsequent runs!

### Running the HTML project
`gradlew html:superDev`

This will start your application in [GWT Super Dev Mode](http://www.badlogicgames.com/wordpress/?p=3073), which compiles your Java code to Javascript, and allows you to debug your Java code directly in the browser. If you see the message `Next, visit: http://localhost:9876` in your shell, open a browser and navigate to the address. Drag the "Dev Mode On" bookmarklet to your browser bookmarks bar. Next open [http://localhost:8080/index.html](http://localhost:8080/index.html). This is your application running in the browser! If you change any of your Java code in the core project, just click the bookmarklet, then click "Compile". The changes will take effect in a few seconds. If you modify your assets, you have to restart the server with the above command.

## Packaging the project
Every platform has a different kind of distribution format. In this section we'll see how we can generate those distributions via Gradle.

[**Desktop**](#packaging-for-the-desktop) - [**Android**](#packaging-for-android) - [**iOS**](#packaging-for-ios) - [**HTML**](#packaging-for-the-web) 

### Packaging for the desktop
`gradlew desktop:dist`

This will create a runnable JAR file located in the `desktop/build/libs/` folder. It contains all necessary code as well as all your art assets from the android/assets folder and can be run either by double clicking or on the command line via `java -jar jar-file-name.jar`. Your audience must have a JVM installed for this to work. The JAR will work on Windows, Linux and Mac OS X!

**If you want to package your JAR with a JVM for distribution (bundling), see [Bundling a JRE](https://github.com/libgdx/libgdx/wiki/Bundling-a-JRE)**

### Packaging for Android
`gradlew android:assembleRelease`

This will create an unsigned APK file in the `android/build/outputs/apk` folder. Before you can install or publish this APK, you must [sign it](http://developer.android.com/tools/publishing/app-signing.html). The APK build by the above command is already in release mode, you only need to follow the steps for keytool and jarsigner. You can install this APK file on any Android device that allows [installation from unknown sources](http://developer.android.com/distribute/open.html#unknown-sources). 

### Packaging for iOS

#### Pre-requisites
In order to upload the IPA to the app store, it must be signed with your distribution signature and linked to your provisioning profile. 
You can follow Apple's guide on [app store distribution](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html) to create provisioning profiles and certificates.
Once you have done that, you must define them in your root build.gradle file, in your IOS Project

```
project(":ios") {
    apply plugin: "java"
    apply plugin: "robovm"

    dependencies {

        compile project(":core")

        compile "com.mobidevelop.robovm:robovm-rt:$roboVMVersion"

        compile "com.mobidevelop.robovm:robovm-cocoatouch:$roboVMVersion"

        compile "com.badlogicgames.gdx:gdx-backend-robovm:$gdxVersion"

        compile "com.badlogicgames.gdx:gdx-platform:$gdxVersion:natives-ios"

        compile "com.badlogicgames.gdxpay:gdx-pay-iosrobovm-apple:$gdxPayVersion"

    }

    robovm {

        iosSignIdentity = "[Signing identity name]"

        iosProvisioningProfile = "[provisioning profile name]"

        iosSkipSigning = false

        archs = "thumbv7:arm64"

    }
 }
```

- Your provisioning profile name is available in your developer portal (where you created your provisioning profile).
- Your Signing identity name is available in your keychain, under "My Certificates"


#### Packaging

To create your IPA, run 

`gradlew ios:createIPA`



 
This will create an IPA in the `ios/build/robovm` folder that you distribute to the Apple App Store. 
To upload your app you will need to use the application loader within XCode (Xode->Open Developer Tool->Application loader) 



Note: as of iOS 11 instead of simply adding your icons into your data folder within your iOS project you need to include an asset Catalog.
If you do not include one, you can still submit your app but later you receive the following message:
> Dear developer,

> We have discovered one or more issues with your recent delivery for "". To process your delivery, the following issues must be corrected: Missing Info.plist value - A value for the Info.plist key CFBundleIconName is missing in the bundle ''. Apps that provide icons in the asset catalog must also provide this Info.plist key. For more information see http://help.apple.com/xcode/mac/current/#/dev10510b1f7. Once these issues have been corrected, you can then redeliver the corrected binary. Regards, The App Store team

To fix this, follow these [instructions to include an asset catalog](https://github.com/MobiVM/robovm/wiki/Howto-Create-an-Asset-Catalog-for-XCode-9-Appstore-Submission%3F)

### Packaging for the Web
`gradlew html:dist`

This will compile your app to Javascript and place the resulting Javascript, HTML and asset files in the `html/build/dist/` folder. The contents of this folder have to be served up by a web server, e.g. Apache or Nginx. Just treat the contents like you'd treat any other static HTML/Javascript site. There is no Java or Java Applets involved!

With Python installed, you can test your distribution by executing the following in the `html/build/dist` folder:

**Python 2.x**

`python -m SimpleHTTPServer`

**Python 3.x**

`python -m http.server 8000`

You can then open a browser to [http://localhost:8000](http://localhost:8000) and see your project in action.

With Node.js `npm install http-server -g` then `http-server html/build/dist` and browse at <http://localhost:8080>. [docs](https://github.com/indexzero/http-server)

With PHP you may type `php -S localhost:8000` and browse at <http://localhost:8080>. [docs](http://php.net/manual/en/features.commandline.webserver.php)

## Debugging and common problems
### Gradle tasks are failing
If when you invoke gradle, the build or refresh fails to get more information, run the same command again and add the --debug arguments to the command.
e.g
```./gradlew tasks --debug```
This will provide you with a stacktrace and give you a better idea of why gradle is failing.

### Common problems
(Confirmed) AVG - When running gradlew desktop:dist antivirus will cause this to fail. Add an exception to your antivirus to allow access.
### Debugging Projects
Start Gradle with the --debug-jvm parameter, and attach your debugger to the right port (5005 typically).
```./gradlew desktop:run --debug-jvm```

## Tweaking

You may be like me and wish to have the output jar from the dist task. Gradle seems to name it as the name of the directory and a version number, which is in my case, desktop-1.0. You may also wish to have each build have a unique version/build date of some kind, do as follows:

In the root project build.gradle, add:

    def getDate() {
        def date = new Date()
        def formattedDate = date.format('yyyyMMddHHmmss')
        return formattedDate
    }

(at top level)

Within allprojects{}, add: 'version = "0.1-build-" + getDate()'

Within the desktop target add this within task dist(type: Jar) { }...

'baseName = "myproject"'

I'm sure there are better ways, but it took me a while to get together that info and then get it to work/figure out where to put it. You will now have a jar file in your desktop/libs, named something like 'project-0.1-build-20150120033412.jar' which makes distribution a lot easier and more trackable, less conflicts etc.