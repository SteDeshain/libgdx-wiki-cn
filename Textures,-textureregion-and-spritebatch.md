This article introduces the basics of 2D graphics, namely the use of **Texture**, **TextureRegion** and **SpriteBatch**. 

## Texture, TextureRegion and SpriteBatch

LibGdx uses OpenGL exclusively thus every image, or part of it that gets used in a game and displayed, first needs to be transformed into a texture. 

## Texture

A **`Texture`** is nothing more than a decoded image loaded into the GPU’s memory in raw format. Traditionally to display textures (images) on the screen, a few things have to be done first. A mesh, usually a rectangular polygon is used to describe the geometry for the texture mapping to be made on.

_Texture mapping_ is the process of working out where in space the texture (image) will be applied. 
It is very simple to imagine it in 2D. To stick a poster on the wall, one needs to figure out where on the wall he will be gluing the corners of the paper.

The _wall_ is the _space_, the _paper_ is the _mesh_ (rectangle), the _image_ printed on the paper is the _texture_.

In a software application, the wall is the canvas (window), the paper is the rectangular mesh (consisting of 4 points - vertices) and the image is the texture.

## SpriteBatch

LibGdx comes with **`SpriteBatch`** which takes care of all the steps needed to achieve texture mapping and displaying texture mapped rectangles on the screen. It is a convenience class which makes drawing onto the screen extremely easy and it is also optimised.

It works with screen coordinates and uses pixel perfect screen resolution. The lower left corner is the origin (coordinate 0,0) with the X axis pointing right and the Y axis pointing up.

Because `SpriteBatch` can handle multiple drawable elements at once and optimises the work with the GPU and it queues up commands, the programmer needs to trigger the execution.

The following snippet draws an image onto the screen. It is a very simple `ApplicationListener` that contains a `Texture` and a `SpriteBatch`.

```java
public class TextureFun implements ApplicationListener {

	private Texture 	druidTexture;			// #1
	private SpriteBatch 	batch;				// #2
	
	@Override
	public void create() {
		druidTexture = new Texture(Gdx.files.internal("druid.png"));	// #3
		batch = new SpriteBatch();					// #4
	}

	@Override
	public void render() {
		batch.begin();					// #5
		batch.draw(druidTexture, 100, 100);		// #6
		batch.end();					// #7
	}

	// … rest of methods omitted … //
}
```

1. The `druidTexture` is declared. This will be the decoded image from the hard disk or SD card.
2. The `SpriteBatch` which will be used to display the `druidTexture` onto the screen.
3. The texture is created. The file `druid.png` found in the asset (see [FileModule internal file]) directory is loaded, decoded and uploaded to the GPU and is ready to be used by OpenGL. Note that image format needs to be supported (.jpg, .png or .bmp). On OpenGL 1.0 the width and height of a texture had to always be power of two (POT), they didn't have to be equal though, so 64x32 pixels is fine. Even though LibGDX now uses OpenGL ES 2.0, it's still a good practice to use POT textures, for some features (e.g. texture wrapping) might not be supported for non-POT texture sizes. Also, in practice, a non-POT sized texture will use the same amount of memory as the nearest bigger POT size.
5. As pointed out earlier, the `SpriteBatch` is instructed to receive commands.
6. The first command is issued. It tells `SpriteBatch` to draw the the texture (`druidTexture`) to the position X=100, Y=100.
7. The `SpriteBatch` is notified that no more commands will be issued and can proceed to process the batch of commands it received, which currently consists of the single draw instruction.

By creating a simple test application that runs the above code, the following result is obtained.

```java
public class TextureFunDesktop {
	public static void main(String[] args) {
		new LwjglApplication(new TextureFun(), "Textures", 480, 320, false);
	}
}
```
[[images/textures_regions_batch_result_1.png]]

The following image explains the coordinate system and the texture borders.

[[images/textures_regions_batch_explanation.png]]

Multiple commands can be issued to the `SpriteBatch` by including them between the `begin()` and the `end()` calls.

The following code issues 3 draw commands that will draw the same texture at 3 different positions. Notice how the texture gets reused.

```java
public void render() {
	batch.begin();
	batch.draw(druidTexture, 100, 100);
	batch.draw(druidTexture, 200, 100);
	batch.draw(druidTexture, 300, 100);
	batch.end();
}
```

The commands are executed in the order they are batched (issued), so if the second drawing overlaps the first drawn texture, it will cover the overlapping region.

The result of the above code.

[[images/textures_regions_batch_result_2.png]]

The following code draws the textures on top of each other:

```java
public void render() {
	batch.begin();
	batch.draw(druidTexture, 100, 100);
	batch.draw(druidTexture, 132, 132);
	batch.draw(druidTexture, 164, 164);
	batch.end();
}
```

[[images/textures_regions_batch_overlap.png]]

Besides drawing, there are a few other methods that can change the behaviour of 'SpriteBatch', like enabling blending, setting the colour for tinting and setting the transformation matrix directly. However the transformation matrix has to be set before the 'SpriteBatch' is in the state where it accepts commands.

The following code enables blending and sets the algorithms to be used when performing the blending operation. Blending is enabled by default.

```java
...
batch.enableBlending();
batch.setBlendFunction(GL20.GL_SRC_ALPHA, GL20.GL_ONE_MINUS_SRC_ALPHA);
...
```

