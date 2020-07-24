# No Op: Not Translating


# Summary 

Learn about a viewport's proportions, how to specify a projection, and to use Camera

# Introduction
In the [MyFirstTriangle](My-First-Triangle-(Example)) and [ColorMeshTexture](Mesh%2C-color-%26-texture) tutorials, we learned the basics of setting up and rendering meshes. Did you notice, however, that the triangles we created appear stretched? Lets start with an example that will make this issue clear.

# Creating a Square

Create a new libgdx desktop project named `projection-viewport-camera`. Create a class called `ProjectionViewportCamera` that implements `ApplicationListener` and put it in the package `com.example.projectionviewportcamera`. For full instruction on how to set up a libgdx desktop (and Android) project, look at the [MyFirstTriangle](My-First-Triangle-(Example)) tutorial. Unlike in that tutorial, we're going to show a square instead of a rectangle. Here's the code for `ProjectionViewportCamera`:

```java
package com.example.projectionviewportcamera;

import com.badlogic.gdx.ApplicationListener;
import com.badlogic.gdx.Gdx;
import com.badlogic.gdx.graphics.Color;
import com.badlogic.gdx.graphics.GL10;
import com.badlogic.gdx.graphics.Mesh;
import com.badlogic.gdx.graphics.VertexAttribute;
import com.badlogic.gdx.graphics.VertexAttributes.Usage;

public class ProjectionViewportCamera implements ApplicationListener {
    private Mesh squarePartOne;
    private Mesh squarePartTwo;

    @Override
    public void create() {
        if (squarePartOne == null) {
            squarePartOne = new Mesh(true, 3, 3, 
                    new VertexAttribute(Usage.Position, 3, "a_position"),
                    new VertexAttribute(Usage.ColorPacked, 4, "a_color"));

            squarePartOne.setVertices(new float[] {
                    -0.5f, -0.5f, 0, Color.toFloatBits(128, 0, 0, 255),
                    0.5f, -0.5f, 0, Color.toFloatBits(192, 0, 0, 255),
                    -0.5f, 0.5f, 0, Color.toFloatBits(192, 0, 0, 255) });   
            squarePartOne.setIndices(new short[] { 0, 1, 2});
        }

        if (squarePartTwo == null) {
            squarePartTwo = new Mesh(true, 3, 3, 
                    new VertexAttribute(Usage.Position, 3, "a_position"),
                    new VertexAttribute(Usage.ColorPacked, 4, "a_color"));

            squarePartTwo.setVertices(new float[] {
                    0.5f, -0.5f, 0, Color.toFloatBits(192, 0, 0, 255),
                    -0.5f, 0.5f, 0, Color.toFloatBits(192, 0, 0, 255),
                    0.5f, 0.5f, 0, Color.toFloatBits(255, 0, 0, 255) });   
            squarePartTwo.setIndices(new short[] { 0, 1, 2});
        }
    }

    @Override
    public void dispose() { }

    @Override
    public void pause() { }

    @Override
    public void render() {
        Gdx.gl.glClear(GL10.GL_COLOR_BUFFER_BIT);
        squarePartOne.render(GL10.GL_TRIANGLES, 0, 3);
        squarePartTwo.render(GL10.GL_TRIANGLES, 0, 3);
    }

    @Override
    public void resize(int width, int height) { }

    @Override
    public void resume() { }
}
```

It's very similar to the class described in the MyFirstTriangleTutorial except that it has two triangles instead of one. The two triangles are positioned to form a square, with corners at position (x, y) coordinates (-0.5, -0.5), (0.5, -0.5), (0.5, 0.5), and (0.5, -0.5). When you run the desktop app it should look like this:

[[images/projection_viewport_camera_rectangle.png]]

Obviously, even though we specified the coordinates of a square, the resulting shape turned out to be a wide rectangle. Before tackling this issue, I will digress for a moment and offer a rendering optimzation for this square.

