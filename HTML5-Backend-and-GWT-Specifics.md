Welcome to a place of magic and wonder, the World Wide Web! Even though some folks say this Internet thing is "just a fad," and we should keep using usenet and gopher, there's at least one thing the WWW has that those technologies don't:

## libGDX Games

That's right! You can make your very own libGDX games that run in an HTML5-capable web browser, which I assume is some advanced form of Netscape Navigator. This is possible by GWT, or Google Web Toolkit! I know what you're thinking, Google? The guys who are trying to let people search the Internet with a form? What are they doing with libGDX? I have no idea either. If you want to make your own libGDX game deploy to the web using GWT, well, just make a project in the latest setup jar and make sure to check the `Html` checkbox. The rest should be straightforward!

**BUT IT SOMETIMES ISN'T, AT FIRST** 

So there are a few things that are fundamentally different about developing using GWT as opposed to running a desktop project. You'll want to get familiar with two Gradle tasks in particular; you can launch these tasks from your IDE if you aren't comfortable on the command line, but command-line Gradle tends to avoid problems when the IDE isn't working as well as we would like. `gradlew html:superDev` will be your main tool during development; it allows for a much-improved debugging experience and allows quickly reloading changes to the Java code. `gradlew html:dist` produces a fully-functioning web page that can be uploaded to a static web host (such as the free GitHub Pages service); it also optimizes the web page so the game in it will perform better, which makes `dist` take a little longer than `superDev`.

## superDev

superDev allows you to debug your HTML5 application. This is not necessary in most cases: if there are problems in your core game, you can debug the desktop application. But sometimes, there are bugs only appearing when running on HTML5. You can debug the application with the following steps:

* Run the `html:superdev` Gradle task. It compiles the game and sets up a local HTTP server. When it is done, it will idle to keep the server running.
* Your game is available here: http://localhost:8080/index.html (current config) or http://localhost:8080/html/ (older Gradle configuration with Jetty plugin) - open the page with *Chrome* to debug
* Hit the big reload button and hit compile. The game will recompile and source maps will be set up.
* After the game restarted, open Chrome's dev console with F12 and navigate to the sources tab. Hit Ctrl-P and enter the name of the Java file you want to debug. The Java file will open within Chrome's dev console and you can set a break point. You are able to step through the Java code lines. However, debug variables will be generated JS names but you'll be able to make sense of it.
* When you are done, you can stop the Gradle task with Ctrl-C.

## dist Information

Should be pretty straightforward; the dist is generated in `html/build/dist/`. You can delete the sourcemap files if you feel you won't be debugging the dist; they're usually a few MB in size and are in `html/build/dist/WEB-INF/deploy/html/symbolMaps`.

## Fullscreen Functionality

Surprisingly, fullscreen functionality actually works on the HTML backend. To enable fullscreen, call the following method from within your core project:

```java
Gdx.graphics.setFullscreenMode(Gdx.graphics.getDisplayMode());
```

