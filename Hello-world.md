# Introduction #

Importing an existing project will help demonstrate the structure of a libgdx project, and also serves as an introduction into the development workflow.

# Requirements #

All that is required is Eclipse with the Android SDK installed. Go to the [Android SDK Instructions](http://developer.android.com/sdk/index.html) webpage for more details.

# Download and Import #

Download the `helloworld.zip` source code at [downloads page](http://code.google.com/p/libgdx/downloads/list).

In Eclipse, go to File -> Import... -> General -> Existing Projects Into Workspace. In the Import Projects screen, click Select archive file, and browse to the `helloworld.zip`. After that, make sure the `gdx-helloworld` and `gdx-helloworld-android` projects are found and selected, then click Finish.

# Running #

The first project we'll look at, `gdx-helloworld`, is where the game's code is located. You can run it as a stand-alone Java Desktop application, that is, it requires no simulator or Android handset. To run the application, in your Package Explorer view, right click on the `gdx-helloworld` folder, select Run As -> Java Application. If a dialog pops up, asking for the main class, select `HelloWorldDesktop`. The `Hello World` desktop application should start up:

The `gdx-helloworld-android` project adds a thin layer of code ontop of `gdx-helloworld` to allow it to run on an Android device. If you look at its `src` source directory, you'll see that it contains very little code. That's because the project references everything from `gdx-helloworld`. To run the application, in your Package Explorer view, right-click on the `gdx-helloworld-android` folder and select Run As -> Android Application. At this point, the application will be loaded and run on a compatible simulator (i.e. Android Virtual Device) or an Android device connected to the computer:

# Going Forward #

Since the Android simulator is too slow to properly test games with, being able to run the game as a desktop application helps streamline the development process. Developers avoid having to load the game onto a device every time they want to preview it. Furthermore, a team of developers and designers can more quickly test and share changes with one another.

You can experiment with this process by making some changes to `gdx-helloworld` project, then running it as a desktop application. When you're happy with the end result, see how your changes look on Android by running `gdx-helloworld-android`. A more pain-free development process is just one of the advantages of libgdx. Keep an eye out for upcoming tutorials about the different features of libgdx.