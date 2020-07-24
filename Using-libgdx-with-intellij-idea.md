The newest, and easiest way to setup libgdx for use with IntelliJ IDEA is to use the gradle build system.
[A detailed tutorial can be read here](http://www.gamefromscratch.com/post/2013/12/01/Using-LibGDX-with-IntelliJ-with-Gradle.aspx)

First there will be a short version, outlining only the major steps. This is for people familiar with IntelliJ IDEA or those that have set up libgdx project before and only need a small recap. Below there is a step-by-step guide with screenshots.

Following this guide will enable you to use IntelliJ IDEA while others on your team use the standard Eclipse setup.
Note that a _workspace_ in Eclipse is  a _project_ in IntelliJ IDEA, and that a _project_ in Eclipse is a _module_ in IntelliJ IDEA.

There is also a github repo containing a pre-built skeleton project for libgdx + IntelliJ that you can clone for a quick start: https://github.com/jrenner/libgdx-intellij-project-skeleton

## Quick reference ##

  1. Create a new project from scratch, name it and name the module _Main_.
  2. Add a new module named _Desktop_.
  3. Add a second module, name it _Android_ and select the _Android Module_ type.
  4. Create directories in the Desktop and Main module and name them _libs_.
  5. Add the following files and directories from the libgdx zip:
    * Main/libs:
      * gdx.jar
      * sources/gdx-sources.jar
    * Desktop/libs:
      * gdx-natives.jar
      * gdx-backend-lwjgl.jar
      * gdx-backend-lwjgl-natives.jar
    * Android/libs:
      * gdx-backend-android.jar
      * armeabi (directory)
      * armeabi-v7a (directory)
  6. Open up the Module Settings and add the content of each libs-directory to the corresponding module.
  7. Make the Main module export its libraries.
  8. Give the Android module a Module Dependency pointing to the Main module. 
  9. Do the same for the Desktop module.
  10. Add starter classes.
  11. Remember to configure the manifest.
  12. Link the assets between Android module and Main module:
    * Make a run configuration for the DesktopStarter and set the Working directory to be the Android/assets directory.
    * *_OR_*
    * Create a directory named _data_ in the main module, and make a sym-link from the assets directory in the Android module.
  13. Create an awesome app!


----
## Step-by-step guide with screenshots ##
The screenshots were taken when running IntelliJ IDEA Ultimate 11.1.1 on Windows, so everything may not look exactly the same on your computer.

The project in this guide is located in _C:\Android_ and named _MyLibgdxGame_. All the code for the game will be in the _Main module_.

### Preparing ##
  * You will need to install the [Android SDK](http://developer.android.com/sdk/index.html) and the Android versions you want to target.

  * Make sure the Android plugin in IntelliJ IDEA is enabled. _File->Settings->Plugins_

[[images/using-libgdx-with-intellij-idea01.png]]

### Creating the project ###
  * Select _File->New Project_
  * Select _Create project from scratch_ in the dialog and press _Next_.
  * In the next screen, enter the name _(1)_ and select the location _(2)_ of the project. Give the module to be created the name _Main_ _(3)_ and then press _Next_.

[[images/using-libgdx-with-intellij-idea02.png]]

  * In the next screen, select _Create source directory_, name it _src_ and press _Next_.
  * Press _Finish_ in the last screen.

### Creating the Desktop module ###
  * Select _File->New Module_ _(1)_.
  * Select _Create module from scratch_ and press _Next_.
  * Name it _Desktop_ _(2)_ and press _Next_.

[[images/using-libgdx-with-intellij-idea03.png]]

  * Select _Create source directory_, name it _src_ and press _Next_.
  * In the next screen press _Finish_.

### Creating the Android module ###
  * Again, select _File->New Module_.
  * Select _Create module from scratch_ and press _Next_.
  * In this screen, give the module the name _Android_ _(1)_ and select the _Android Module_ type _(2)_. Then press _Next_.
  
[[images/using-libgdx-with-intellij-idea04.png]]  

  * Select _Create source directory_, name it _src_ and press _Next_.
  * In the next screen, select the _Android SDK_ you want to target _(1)_. Give the application a name _(2)_, a package and create an activity _(3)_. If you have no Android SDK in the drop down menu, or would like to add more, read below.
  
[[images/using-libgdx-with-intellij-idea05.png]]
  * Select a run configuration and press _Finish_.

### Adding Android SDKs to IntelliJ IDEA ###
This step is only if you need a new Android SDK to select when creating the Android module.
  * Press the [...] button next to the drop down ( _(4)_ in the previous picture).
  * In the new window that appears, press the [+] button _(1)_, select _Android SDK_ _(2)_.
  * Yet another window will open. Here, select where you installed the Android SDK _(3)_ and press OK _(4)_.
  * Select _Java 1.6_ if prompted, and then select the Android SDK you would like to target _(5)_ and press OK _(6)_.
  
[[images/using-libgdx-with-intellij-idea06.png]]

  * If the Android version you would like to target is not in list _(5)_, you will need to download it through the Android SDK tool first.

### Add the libgdx files ###

  * Right click the Desktop module _(1)_, select _New->Directory_ _(2)_. Name the directory _libs_ and press _OK_.

  * Do the same for the Main module _(3)_.
 
[[images/using-libgdx-with-intellij-idea07.png]]

  * Download libgdx, either the last stable or the [nightlies](http://libgdx.badlogicgames.com/nightlies/).

  * In the _libs_ folder for the Main module, place these files from the libgdx zip file:
    * gdx.jar
    * sources/gdx-sources.jar 

  * In the _libs_ folder for the Desktop module, place these files from the libgdx zip file:
    * gdx-natives.jar
    * gdx-backend-lwjgl.jar
    * gdx-backend-lwjgl-natives.jar

  * In the _libs_ folder for the Android module, place these files and directories from the libgdx zip file:
    * gdx-backend-android.jar
    * armeabi (the whole directory)
    * armeabi-v7a (the whole directory)

  * The picture below shows how the libraries should be placed when done with this step.

[[images/using-libgdx-with-intellij-idea08.png]]

### Add the libraries ###

  * Right click the Main module and select _Open Module Settings_.
  * Select _Libraries_ _(1)_ in the first panel.
  * Press the [+] _(2)_ above the second column and choose _Java_ if prompted.
  * Select the _libs_ directory of the Android module _(3)_ and press _OK_.
  * Select the Android module _(4)_ and press _OK_ again.
  * Name it _Androidlibs_ _(5)_.

[[images/using-libgdx-with-intellij-idea09.png]]

  * Press the [+] _(2)_ again, but this time select the _libs_ directory of the Desktop module in _(3)_, select the Desktop module in  _(4)_ and name it _Desktoplibs_ in _(5)_.
  * Press the [+] _(2)_ again, but this time select the _libs_ directory of the Main module in _(3)_, select the Main module in  _(4)_ and name it _Mainlibs_ in _(5)_.

### Adding dependencies ###
  * Select _Modules_ _(1)_ in the first panel.
  * Select the Main module _(2)_ in the second panel.
  * Select the Dependencies tab _(3)_ in the third panel.
  * Check _Export_ in front of Mainlibs _(4)_
  
[[images/using-libgdx-with-intellij-idea10.png]]

  * Select the Desktop module _(5)_ in the second panel.
  * Press the [+] to the right _(6)_.
  * Select _Module Dependency_ _(7)_.
  * Select the Main module _(8)_ and press _OK_.
  * Select the Android module _(9)_ in the second panel and repeat _(6), (7) and (8)_.

[[images/using-libgdx-with-intellij-idea11.png]]

  * Press _OK_ to close the Project Structure window.

### Add the starter classes ###
  * Right click the Main module's _src_ directory and select _New->Package_. Give it a name, here _com.example.mylibgdxgame_ is used.
  * Right click the new package and select _New->Java Class_ and name it. Here _MyLibgdxGame_ is used.
  * Make the class implement _ApplicationListener_, or extend a class that does so. E.g. make it extend _Game_.

```java
public class MyLibgdxGame extends Game {
    @Override
    public void create() {
    }
}
```

  * This is the entry point for your game.

 
  * Right click the source directory in the Desktop module and add a new package here as well.
  * Then add a Java Class to that package and name it _DesktopStarter_.
  * Make the class look like the run configuration below, substitute the name of the game with the class you created in the Main module.

```java
public class DesktopStarter {
    public static void main(String[] args) {
        LwjglApplicationConfiguration cfg = new LwjglApplicationConfiguration();
        cfg.title = "Title";
        cfg.width = 800;
        cfg.height = 480;
        new LwjglApplication(new MyLibgdxGame(), cfg);
    }
}
```

 
  * Open up the class in the Android module, which was created when we created the Android module.
  * Instead of extending _Activity_, the class should extend _AndroidApplication_.
  * Add _initialize_ and run configurations like below:

```java
public class MyLibgdxGameAndroidStarter extends AndroidApplication
{
    @Override
    public void onCreate(Bundle savedInstanceState)
    {
        super.onCreate(savedInstanceState);
        AndroidApplicationConfiguration cfg = new AndroidApplicationConfiguration();
        cfg.useAccelerometer = false;
        cfg.useCompass = false;
        cfg.useWakelock = true;
        initialize(new MyLibgdxGame(), cfg);
    }
}
```

See [[Starter Classes & Configuration]] on how to use these settings and how to set up the manifest.

### Linking the asset directories ###
Android is a bit picky when it comes to assets, so it needs to have all the assets in its own assets-directory and will not fetch assets from the Main module (as the Desktop module would).

This leaves us with three options:
  1. (bad) Keep a copy of each asset in the Android/assets folder, and one copy in the Main/data folder. This can be a lot of work, and you will get crashes every time you try to launch and haven't copied the latest assets.  
  2. (good) Make the Desktop module use the Android/assets directory as its working directory. When collaborating with people using the Eclipse set up, this may be the preferred way as you don't have to alter any files or directories in the project.
  3. (good) Create a sym-link between the two directories in your OS. This will make both the directories be "the same" directory.

#### Alternative 2: Working directory ####
  * Select _Run->Edit Configurations..._ _(1)_.

[[images/using-libgdx-with-intellij-idea12.png]]

  * Press the [+] button _(2)_ and select _Application_ _(3)_.
  * Give the configuration a name _(4)_.
  * Select the _DesktopStarter_ class _(5)_.
  * Set the Working directory to the _Android\assets_ directory _(6)_.
  * Select the Desktop module _(7)_.

[[images/using-libgdx-with-intellij-idea13.png]]

#### Alternative 3: Sym-link ####
See [this image on how to set up sym-link](images/using-libgdx-with-intellij-idea14.png) in windows. (will need to run Command Prompt as admin). Linux/OSX can do the same with the _ln -s_-command.

*Note:* You will need a _data_-directory in the Main module, and you will need to prefix all your assets with _data/_.

And see [this image on the run configuration](images/using-libgdx-with-intellij-idea15.png) you should use.
  
----
## Tips for using IntelliJ IDEA ##
_CTRL + Q_ when having the caret positioned over any method or variable or when viewing quick-suggestions will bring up the Javadoc.

[[images/using-libgdx-with-intellij-idea16.png]]

_CTRL + SHIFT + I_ will show the code for a method in a small window, nice when you want to quick check the inner workings of a gdx class. This works because we added the gdx-sources.jar

[[images/using-libgdx-with-intellij-idea17.png]]

_CTRL + LEFT MOUSE CLICK_ will open up the code in the editor.

_CTRL + P_ will show all parameters to a method.