The user will be prompted to press "ESC" to exit fullscreen. And it even works on mobile. Great! It does have some caveats though. Turns out you can't activate full screen on iPads. Also, if you choose to use the "Resizable Application" option in the HTML Launcher, you'll need to rewrite the ResizeListener to the following ([PR pending](https://github.com/libgdx/libgdx/pull/5691)):

```java
class ResizeListener implements ResizeHandler {
    @Override
    public void onResize(ResizeEvent event) {
        if (Gdx.graphics.isFullscreen()) {
            Gdx.graphics.setFullscreenMode(Gdx.graphics.getDisplayMode());
        } else {
            int width = event.getWidth() - PADDING;
            int height = event.getHeight() - PADDING;
            getRootPanel().setWidth("" + width + "px");
            getRootPanel().setHeight("" + height + "px");
            getApplicationListener().resize(width, height);
            Gdx.graphics.setWindowedMode(width, height);
        }
    }
}
```

Don't forget to also set the fullscreen orientation for mobile in the getConfig():

```java
cfg.fullscreenOrientation = GwtGraphics.OrientationLockType.LANDSCAPE;
```

## Changing the Load Screen Progress Bar

As much as we love libGDX, the default loading progress bar when preparing the HTML game screams "newbie". Impress your friends and bring honor to your family name by making a custom progress bar! Add the following to your HtmlLauncher class in your HTML project:

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

"preloadlogo.png" is an image you place in the "webapp" folder in the HTML project for DIST builds. Place the image in your "war" folder as well for your SUPERDEV builds. Adjust your color to fit the theme of your game. Enjoy yourself.

## Preventing Keys From Triggering Scrolling and Other Browser Functions

On a normal web page, if you press the down arrow on your keyboard, it will scroll the page up. That's nice and all, but maybe you don't want that to happen when players are trying to move the character in your game. To prevent this, you have to add overriding functions to prevent the default actions of special keys. The following must be added to the script block of your index.html in the "html/webapp" folder (dist) and "html/war" folder (superDev):

```javascript
window.onkeydown =
function(event) {
    // prevent all navigation keys except the space key
    if([33, 34, 35, 36, 37, 38, 39, 40].indexOf(event.keyCode) > -1) {
        event.preventDefault();
        return false;
    }
};
```

You can add any key code to that list to prevent it from activating its corresponding action in the browser. Get the keycodes by testing your keyboard on http://www.keycode.info/

The space key needs to be handled in a special way. Add the following to the script block:

```javascript
window.onkeypress =
function(event) {
    // prevent the space key (page scroll)
    if(event.keyCode == 32){
        event.preventDefault();
        return false;
    }
};
```

## Preventing Right Click Context Menu

Similarly to keyboard keys, the right click context menu can be prevented from interrupting your game. You'll notice that there are already functions to prevent left click from doing anything unexpected. You just need to add an additional line to apply the fix to right click as well. The following must be added to the script block of your index.html in the "html/webapp" folder (dist) and "html/war" folder (superDev):

```javascript
// prevent right click
document.getElementById('embed-html').addEventListener('contextmenu', handleMouseDown, false);
```

## Differences Between GWT and Desktop Java

### Numbers

* When some number is very important and you want to make sure it is treated identically on desktop/Android and GWT, use a `long`.
* When you know a number will never be especially large (specifically, that it won't encounter numeric overflow by exceeding roughly 2 billion or negative 2 billion), feel free to use an `int`.
  * Math with `int`s is much faster than math with `long`s on GWT, because any `int` is represented by a JavaScript Number and web browsers are used to working with Numbers all the time. On the other hand, any `long` is represented by a specific type of JavaScript Object that stores three Numbers to help ensure precision.
  * A JavaScript Number, so an `int`, is almost the same as a `double` in Java, but it also allows bitwise operations to be used on it.
    * Because Numbers act like `double`s, they don't overflow, and can go higher than `Integer.MAX_VALUE` (2147483647) and lower than `Integer.MIN_VALUE` (-2147483648). Using any bitwise operation on them will bring any numbers that got too big back into the normal `int` range. If you encounter fishy numeric results that seem way too large for an int, try using this simple trick: `int fishy = Integer.MAX_VALUE * 5; int fixed = (Integer.MAX_VALUE * 5) | 0;` On desktop, adding `| 0` won't change anything, but it can correct numbers that got weird on GWT. Or, you can use a `long`.
* The problem with `long` values on GWT is that they aren't visible to reflection, so libGDX's Json class won't automatically write them or read them. You can work around this with Json's handy custom serializer behavior, so it isn't a huge issue.
* Floats can have more equality check problems than usual. Make sure you make all equality checks for floats by using `MathUtils.isEqual()`.

### Other Known Limitations

* Some java classes/features that are not supported: 
  * System.nanoTime
  * Java reflection. You must only use libGDX reflection utils, see [this wiki page](https://github.com/libgdx/libgdx/wiki/Reflection#gwt) for more details.
  * Multithreading is not supported.
* Audio:
  * Sound pitch is not implemented by the default backend. You can use [an alternative backend](https://github.com/MrStahlfelge/gdx-backends) which is based on WebAudioAPI and supports it.
  * Your game needs a user interaction (eg. click on a button) before playing any music or sounds. This is a limitation for any games played in a browser.
* TiledMaps should be saved with Base64 encoding.
* Most Pixmap methods are not supported.
* All assets are downloaded at launch time. This can lead to a long startup time. If you use AssetManager to load assets on level launch or something similar, you can use [an alternative backend](https://github.com/MrStahlfelge/gdx-backends) that supports [lazy loading of assets](https://github.com/libgdx/libgdx/pull/5677) via AssetManager.
* WebGL 1.0 is used and has its own limitations compared with OpenGL or GLES, among them: 
  * NPOT (non power of two) textures are not supported with MipMap filters and/or Repeat wrapping.
  * Gdx.graphics.supportsExtension(...) should be called for each extension prior to enabling it in shaders
* Some libGDX extensions are not supported or require additional libraries : 
  * Bullet
  * Freetype requires https://github.com/intrigus/gdx-freetype-gwt

## Further Reading

[The original Super Dev Instructions from Mario](https://www.badlogicgames.com/wordpress/?p=3073)

[How to speed up GWT compilation](https://www.gamefromscratch.com/post/2013/10/07/Speeding-up-GWT-compilation-speeds-in-a-LibGDX-project.aspx)

[Building libGDX from source and adding new files to gdx.gwt.xml](https://github.com/libgdx/libgdx/wiki/Local-LibGDX-package-use-with-GWT)

[HTML5 - GWT Explained on YouTube](https://youtu.be/I_85usDvJvQ)