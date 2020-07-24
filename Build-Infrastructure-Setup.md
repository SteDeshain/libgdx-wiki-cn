Our build infrastructure has the responsibility of building libgdx for all supported platforms (Windows, Linux, Mac OS X, Android, iOS, HTML). On top of the boring old Java code, we also need to compile the native code for each platform and architecture (32-, 64-bit if available).

We solve this issue by using cross-compilation on a Linux host to compile native code for Windows, Linux and Android. We additionally use a Mac to compile the iOS and Mac OS X native binaries.

The following sections document how to setup this build infrastructure.

## Setting up the Linux host
The Linux host has to be a 64-bit installation. We use Ubuntu 13.10, newer releases or other Linux distributions should work just as well. The following Debian packages can be installed from the official repositories

* unzip
* zip
* daemon
* git
* openjdk-7-jdk
* jenkins (latest)
  * `wget -q -O - http://pkg.jenkins-ci.org/debian/jenkins-ci.org.key | sudo apt-key add -`
  * add `deb http://pkg.jenkins-ci.org/debian binary/` to `/etc/apt/sources.list`
  * `sudo apt-get update` followed by `sudo apt-get install jenkins`, this will install the latest Jenkins version and provide an update path in the future instead of using the Jenkins version from the Ubuntu repositories
  * after installation, open `/etc/default/jenkins` and set `HTTP_HOST` and `AJP_HOST` to `0.0.0.0` to listen for connections on all net devices
  * setup users, enable global security using the Jenkins user database, letting any logged in user to anything
  * install the following plugins: Copy to Slave Plugin, Git Plugin, Mailer, Maven Project Plugin, Parameterized Trigger Plugin, Gradle Plugin
  * Go to the "Configure System" site and set the Ant and installation locations
* gcc, g++, gcc-multilib, g++-multilib (linux compilers, 32- and 64-bit)
* mesa-common-dev, libxxf86vm-dev, libxrandr-dev, libx11-dev:i386 (Linux OpenGL development, only needed for jglfw, 64-bit only)
* mingw-w64 (windows compilers, 32- and 64-bit)
* ccache (optional, see ccache section below)
* lib32z1 (cause Android SDK tools require it, aapt to be specific...)

The following software has to be installed manually

