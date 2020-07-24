# fixme: weird image issues.

Every application or game that uses graphics, needs to work with the screen in a way. This page describes the different projections (for 2D and 3D) and how to set them up with libGDX.

For every application that uses graphics, a **viewport** needs to be defined. The **viewport** is a rectangular viewing region of the screen where the 3D scene is projected. It is nothing more than mapping the 3 dimensional objects to the 2 dimensional plane. 

The following image shows a typical 3D scene with a perspective projection.

[[images/screen-and-viewport1.png]]

The eye is where the camera is and the *viewport* is the plane where all the objects found in the *view frustum* are projected to.

The **view frustum** is a pyramid with two **clipping planes**. Objects or parts of objects found outside the frustum, won't be rendered.
It is not possible to define a near clipping plane closer to the observer than the viewport.

When the human eye looks at a scene, objects in the distance appear smaller than objects close by. This is called **perspective** and this is used for 3D games and applications. Perspective projection is the final image rendered onto the viewport with the objects scaled according to their distance from the observation point.

**Orthogonal projection** on the other hand, ignores the scaling effect and projects objects to the viewport maintaining their original size.

The following diagram shows the orthogonal projection.

[[images/screen-and-viewport2.png]]

For examples on how to work with the viewport, refer to [[this page | Projection, Viewport, & Camera]] where the notions are presented extensively with great examples.

To learn how to use the [[Orthographic Camera]] for simple 2D worlds.
