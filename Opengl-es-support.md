Libgdx has interfaces for direct OpenGL ES 2.0 and 3.0 access. (If using LibGDX 0.9.9 or earlier, 1.0 and 1.x is available) OpenGL ES is emulated on the desktop by mapping OpenGL ES functions to desktop OpenGL functions. On some platforms (iOS, GWT/HTML) only OpenGL ES 2.0 is supported.

**Note:** On Android, if your application requires OpenGL ES 2 to function please add the following to the Android Manifest: `<uses-feature android:glEsVersion="0x00020000" android:required="true" />` 

If your Android application requires OpenGL ES 3, please add the following to the manifest:
`<uses-feature android:glEsVersion="0x00030000" android:required="true" />`

This prevents your application from being seen by unsupported devices in the Play Store (formerly Android Market)

## Precision modifiers ##
OpenGL ES 2.0 requires the specification of precision modifiers for attributes, uniforms and locals. Desktop OpenGL does not support this. You will have to guard against that in your fragment shader with something similar to this code snippet:

```java
#ifdef GL_ES 
#define LOW lowp
#define MED mediump
#define HIGH highp
precision mediump float;
#else
#define MED
#define LOW
#define HIGH
#endif
```

This will define the LOWP, MED, and HIGH macros to equivalent OpenGL ES precision modifiers and sets the default precision for float to medium. This will only happen on platforms actually running OpenGL ES, on the desktop, the macros are defined to be empty.

## OpenGL ES 2.0 Documentation ##
* [OpenGL ES 2.0 Reference Pages](http://www.khronos.org/opengles/sdk/docs/man/ "OpenGL ES 2.0 Reference Pages")
* [OpenGL ES 2.0 Reference Card](http://www.khronos.org/opengles/sdk/docs/reference_cards/OpenGL-ES-2_0-Reference-card.pdf "OpenGL ES 2.0 Reference Card")
* [OpenGL ES 2.0 Specification](http://www.khronos.org/registry/gles/#specs2 "OpenGL ES 2.0 Specification")
* [OpenGL ES Programming Guide for iOS](https://developer.apple.com/library/ios/documentation/3DDrawing/Conceptual/OpenGLES_ProgrammingGuide/Introduction/Introduction.html "OpenGL ES Programming Guide for iOS"): the core concepts are valid for other platforms as well
* [OpenGL ES 2.0 Pipeline Structure](http://en.wikibooks.org/wiki/OpenGL_Programming/OpenGL_ES_Overview#OpenGL_ES_2.0_Pipeline_Structure "OpenGL ES 2.0 Pipeline Structure")