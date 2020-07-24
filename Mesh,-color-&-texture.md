## Outdated ##
Refer to this page: https://github.com/libgdx/libgdx/wiki/3D-Graphics

# Summary
Going more indepth into Mesh, including colors and textures

## Introduction ##

The MyFirstTriangle tutorial showed us how to create a new project and use the Mesh class to render a simple triangle. This tutorial will explain the Mesh class methods in more detail, and also show how to add colors and a texture onto a mesh object.

## Setting up a Project ##

Follow the instructions in the MyFirstTriangle tutorial to create a new libgdx desktop project. We'll call the desktop project `mesh-color-texture` and we'll defer creating the Android project until later. Remember to include the libgdx library JAR files. In the project, create a package and name it `com.test.meshcolortexture`. In that package, create a class named `MeshColorTexture` which implements the `ApplicationListener` interfrace. Inside, we'll just copy most of what we created from the MyFirstTriangle tutorial:

```java
package com.test.meshcolortexture;

import com.badlogic.gdx.ApplicationListener;
import com.badlogic.gdx.graphics.GL10;
import com.badlogic.gdx.graphics.Mesh;
import com.badlogic.gdx.graphics.VertexAttribute;
import com.badlogic.gdx.graphics.VertexAttributes.Usage;

public class MeshColorTexture implements ApplicationListener {
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

Then, create a class named `MeshColorTextureDesktop` that creates a `JoglApplication` using an instance of the `MeshColorTexture` class. Again, it's very similiar to the MyFirstTriangle tutorial:

```java
package com.test.meshcolortexture;

import com.badlogic.gdx.backends.jogl.JoglApplication;

public class MeshColorTextureDesktop {
    public static void main (String[] argv) {
        new JoglApplication(new MeshColorTexture(), "Mesh with Color and Texture", 480, 320, false);
    }
}
```

## Mesh ##

Now lets go over in more detail what some of these methods do, starting with the `MeshColorTexture` class. In `create()`, we see:

```java
mesh = new Mesh(true, 3, 3, 
        new VertexAttribute(Usage.Position, 3, "a_position"));
```

The `Mesh` class constructor has several parameters. The first one specifies whether the mesh is static. Setting this to true will enable OpenGL optimizations beneficial to a mesh that doesn't change very much (i.e. every frame). The second and third parameters specify the number of *vertices* and *indices* this mesh will have. Since the mesh represents a triangle, it has three vertices. Each vertex is used once in the rendering of the triangle, so the Mesh requires three indices.

Finally, we pass in a `VertexAttribute` object to specified which type of information we will set for this mesh. This is a very simple mesh, and we're only going to set its position. In the `VertexAttribute` constructor, we pass in a `Usage` constant that denotes positional information. We then pass in an integer representing the number of components needed for each position. Since a position is represented by three values (x, y, and z), we pass in `3`. Finally, we give the `VertexAttribute` the alias `a_position`, but any word will do.

```java
mesh.setVertices(new float[] { -0.5f, -0.5f, 0,
                               0.5f, -0.5f, 0,
                               0, 0.5f, 0 });	
mesh.setIndices(new short[] { 0, 1, 2 });
```

After creating a mesh, we specify its vertices using a float array. The first three floats in the array are the x, y, z components of vertex 0, the next three floats make up vertex 1, and the last three vertex 2. We then set the indices, which is the order by which vertices are traversed to create the triangle.

Vertex 1 is located at 0.5(x), -0.5(y), 0(z). To visualize where this point is on the screen, imagine that the center of the screen is at 0(x), 0(y), 0(z). The left edge of the screen is at -1(x), the right edge 1(x). The bottom of the screen is at -1(y), the top 1(y). Here's an image of the concept:

![Image of Coordinate System](http://i.imgur.com/yLnMRu5.png)

```java
public void render() {
    Gdx.gl.glClear(GL10.GL_COLOR_BUFFER_BIT);
    mesh.render(GL10.GL_TRIANGLES, 0, 3);
}
```

The `render()` method is where the meshes in the application are drawn. We call `glClear(int)` to clear the buffer, turning the scene black (by default). Then the mesh just has to call `render(int primitiveType, int offset, int type)`. The first parameter specifies what type of primitive shape makes up this mesh. The second parameter would be used as an offset if this mesh was represented by a vertex buffer. Vertex buffers will be touched on in a later tutorial, for now just use `0`. The final parameter specifies the number of indices this mesh uses.

## Color ##

If we wanted to this mesh to be colored, we only need to add another `VertexAttribute` and specify color information for the vertices. Modify the code to to look like following (you will need to import the `com.badlogic.gdx.graphics.Color` class):

```java
mesh = new Mesh(true, 3, 3, 
        new VertexAttribute(Usage.Position, 3, "a_position"),
        new VertexAttribute(Usage.ColorPacked, 4, "a_color"));		

mesh.setVertices(new float[] { -0.5f, -0.5f, 0, Color.toFloatBits(255, 0, 0, 255),
                               0.5f, -0.5f, 0, Color.toFloatBits(0, 255, 0, 255),
                               0, 0.5f, 0, Color.toFloatBits(0, 0, 255, 255) });	
