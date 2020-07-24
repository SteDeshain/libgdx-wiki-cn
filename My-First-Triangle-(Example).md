Note: The following article is outdated and no longer works due to the exclusion of GL10 in the latest libGDX. Please use [the following instead](https://github.com/mattdesl/lwjgl-basics/wiki/LibGDX-Meshes-Lesson-1).

# Summary

* Create a libgdx project from scratch
* Create your first triangle

# Introduction

The HelloWorld tutorial demonstrated how a libgdx application is structured by importing an existing project. Lets now go one step further and create project from scratch. This tutorial is more verbose in case the reader is also a novice with Eclipse. Future tutorials will have more concise instructions.

The source files for this guide is contained within `MyFirstTriangle_<data>.zip` in the [http://code.google.com/p/libgdx/downloads/list Downloads] page.

# Creating the Desktop Project

Download the latest [http://libgdx.l33tlabs.org/ nightly zip] and uncompress it to a temporary directory called `libgdx-nightly`.

Most of the code, including all of the game logic, will reside in a regular Java project. In Eclipse, click File -> New -> Java Project. Use `my-first-triangle` as the project name. In the JRE section, select `JavaSE-1.6` or something similar, then click Finish.

Lets copy libraries contain all the necessary libgdx classes and methods into our workspace. In the Package Explorer view, right-click on the my-first-triangle folder, select New -> Folder, and name the new folder `libs`. From the `libgdx-nightly` directory, copy the following files into `libs`:
  * gdx-backend-jogl.jar
  * gdx-backend-jogl-natives.jar
  * gdx-sources.jar
  * gdx-natives.jar
  * gdx.jar

Now we'll add the libraries to our project so that our Java code  can reference the classes in the libgdx libraries. Right click on the `my-first-triangle` folder again and select Properties. Go to the Java Build Path section, select the Libraries tab, click Add JARs..., navigate to the `my-first-triangle/libs` directory and select the files:
  * gdx-backend-jogl.jar
  * gdx-backend-jogl-natives.jar
  * gdx-natives.jar
  * gdx.jar

Eclipse can also show you the javadoc documention for the libgdx classes and methods you use. While still in the Libraries tab, expand the `gdx.jar` listing and double click on the entry named Source attachment. Click Workspace and navigate to the my-first-triangle/libs directory again, this time selecting `gdx-sources.jar`.

Now we can create a class that will draw our triangle. Expand the my-first-triangle folder, right-click on the `src` source directory and select New -> Package. Name it anything you want, in this example we'll use `com.test.myfirsttriangle`. Right-click on the new package, and select New -> Class, use the name `MyFirstTriangle`. In the Interfaces section, add the `com.badlogic.gdx.ApplicationListener` interface. Your new class should look something like this:

```java
package com.test.myfirsttriangle;

import com.badlogic.gdx.ApplicationListener;

public class MyFirstTriangle implements ApplicationListener {

	@Override
	public void create() {
		// TODO Auto-generated method stub

	}

	@Override
	public void dispose() {
		// TODO Auto-generated method stub

	}

	@Override
	public void pause() {
		// TODO Auto-generated method stub

	}

	@Override
	public void render() {
		// TODO Auto-generated method stub

	}

	@Override
	public void resize(int width, int height) {
		// TODO Auto-generated method stub

	}

	@Override
	public void resume() {
		// TODO Auto-generated method stub

	}

}
```

We will soon add logic to `MyFirstTriangle` to draw a triangle. It will be capable of being run in either the desktop or Android environment. Lets create the desktop entry point now. In the same package as the `MyFirstTriangle` class, create another class, this time named `MyFirstTriangleDesktop`. Put a `main` method in the class so that it looks like this:

```java
package com.test.myfirsttriangle;

import com.badlogic.gdx.backends.jogl.JoglApplication;

public class MyFirstTriangleDesktop {
	public static void main (String[] argv) {
		new JoglApplication(new MyFirstTriangle(), "My First Triangle", 480, 320, false);		
	}
}
```

The `MyFirstTriangleDesktop` class gives a starting point to launch a desktop application. It does that by creating a `JoglApplication`, passing in an instance of the `MyFirstTriangle` class to perform the rendering.

You can run the application right now by right-clicking on the `MyFirstTriangleDesktop` class in the Package Explorer and selecting Run As -> Java Application. The application should either look blank or display rendering artifacts (we'll fix that now).

# Rendering a Triangle

The application shows a blank screen because our `MyFirstTriangle` class contains no rendering logic. Now it's time to finally create the code to draw a triangle. Replace the contents of the `MyFirstTriangle` class with this:

```java
package com.test.myfirsttriangle;

import com.badlogic.gdx.ApplicationListener;
import com.badlogic.gdx.Gdx;
import com.badlogic.gdx.graphics.GL10;
import com.badlogic.gdx.graphics.Mesh;
import com.badlogic.gdx.graphics.VertexAttribute;
import com.badlogic.gdx.graphics.VertexAttributes.Usage;

public class MyFirstTriangle implements ApplicationListener {
	private Mesh mesh;

	@Override
	public void create() {
		if (mesh == null) {
			mesh = new Mesh(true, 3, 3, 
			        new VertexAttribute(Usage.Position, 3, "a_position"));		

			mesh.setVertices(new float[] { -0.5f, -0.5f, 0,
			                               0.5f, -0.5f, 0,
			                               0, 0.5f, 0 });	
			mesh.setIndices(new short[] { 0, 1, 2 });			
		}
	}

	@Override
	public void dispose() { }

	@Override
	public void pause() { }

	@Override
	public void render() {
                Gdx.gl.glClear(GL10.GL_COLOR_BUFFER_BIT);
		mesh.render(GL10.GL_TRIANGLES, 0, 3);
	}

	@Override
	public void resize(int width, int height) { }

	@Override
	public void resume() { }
}
```

Run the project again. The application should now have a white triangle rendered in the middle of the screen:

![Triangle Desktop](http://i.imgur.com/3Qi1ORl.png)

Lets look at the changes we made. `create()` is called when the the rendering surface is created. We create a `Mesh`, which represents an object that will be drawn onscreen. Its constructor takes several parameters, but for now just take note of the `VertexAttribute` object we pass in. `VertexAttribe`s are passed into the constructor to indicate what type of information the resulting mesh will contain. A mesh can potentially contain information regarding an object's position, color, texture, and more, but right now we'll only assign positional information. Since this mesh is a triangle, its position (and shape) is made up of three *vertices*, each denoted by three values (x, y, z) in the Cartesian coordiante. The three vertices for this mesh are located at (-0.5f, -0.5f, 0), (0.5f, -0.5f, 0), and (0, 0.5f, 0). We then set the mesh's *indices*, which is the order of the vertices, sort of like the order in a game of connecting the dots. In a simple shape like a triangle, the order doesn't matter because no matter which order you connect the vertices, it will always form a triangle. With more vertices, the order will greatly affect the final shape of the object.

When the application starts running the `render()` method is called again and again, basically repainting the screen as fast as it can. In `render()` we just tell the mesh we created earlier in `create()` to render itself. We also clear the screen each time `render()` is called. This is done via OpenGL directly.

For simplicity, we've skipped over many details of how different parameters change the behavior of the methods we used. These we'll go over in more detail in a later tutorials. In the mean time, you can see the javadoc of any method simply by hovering over the method name in the editor.

# Setting Up an Android Project

Testing the application on Android only requires setting up a small Android project that refers to the desktop project we just created. In Eclipse, click File -> New -> Other.... Under the Android section, select Android Project and click Next. Name the project anything you want, for our example we'll use `my-first-triangle-android`. Under the Build Target section, select the Android platform version you're targetting. Different platform version have different support for OpenGL ES, but for our simple example, selecting Android 1.5 is fine. Enter any application name, we'll use `my-first-triangle-android` again. For package name, use the same package from the desktop project, enter `com.test.myfirsttriangle`. Check the Create Activity checkbox and give the activity a name, we'll use `MyFirstTriangleAndroid`. For Min SDK Version, type in the API Level of the platform version you chose earlier. That means for Android 1.5, enter `3` as the Min SDK Version. Click Finish.

Similar to the desktop project, we'll copy some libgdx libraries into our workspace. In the Package Explorer view, right-click on the my-first-triangle-android folder and select New -> Folder. Name the folder `libs`. Go back to the `libgdx-nightly` directory that stores all the libgdx library files and copy the following folders and files into the new `libs` directory:
  * armeabi/
  * armeabi-v7a/
  * gdx-backend-android.jar
  * gdx.jar
  
Again, we'll have to reference these files in our project settings. Right-click on the my-first-triangle-android folder and select Properties. Under the Java Build Path section, go to the Libraries tab. Click Add JARs... and navigate to my-first-triangle-android/libs and select `gdx-backend-android.jar` and `gdx.jar`.

We need to be able to reference the libgdx classes and also our previous project. While still in the Java Build Path screen, go to the Projects tab and click Add... and add the `my-first-triangle` project we created earlier.

The last task remaining is to modify our Activity to act as the Android entry point into the game code we wrote earlier. In the Package Explorer view, go into the `MyFirstTriangleAndroid` class and replace the contents with the following:

```java
package com.test.myfirsttriangle;

import android.os.Bundle;

import com.badlogic.gdx.backends.android.AndroidApplication;

public class MyFirstTriangleAndroid extends AndroidApplication {
    @Override
	public void onCreate (Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		initialize(new MyFirstTriangle(), false);		
	}
}
```

Similar to our desktop entry point in the `MyFirstTirangleDesktop` class, all this code does is to create an instance of `MyFirstTriangle` to manage the rendering process. Run the application by right-clicking on the my-first-triangle-android folder and selecting Run As -> Android Application. The application is then installed on the device or compatible simulator. After the simulator is finished loading, the application should start automatically. If it doesn't, find and click on the my-first-triangle-android application icon. The application should look like this:

![Triangle Android](http://i.imgur.com/xYU6Mjf.png)

# What's Next

That's it for your first triangle. To learn more about the fundamentals of the libgdx framework, check out the MeshColorTexture guide.

# Updates 
## Feb 22 2011
  * use nightly build
  * added `gdx-backend-jogl-natives.jar` to libraries to hopefully remove link errors
  * `MyFirstTriangle` class is now importing `com.badlogic.gdx.Gdx`
  * updated the `MyFirstTriangle_<date>.zip` source files for this guide
  * added screen shots