The two triangles each have a vertex at (0.5, -0.5) and a vertex at (-0.5, 0.5). We can eliminate the duplication of vertices at identical coordinates by implementing the square using [triangle strips](http://en.wikipedia.org/wiki/Triangle_strip). Using triangle strips allow the two triangles to share vertices and therefore use memory more efficiently (check out [Jeff LaMarche's tutorial](http://iphonedevelopment.blogspot.com/2009/04/opengl-es-from-ground-up-part-2-look-at.html) for a better illustration). Here's the new implementation:

```java
package com.example.projectionviewportcamera;

import com.badlogic.gdx.ApplicationListener;
import com.badlogic.gdx.Gdx;
import com.badlogic.gdx.graphics.Color;
import com.badlogic.gdx.graphics.GL10;
import com.badlogic.gdx.graphics.Mesh;
import com.badlogic.gdx.graphics.VertexAttribute;
import com.badlogic.gdx.graphics.VertexAttributes.Usage;

public class ProjectionViewportCamera implements ApplicationListener {
    private Mesh squareMesh;

    @Override
    public void create() {
        if (squareMesh == null) {
            squareMesh = new Mesh(true, 4, 4, 
                    new VertexAttribute(Usage.Position, 3, "a_position"),
                    new VertexAttribute(Usage.ColorPacked, 4, "a_color"));

            squareMesh.setVertices(new float[] {
                    -0.5f, -0.5f, 0, Color.toFloatBits(128, 0, 0, 255),
                    0.5f, -0.5f, 0, Color.toFloatBits(192, 0, 0, 255),
                    -0.5f, 0.5f, 0, Color.toFloatBits(192, 0, 0, 255),
                    0.5f, 0.5f, 0, Color.toFloatBits(255, 0, 0, 255) });   
            squareMesh.setIndices(new short[] { 0, 1, 2, 3});
        }

    }

    @Override
    public void dispose() { }

    @Override
    public void pause() { }

    @Override
    public void render() {
        Gdx.gl.glClear(GL10.GL_COLOR_BUFFER_BIT);
        squareMesh.render(GL10.GL_TRIANGLE_STRIP, 0, 4);
    }

    @Override
    public void resize(int width, int height) { }

    @Override
    public void resume() { }
}
```

The two triangles have been combined in a single mesh with four vertices and four indices. Inside `render()`, we specify that the square was defined using triangle strips. Fewer vertices are necessary and the code is simpler too.

# Viewport

Back to the issue of the stretched square. By default the viewport reveals a rectangular area with a left edge at x = -1, right edge at x = 1, top edge at y = 1, and bottom edge at y = -1. Here's a picture of what a blank area like that would look like if we were to create mockup:

[[images/projection_viewport_camera_normal_canvas.png]]

When we place out red square onto that area, the result looks like this:

[[images/projection_viewport_camera_normal_square.png]]

However, the viewport itself is wider than a square. In order to fill up the extra space on the sides, the area is stretched. Imagine a painter stretching a square canvas to fit a rectangular frame. The result looks like what we see in the MeshColorTexture tutorial:

[[images/projection_viewport_camera_stretched_view.png]]

# Camera

To correct this effect, we allow the viewport to render an area that is rectangular instead of square. The shape of this area will match the shape of viewport, sort of like a painter choosing a canvas size based on the picture frame's size. To accomplish this, we use the `Camera` class, in this a case a subclass called `OrthographicCamera`. The `Camera` class allows us to change the area shown by the viewport. Here's the new code: 

```java
package com.example.projectionviewportcamera;

import com.badlogic.gdx.ApplicationListener;
import com.badlogic.gdx.Gdx;
import com.badlogic.gdx.graphics.Color;
import com.badlogic.gdx.graphics.GL10;
import com.badlogic.gdx.graphics.Mesh;
import com.badlogic.gdx.graphics.OrthographicCamera;
import com.badlogic.gdx.graphics.VertexAttribute;
import com.badlogic.gdx.graphics.VertexAttributes.Usage;

public class ProjectionViewportCamera implements ApplicationListener {
    private Mesh squareMesh;
    private OrthographicCamera camera;

    @Override
    public void create() {
        if (squareMesh == null) {
            squareMesh = new Mesh(true, 4, 4, 
                    new VertexAttribute(Usage.Position, 3, "a_position"),
                    new VertexAttribute(Usage.ColorPacked, 4, "a_color"));

            squareMesh.setVertices(new float[] {
                    -0.5f, -0.5f, 0, Color.toFloatBits(128, 0, 0, 255),
                    0.5f, -0.5f, 0, Color.toFloatBits(192, 0, 0, 255),
                    -0.5f, 0.5f, 0, Color.toFloatBits(192, 0, 0, 255),
                    0.5f, 0.5f, 0, Color.toFloatBits(255, 0, 0, 255) });   
            squareMesh.setIndices(new short[] { 0, 1, 2, 3});
        }
    }

    @Override
    public void dispose() { }

    @Override
    public void pause() { }

    @Override
    public void render() {
        camera.update();
        camera.apply(Gdx.gl10);

        Gdx.gl.glClear(GL10.GL_COLOR_BUFFER_BIT);
        squareMesh.render(GL10.GL_TRIANGLE_STRIP, 0, 4);
    }

    @Override
    public void resize(int width, int height) {
        float aspectRatio = (float) width / (float) height;
        camera = new OrthographicCamera(2f * aspectRatio, 2f);
    }

    @Override
    public void resume() { }
}
```

In `resize()`, we construct a new instance of `OrthographicCamera`, specifiying the area we want to display. In this example, we tell the camera to display an area two units tall, with a width that is determined by the shape of the viewport. In `render()`, we call simply call `update()` and `apply()` on the `Camera` instance. Here's how it looks when we run the app:

[[images/projection_viewport_camera_corrected_view.png]]

The desktop viewport in our tutorials have been 480 pixels wide and 320 pixels high. These are the numbers are passed into `resize()` when the app launches and when the screen rotates. The width of the screen is then calculated using the ratio of these numbers. In our example, width = 2 ˟ 480 / 320 = 3. Here's how the app looks with these coordinates overlayed:

[[images/projection_viewport_camera_corrected_coordinate.png]]

The square's vertices are still located at the same (x, y) coordinates as before. Now, we corrected its previous distortion by using a camera that projects the scene differently.

# Camera and Sprites

We haven't dealt with sprites since the HelloWorld example, but it's important to note that the `SpriteBatch` class actually has an internal camera different than the one we constructed. Drawing a sprite at coordinate (0, 0), for example, will not necessarily result in the sprite overlapping the square we're created. To solve this issue, it's a simple matter of synchronizing the two cameras at the start of the rendering phase. The code is included below (look at the MeshColorTexture tutorial to remind yourself how to load assets for a texture).

```java
package com.example.projectionviewportcamera;

import com.badlogic.gdx.ApplicationListener;
import com.badlogic.gdx.Gdx;
import com.badlogic.gdx.graphics.Color;
import com.badlogic.gdx.graphics.GL10;
import com.badlogic.gdx.graphics.Mesh;
import com.badlogic.gdx.graphics.OrthographicCamera;
import com.badlogic.gdx.graphics.Texture;
import com.badlogic.gdx.graphics.VertexAttribute;
import com.badlogic.gdx.graphics.VertexAttributes.Usage;
import com.badlogic.gdx.graphics.g2d.SpriteBatch;

public class ProjectionViewportCamera implements ApplicationListener {
    private Mesh squareMesh;
    private OrthographicCamera camera;
    private Texture texture;
    private SpriteBatch spriteBatch;

    @Override
    public void create() {
        if (squareMesh == null) {
            squareMesh = new Mesh(true, 4, 4, 
                    new VertexAttribute(Usage.Position, 3, "a_position"),
                    new VertexAttribute(Usage.ColorPacked, 4, "a_color"));

            squareMesh.setVertices(new float[] {
                    -0.5f, -0.5f, 0, Color.toFloatBits(128, 0, 0, 255),
                    0.5f, -0.5f, 0, Color.toFloatBits(192, 0, 0, 255),
                    -0.5f, 0.5f, 0, Color.toFloatBits(192, 0, 0, 255),
                    0.5f, 0.5f, 0, Color.toFloatBits(255, 0, 0, 255) });   
            squareMesh.setIndices(new short[] { 0, 1, 2, 3});
        }

        texture = new Texture(Gdx.files.internal("data/badlogic.jpg"));
        spriteBatch = new SpriteBatch();
    }

    @Override
    public void dispose() { }

    @Override
    public void pause() { }

    @Override
    public void render() {
        camera.update();
        camera.apply(Gdx.gl10);
        spriteBatch.setProjectionMatrix(camera.combined);

        Gdx.gl.glClear(GL10.GL_COLOR_BUFFER_BIT);
        squareMesh.render(GL10.GL_TRIANGLE_STRIP, 0, 4);

        spriteBatch.begin();
        spriteBatch.draw(texture, 0, 0, 1, 1, 0, 0,
                texture.getWidth(), texture.getHeight(), false, false);
        spriteBatch.end();
    }

    @Override
    public void resize(int width, int height) {
        float aspectRatio = (float) width / (float) height;
        camera = new OrthographicCamera(2f * aspectRatio, 2f);
    }

    @Override
    public void resume() { }
}
```

In `render()`, right after we update our regular camera, we provide the camera's projection information to the `SpriteBatch` instance we use to draw our sprite (we'll talk more about projection later). The result should look something like this:

