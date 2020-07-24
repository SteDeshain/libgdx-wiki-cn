# Introduction #

If you want to draw text in your game, you usually use a [[BitmapFont | Bitmap Fonts]].
However, there is a downside:

* **BitmapFonts rely on an image, so you have to scale them if you want a different size, which may look ugly.**

You could just save a BitmapFont of the biggest size needed in your game then and you never have to scale up, just down, right?
Well, that's true, but such a BitmapFont can easily take up two times as much space on your hard drive as the corresponding TrueType Font (.ttf).
Now imagine you have to ship all your big BitmapFonts with your game and your game uses ten different fonts... on an Android device. Bye bye, free storage!

The solution to your problem is the `gdx-freetype` extension:
  * ship only lightweight .ttf files with your game
  * generate a BitmapFont of your desired size on the fly
  * user might put his own fonts into your game

Tutorial available on [LibGDX.info](https://libgdx.info/basic-label/)

# Details #

Since this is an extension, it is not included in your LibGDX project by default. How you add the extension differs based on the setup of your project.

## How to put gdx-freetype in your project ##

**For Projects Using Gradle**

For new projects, simply select the Freetype option under extensions in the [setup ui](https://github.com/libgdx/libgdx/wiki/Project-Setup-Gradle).

To add to an existing Gradle project, see [Dependency management with Gradle](https://github.com/libgdx/libgdx/wiki/Dependency-management-with-Gradle#freetypefont-gradle).

**For Projects NOT Using Gradle**

In case you don't have it anymore, download the latest [nightly build](http://libgdx.badlogicgames.com/nightlies/).

Open _libgdx-nightly-latest.zip/extensions/gdx-freetype_ and do the following:
  * extract gdx-freetype.jar to your core project's libs folder
  * link gdx-freetype.jar to your core, android and desktop project
  * extract gdx-freetype-natives.jar to your desktop project's libs folder
  * link gdx-freetype-natives.jar to your desktop project
  * copy armeabi/libgdx-freetype.so to your android project's libs/armeabi folder
  * copy armeabi-v7a/libgdx-freetype.so to your android project's libs/armeabi-v7a folder

**HTML5**

GDX Freetype is not compatible with HTML5. However, you may use the gdx-freetype-gwt lib by Intrigus to enable HTML5 functionality. Please follow the instructions here: https://github.com/intrigus/gdx-freetype-gwt

You're ready to go.

## How to use gdx-freetype in code ##

Using the gdx-freetype extension in your code is really simple.

```java
FreeTypeFontGenerator generator = new FreeTypeFontGenerator(Gdx.files.internal("fonts/myfont.ttf"));
FreeTypeFontParameter parameter = new FreeTypeFontParameter();
parameter.size = 12;
BitmapFont font12 = generator.generateFont(parameter); // font size 12 pixels
generator.dispose(); // don't forget to dispose to avoid memory leaks!
```
A much simpler way to display your font is by placing your font file in project's assets folder. You will have to modify the first line of the above code and mention just your font file name in the parameters.

The defaults for the [FreeTypeFontParameter](https://github.com/libgdx/libgdx/blob/master/extensions/gdx-freetype/src/com/badlogic/gdx/graphics/g2d/freetype/FreeTypeFontGenerator.java):
```java
/** The size in pixels */
public int size = 16;
/** Foreground color (required for non-black borders) */
public Color color = Color.WHITE;
/** Border width in pixels, 0 to disable */
public float borderWidth = 0;
/** Border color; only used if borderWidth > 0 */
public Color borderColor = Color.BLACK;
/** true for straight (mitered), false for rounded borders */
public boolean borderStraight = false;
/** Offset of text shadow on X axis in pixels, 0 to disable */
public int shadowOffsetX = 0;
/** Offset of text shadow on Y axis in pixels, 0 to disable */
public int shadowOffsetY = 0;
/** Shadow color; only used if shadowOffset > 0 */
public Color shadowColor = new Color(0, 0, 0, 0.75f);
/** The characters the font should contain */
public String characters = DEFAULT_CHARS;
/** Whether the font should include kerning */
public boolean kerning = true;
/** The optional PixmapPacker to use */
public PixmapPacker packer = null;
/** Whether to flip the font vertically */
public boolean flip = false;
/** Whether or not to generate mip maps for the resulting texture */
public boolean genMipMaps = false;
/** Minification filter */
public TextureFilter minFilter = TextureFilter.Nearest;
/** Magnification filter */
public TextureFilter magFilter = TextureFilter.Nearest;
```

If rendering large fonts, the default PixmapPacker page size may be too small. You can provide your own PixmapPacker or use `FreeTypeFontGenerator.setMaxTextureSize` to set the default page size.

Examples:
```java
parameter.borderColor = Color.BLACK;
parameter.borderWidth = 3;
```
[[images/border.png]]

```java
parameter.shadowColor = Color.BLACK;
parameter.shadowOffsetX = 3;
parameter.shadowOffsetY = 3;
```
[[images/shadow.png]]

You can also load `BitmapFont`s generated via the FreeType extension using AssetManager. See [FreeTypeFontLoaderTest](https://github.com/libgdx/libgdx/blob/master/tests/gdx-tests/src/com/badlogic/gdx/tests/extensions/FreeTypeFontLoaderTest.java)

### latest info about caveats ###

Quoting from http://www.badlogicgames.com/wordpress/?p=2300:
  * Asian scripts “might” work, see caveat above though. They contain just too many glyphs. I’m thinking about ways to fix this.
  * Right-to-left scripts like Arabic are a no-go. The layout “algorithms” in BitmapFont and BitmapFontCache have no idea how to handle that.
  * Throwing just any font at FreeType is not a super awesome idea. Some fonts in the wild are just terrible, with bad or no hinting information and will look like poopoo.

----

download an [example](https://bitbucket.org/dermetfan/somelibgdxtests/downloads/gdx-freetype%20test.jar)