mesh.setIndices(new short[] { 0, 1, 2 });
```

In the mesh construction, we added a `VertexAttribute` for color. It contains 4 components (R, G, B, Alpha) and we give it the alias `a_color`. We then add color information to the float array. When we run the app, it should look like this:

![Image of Mesh Color Texture Colored](http://i.imgur.com/mPCd20W.png)

## Texture ##

To put a texture on the mesh, we will need to put an image file in our workspace, create a texture object in our code referencing that image, and then add a texture `VertexAttribute` to our mesh.

To start, inside the `mesh-color-texture` project, create a folder named `data` on the same level as the `src` and `libs` folders. Copy any image into the folder, in this example we'll use [http://i.imgur.com/joJOP1R.jpg]. It's important to note that any image used to create a texture must have width and height each be a power of two.

![Image of Badlogic](http://i.imgur.com/joJOP1R.jpg)

In the `MeshColorTexture` class, we need to add a `Texture` object field, then modify the `create()` and `render` methods. Here are the changes:

```java
private Mesh mesh;
private Texture texture;

@Override
public void create() {
    if (mesh == null) {
        mesh = new Mesh(true, 3, 3, 
                new VertexAttribute(Usage.Position, 3, "a_position"),
                new VertexAttribute(Usage.ColorPacked, 4, "a_color"),
                new VertexAttribute(Usage.TextureCoordinates, 2, "a_texCoords"));

        mesh.setVertices(new float[] { -0.5f, -0.5f, 0, Color.toFloatBits(255, 0, 0, 255), 0, 1,
                                       0.5f, -0.5f, 0, Color.toFloatBits(0, 255, 0, 255), 1, 1,
                                       0, 0.5f, 0, Color.toFloatBits(0, 0, 255, 255), 0.5f, 0 });
                                       
        mesh.setIndices(new short[] { 0, 1, 2 });

        FileHandle imageFileHandle = Gdx.files.internal("data/badlogic.jpg"); 
        texture = new Texture(imageFileHandle);
    }
}

@Override
public void render() {
    Gdx.gl.glClear(GL10.GL_COLOR_BUFFER_BIT);
    Gdx.graphics.getGL10().glEnable(GL10.GL_TEXTURE_2D);
    texture.bind();
    mesh.render(GL10.GL_TRIANGLES, 0, 3);
}
```

Import the necessary classes and run the `MeshColorTextureDesktop` application. The result should look like this:

![Image of Mesh Color Texture Textured](http://i.imgur.com/ODswR8B.png)

```java
new VertexAttribute(Usage.TextureCoordinates, 2, "a_texCoords")
```

Similar to when color was added, we used another `VertexAttribute` in the mesh's constructor. It specifies that the mesh will use a texture, which adds two components to each vertex.

```java
mesh.setVertices(new float[] { -0.5f, -0.5f, 0, Color.toFloatBits(255, 0, 0, 255), 0, 1,
                               0.5f, -0.5f, 0, Color.toFloatBits(0, 255, 0, 255), 1, 1,
                               0, 0.5f, 0, Color.toFloatBits(0, 0, 255, 255), 0.5f, 0 });
```

We add the texture coordinate components to the float array of `VertexAttribute` data components, right after the color data. To interpret the data, imagine that the texture is mapped to a 2D coordinate system. The texture's upper-left corner lies at (0, 0), upper-right at (1, 0), lower-left (0, 1), lower-right (1, 1). Looking at the three rows of vertex components we just modified, we can see that the vertex representing the lower-left corner of the triangle is mapped to the lower-left corner of the texture. The lower-right corner is mapped to the lower-right corner of the texture. The upper-center corner is mapped ot the upper-center of the texture.

```java
FileHandle imageFileHandle = Gdx.files.internal("data/badlogic.jpg"); 
texture = new Texture(imageFileHandle);
```

To create a `Texture` object from the JPG file we added to the workspace earlier, first create a file handle to the image. Then, just pass the file handle into the `Texture` class contractor.

```java
public void render() {
    Gdx.gl.glClear(GL10.GL_COLOR_BUFFER_BIT);
    Gdx.graphics.getGL10().glEnable(GL10.GL_TEXTURE_2D);
    texture.bind();
    mesh.render(GL10.GL_TRIANGLES, 0, 3);
}
```

Finally, when rendering, we enable textures, set the texture we created as the active texture, and render the mesh.

## Running in Android ##

To run this example on an Android device or simulator, we just need to follow steps similiar to those in the MyFirstTriangle tutorial. Here's a quick summary:

  - Create an Android project
  - Copy the libgdx libraries into the workspace and reference to them in the project settings
  - Reference the desktop project in the project settings
  - Modify the main `Activity` so that it extends `AndroidApplication` instead of `Activity`
  - Inside `onCreate()`, initialize the application with a new instance of the `MeshColorTexture` class

The one last thing thing we need to do is to add the image file to the Android workspace. Under your Android project, if a folder named `assets` does not already exist adjacent to the `src` and `libs` directory, then create it. Inside `assets`, create a folder named `data`, and put your image file in it, in our case it was `badlogic.jpg`. Now you can run the project as an Android application.

![Image of Mesh Color Texture Android](http://i.imgur.com/qhCuLox.png)

## Conclusion ##

There are several resources to learn more about meshes, colors, and textures. In the [http://code.google.com/p/libgdx/downloads/list Downloads page], you can get the source code of the game Invaders, which uses these element more extensively. The Javadoc is quite complete, and provides better details on the requirements of different classes and methods. Check out this [http://www.badlogicgames.com/wordpress/?p=1199 article] for more information about file access within libgdx. If you have more questions, please visit our forum or go on our IRC channel `#libgdx` hosted on `Freenode`. Also on the [http://code.google.com/p/libgdx/downloads/list Downloads page], you can find `MeshColorTexture.zip`, which contains the source code to the examples used in this tutorial.

= Change Log =
== Feb 22 2011 ==
  * fixed rendering on Mac OS X
  * added screen shots
  * moved `badlogic.jpg` into the wiki
  * use the new `Texture` constructor instead of old factory method