###  ###

* [Forward](#forward)
* [Deploy to Windows/Linux/Mac](#deploy-to-windowslinuxmac-os-x)
 * [Users Java (JRE) is not installed or out of date](#users-java-jre-is-not-installed-or-out-of-date)
* [Deploy to Android](#deploy-to-android)
* [Deploy to iOS](#deploy-to-ios)
* [Deploy to HTML/JS](#deploy-to-htmljs)
    + [Deploy to HTML/JS Eclipse method](#deploy-to-htmljs-eclipse-method)
    + [Deploy to HTML/JS Other IDEs / Gradle method](#deploy-to-htmljs-other-ides--gradle-method)

### <a id="Forward"></a>Forward ###
The mechanism to deploy your game differs between platforms. This article aims to articulate what is necessary to deploy to each platform that Libgdx officially supports.

**_Note: if you are using gradle (recommended) you may prefer following the guidelines provided on [this page](https://github.com/libgdx/libgdx/wiki/Gradle-on-the-Commandline#packaging-the-project)._**

## <a id="Deploy_to_Windows/Linux/Mac"></a>Deploy to Windows/Linux/Mac OS X ##
The easiest way to deploy to Windows/Linux/Mac is to create a runnable JAR in Eclipse, which includes all of your code, the code of libgdx and any extensions, and your assets. To create a runnable JAR:

  1. Right click your desktop project, select "Export"
  2. In the next dialog, select "Java -> Runnable Jar", click "Next"

<img src="http://libgdx.badlogicgames.com/uploads/Screen%20Shot%202013-08-23%20at%2011.12.45-xTDyPTSMu2.png" width="400"></img>
  3. In the next dialog, specify the launch configuration you use to start the game on the desktop from within Eclipse, and select the output JAR file name

<img src="http://libgdx.badlogicgames.com/uploads/Screen%20Shot%202013-08-23%20at%2011.16.10-0STXrLHoAH.png" width="400"></img>

This will package all code and your assets into a single JAR file. To run the JAR file, a user can just double click it, provided she/he has Java (the JRE) installed and their version satisfies your target java byte code version. See below.

### Users Java (JRE) is not installed or out of date

Some users may not have the right version of JRE installed and would encounter issues/be unable to even launch your game. Additionally, perhaps you want to use Java 8 features, but some of your users only have JRE 7 or JRE 6, or not at all. One may also want to "hide" the fact that it's a Java app.

In such cases, a JRE can be bundled with your application, all in one. This will enable a single file to include everything any system can use to run the game.

See: [Bundling a JRE](https://github.com/libgdx/libgdx/wiki/Bundling-a-JRE)

## <a id="Deploy_to_Android"></a>Deploy to Android ##
Deploying to Android takes some extra steps. This tutorial is done using Eclipse and the Android ADT plugin.
- Right click your Android project and select "Export"
- Select "Android -> Export Android Application"
- Make sure the project you want to export is your project, select next
- Select or create your keystore. If you are new to Android development, a keystore is used to sign your applications **and is required for putting your application on the market.** More information is available at: http://developer.android.com/tools/publishing/app-signing.html
- Select your alias to sign with and enter the password. If you don't have an alias or want to create a new alias, select create new key and select next.
- Now select the destination of where you want your app being exported to. This will export an .apk file in the selected directory capable of being uploaded to Google Play (formerly Android Market) and alternative app stores.
- Select finish and your .apk will be built! 

**Notes**
- The .apk file can be used to install on devices directly such as via an email attachment or download. This requires the devices to have the 'Allow Unknown Sources' option enabled in settings. Some carriers unfortunately disable this setting.
- LibGDX is only capable of *officially* exporting to devices running Android 2.2 or higher. If you are using LibGDX 0.9.9, you can export to Android 1.5+ Note that features supported by devices can vary, such as OpenGL ES 2 may not be supported on certain devices. 
- OpenGL ES 2 requires Android 2.2 or higher and a graphics card supporting it.
- Features that are incompatible with the device such as trying to use immersive mode on a pre-kitkat device will simply not do anything on unsupported devices.
- If you want to upload to google play, here is a helpful link: http://developer.android.com/distribute/googleplay/publish/register.html
- LibGDX supports x86 Android devices starting with 0.9.9 and up. Make sure the x86 folder with libraries is in your project. Gdx-setup-ui should now generate your project with these. 

## <a id="Deploy_to_iOS"></a>Deploy to iOS ##
*This section assumes you're familiar with the basic deployment steps for iOS apps.*

Prerequisites:
 * An OSX machine with Xcode installed (or the equivalent Hackintosh) and an Apple developer license account all established and paid for
 * Your app is configured in `itunesconnect.apple.com` with a version number that matches your `ios/robovm.properties` file
 * You have a matching distribution certificate set up at `developer.apple.com` per usual

On your build machine, open Xcode, go to `Preferences -> Accounts`, provide your login and choose *View Details*. Tap the "Recycle" button at bottom left. This will download all necessary certificates to a place that the RoboVM Eclipse plugin expects to find them. Click *Done* and exit Xcode.

> *Insert more here about tweaking the files: info.plist.xml, robovm.properties and robovm.xml.*

Go into Eclipse, right click on your `projectname-ios` and choose `RoboVM Tools -> Package for App-Store/Adhoc Distribution`.  In the resulting dialog, pick a directory where you wish the IPA and associated files to be placed, choose your signing identity (usually your company, not your machine's identifier), and the provisioning profile for this app (from `developer.apple.com`).

This will generate the `projectname.IPA` file. Now you are able to use the `Application Loader` to locate your IPA and submit it to iTunesConnect. It will appear under the Prerelease tab for your app, per usual. Then you can distribute to your TestFlight testers etc., per the usual Apple procedure.


## <a id="Deploy_to_HTML/JS"></a>Deploy to HTML/JS##
#### <a id="Deploy_to_HTML/JS_Eclipse"></a>Deploy to HTML/JS Eclipse method
Deploying to HTML/JS is straightforward for most cases.
  1. Right click your HTML project and select "Google -> GWT Compile"
  2. Keep the default settings and click compile
  3. A window will appear asking you to select the WAR directory. It is in your HTML project's folder.
  4. GWT will proceed to cross-compile your code into javascript. It will take awhile, but you can view the progress by pulling up the console in Eclipse.

Once the compile is complete everything you need to run your game on the web will be inside the WAR directory. Unless you are using RPC calls or other server side operations in your game, there are only a few things left to do.
  1. Copy the "index.html" file, your assets folder, and the folder ending in "mygdxgame.GWTDefinition" to a directory on your web server
  2. Navigate to the URL and you should see a loading screen for your game

Notes:
  * If you are using server-side operations in your code, you will need to install Tomcat or similar software on your web server and place the full contents of your project's WAR directory in the "webapp" directory. More details [here.](https://tomcat.apache.org/tomcat-6.0-doc/appdev/deployment.html)  

#### <a id="Deploy_to_HTML/JS_Gradle"></a>Deploy to HTML/JS Other IDEs / Gradle method
In your project root, run the command `./gradlew html:dist` (Unix) or `gradlew.bat html:dist` (Windows) to build.

The result will be placed in the `html/build/dist` folder. You can symlink your webroot to this directory, or just copy/paste all the files into your webroot instead.

When running the result, you might encounter errors like `Couldn't find Type for class ...`. To fix this, please see our wiki page [Reflection](https://github.com/libgdx/libgdx/wiki/Reflection) and include the needed classes/packages.

Make sure you only add classes/packages you really need, because the more packages you include, the slower the build process gets. 

See more at [Gradle on the Commandline](https://github.com/libgdx/libgdx/wiki/Gradle-on-the-Commandline#packaging-for-the-web)