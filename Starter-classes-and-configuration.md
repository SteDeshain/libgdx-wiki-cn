* [Desktop (LWJGL)](#desktop-lwjgl)
* [Desktop (LWJGL3)](#desktop-lwjgl3)
* [Android](#android)
  - [Fragment based libgdx](#fragment-based-libgdx)
  - [Live Wallpapers](#live-wallpapers)
  - [Daydreams](#daydreams)
* [iOS/Robovm](#iosrobovm)
* [HTML5/GWT](#html5gwt)


For each target platform, a starter class has to be written. This class instantiates a back-end specific `Application` implementation and the `ApplicationListener` that implements the application logic. The starter classes are platform dependent, let's have a look at how to instantiate and configure these for each back-end.

This article assumes you have followed the instruction in [Project Setup, Running & Debugging](https://github.com/libgdx/libgdx/wiki/Project-setup,-running-&-debugging) and have imported the generated core, desktop, Android and HTML5 projects into Eclipse.

# Desktop (LWJGL) #
Opening the `Main.java` class in `my-gdx-game` shows the following:

```java
package com.me.mygdxgame;

import com.badlogic.gdx.backends.lwjgl.LwjglApplication;
import com.badlogic.gdx.backends.lwjgl.LwjglApplicationConfiguration;

public class Main {
   public static void main(String[] args) {
      LwjglApplicationConfiguration cfg = new LwjglApplicationConfiguration();
      cfg.title = "my-gdx-game";
      cfg.useGL30 = false;
      cfg.width = 480;
      cfg.height = 320;
		
      new LwjglApplication(new MyGdxGame(), cfg);
   }
}
```

First an [LwjglApplicationConfiguration](https://github.com/libgdx/libgdx/tree/master/backends/gdx-backend-lwjgl/src/com/badlogic/gdx/backends/lwjgl/LwjglApplicationConfiguration.java) is instantiated. This class lets one specify various configuration settings, such as the initial screen resolution, whether to use OpenGL ES 2.0 or 3.0 (Experimental) and so on. Refer to the [Javadocs](https://libgdx.badlogicgames.com/nightlies/docs/api/com/badlogic/gdx/backends/lwjgl/LwjglApplicationConfiguration.html) of this class for more information.

Once the configuration object is set, an `LwjglApplication` is instantiated. The `MyGdxGame()` class is the ApplicationListener implementing the game logic. 

From there on a window is created and the ApplicationListener is invoked as described in [[The Life-Cycle]]

When using a JDK of version 8 or later, an "illegal reflective access" warning is shown. This is nothing to be worried about. If it bothers you, downgrade the used JDK or switch to the LWJGL 3 backend.

# Desktop (LWJGL3) #

LWJGL3 is the modern backend recommended for new projects. Unfortunately, it is not set as the default in the setup project. To enable it, you must change the following line in the build.gradle for your project:

From:
```groovy
api "com.badlogicgames.gdx:gdx-backend-lwjgl:$gdxVersion"
```
To:
```groovy
api "com.badlogicgames.gdx:gdx-backend-lwjgl3:$gdxVersion"
```

Make sure to refresh your Gradle dependencies in your IDE. You will have to make the following changes to your DesktopLauncher class as well:

```java
package com.me.mygdxgame;

import com.badlogic.gdx.backends.lwjgl3.Lwjgl3Application;
import com.badlogic.gdx.backends.lwjgl3.Lwjgl3ApplicationConfiguration;

public class Main {
   public static void main(String[] args) {
      Lwjgl3ApplicationConfiguration config = new Lwjgl3ApplicationConfiguration();
      config.setTitle("my-gdx-game");
      config.setWindowedMode(480, 320);
		
      new Lwjgl3Application(new MyGdxGame(), config);
   }
}
```

On Mac OS X the LWJGL 3 backend is only working when the JVM is run with the "-XstartOnFirstThread" argument.
To do that, add this line to `run` task of desktop gradle file (works with gradle 5.4.1):
```
    jvmArgs = ['-XstartOnFirstThread']
```

# Android #
Android applications do not use a `main()` method as the entry-point, but instead require an Activity. Open the `MainActivity.java` class in the `my-gdx-game-android` project:

```java
package com.me.mygdxgame;

import android.os.Bundle;

import com.badlogic.gdx.backends.android.AndroidApplication;
import com.badlogic.gdx.backends.android.AndroidApplicationConfiguration;

public class MainActivity extends AndroidApplication {
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        
        AndroidApplicationConfiguration cfg = new AndroidApplicationConfiguration();
        
        initialize(new MyGdxGame(), cfg);
    }
}
```

The main entry-point method is the Activity's `onCreate()` method. Note that `MainActivity` derives from `AndroidApplication`, which itself derives from `Activity`. As in the desktop starter class, a configuration instance is created ([AndroidApplicationConfiguration](https://github.com/libgdx/libgdx/tree/master/backends/gdx-backend-android/src/com/badlogic/gdx/backends/android/AndroidApplicationConfiguration.java)). Once configured, the `AndroidApplication.initialize()` method is called, passing in the `ApplicationListener`, `MyGdxGame`)\ as well as the configuration. Refer to the [AndroidApplicationConfiguration Javadocs](http://libgdx.badlogicgames.com/nightlies/docs/api/com/badlogic/gdx/backends/android/AndroidApplicationConfiguration.html) for more information on what configuration settings are available.

Android applications can have multiple activities. Libgdx games should usually only consist of a single activity. Different screens of the game are implemented within libgdx, not as separate activities. The reason for this is that creating a new `Activity` also implies creating a new OpenGL context, which is time consuming and also means that all graphical resources have to be reloaded.

## Fragment based libgdx ##

The Android SDK has introduced an API to create controllers for specific parts of a screen, that can be easily re-used on multiple screens. This API is called the [Fragments API](http://developer.android.com/guide/components/fragments.html). Libgdx can now also be used as a part of a larger screen, inside a Fragment. To create a Libgdx fragment, subclass `AndroidFragmentApplication` and implement the `onCreateView()` with the following initialization:
```java
    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
        return initializeForView(new MyGdxGame());
    }
```

That code depends on some other changes to the -android project:
1. [Add AndroidX Fragment Library to the -android project and its build path](https://developer.android.com/jetpack/androidx/releases/fragment) if you haven't already added it. This is needed in order to Extend FragmentActivity later
2. Change AndroidLauncher activity to extend FragmentActivity, not AndroidApplication
3. Implement AndroidFragmentApplication.Callbacks on the AndroidLauncher activity
4. Create a Class that extends AndroidFragmentApplication which is the Fragment implementation for Libgdx.
5. Add the initializeForView() code in the Fragment's onCreateView method.
6. Finally, replace the AndroidLauncher activity content with the Libgdx Fragment.

For example:
```java
// 2. Change AndroidLauncher activity to extend FragmentActivity, not AndroidApplication
// 3. Implement AndroidFragmentApplication.Callbacks on the AndroidLauncher activity
public class AndroidLauncher extends FragmentActivity implements AndroidFragmentApplication.Callbacks
{
   @Override
   protected void onCreate (Bundle savedInstanceState)
   {
      super.onCreate(savedInstanceState);

      // 6. Finally, replace the AndroidLauncher activity content with the Libgdx Fragment.
      GameFragment fragment = new GameFragment();
      FragmentTransaction trans = getSupportFragmentManager().beginTransaction();
      trans.replace(android.R.id.content, fragment);
      trans.commit();
   }

   // 4. Create a Class that extends AndroidFragmentApplication which is the Fragment implementation for Libgdx.
   public static class GameFragment extends AndroidFragmentApplication
   {
      // 5. Add the initializeForView() code in the Fragment's onCreateView method.
      @Override
      public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
      {  return initializeForView(new MyGdxGame());   }
   }


   @Override
   public void exit() {}
}
```

### The AndroidManifest.xml File ###
Besides the `AndroidApplicationConfiguration`, an Android application is also configured via the `AndroidManifest.xml` file, found in the root directory of the Android project. This might look something like this:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.me.mygdxgame"
    android:versionCode="1"
    android:versionName="1.0" >

    <uses-sdk android:minSdkVersion="8" android:targetSdkVersion="15" />

    <application
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name" >
        <activity
            android:name=".MainActivity"
            android:label="@string/app_name"
            android:screenOrientation="landscape"
            android:configChanges="keyboard|keyboardHidden|orientation">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```

#### Target Sdk Version ####
Set this to the version of Android you want to target.

#### Screen Orientation & Configuration Changes ####
In addition to the targetSdkVersion, the `screenOrientation` and `configChanges` attributes of the activity element should always be set.

The `screenOrientation` attribute specifies a fixed orientation for the application. One may omit this if the application can work with both landscape and portrait mode.

The `configChanges` attribute is *crucial* and should always have the values shown above. Omitting this attribute means that the application will be restarted every time a physical keyboard is slid out/in or if the orientation of the device changes. If the `screenOrientation` attribute is omitted, a libgdx application will receive calls to `ApplicationListener.resize()` to indicate the orientation change. API clients can then re-layout the application accordingly.

#### Permissions ####
If an application needs to be able to write to the external storage of a device (e.g. SD-card), needs internet access, uses the vibrator or wants to record audio, the following permissions need to be added to the `AndroidManifest.xml` file:

```xml
	<uses-permission android:name="android.permission.RECORD_AUDIO"/>
	<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
	<uses-permission android:name="android.permission.VIBRATE"/>
```

Users are generally suspicious of applications with many permissions, so choose these wisely. 

For wake locking to work, `AndroidApplicationConfiguration.useWakeLock` needs to be set to true. 

If a game doesn't need accelerometer or compass access, it is advised to disable these by setting the 
`useAccelerometer` and `useCompass` fields of `AndroidApplicationConfiguration` to false.
 
If your game needs the gyroscope sensor, you have to set `useGyroscope` to true in `AndroidApplicationConfiguration` (It's disabled by default, to save energy).



Please refer to the [Android Developer's Guide](http://developer.android.com/guide/index.html) for more information on how to set other attributes like icons for your application.

## Live Wallpapers ##
Libgdx features a simple to use way to create [Live Wallpapers](http://android-developers.blogspot.co.at/2010/02/live-wallpapers.html) for Android. The starter class for a live wallpaper is called `AndroidLiveWallpaperService`, here's an example:

```java
package com.mypackage;

// imports snipped for brevity 

public class LiveWallpaper extends AndroidLiveWallpaperService {
	@Override
	public ApplicationListener createListener () {
		return new MyApplicationListener();
	}

	@Override
	public AndroidApplicationConfiguration createConfig () {
		return new AndroidApplicationConfiguration();
	}

	@Override
	public void offsetChange (ApplicationListener listener, float xOffset, float yOffset, float xOffsetStep, float yOffsetStep,
		int xPixelOffset, int yPixelOffset) {
		Gdx.app.log("LiveWallpaper", "offset changed: " + xOffset + ", " + yOffset);
	}
}
```

The methods `createListener()` and `createConfig()` will be called when your live wallpaper is shown in the picker or when it is created to be displayed on the home screen.

The `offsetChange()` method is scaled when the user swipes through screens on the home screen and tells you by how much the screen is offset from the center screen. This method will be called on the rendering thread, so you don't have to synchronize anything.

In addition to a starter class, you also have to create an XML file describing your wallpaper. Let's call that livewallpaper.xml. Create a folder called `xml/` in your Android project's `res/` folder and put the file in there (`res/xml/livewallpaper.xml`). Here's what to put into that file:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<wallpaper
       xmlns:android="http://schemas.android.com/apk/res/android"  
       android:thumbnail="@drawable/ic_launcher"
       android:description="@string/description"
       android:settingsActivity="com.mypackage.LivewallpaperSettings"/>
```

This defines the thumbnail to be displayed for your LWP in the picker, the description and an Activity that will be displayed when the user hits "Settings" in the LWP picker. This should be just a standard Activity that has a few widgets to change settings such as the background color and similar things. You can store those settings in SharedPreferences and load them later in your LWPs ApplicationListener via `Gdx.app.getPreferences()`.

Finally, you'll need to add things to your `AndroidManifest.xml` files. Here's an example for an LWP with a simple settings Activity:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
      package="com.mypackage"
      android:versionCode="1"
      android:versionName="1.0"
      android:installLocation="preferExternal">
	<uses-sdk android:minSdkVersion="7" android:targetSdkVersion="14"/>	
	<uses-feature android:name="android.software.live_wallpaper" />
		
	<application android:icon="@drawable/icon" android:label="@string/app_name">
		<activity android:name=".LivewallpaperSettings" 
				  android:label="Livewallpaper Settings"/>
		
		<service android:name=".LiveWallpaper"
            android:label="@string/app_name"
            android:icon="@drawable/icon"
            android:permission="android.permission.BIND_WALLPAPER">
            <intent-filter>
                <action android:name="android.service.wallpaper.WallpaperService" />
            </intent-filter>
            <meta-data android:name="android.service.wallpaper"
                android:resource="@xml/livewallpaper" />
        </service>				  	
	</application>
</manifest> 
```

The manifest defines:

  * it uses the live wallpaper feature, see `<uses-feature>`.
  * a permission to be allowed to bind the wallpaper, see `android:permission`
  * the settings activity
  * the livewallpaper service, pointing at the livewallpaper.xml file, see `meta-data`

Note that live wallpapers are only supported starting from Android 2.1 (SDK level 7).

LWPs have some limitations concerning touch input. In general only tap/drop will be reported. If you want full touch you can see the `AndroidApplicationConfiguration#getTouchEventsForLiveWallpaper` flag to true to receive full multi-touch events.

## Daydreams ##
Since Android 4.2, users can set [Daydreams](http://developer.android.com/about/versions/android-4.2.html#Daydream) that will get displayed if the device is idle or docked. These daydreams are similar to screensavers and can display things like photo albums etc. Libgdx let's you write such daydreams easily.

The starter class for a Daydream is called AndroidDaydream. Here's an example:

```java
package com.badlogic.gdx.tests.android;

import android.annotation.TargetApi;
import android.util.Log;

import com.badlogic.gdx.ApplicationListener;
import com.badlogic.gdx.backends.android.AndroidApplicationConfiguration;
import com.badlogic.gdx.backends.android.AndroidDaydream;
import com.badlogic.gdx.tests.MeshShaderTest;

@TargetApi(17)
public class Daydream extends AndroidDaydream {
   @Override
   public void onAttachedToWindow() {
      super.onAttachedToWindow();      
      setInteractive(false);

      AndroidApplicationConfiguration cfg = new AndroidApplicationConfiguration();
      ApplicationListener app = new MeshShaderTest();
      initialize(app, cfg);
   }
}
```

Simply derive from AndroidDaydream, override onAttachedToWindow, setup your configuration and ApplicationListener and initialize your daydream.

In addition to the daydream itself you can provide a settings activity that lets the user configure your daydream. This can be a normal activity, or a libgdx `AndroidApplication`. An empty activity as an example: 

```java
package com.badlogic.gdx.tests.android;

import android.app.Activity;

public class DaydreamSettings extends Activity {

}
```

This settings activity must be specified as metadata to the Daydream service. Create an xml file in the res/xml folder of your Android project and specify the activity like this:

```xml
<dream xmlns:android="http://schemas.android.com/apk/res/android"
 android:settingsActivity="com.badlogic.gdx.tests.android/.DaydreamSettings" />
```

Finally, add a section for the settings activity in the AndroidManifest.xml as usual, and a service description for the daydream, like this:

```xml
<service android:name=".Daydream"
   android:label="@string/app_name"
   android:icon="@drawable/icon"
   android:exported="true">
   <intent-filter>
	   <action android:name="android.service.dreams.DreamService" />
	   <category android:name="android.intent.category.DEFAULT" />
   </intent-filter>
   <meta-data android:name="android.service.dream"
	   android:resource="@xml/daydream" />
</service>
```
# iOS/Robovm #
To come..
# HTML5/GWT #
The main entry-point for an HTML5/GWT application is a `GwtApplication`. Open `GwtLauncher.java` in the my-gdx-game-html5 project:

```java
package com.me.mygdxgame.client;

import com.me.mygdxgame.MyGdxGame;
import com.badlogic.gdx.ApplicationListener;
import com.badlogic.gdx.backends.gwt.GwtApplication;
import com.badlogic.gdx.backends.gwt.GwtApplicationConfiguration;

public class GwtLauncher extends GwtApplication {
   @Override
   public GwtApplicationConfiguration getConfig () {
      GwtApplicationConfiguration cfg = new GwtApplicationConfiguration(480, 320);
      return cfg;
   }

   @Override
   public ApplicationListener createApplicationListener () {
      return new MyGdxGame();
   }
}
```

The main entry-point is composed of two methods, `GwtApplication.getConfig()` and `GwtApplication.createApplicationListener()`. The former has to return a [GwtApplicationConfiguration](https://github.com/libgdx/libgdx/tree/master/backends/gdx-backends-gwt/src/com/badlogic/gdx/backends/gwt/GwtApplicationConfiguration.java) instance, which specifies various configuration settings for the HTML5 application. The `GwtApplication.createApplicatonListener()` method returns the `ApplicationListener` to run.

### Module Files ###
GWT needs the actual Java code for each jar/project that is referenced. Additionally, each of these jars/projects needs to have one module definition file, having the suffix gwt.xml. 

In the example project setup, the module file of the html5 project looks like this:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE module PUBLIC "-//Google Inc.//DTD Google Web Toolkit trunk//EN" "http://google-web-toolkit.googlecode.com/svn/trunk/distro-source/core/src/gwt-module.dtd">
<module>
   <inherits name='com.badlogic.gdx.backends.gdx_backends_gwt' />
   <inherits name='MyGdxGame' />
   <entry-point class='com.me.mygdxgame.client.GwtLauncher' />
   <set-configuration-property name="gdx.assetpath" value="../my-gdx-game-android/assets" />
</module>
```

This specifies two other modules to inherit from (gdx-backends-gwt and the core project) as well as the entry-point class (`GwtLauncher` above) and a path relative to the html5 project's root directory, pointing to the assets directory.

Both the gdx-backend-gwt jar and the core project have a similar module file, specifying other dependencies. *You can not use jars/projects which do not contain a module file and source!*

For more information on modules and dependencies refer to the [GWT Developer Guide](https://developers.google.com/web-toolkit/doc/1.6/DevGuide).

### Reflection Support ###
GWT does not support Java reflection for various reasons. Libgdx has an internal emulation layer that will generate reflection information for a select few internal classes. This means that if you use the [Json serialization](https://github.com/libgdx/libgdx/wiki/Reading-%26-writing-JSON) capabilities of libgdx, you'll run into issues. You can fix this by specifying for which packages and classes reflection information should be generated for. To do so, you can put configuration properties in your GWT project's gwt.xml file like so:

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<module>
    ... other elements ...
    <extend-configuration-property name="gdx.reflect.include" value="org.softmotion.explorers.model" />
    <extend-configuration-property name="gdx.reflect.exclude" value="org.softmotion.explorers.model.HexMap" />
</module>
```

You can add multiple packages and classes by adding more extend-configuration-property elements.

This feature is experimental, use at your own risk.

### Loading Screen ###

A libgdx HTML5 application preloads all assets found in the `gdx.assetpath`. During this loading process, a loading screen is displayed which is implemented via GWT widget. If you want to customize this loading screen, you can simply overwrite the `GwtApplication.getPreloaderCallback()` method (`GwtLauncher` in the above example). 

From 1.9.10 on, the following code changes the colors of the progress bar and the displayed logo to a file placed within your webapp folder:

```java
@Override
public Preloader.PreloaderCallback getPreloaderCallback() {
    return createPreloaderPanel(GWT.getHostPageBaseURL() + "preloadlogo.png");
}

@Override
protected void adjustMeterPanel(Panel meterPanel, Style meterStyle) {
    meterPanel.setStyleName("gdx-meter");
    meterPanel.addStyleName("nostripes");
    meterStyle.setProperty("backgroundColor", "#ffffff");
    meterStyle.setProperty("backgroundImage", "none");
}
```

Prior 1.9.10, best is to copy all `getPreloaderCallback()` content from libGDX' sources and adjust it to your needs.

Note that you can only use pure GWT facilities to display the loading screen, libgdx APIs will only be available after the preloading is complete.

[[Prev|Modules Overview]] | [[Next|Querying]]