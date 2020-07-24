LibGdx makes use of bitmap files (pngs) to render fonts.  Each glyph in the font has a corresponding TextureRegion.

[BitmapFont class](http://libgdx.badlogicgames.com/nightlies/docs/api/com/badlogic/gdx/graphics/g2d/BitmapFont.html) [(code)](https://github.com/libgdx/libgdx/blob/master/gdx/src/com/badlogic/gdx/graphics/g2d/BitmapFont.java)

BitmapFont was refactored for the LibGdx 1.5.6 release. [This blog post](http://www.badlogicgames.com/wordpress/?p=3658) has details about the changes and also a small example showing how to move from pre 1.5.6 code to the new API.

A tutorial on using BitmapFont is available on [LibGDX.info](https://libgdx.info/basic-label/)

## File format specifications for the font file

References point to bmFont being originally created by Andreas Jönsson over at [AngelCode](http://www.angelcode.com/)

[BMFont](http://www.angelcode.com/products/bmfont/doc/file_format.html) - the original specification for the file format.

[Glyph Designer](https://71squared.com/blog/bitmap-font-file-format) - Details about output, include a binary format.


## Tools for Creating Bitmaps

[[Hiero]] - a utility for converting a system font to a bitmap

[ShoeBox](http://renderhjs.net/shoebox/)  - lets you load customized glyphs from an image, and then create a bitmap font from them. [There's a great tutorial for using it with libgdx](https://www.youtube.com/watch?v=dxPf1M7YORU&feature=youtu.be).

[Glyph Designer](http://71squared.com/en/glyphdesigner) - a commercial bitmap font tool with a wide variety of options for shadows, gradients, stroke, etc.

[Littera](http://kvazars.com/littera) - online bitmap font generator, with a great amount of customizations (needs Adobe Flash).

## Other Tools

[FreeTypeFontGenerator](http://www.badlogicgames.com/wordpress/?p=2300) - generating bitmaps for fonts instead of supplying a pre-rendered bitmap made by utilities like Hiero

Examples 
: [(more)](https://github.com/libgdx/libgdx/blob/master/tests/gdx-tests/src/com/badlogic/gdx/tests/extensions/InternationalFontsTest.java)

	FreeTypeFontGenerator generator = new FreeTypeFontGenerator(Gdx.files.internal("data/unbom.ttf"));
 
	FreeTypeFontParameter parameter = new FreeTypeFontParameter();
	parameter.size = 18;
	parameter.characters = "한국어/조선�?";

	BitmapFont koreanFont = generator.generateFont(parameter);

	parameter.characters = FreeTypeFontGenerator.DEFAULT_CHARS;
	generator = new FreeTypeFontGenerator(Gdx.files.internal("data/russkij.ttf"));
	BitmapFont cyrillicFont = generator.generateFont(parameter);
	generator.dispose();



[[Distance field fonts]] - useful for scaling/rotating fonts without ugly artifacts

[gdx-smart-font](https://github.com/jrenner/gdx-smart-font) - unofficial libgdx addon for automatically generating and caching bitmap fonts based on screen size. (Uses FreeTypeFontGenerator)

## Fonts in 3D space
While libGDX does not support placing text in 3D space directly, it is still possible to do so relatively easily. Check [this gist](https://gist.github.com/Darkyenus/e9427b0655816d2a521227cb9313d303) for an example. Note that such text won't be occluded by objects in front of it, as `SpriteBatch` draws with constant `z`, but it would not be hard to fix that with a custom shader, that would set the appropriate 'z' from an uniform.