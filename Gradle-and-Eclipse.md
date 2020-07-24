You just [[generated your libgdx project|Project Setup Gradle]], now it's time to start developing its guts in Eclipse! 

**Before you can import your project into Eclipse, make sure you [[setup your development environment|Setting up your Development Environment (Eclipse, Intellij IDEA, NetBeans)]]!**

## Importing Your Project
Follow carefully the [Official Documentation](https://libgdx.badlogicgames.com/documentation/gettingstarted/Importing%20into%20IDE.html#eclipse) steps.

### Common Problems
Your Eclipse workspace folder must not be the same as your project's root folder (see this [issue](https://github.com/libgdx/libgdx/issues/1537)).

If you run into problems due to a missing validation-api:1.0.0.GA artifact, delete your Maven cache at `C:\Users\username\.m2` or `/Users/username/.m2` or `/home/username/.m2`.
[[images/URxvrYe.png]]

When importing your first project you receive the error:
`com/github/jtakakura/gradle/plugins/robovm/RoboVMPlugin : Unsupported major.minor version 51.0`
Make sure you are running Oracle's JDK version 7 or above.

## Running Your Project ##

  * **Desktop**: Right click the desktop project, `Run As -> Java Application`. Select the desktop starter class (e.g. DesktopLauncher.java).
  * **Android**: make sure you have a device connected and that it shows up in DDMS (see  [Android Developer Guide](http://developer.android.com/guide/index.html)). Right click your Android project, `Run As -> Android Application`.
  * **BlackBerry 10**: You need have the BlackBerry Eclipse plugin installed (http://developer.blackberry.com/android/documentation/install_bb_plugin_adt.html). You also need a BlackBerry ID signing token, which you can get by following [these](https://developer.blackberry.com/android/documentation/install_bb_plugin_adt.html#alk1340635272845) steps. Right click the project and under BlackBerry Tools -> Start ADB Proxy Manager. In there you can click the play button on your device. After it says "Running at port #49570" the device will show up in the ADB devices list and you can run it just like a real Android device.
  * **iOS RoboVM**: Before you can run/debug on iOS, make sure you have the [RoboVM Eclipse plugin](https://github.com/libgdx/libgdx/wiki/Setting-up-your-Development-Environment-(Eclipse,-Intellij-IDEA,-NetBeans)#setup-for-ios-development) installed. Once the plugin is installed, right click the robovm project, `Run As -> iOS Device App` to run on a connected device, or `Run As -> iOS Simulator App` to run on the iOS simulator. If you run on a device, you need to [provision](http://docs.robovm.com/getting-started/provisioning.html) it to be able to deploy to it!
  * **iOS Multi-OS Engine**: the Multi-OS Engine supports Eclipse. [Get Started](https://multi-os-engine.org/start/)
  * **HTML5**: Right click the html project, `Run As -> External Tools Configuration`. Create a new configuration by double clicking the `Program` entry in the left sidebar. Give the configuration a name, e.g. GWT SuperDev. Set the location field to the `gradlew.bat` (Windows) or `gradlew` (Linux, Mac) file. Set the working directory to the root folder of your project. Specify `html:superDev` as the Argument. Press 'Apply', then 'Run'. Wait until you see the message `The code server is ready.` in the console view, then open the URL [http://localhost:8080/html](http://localhost:8080/html). You can leave the server running. If you change code or assets, simply click the `SuperDev Refresh` button in the browser. This will recompile your app and reload the site.

Once this [bug in the Gradle tooling API](http://issues.gradle.org/browse/GRADLE-1539) is fixed, we can simplify running the HTML5 by using the Gradle integration. At the moment, the Gradle process will run forever even if canceled.

## Debugging Your Project ##
Follow the steps for running the project, but instead of launching via `Run as`, launch your configuration via `Debug as`. Debugging of the html build can be done in the browser as follows:

Run the superDev run configuration as before. Go to [http://localhost:8080/html](http://localhost:8080/html), click the `SuperDev Refresh`button. In Chrome, press `F12` to bring up the developer tools, go to the sources tab and find the Java file you want to debug. Set breakpoints, step and inspect variables using the power of source maps! Click the `SuperDev Refresh` button in case you changed code or assets (leave the server process running!).

[[https://libgdx.badlogicgames.com/uploads/Screen%20Shot%202014-03-23%20at%2019.11.27-BkaIpjttPQ.png]]

## Packaging Your Project
It's easiest to package your application from the command line, or using the Gradle tasks from within Eclipse. To see the relevant Gradle tasks, check the [[Gradle command line documentation|Gradle on the Commandline]].