The 'Texture' can be constructed in many different ways and the different constructors are well documented in the [javadocs](http://libgdx.badlogicgames.com/nightlies/docs/api/com/badlogic/gdx/graphics/Texture.html).

When opting to use *mipmaps*, the `Texture` will create them at instantiation time. 
*MipMaps* are pre-calculated, optimized resized copies of the same image to save on computation time when resizing the texture to fit a rectangle. Smaller (halved) copies of the same image are created and uploaded to the GPU and used for different sized geometries instead of being shrunk on the fly. It adds to memory consumption but makes rendering faster.

`SpriteBatch` comes with quite a few draw methods that allow a lot of flexibility.
The code is well documented and worth spending time to try out different draw methods and get accustomed with them.

The following code has the result from the image: 

```java
public void render() {
	batch.begin();
	batch.draw(druidTexture, 100, 100);
	batch.draw(druidTexture, 200, 100, 32, 32, 64, 64, 1f, 2.0f, 45f, 0, 0, 64, 64, false, false);
	batch.end();
}
```

[[images/textures_regions_batch_draw_methods_comparison.png]]

The two draw methods draw the same texture differently. The second draw method modifies to the resulting image quite a lot.

In the documentation _Texel_ is mentioned. A _texel_ is one pixel from the texture. It is a Texture Element (TEXture ELement) and because we are in a pixel perfect environment, one pixel is the unit of measurement.

## TextureRegion

Most of the times the game has multiple drawable elements. To draw all the images at once onto the screen, each element needs to be turned into a texture. It sounds straight-forward but in reality it is very expensive on the GPU side as each texture needs to be uploaded to the GPU, before being drawn and bound so OpenGL will take the active one to map it. The texture binding and switching is very expensive.

*TextureRegion* solves this issue by cutting out a region from a texture and lets `SpriteBatch` work with it. This means that a single texture can be created for all the elements needed to be drawn and when drawing them, only the region corresponding to the element will be drawn.
A texture that contains multiple elements (sprites) is also called a sprite sheet.

The following image shows a sprite sheet and how it is cut up into regions. A single image is used for the whole texture thus avoiding the expensive texture switching.

_Note: the size of the texture has to be POT (power of two), but the sizes of the regions can be anything._

[[images/textures_regions_batch_txregions_2.png]]

The following code creates and draws the 4 regions from the big texture. It also draws the scaled down texture at 0,0 coordinate for reference.

```java
public class TextureFun implements ApplicationListener {

	private Texture 		texture;			// #1
	private SpriteBatch 		batch;
	private TextureRegion[] 	regions = new TextureRegion[4];	// #2
	
	@Override
	public void create() {
		texture = new Texture(Gdx.files.internal("sprite_sheet.png"));
		batch = new SpriteBatch();
		regions[0] = new TextureRegion(texture, 0, 0, 64, 64);		// #3
		regions[1] = new TextureRegion(texture, 0.5f, 0f, 1f, 0.5f);	// #4
		regions[2] = new TextureRegion(texture, 0, 63, 64, 64);		// #5
		regions[3] = new TextureRegion(texture, 0.5f, 0.5f, 1f, 1f);	// #6
	}

	@Override
	public void render() {
		batch.begin();
		batch.draw(texture, 0, 0, 64, 64);				// #7
		for (int i = 0; i < regions.length; i++) {
			batch.draw(regions[i], 75 * (i + 1), 100);		// #8
		}
		batch.end();
	}
	
	// … rest of methods ommited... //

}
```

1. The texture that will hold the sprite sheet.
2. The array of TextureRegion objects that are cut out from the texture. (4 regions)
3. Cuts out a region from the `texture` starting at `(0,0)` having both the width and height 64 pixels. This method which takes `int` types as parameter, works in the pixel coordinate system. 

    The first parameter is the texture that holds the image we want to cut out, the 2nd and 3rd parameters are the X and Y coordinates in the texture (the origin is in the top left corner), while the 4th and 5th parameters are the `width` and `height` of the region in pixels that will be cut out.

4. Cuts out a region from `texture` using normalized device coordinates. Using a different coordinate system, the 2nd and 3rd parameters are the X and Y coordinates of the top left point *but* the 4th and 5th parameters are the X and Y coordinates of the bottom right point. In _normalized device coordinate system_, the width of the texture is 1.0. 0.5 means the half of the width. In this case, 0.5 length means 64 pixels (width = 128 pixels).
5. Cuts out the bottom left region.
6. Cuts out the bottom right region.
7. Draws the original `texture` scaled to 64 pixels wide and high at the bottom left corner. Remember that SpriteBatch’s default viewport’s coordinate system has the origin at `0,0`.
8. Draws each region at 75 pixels distance from each other horizontally.

The result is the following image:

[[images/textures_regions_batch_regions_result.png]]

Texture regions can be created by using `TextureRegion's` `split` convenience method too. It is very easy if the sprite-sheet consists of regions of the same size with no padding.

The following code creates a two dimensional array from the previously used `texture`. 

```java
...
TextureRegion[][] regions = TextureRegion.split(texture, 64, 64)
...
```

It starts the cutting from the top left corner and cuts out regions of 64x64 pixels and does that until it reaches the end of the row. Then it goes onto the second row if any, and starts it from the beginning until there are no more rows left. In the previous case it means that 2 rows are generated each having 2 columns (2 regions per row).

---

For a complete description of Texture, TextureRegion and SpriteBatch, check out their [javadocs](http://libgdx.badlogicgames.com/nightlies/docs/api/).

Sprites used from [here](http://blogoscoped.com/archive/2006-08-08-n51.html).