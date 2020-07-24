# Creating a libgdx project

**If you haven't done yet, make sure you've read the [Official Documentation](https://libgdx.badlogicgames.com/documentation/gettingstarted/Creating%20Projects.html) on how to create a libGDX project.**

Libgdx provides a simple wizard tool (`gdx-setup.jar`) allowing you to easily get started.
This will create a gradle ready project, which can then be imported into your IDE (Android Studio, Eclipse..).

1. Download [LibGDX Project setup tool "gdx-setup.jar"](https://libgdx.badlogicgames.com/nightlies/dist/gdx-setup.jar)
2. Open your command line tool, go to the download folder and run <br>`java -jar ./gdx-setup.jar`

This will open the following setup that will allow you to generate your project<br>

![screen capture of gdx-setup.jar](http://i.imgur.com/nI5lQKT.jpg)

You are asked to provide the following parameters:
* **Name**: the name of the application, lower-case with minuses is usually a good idea, e.g. mygame
* **Package**: the Java package under which your code will live, e.g. com.badlogic.mygame
* **Game Class**: the name of the main game java class of your app, e.g. MyGame
* **Destination**: Folder where your app will be created
* **Android SDK**: the location of your android sdk. With Android Studio, to find out where it is, start Android Studio and click "Configure"->"SDK Manager". By default it is in /Users/username/Library/Android/sdk
![Android studio 1](http://i.imgur.com/re4m4ZW.png)
![Android studio 2](http://i.imgur.com/Y4F3UsH.png)

* **Sub Projects**: LibGDX is crossplatform. By default all the target platform except ios-moe are included (Desktop; Android; iOS; HTML). No need to change the default value unless you are sure you will never compile for a specific target. ios-moe is an alternative to robovm supported by Intel.

* **extensions**: the extensions to include:<br>
    **[Bullet](https://github.com/libgdx/libgdx/wiki/Bullet-physics)**: 3D Collision Detection and Rigid Body Dynamics Library.<br>
    **[FreeType](https://github.com/libgdx/libgdx/wiki/Gdx-freetype)** Scalable font. Great to manipulate font size dynamically. However be aware that it does not work with HTML target if you cross compile for that target.<br>
    **[Tools](https://libgdx.badlogicgames.com/tools.html)** Set of tools including: particle editor (2d/3d), bitmap font and image texture packers.<br>
    **[Controller](https://github.com/libgdx/libgdx/wiki/Controllers)** Library to handle controllers (e.g.:XBox 360 controller).<br>
    **[Box2d](https://github.com/libgdx/libgdx/wiki/Box2d)**: Box2D is a 2D physics library.<br>
    **[Box2dlights](https://github.com/libgdx/box2dlights)**: 2D lighting framework that uses box2d for raycasting and OpenGL ES 2.0 for rendering.<br>
    **[Ashley](https://github.com/libgdx/ashley)**:A tiny entity framework.<br>
    **[Ai](https://github.com/libgdx/gdx-ai)**: An artificial intelligence framework.<br>

by clicking "Show Third Party Extensions" you can access the list of other popular LibGDX extensions

Note that the Advanced button lets you set the project generation to generate Eclipse and/or IDEA projects **without** Gradle integration, as described in more detail in the wiki article about [workflow without Gradle](Improving-workflow-with-Gradle#how-to-remove-gradle-ide-integration-from-your-project), as well as options to use an alternative repository to Maven Central and to not force downloading dependencies. You do not need to change the advance settings if you are a beginner.

When ready, click "Generate". 

note: You may get a message indicating that you have a more recent version of android build tools or android API than the recommended. This is not a blocking message and you may continue.

**Now you are ready to import the project into your IDE, run, debug and package it!**

  * [[Eclipse|Gradle and Eclipse]]
  * [[Intellij IDEA and Android studio|Gradle and Intellij IDEA]]
  * [[NetBeans|Gradle and NetBeans]]
  * [[Commandline|Gradle on the Commandline]]

# Additional reading on LibGDX projects

This section provides additional information regarding:
* How to create a project from the command line, without the wizard.
* The structure of a libGDX project.
* What is Gradle

## Creating a libgdx project using the command line

This section is to create your project from the command line. This is not required if you use the wizard above.
IF you run it from the command line, specify the following arguments.

* **dir**: the directory to write the project to, relative or absolute
* **name**: the name of the application, lower-case with minuses is usually a good idea, e.g. mygame
* **package**: the Java package under which your code will live, e.g. com.badlogic.mygame
* **mainClass**: the name of the main ApplicationListener of your app, e.g. MyGame
* **sdkLocation**: the location of your android sdk, Intellij uses this if ANDROID_HOME is not set
* **excludeModules**: the modules to exclude (Desktop; Android; iOS; HTML) separated by ';' and not case sensitive, e.g. Android;ios. Optional. Default it includes all the modules
* **extensions**: the extensions to include (same name as in GUI: Bullet; Freetype; Tools; Controllers; Box2d; Box2dlights; Ashley; Ai) separated by ';' and not case sensitive, e.g. box2d;box2dlights;Ai. Optional

Putting it all together, you can run the project generator on the command line as follows:

`java -jar gdx-setup.jar --dir mygame --name mygame --package com.badlogic.mygame --mainClass MyGame --sdkLocation mySdkLocation [--excludeModules <modules>] [--extensions <extensions>]`

### Project layout
This will create a directory called `mygame`with the following layout:

```
settings.gradle            <- definition of sub-modules. By default core, desktop, android, html, ios
build.gradle               <- main Gradle build file, defines dependencies and plugins
gradlew                    <- local gradle wrapper
gradlew.bat                <- script that will run Gradle on Windows
gradle                     <- script that will run Gradle on Unix systems
local.properties           <- Intellij only file, defines android sdk location

core/
    build.gradle           <- Gradle build file for core project*
    src/                   <- Source folder for all your game's code

desktop/
    build.gradle           <- Gradle build file for desktop project*
    src/                   <- Source folder for your desktop project, contains Lwjgl launcher class

android/
    build.gradle           <- Gradle build file for android project*
    AndroidManifest.xml    <- Android specific config
    assets/                <- contains for your graphics, audio, etc.  Shared with other projects.
    res/                   <- contains icons for your app and other resources
    src/                   <- Source folder for your Android project, contains android launcher class

html/
    build.gradle           <- Gradle build file for the html project*
    src/                   <- Source folder for your html project, contains launcher and html definition
    webapp/                <- War template, on generation the contents are copied to war. Contains startup url index page and web.xml


ios/
    build.gradle           <- Gradle build file for the ios project*
    src/                   <- Source folder for your ios project, contains launcher
```
\* These scripts contain tasks that package natives and distribute your applications on the respective platforms, you can add/maintain these tasks yourself, but only do so if you are familiar with Gradle, and what these tasks are doing, otherwise you will break your project.

Here is a good [tutorial](http://www.todroid.com/android-gdx-game-creation-part-i-setting-up-up-android-studio-for-creating-games/) on how to install libGDX using a tool provided by Bad Logic.

### What is Gradle?
[Gradle](http://www.gradle.org/) is a dependency management and build system. 

A dependency management system is an easy way to pull in 3rd party libraries into your project, without having to store the libraries in your source tree. Instead, the dependency management system relies on a file in your source tree that specifies the names and versions of the libraries you need to be included in your application. Adding, removing and changing the version of a 3rd party library is as easy as changing a few lines in that configuration file. The dependency management system will pull in the libraries you specified from a central repository (in our case [Maven Central](http://search.maven.org/)) and store them in a directory outside of your project.

A build system helps with building and packaging your application, without being tied to a specific IDE. This is especially useful if you use a build or continuous integration server, where IDEs aren't readily available. Instead, the build server can call the build system, providing it with a build configuration so it knows how to build your application for different platforms.

In case of Gradle, both dependency management and build system go hand in hand. Both are configured in the same set of files. See the [[Dependency management with Gradle]] and "Packaging" sections below for more information.