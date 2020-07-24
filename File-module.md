The File module provides methods for easy file manipulation. It provides all the functionality needed for most common tasks.

The following tasks are supported
 * reading from a file
 * writing to a file
 * copying of a file
 * moving of a file
 * listing files and directories

# Details #

LibGDX defines 4 file types:

| *Type* | *Description, file path and features* | *Dsktp.* | *And.* |
|:------:|:--------------------------------------|:--------:|:------:|
| Classpath | Classpath file paths are relative to the classpath which is usually the directory where the jar files and compiled classes are located. These files are always *read-only*. | Yes | Yes |
| Internal | Internal files are relative to the application’s *root* directory on desktops and relative to the *assets* directory on Android. | Yes | Yes |
| External | External files paths are relative to the SD card root on Android and to the home directory of the current user on desktop systems. | Yes | Yes |
| Absolute | Absolute files need to have their fully qualified paths specified. <br/>*Note*: For the sake of portability, this option must be used only when absolutely necessary | Yes | Yes |

In libGDX access to the physical file is gained through file handles. A `FileHandle` is just a wrapper for a file. It provides convenience methods for reading, writing, copying, deleting and doing other file related actions.
It is also possible to obtain the input and output streams on a file via the handle for a more controlled access.

A `FileHandle` is obtained by using one of the aforementioned types directly from the *Files* module.
The following code obtains a handle for the myfile.txt which is inside the data directory which resides in the application’s root folder.
```java
FileHandle handle = Gdx.files.internal("data/myfile.txt");
```
On both desktop there will be a directory named “data” in the application’s root and it will contain the “myfile.txt”. On Android the “data” directory must be in the “assets” directory.
It is a good practice to use the same directory structure for the desktop application too, in case the assets are the same.
```java
FileHandle handle = Gdx.files.classpath("myfile.txt");
```
The “myfile.txt” file is located in the directory where the compiled classes reside or the included jar files.

```java
FileHandle handle = Gdx.files.external("myfile.txt");
```
In this case, “`myfile.txt`” needs to be in the users’ home directory (`/home/<user>/myfile.txt` on linux or `\Users\<user>\myfile.txt` on Windows and MacOS) on desktop, and in the root of the SD card on Android.

```java
FileHandle handle = Gdx.files.absolute("/some_dir/subdir/myfile.txt");
```
In the case of absolute file handle, the file has to be exactly where the full path points. In `/some_dir/subdir/` of the current drive on Windows or the exact path on linux, MacOS and Android.

There are a few common use cases in game development which involve reading and writing files.
One of them is loading the game assets. These can be simple text files or binary files like images and sounds.

### Reading from a File ###

The following example illustrates the loading of an image to be used as a background and the loading of a text file that will be displayed on the screen.

```java
public class FilesExample implements ApplicationListener {

	private Texture 	background;  // the texture loaded from a file
	private Sprite 		bgSprite;    // the sprite that will be used as the background
	private SpriteBatch     spriteBatch; // not important - it will be used to draw onto the screen
	private BitmapFont 	font;	     // font we'll be using to draw the text
	private String		fileContent; // the content of the file we will be reading in
	
	public void create() {
		// obtain the handle to the file
		FileHandle handle = Gdx.files.internal("img/clouds.png");	

		// using the handle (file) to create a texture from
		background = new Texture(handle);							

		// creating a sprite with a cut out region of the background texture applied
		bgSprite = new Sprite(background, 0, 0, 480, 320);			 
																	
		// setting where to draw the sprite
		bgSprite.setBounds(0, 0, background.getWidth(), background.getHeight());	
		
		// initialise the spriteBatch
		spriteBatch = new SpriteBatch();							
		
		// create the bitmap font used to draw the text
		font = new BitmapFont();									
		
		// set colour to red
		font.setColor(Color.RED);									
		
		// Loading of the text file
		
		// reusing the handle and loading the file
		// from inside the package relative to the classpath
		handle = Gdx.files.classpath("com/badlogic/examples/mytext.txt");					
		
		// reading the file into a String
		fileContent = handle.readString();							
	}

	@Override
	public void render() {
		// entering the drawing state
		spriteBatch.begin();			

		// drawing the background sprite
		bgSprite.draw(spriteBatch);		
		
		// drawing the filecontent onto the screen
		font.drawMultiLine(spriteBatch, fileContent, 20, Gdx.graphics.getHeight() - 20);

		// finishes the rendering
		spriteBatch.end();				
	}
}
```

Examine the code snippet carefully as each line contains a short comment describing what it does.
It is worth focusing on the file handling for the time being, ignoring other functionalities and notions.

The `clouds.png` file needs to be in the `img` directory which needs to be included in the build. Just use any `png` image. _The image sizes (width and height) must be powers of two_! See [Texture Texture] on why this is so important. 512x512 will do it as a region from the texture is used for the background.

The `mytxt.txt` file also needs to be created and added to the `com.badlogic.examples` package. Note that it is not the asset directory, this is to demonstrate the `classpath` typed file handle. The content of this file will be displayed in the resulting application.


As a best practice, all the assets should be grouped under a common directory named `assets`.
For desktop applications, this directory needs to be included in the final build. To have eclipse do this automatically, the directory needs to be added to the build path as a source directory. Right click on the newly created `assets` directory and select `Build Path -> Use as source folder`. This will make sure that each time eclipse builds the project, it also copies the content of the `assets` directory into the final build.


Running the following code will result in a window with a the `clouds.png` as the background, and overlaying the text read from the `mytext.txt` file.

```java

public class FilesExampleDesktop {

	public static void main(String[] args) {
		new LwjglApplication(new FilesExample(), "Files Example", 480, 320, false);
	}
}
```

Sample result:

[[images/file-module.png]]

The Android version is analogous.

### Writing to a File ###

_TBD_
