Libgdx is a cross-platform game and visualization development framework. It currently supports Windows, Linux, Mac OS X, Android, Blackberry, iOS, and HTML5 as target platforms. 

Libgdx allows you to write your code once and deploy it to multiple platforms without modification. Instead of waiting for your latest modifications to be deployed to your device or to be compiled to HTML5, you can benefit from an extremely fast iteration cycle by coding your application mainly in a desktop environment. You can use all the tools of the Java ecosystem to be as productive as you can be. Because it is on the JVM, you can also feel free to [use other awesome and popular non-Java languages (Kotlin, Scala, Clojure, etc)](Using-libgdx-with-other-jvm-languages)

Libgdx lets you go as low-level as you want, giving you direct access to file systems, input devices, audio devices and OpenGL via a unified OpenGL ES 2.0 and 3.0 interface. 

On top of these low-level facilities we built a powerful set of APIs that help you with common game development tasks like rendering sprites and text, building user interfaces, playing back sound effects and music streams, linear algebra and trigonometry calculations, parsing JSON and XML, and so on.

Where necessary, libgdx leaves the realm of Java and resorts to native code to aim for the best possible performance. All of this functionality is hidden behind Java APIs so you don't have to worry about cross-compiling native code for all platforms. Many parts of libgdx work around known platform issues so you don't have to deal with them.

Libgdx aims to be a framework rather than an engine, acknowledging that there is no one-size-fits-all solution. Instead we give you powerful abstractions that let you choose how you want to write your game or application.

# Standing on the Shoulders of Giants

Libgdx ties in a lot of third party libraries to provide its functionality:

- [OpenGL](http://www.opengl.org)
- [FreeType](http://www.freetype.org)
- [mpg123](http://www.mpg123.de)
- [xiph.org](http://www.xiph.org/vorbis)
- [soundtouch](http://www.surina.net/soundtouch)
- [box2d](http://www.box2d.org)
- [LWJGL](http://www.lwjgl.org)
- [OpenAL](http://en.wikipedia.org/wiki/OpenAL)
- [KissFFT](http://sourceforge.net/projects/kissfft)