* [Ant 1.9.3+](http://ant.apache.org/) (fixes a bug that's vital for build time reduction!)
  * Download the latest binary distribution, unzip it to /opt/ant
  * Add `export PATH=$PATH:/opt/ant/bin` to `/etc/profile`
  * Make sure the Jenkins user can execute Ant
* [Maven 3+](http://maven.apache.org/download.cgi)
  * Download the latest binary distribution, unzip it to /opt/maven
  * Add `export PATH=$PATH:/opt/mave/bin` to `/etc/profile`
  * Make sure the Jenkins user can execute Maven
  * modify /opt/maven/conf/settings.xml to include the credentials for SonaType snapshot deployment (ask Mario)
* [Android NDK](https://developer.android.com/tools/sdk/ndk/index.html)
  * Download the latest tar.bz2, unpack to `/opt/android-ndk`
  * Add `export ANDROID_NDK=/opt/android-ndk` to `/etc/profile`
  * Add `export NDK_HOME=/opt/android-ndk` to `/etc/profile`
  * Make sure the Jenkins user can execute ndk-build
* [Android SDK]
  * Download the latest .tgz, unpack to /opt/android-sdk
  * Add `export ANDROID_HOME=/opt/android-sdk` to `/etc/profile`
  * In `/opt/android-sdk/tools`, run `./android update sdk --no-ui` to install all platforms, build tools etc.

## Setting up the Mac OS X host
We need a Mac to build natives for iOS and Mac OS X. This host will then be used as a Jenkins slave to build parts of the libgdx project. This dependency is expressed in the Jenkins Jobs responsible for building libgdx, see below. The Mac needs to have the following things installed:

* [JDK 8+](http://www.oracle.com/technetwork/java/javase/downloads/index-jsp-138363.html)
* XCode, install via the Mac App Store
  * Make sure the command line utilities are also installed, search Google for how to do this with the latest XCode.
  * you don't need a developers license to compile for a device, you just can't deploy to one
* Ant 1.9.3, install via [Homebrew](http://brew.sh/)
* (optional) ccache via Homebrew

Once the Mac is setup, you can move on to setting up the Jobs on Jenkins

## Setting up the libgdx Jenkins jobs
With your Linux host running Jenkins, go to http://JENKINS_ADDRESS:8080.

### Setting up the Jenkins Mac Slave
* On the landing page, click on `Manage Jenkins`, then go to `Manage Nodes`. 
* Click on `New Node`, give it the name `Mac`, select `Dumb Slave` and press `OK`.
* Set the `Remote FS root` to a folder of your choice, e.g. `/Users/badlogic/jenkins`
* Set `Usage` to `Leave this machine for tied jobs only`
* Set `Launch method` to `Launch Slave agents via Java Web Start`

You can now startup your Mac, open a browser and navigate to http://JENKINS_ADDRESS:8080/computer/Mac/. Click on the `Launch` button to download the Java Web start file, double click it to run the slave. Once it's running, you should see marked as being online in your Jenkins instance.

### Setting up the Jenkins libgdx Mac Job
The Mac job calls into the build-mac-ios.xml Ant script, then copies all the Mac and iOS native libraries to the Linux master node which will then continue building the rest of libgdx. For this we need to create a job, let's call it libgdx-mac.

* Click `New Item`, enter `libgdx-mac` as the name, and select `Freestyle Project`, press `OK`
* Click `Discard Old Builds`, set max # builds to keep to 1
* Click `Restrict where this project can be run` and enter the name of the Mac slave (`Mac`)
* Under `Source Code Management`, select git and specify the libgdx repo URL `https://github.com/libgdx/libgdx.git`, leave the rest as is (branch master etc.)
* Under `Build`, click `Add build step`, select `Invoke Ant`, set `Targets` to `-f build-mac-ios.xml -v`
* Under `Post-build Actions`, click `Add post-build action`, select `Copy files back to the job's workspace on the master node`, and for `Files to copy` specify `**/*.a,**/*.dylib`. Uncheck `Run After Result Is Finalised?`. Click on `Advanced`, then check `Override destination folder` and set it to `/var/lib/jenkins/workspace/libgdx` or wherever your libgdx workspace lives on the Linux host. This will copy all the Mac and iOS native libraries back to the master node, which will pack it with the native libraries for the other platforms
* Under `Post-build Actions`, click `Add post-build action`, select `E-mail Notification`, specify the recipients, use a space as a separator.

### Setting up the Jenkins libgdx Job
The main job is executed on the Linux host, which builds all Java related things, all Windows, Linux and Android native files, invokes the Mac job and copies the Mac and iOS natives over, then finally packages it all up neatly and distributes the nightly build to the website and pushes the Maven artifacts to SonaType. Here's how that job's setup

* Click `New Item`, enter `libgdx` as the name, and select `Freestyle Project`, press `OK`
* Click `Discard Old Builds`, set max # builds to keep to 1
* Under `Source Code Management`, select git and specify the libgdx repo URL `https://github.com/libgdx/libgdx.git`, leave the rest as is (branch master etc.)
* Under `Build Triggers`, select `Poll SCM` and set `Schedule` to `@hourly`
* Under `Build`, click `Add build step`, select `Trigger/call builds on other projects`, set `Projects to build` to `libgdx-mac`, check `Block until the triggered projects finish their builds`
* Under `Build`, click `Add build step`, select `Invoke Ant`, set `Targets` to `-f build.xml -Dbuild-natives=true -Dversion=nightly -v`
* (Optional) Under `Build`, click `Add build step`, select `Execute shell`, then write a shell script to copy the outputs of the libgdx build (nightly zip) to a webserver of your choice. See below for an example
* Under `Build`, click `Add build step`, select `Invoke top-level Maven targets`, set `Goals` to `clean deploy`
* Under `Post-build Actions`, click `Add post-build action`, select `E-mail Notification`, specify the recipients, use a space as a separator.

### Shell Script to copy nightly build to webserver
```shell
rm -rf /srv/www/libgdx.badlogicgames.com/public_html/nightlies/*.zip
rm -rf /srv/www/libgdx.badlogicgames.com/public_html/nightlies/*.zip.MD5
rm -rf /srv/www/libgdx.badlogicgames.com/public_html/nightlies/dist
rm -rf /srv/www/libgdx.badlogicgames.com/public_html/nightlies/docs
rm -rf /srv/www/libgdx.badlogicgames.com/public_html/nightlies/config
cp $WORKSPACE/libgdx-nightly.zip /srv/www/libgdx.badlogicgames.com/public_html/nightlies/libgdx-nightly-`date +%Y%m%d`.zip
cp $WORKSPACE/libgdx-nightly.zip.MD5 /srv/www/libgdx.badlogicgames.com/public_html/nightlies/libgdx-nightly-`date +%Y%m%d`.zip.MD5
ln -s /srv/www/libgdx.badlogicgames.com/public_html/nightlies/libgdx-nightly-`date +%Y%m%d`.zip /srv/www/libgdx.badlogicgames.com/public_html/nightlies/libgdx-nightly-latest.zip
ln -s /srv/www/libgdx.badlogicgames.com/public_html/nightlies/libgdx-nightly-`date +%Y%m%d`.zip.MD5 /srv/www/libgdx.badlogicgames.com/public_html/nightlies/libgdx-nightly-latest.zip.MD5
cp -r  $WORKSPACE/dist/docs /srv/www/libgdx.badlogicgames.com/public_html/nightlies/docs
cp -r  $WORKSPACE/dist/ /srv/www/libgdx.badlogicgames.com/public_html/nightlies/
cp -r $WORKSPACE/extensions/gdx-setup-ui/config /srv/www/libgdx.badlogicgames.com/public_html/nightlies/
rm -rf /srv/www/libgdx.badlogicgames.com/public_html/nightlies/.svn
```

## CCACHE
The native builds can take considerable time, as such we'd like to only build what changed. Our C/C++ build system is based on a custom set of Ant scripts which do not support incremental builds (but are super easy to setup, see [jnigen](https://github.com/libgdx/libgdx/wiki/jnigen). And even (C)Make builds don't necessarily help you with incremental builds. Also, you want to clear your build if you publish snapshots, so any incremental build you get from a proper C/C++ build tool doesn't really help.

To solve this issue, we use [ccache](http://ccache.samba.org/) which caches compilation results (object files). Ccache acts as a proxy to C/C++, hashes any source code file you push through it (expanding preprocessor macros and includes), checking if it has a binary fitting that hash in its cache. If it does, it can directly return the object file instead of having to invoke GCC. Which is nice, as it gives us sort of incremental builds even if we clean out any previous build results!

Setting up ccache for cross-compilation is pretty simple.

### Linux Host ccache setup
* Install ccache via `apt-get install ccache`
* Create a directory `/opt/ccache` owned by the jenkins user
* Add a line at the bottom of `/etc/profile` like this: `export PATH=/opt/ccache:$PATH`
* Add a line at the bottom of `/etc/profile` like this: `export NDK_CCACHE="ccache"`, this will make the Android NDK use ccache!
* For each compiler executable (gcc, g++, i686-w64-mingw32-gcc, i686-w64-mingw32-g++, x86_64-w64-mingw32-gcc, x86_64-w64-mingw32-g++), create a script that looks analogous to this:
```
ccache /usr/bin/x86_64-w64-mingw32-gcc "$@"
```

And that's it. Through this, we proxy all compiler calls through ccache which will speed up our build considerably.

### Mac Host ccache setup
* Install ccache through Homebrew
* Create a directory `/opt/ccache` owned by the jenkins user
* Add a line at the bottom of `/etc/profile` like this: `export PATH=/opt/ccache:$PATH`
* For each compiler executable (gcc, g++), create a script that looks analogous to this:
```
ccache /usr/bin/g++ "$@"
```

Through this, we proxy all compiler calls for Mac OS X native compilation through ccache, speeding up our build. I have yet to figure out a way to make this work with our iOS native builds.