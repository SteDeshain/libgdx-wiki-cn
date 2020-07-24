### Attention: this is extremely outdated and NOT supported anymore! Please use the [Gradle-based setup](https://github.com/libgdx/libgdx/wiki/Project-Setup-Gradle) instead

*Note: Java 1.7 currently causes problems with Android. Please make sure your project uses Java 1.6*
*Note: use of setup-ui is highly recommended if you want to deploy to iOS*

This page describes how to configure Eclipse to develop and run libgdx applications on both the desktop and Android.

## Main project setup ##

  1. Create a new Java project in Eclipse: File -> New -> Project -> Java Project. Give it a name and click Finish.
  2. Go to the project folder on the file system and create a sub-folder named `libs`. Download the [`libgdx-nightly-latest.zip`](http://libgdx.badlogicgames.com/nightlies/) and place `gdx.jar` and `gdx-sources.jar` in the libs folder.
  3. In Eclipse, right click the project -> Refresh. Right click again -> Properties -> Java Build Path -> Libraries -> Add JARs, select `gdx.jar` and click OK.
  4. Under the `gdx.jar` file, double click "Source attachment", click Workspace, select the `gdx-sources.jar` and click OK.
  5. Click the Order and Export tab, check the `gdx.jar` and click OK.

_Note: Step 5 makes `gdx.jar` a transitive dependency. That means that projects that depend on the main project will also have `gdx.jar` on their classpath. However, this doesn't work for Android projects._

## Desktop project setup ##

  1. Create a new Java project in Eclipse: File -> New -> Project -> Java Project. Name it appropriately (e.g., "gamename-desktop") and click Finish.
  2. Go to the project folder on the file system and create a subfolder named `libs`. From the [`libgdx-nightly-latest.zip`](http://libgdx.badlogicgames.com/nightlies/), place `gdx-natives.jar`, `gdx-backend-lwjgl.jar` and `gdx-backend-lwjgl-natives.jar` in the libs folder.
  3. In Eclipse, right click the project -> Refresh. Right click again -> Properties -> Java Build Path -> Libraries -> Add JARs, select the three JARs, and click OK.
  4. Click the Projects tab, click Add, check the main project, and click OK.

## Android project setup ##

Before doing these steps, the [Android SDK](http://developer.android.com/sdk/installing.html) must first be installed.

  1. Create a new Android project in Eclipse: File -> New -> Project -> Android Project. Name it appropriately (e.g., "gamename-android"). For build target, check "Android 1.5". Specify a package name (e.g., "com.gamename"). Next to "Create Activity" enter "AndroidGame". Click Finish.
  2. Go to the project folder on the file system and create a subfolder named `libs`. From the [nightly zip](http://libgdx.badlogicgames.com/nightlies/), place `gdx-backend-android.jar` and the `armeabi` and `armeabi-v7a` folders in the libs folder.
  3. In Eclipse, right click the project -> Refresh. Right click again -> Properties -> Java Build Path -> Libraries -> Add JARs, select `gdx-backend-android.jar` and click OK.
  4. Click Add JARs again, select `gdx.jar` under the main project and click OK.
  5. On the Projects tab: click Add, check the main project, and click OK.
  6. On the Order and Export tab: check the main project.

_Note: The folder really must be called "libs", a naming convention forced on us by the Android Eclipse plugin._

## Asset folder setup ##

The Android project has a subfolder named `assets`, which is created automatically. Files available to the Android application must be placed here. This is problematic, because these same files must be available to the desktop application. Rather than maintain two copies of all the files, the desktop project should be configured to find the assets in the Android project:

  * In the Properties for the core project: click the Source tab, click Link Source, Browse, select the "assets" folder from your Android project and click OK.
  * Specify "assets" for the folder name and click Finish then OK.

_Note: If your desktop and Android projects are in the same parent folder, you can use "`PARENT-1-PROJECT_LOC/gamename-android/assets`" for the location of the linked assets folder, where "`gamename-android`" is the name of your Android project. This is better than a hard-coded path if you plan on sharing your projects with others._

## Creating a game ##

In your main project, create a new class: right click the project -> New -> Class. Name it "Game" and specify a package (eg, "com.gamename"). Next to Interfaces, click Add, choose `ApplicationListener`, and click OK. It will look like this:

```java
import com.badlogic.gdx.ApplicationListener;

public class Game implements ApplicationListener {
	public void create () {
	}

	public void render () {
	}

	public void resize (int width, int height) {
	}

	public void pause () {
	}

	public void resume () {
	}

	public void dispose () {
	}
}
```

These methods allow you to setup and render your game. Since they are empty, this particular game is simply a blank screen. We will first get this simple game running before doing something more interesting.

## Running the game on the desktop ##

Right click the desktop project -> New -> Class. Name it `DesktopGame` and specify a package (eg, "com.gamename"). Click OK. Make the class look like this:

```java
import com.badlogic.gdx.backends.lwjgl.LwjglApplication;

public class DesktopGame {
	public static void main (String[] args) {
		new LwjglApplication(new Game(), "Game", 480, 320);
	}
}
```

This code creates an LwjglApplication, gives it an instance of your game, a title, and size.

To run the game on the desktop, right click the project -> Debug As -> Java Application. You should get a black window titled "Game".

## Running the game on Android ##

Open the `AndroidGame` class in the Android project that was automatically created and make it look like this:

```java
import com.badlogic.gdx.backends.android.AndroidApplication;

public class AndroidGame extends AndroidApplication {
	public void onCreate (android.os.Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		initialize(new Game());
	}
}
```

This code is an Android activity that calls initialize, passing an instance of your game.

To run the game on Android, right click the project -> Debug As -> Android Application. The screen will turn black, since the game doesn't yet do anything. If any errors occur running the application, they show up in the Logcat view, which can be seen by clicking Window -> Show View -> Other -> Android -> Logcat.

## Updating libgdx ##

At some point you may want to update libgdx to the latest. To do this, download the [`libgdx-nightly-latest.zip`](http://libgdx.badlogicgames.com/nightlies/) again and update these files in your projects:

| Project | Files |
| ------- | :------: |
| main | libs/gdx.jar<br>libs/gdx-sources.jar |
| Android | libs/gdx-backend-android.jar<br>libs/armeabi _(directory)_<br>libs/armeabi-v7a _(directory)_ |
| desktop | libs/gdx-natives.jar<br>libs/gdx-backend-lwjgl.jar<br>libs/gdx-backend-lwjgl-natives.jar |

There also exists a python script that will automatically update your libraries by fetching the latest nightlies: https://github.com/jrenner/libgdx-updater