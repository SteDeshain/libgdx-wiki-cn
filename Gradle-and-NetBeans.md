You just [[generated your libgdx project|Project Setup Gradle]], now it's time to start developing its guts in NetBeans! Before you can import your project into NetBeans, make sure you [[setup your development environment|Setting up your Development Environment (Eclipse, Intellij IDEA, NetBeans)]]!

## Importing Your Project
Follow carefully the [Official Documentation](https://libgdx.badlogicgames.com/documentation/gettingstarted/Importing%20into%20IDE.html#netbeans) steps.


### Common Problems
If you run into problems due to a missing validation-api:1.0.0.GA artifact, delete your Maven cache at `C:\Users\username\.m2` or `/Users/username/.m2` or `/home/username/.m2`.

## Running Your Project
In the project view: 

  * **Desktop**: Right click the desktop project, `Run`. You can run it.
  * **Android**: make sure you have a device connected. Right click your Android project, `Tasks -> installDebug`.
  * **iOS RoboVM**: Right click the robovm project, `Tasks -> launchIPhoneSimulator` (alternatives are `launchIPadSimulator` and `launchIOSDevice`for [provisioned devices](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html))
The first run will take a bit longer as RoboVM has to compile the entire JDK for iOS. Subsequent runs will compile considerably faster!
  * **HTML5**: Right click the html project, `Tasks -> superDev`. You can now follow the build process in the console. This will take a while, as your Java code is compiled to Javascript. Once you see the message `The code server is ready`, fire up your browser and go to [http://localhost:8080/html/](http://localhost:8080/html/). This is your app running in the browser! When you change any of your Java code or assets, just click the `SuperDev Refresh` button while you are on the site and the server will recompile your code and reload the page!

## Debugging Your Project ##
Follow the steps for running the project, but instead of launching via `Run`, launch your configuration via `Debug`. Note that RoboVM currently does not support debugging. Debugging of the html build can be done in the browser as follows:

Run the superDev Gradle task as before. Go to [http://localhost:8080/html](http://localhost:8080/html), click on the `SuperDev Refresh` button and hit `Compile`. In Chrome, press `F12` to bring up the developer tools, go to the sources tab and find the Java file you want to debug. Set breakpoints, step and inspect variables using the power of source maps!

[[images/Screen%20Shot%202014-03-23%20at%2019.11.27-BkaIpjttPQ.png]]

## Packaging Your Project
It's easiest to package your application from the command line, or using the Gradle tasks from within NetBeans. To see the relevant Gradle tasks, check the [[Gradle command line documentation|Gradle on the Commandline]].