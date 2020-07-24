The Graphics module hides the complexity that comes with graphics programming using the OpenGL ES API. The module provides convenience methods to obtain instances of OpenGL ES wrappers regardless of the platform.

It also comes with a set of utility methods useful for obtaining information about the screen, like resolution, density, orientation, available OpenGL implementations, and also about measuring performance like frames per second, and time spent in frames.

Although the main Graphics interface exposes methods related to the above-mentioned functionality, the graphics module is much bigger. It contains a collection of classes that make graphics development extremely easy. 
It facilitates 2D and 3D graphics programming through the multitude of both high- and low level features and class implementations of the most common objects used in games.

# Features #

### 2D ###
  * Sprite rendering
  * Bitmap fonts
  * Particle systems
  * TMX map rendering
  * Bitmap manipulation
  * Complete 2D scene graph with tweening framework
  * Bitmap manipulation library


### 3D ###
  * Meshes with both fixed and programmable pipeline
  * Orthographic and perspective camera
  * Key-frame and skeletal animation
  * 3D model loaders (OBJ, MD5)

### Common/ Low level ###
  * Textures
  * Texture atlas
  * Vertex arrays
  * Vertex buffer objects (VBO)
  * Frame buffer objects (FBO)
  * Shaders
  * Immediate mode rendering
  * OpenGL context management (handling context loss on events)

### Utilities ###
  * Texture packing (TexturePacker)


LibGDX renders everything through OpenGL ES on all platforms. It supports versions 1.0, 1.1 and 2.0. 
For Android versions 2.0 - 2.2, OpenGL ES version 2.0 is supported through [custom JNI bindings](http://code.google.com/p/gl2-android/). Android 2.3 comes with a complete set of GLES20 bindings which is compatible with the custom one.

To learn about 2D and 3D graphics fundamentals and how to set up the screen in libGDX, go to [[2D & 3D Graphics Fundamentals and Setup | Screen & Viewport]].