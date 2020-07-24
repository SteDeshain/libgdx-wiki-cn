# No Op: Not Translating


= Creating and texturizing 3d models in Blender for libgdx =
==Intro:==
*_You have a beautiful 3d scene pictured in your mind_*. You think you have a pretty good idea of how to code it, the interactions, the flow, but what about the models? What about the assets that are going to ‘Wow’ your users when they see them? Arguably the most important part of a 3d scene, the models are the stars of the show. Unfortunately, a lot of developers can code but don’t know how to create assets. This blog post is meant to introduce you to the answer.

Developer, this is Blender, Blender this is developer.

Libgdx easily lets you load blender exported .obj models in about 3 lines of code.

==Scope:==
This tutorial will guide you through the steps required to create, texturize and export a model from blender that can be loaded with libgdx or other frameworks.

==The Goods:==
Prereqs:
* Blender version 2.5
* Light knowledge of using blender interface, at least the Hello World
* A little prior knowledge of libgdx meshes and textures (Check this wiki)
* 20 minutes.


Fire up Blender, Click in object window, press spacebar.
Type ‘add ico sphere’ (100 ways to add an ico sphere, this is just one), click to add.

http://mobteklabs-wp.nfshost.com/wp-content/uploads/checkpoint1_ico.png

Select the ‘Viewport Shading’ menu next to the object mode menu then select ‘Wireframe’.

http://mobteklabs-wp.nfshost.com/wp-content/uploads/checkpoint2.png

Click on ‘Object Mode’ to the left of where you clicked for the wire mesh and select edit mode.
Type ‘a’ to unselect the object. (Try toggling ‘a’ to select and deselect all)
Type ‘b’ to activate block selecting mode and select the equator of the wireframed mesh. (Note you can use middle mouse button to look around and get a better look)
Click the ‘edge’ button to select the edges, not vertices(Near the center of the screen, check out the screenshot for reference)

http://www.mobteklabs.com/wp-content/uploads/checkpoint31.png

While equator edges are selected press ‘ctrl’ + ‘e’ to bring up the edges menu.
Select ‘Mark Seam’ – More on Seams here. TLDR; basically seams are like folds in your texture, will make more sense later.
Split the screen by selecting the lower left 3 stripes things at the very edge of the corner, click and drag out to create 2 windows.

http://mobteklabs-wp.nfshost.com/wp-content/uploads/checkpoint4.png

Back on the object screen, make sure your object is highlighted, then press ‘u’ for UV mapping menu, then select ‘Unwrap’.
Download this image

http://mobteklabs-wp.nfshost.com/wp-content/uploads/BlueMarble.jpg

On the other texture mapping screen, select the ‘Image’ menu, then ‘open image’, navigate to this image.

http://mobteklabs-wp.nfshost.com/wp-content/uploads/checkpoint5.png

On the texture mapping screen, type ‘a’ to select all and ‘r’ to rotate. Eventually you want to manipulate the texture to look something like below in the screenshot. (Look doesn’t really matter here, just the basics)
To manipulate this wireframe mesh mapping, you need to select different parts of the wireframe, either edges or vertices or whole selections(remember ‘b’ for box select). Now you use s, r, g to scale rotate and grab your selections! Try to cover most of the earths with the wireframe mesh.

http://mobteklabs-wp.nfshost.com/wp-content/uploads/cp6.png

Now go over to your object screen and click where the ‘Viewport shading’ where you selected to make a wireframe, and select ‘Textured’. You should now see your texture!! I don’t care who you are, stand up make two fists and THRUST them into the air. Like a boss. Multiple times if needed.
In the top left, you will see a menu of items, select the ‘Materials’ page, (check out the screenshot below for reference)

http://mobteklabs-wp.nfshost.com/wp-content/uploads/cp7.png

Click on ‘New’
Now click the next icon in the menu up top, the texture menu.
Click on ‘New’

http://mobteklabs-wp.nfshost.com/wp-content/uploads/Screen-shot-2011-06-08-at-11.06.22-PM-21.png

Click on open, navigate to where you saved the image above.
Click on the ‘Mapping’ tab down below
For ‘Coordinates’ select UV.
Now you should be able to export to .obj file and load with your favorite framework .obj loader method. Libgdx makes this as easy as can be. NOTE: Make sure your texture you use is in your ‘data’ directory like other libgdx image assets!

Link to original blog post: http://www.mobteklabs.com/2011/06/how-to-create-3d-models-in-blender-for-libgdx-android-iphone-etc/