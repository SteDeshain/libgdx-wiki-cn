### Attention: this is extremely outdated and NOT supported anymore! Please use the [Gradle-based setup](https://github.com/libgdx/libgdx/wiki/Project-Setup-Gradle) instead

Libgdx is very Eclipse centric due to its Android and HTML5/GWT back-ends. This article discusses how to setup libgdx projects for Eclipse. Check out this [[Using libgdx with IntelliJ IDEA]] if you use IntelliJ. If you prefer to use a different IDE, please refer to the bottom of this page which outlines the dependencies in detail.

## Prerequisites ##
To target **Windows, Linux, Mac OS X, Android and HTML5** you will need:

  * [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
  * [Eclipse](http://www.eclipse.org/downloads/), the "Eclipse IDE for Java Developers" is usually sufficient.
  * [Android SDK and Eclipse ADT Plugin](http://developer.android.com/sdk/installing.html), simply follow the installation instructions. If you work from source, make sure to install **all** platforms in the Android SDK manager!
  * [Google Web Toolkit](https://developers.google.com/web-toolkit/usingeclipse)

To **additionally target iOS** you will also need:

  * A Mac, iOS Development does not work on Windows/Linux thanks to Apple.
  * The latest XCode, which you can get from the Mac OS X App Store for free
  * [RoboVM](http://www.robovm.org/docs.html#eclipse), simply install the Eclipse plugin, or use the command line version.

## Tutorial Video ##

Follow this if you don't want to read a lot of text. Otherwise continue below.


<a href="http://www.youtube.com/watch?feature=player_embedded&v=mmH05k6HFsI
" target="_blank"><img src="http://img.youtube.com/vi/mmH05k6HFsI/0.jpg" 
alt="Libgdx setup video" width="480" height="360" border="10" /></a>


## Using "Libgdx Setup" ##
As of version 0.9.3, libgdx comes with an easy to use application called "Libgdx Setup" by [Aurelien Ribon](http://www.aurelienribon.com/blog/). The following steps will create everything needed to develop for the desktop, android, ios and html5.

  * Download the gdx-setup.jar from http://libgdx.badlogicgames.com/nightlies/dist/gdx-setup.jar. The jar is also contained in all stable and nightly releases.
  * Execute the jar by double clicking it or from the command line via `java -jar gdx-setup.jar`
    * Specify your project's configuration (Configuration Panel)
    * Specify the libgdx stable/nightly release zip file or press one of the buttons to download the latest stable/nightly release (Library Selection Panel).
    * Hit the "Generate projects" button (Generation Panel)
  * Open Eclipse and import the generated projects into your workspace 
    * `File -> Import -> Existing Projects into Workspace`
    * Click "Browse", select the folder containing the generated projects.
    * Make sure all the projects are checked, then click "Finish"
    * To fix the error of the HTML5/GWT project, go to the "Problems" view, right click the error message "The GWT SDK JAR gwt-servlet.jar is missing in the WEB-INF/lib directory" and select "Quick Fix". Click "Finish".

## Project Layout ##
Assuming you named your project "my-gdx-game" in the Setup UI, you will end up with 4 projects:

  * **Core project ("my-gdx-game")**: contains all the code of your application, minus the so called starter classes. All other projects link to this project. This project is usually referred to as the "core project".
  * **Android project ("my-gdx-game-android")**: contains the starter class and other necessary files to run your application on Android. The assets/ folder stores the assets of your application for all platforms.
  * **Desktop project ("my-gdx-game-desktop")**: contains the starter class to run your application on the desktop. Links to the Android project's assets/ folder as well as the core project.
  * **HTML5 project ("my-gdx-game-html")**: contains the starter class and other necessary files to run your application as a native HTML5 application. Links to the Android project's assets/ folder (see gwt.xml file) as well as to the core project.
  * **iOS RoboVM project ("my-gdx-game-robovm")**: contains the starter classes and other necessary files to run your application on iOS through [RoboVM](http://www.robovm.org/). Links to the Android project's assets/ folder (see robovm.xml) as well as the core project.

All your application's assets should be stored in the Android project's assets folder. The desktop, iOS and HTML5 projects link to this folder, so there is no need to store duplicates of these assets.

## Running Your Application ##

  * **Desktop**: Right click the desktop project, `Run As -> Java Application`. Select the desktop starter class (e.g. Main.java).
  * **Android**: make sure you have a device connected and that it shows up in DDMS. If you do not know what this means, please refer to the [Android Developer Guide](http://developer.android.com/guide/index.html). Right click your Android project, `Run As -> Android Application`.
  * **iOS RoboVM**: Right click the robovm project, `Run As -> iOS Device App` to run on a connected device, or `Run As -> iOS Simulator App` to run on the iOS simulator. If you run on a device, you need to [provision](https://developer.apple.com/library/ios/documentation/ToolsLanguages/Conceptual/YourFirstAppStoreSubmission/ProvisionYourDevicesforDevelopment/ProvisionYourDevicesforDevelopment.html) it to be able to deploy to it!
  * **HTML5**: there are two modes in which you can run your application.
    * development/hosted mode: runs the actual Java code via GWT plugin magic and allows debugging and hot swapping code (kind of).
      * Right click the HTML5 project, `Run As -> Web Application`. 
      * In the "Web Development" view, click the URL. Your standard browser will open up. If you have not installed the GWT plugin yet, you will be prompted to do so. After installation of the plugin, simply refresh the site in the browser.
    * production/web mode: you will need a web server that serves the compiled files (e.g. [Nginx](http://nginx.org/) or a browser that allows to execute JavaScript from local files.
      * Right click the HTML5 project, `Google -> GWT Compile`
      * Once compiled, place the contents of the war/ directory into a directory your web server can serve. Omit the WEB-INF folder.
      * Point your browser at an URL which will display the `.html` file.

## Debugging Your Application ##

  * **Desktop**: Right click the desktop project, `Debug As -> Java Application`. Select the desktop starter class (e.g. Main.java). Set breakpoints where you need them, hot swap code (edit & save while debugging) to quickly test changes without restarting the app.
  * **Android**: assuming you have a device connected, `Debug As -> Android Application`. Hot swapping is not supported, but you can use the DDMS perspective to check your memory usage, profile your application and so on. Refer to the Android Development Guide for more information.
  * **iOS RoboVM**: debugging is currently not supported by RoboVM
  * **HTML5**:
    * development/hosted mode: runs the actual Java code via GWT plugin magic and allows debugging and hot swapping code (kind of).
      * Right click the HTML5 project, `Debug As -> Web Application`. 
      * In the "Web Development" view, click the URL. Your standard browser will open up. 
      * Set breakpoints in your Java code. If you edit & save code, you'll have to refresh your browser for the changes to take effect. This will essentially restart your application. It's faster than recompiling though.
    * production/web mode: 
      * Right click the HTML5 project, `Google -> GWT Compile`
      * Change the flags in "Advanced" to `-strict -draftCompile` and the "Output Style" to "detailed". This will produce very readable Javascript that you can map almost 1:1 to your Java code.
      * Once compiled, place the contents of the war/ directory into a directory your web server can serve. Omit the WEB-INF folder.
      * Point your browser at an URL which will display the `.html` file.
      * Open up Firebug or similar tools, go to the JavaScript and set breakpoints, watch the callstack and so on. To find a Java Class method, simply search for the package/class/method name.
    * super dev mode: through the power of source maps, you can also debug compiled JS as if it where Java, directly in your browser. See this blog post on [libgdx and super dev mode](http://www.badlogicgames.com/wordpress/?p=3073)

## Manual Project Setup ##
Please refer to [Manual Project Setup](https://github.com/libgdx/libgdx/wiki/Manual-project-setup)