[[images/projection_viewport_camera_sprite.png]]

It's important to note that drawing a sprite changes the projection of the entire scene to the sprite's projection. If you don't set the sprite's projection to the same as the rest of your scene, the simple act of drawing that sprite might change how the rest of your scene looks. This could be the desired effect, in which case it might be clearer to create a second camera, just for your `SpriteBatch` instance.

# Projection

Up to this point, we've been laying out meshes and sprites as if they're on a flat surface. There's no sense of depth, dimension, or distance. The OrthographicCamera we've been using provides an orthographic projection of our scene. This type of projection is only meant to provide a 2D sense of space, like in the original Super Mario, or Pacman. To show depth and 3D space, we need to use a perspective projection. To learn more about projection, check out [Mario's blog post on the Camera class](http://www.badlogicgames.com/wordpress/?p=1550) (also good is [Jeff LaMarche's article on the subject](http://iphonedevelopment.blogspot.com/2009/04/opengl-es-from-ground-up-part-3.html)).

To show how to use perspective projection in libgdx, lets introduce another square mesh, here's our new code:

```java
package com.example.projectionviewportcamera;

import com.badlogic.gdx.ApplicationListener;
import com.badlogic.gdx.Gdx;
import com.badlogic.gdx.graphics.Camera;
import com.badlogic.gdx.graphics.Color;
import com.badlogic.gdx.graphics.GL10;
import com.badlogic.gdx.graphics.Mesh;
import com.badlogic.gdx.graphics.OrthographicCamera;
import com.badlogic.gdx.graphics.VertexAttribute;
import com.badlogic.gdx.graphics.VertexAttributes.Usage;

public class ProjectionViewportCamera implements ApplicationListener {
    private Mesh squareMesh;
    private Mesh nearSquare;
    private Camera camera;

    @Override
    public void create() {
        if (squareMesh == null) {
            squareMesh = new Mesh(true, 4, 4, 
                    new VertexAttribute(Usage.Position, 3, "a_position"),
                    new VertexAttribute(Usage.ColorPacked, 4, "a_color"));

            squareMesh.setVertices(new float[] {
                    0, -0.5f, -4, Color.toFloatBits(128, 0, 0, 255),
                    1, -0.5f, -4, Color.toFloatBits(192, 0, 0, 255),
                    0, 0.5f, -4, Color.toFloatBits(192, 0, 0, 255),
                    1, 0.5f, -4, Color.toFloatBits(255, 0, 0, 255) });   
            squareMesh.setIndices(new short[] { 0, 1, 2, 3});
        }

        if (nearSquare == null) {
            nearSquare = new Mesh(true, 4, 4, 
                    new VertexAttribute(Usage.Position, 3, "a_position"),
                    new VertexAttribute(Usage.ColorPacked, 4, "a_color"));

            nearSquare.setVertices(new float[] {
                    -1, -0.5f, -1.1f, Color.toFloatBits(0, 0, 128, 255),
                    0, -0.5f, -1.1f, Color.toFloatBits(0, 0, 192, 255),
                    -1, 0.5f, -1.1f, Color.toFloatBits(0, 0, 192, 255),
                    0, 0.5f, -1.1f, Color.toFloatBits(0, 0, 255, 255) });   
            nearSquare.setIndices(new short[] { 0, 1, 2, 3});
        }
    }

    @Override
    public void dispose() { }

    @Override
    public void pause() { }


    @Override
    public void render() {
        camera.update();
        camera.apply(Gdx.gl10);
        Gdx.gl.glClear(GL10.GL_COLOR_BUFFER_BIT);
        squareMesh.render(GL10.GL_TRIANGLE_STRIP, 0, 4);
        nearSquare.render(GL10.GL_TRIANGLE_STRIP, 0, 4);
    }

    @Override
    public void resize(int width, int height) {
        float aspectRatio = (float) width / (float) height;
        camera = new OrthographicCamera(2f * aspectRatio, 2f);

    }

    @Override
    public void resume() { }
}
```

The new square is just another mesh, with a different color than the first. The red square is moved a bit further in the positive x direction, and the new square sits left of it. Here's how it should look:


[[images/projection_viewport_camera_two_squares.png]]

One other thing we changed is the z position of the squares. Notice that the red square is positioned further in the negative z direction (ie. further away from the viewer) and that the new blue square sits between the red square and the viewer. You can't tell any of this by the screenshot however, because we're using the orthographic camera, which basically ignores the z coordinate. Change the `resize()` method to use the `PerspectiveCamera`:

```java
    public void resize(int width, int height) {
        float aspectRatio = (float) width / (float) height;
        camera = new PerspectiveCamera(67, 2f * aspectRatio, 2f);
    }
```

Now the true distance between the squares and the user becomes obvious:

[[images/projection_viewport_camera_perspective.png]]

The only difference between the constructor of the `PerspectiveCamera` and that of the `OrthographicCamera` is that we now have to provide a field of view parameter, in this case `67`. The human field of view is roughly around 60 to 70 degrees, so 67 provides a pretty normal perspective. Note that by default, the `PerspectiveCamera` clips anything closer than one unit away from it. If you place an object too close to the camera, it will disappear. It also has a default far clipping pane one hundred units in front of itself, so objects too far away will not be rendered either. 

The sense of perspective becomes more obvious when we move the camera. Modify the `render()` method to do exactly that:

```java
    private int total = 0;
    private float movementIncrement = 0.0006f;

    @Override
    public void render() {
        total += 1;
        if (total > 500) {
            movementIncrement = -movementIncrement;
            total = -200;
        }

        camera.rotate(movementIncrement * 20, 0, 1, 0);
        camera.translate(movementIncrement, 0, movementIncrement);
        camera.update();
        camera.apply(Gdx.gl10);
        Gdx.gl.glClear(GL10.GL_COLOR_BUFFER_BIT);
        squareMesh.render(GL10.GL_TRIANGLE_STRIP, 0, 4);
        nearSquare.render(GL10.GL_TRIANGLE_STRIP, 0, 4);
    }
```

We use the camera's `rotate()` and `translate()` methods to move the camera around the scene. As mentioned, when the camera moves too close the blue square, you'll see it start to clip and disappear. Calling `translate()` on an `OrthographicCamera` will move the camera around without providing a sense of 3D space. Look at [Mario's post](http://www.badlogicgames.com/wordpress/?p=1550) for other useful methods and start experimenting.

# Running it on Android

Follow the steps of the MyFirstTriangle tutorial and set up the Android portion of this libgdx app. Remember to reference the necessary libraries, reference the desktop project, and add the necessary assets. When you run the app, however, you'll notice that everything appears very big:

[[images/projection_viewport_camera_portrait.png]]

This is the result of us locking the vertical viewable area to two units high. Since by default, the Android emulator is taller than it is wide, our horizontal viewable area is much smaller than three units wide, making it very different than the desktop version of this app. The simplest way to make the Android and desktop version appear similar is by locking the Android app to landscape orientation. Modify your manifest and set the `android:screenOrientation` property to `landscape` (read more about the property at the [Android developer site](http://developer.android.com/guide/topics/manifest/activity-element.html)). Here's an example of a modified `activity` element in the manifest:

```xml
        <activity android:name=".ProjectionViewportCameraAndroid"
                  android:label="@string/app_name" android:screenOrientation="landscape">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
```

Here're screen shots of our app locked in landscape orientation. You can change the emulator orientation by pressing `Ctrl-F11` or `Ctrl-F12`.  

[[images/projection_viewport_camera_locked_portrait.png]]

[[images/projection_viewport_camera_locked_landscape.png]]

Since Android devices have so many different screen sizes, locking the screen is not enough, you'll need to test your game with different aspect ratios. The easiest way to do that is through the desktop project. In your desktop project's launch class, instead of creating a viewport with our tutorial's 480 x 320 resolution, try other aspect ratios to get a sense of how your game feels like on diffferent devices.

# Conclusion

You should now have enough knowledge about meshes, sprites, and cameras to start constructing any basic scene you want. For more examples, look at the libgdx [demos](https://github.com/libgdx/libgdx/tree/master/demos) and [tests](https://github.com/libgdx/libgdx/tree/master/tests). You can grab the source files for this project in our [download page](http://libgdx.badlogicgames.com/download.html).