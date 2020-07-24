Java apps need a Java Runtime Environment to run. Typically this is installed by the user and hopefully already available when they go to run your app. Unfortunately users may not have Java installed and there are differences between JREs that can cause problems with your app, especially between Java 6 and 7 on Mac. These can be difficult for users to explain and worse, difficult for them to fix themselves. Also, you may require, as a minimum, a certain JRE version.

The solution is to bundle a JRE with your app. This way you know exactly what users will be running and users will have fewer problems and they will not have to install a JVM. 

This however will come at the expense of about 23-30mb bigger download sizes per platform. This may improve when the modularity of the runtime improves, with JRE 9 (slated for march 2017 [countdown ] (http://www.java9countdown.xyz/))

## Contents

* [**Packaging**](#packaging) 
 * [**launch4j**](#launch4j) 
 * [**Parcl**](#parcl) 
 * [**packr**](#packr) 
* [**Manually, using the OpenJDK**](#openjdk) 

## Packaging ##
There are a few tools/plugins for helping the process of bundling a JRE, in no specific order... 

### Launch4J

[launch4j](http://launch4j.sourceforge.net/)

On Windows, launch4j can be used to create an EXE that starts the embedded JRE.

Mac uses a specific folder structure that will appear as an application in OSX. A shell script is invoked when the app is run. The app files should be packaged in a DMG so that execute flags are preserved, otherwise some ZIP extractors don't give the resulting files permission to execute.

Linux uses a shell script.

### Parcl
A gradle plugin that performs similar actions as launch4j:
https://github.com/mini2Dx/parcl

### Packr
Tool created/maintained by the libgdx team.
https://github.com/libgdx/packr

## OpenJDK ##

OpenJDK builds can be found here:
https://github.com/ojdkbuild/ojdkbuild

For Windows you'll want 32 bit. Mac has 32 and 64 bit combined. For Linux you might want separate 32 and 64 bit JREs. You can get away with only 32 bit for Linux, but users may need to do:

```java
sudo apt-get install ia32-libs
```

You can combine 32 and 64 bit for Linux since the JREs share many of the same files. This makes the JRE about 4MB larger but simplifies things by only having a single download. You would use a shell script to choose which to run, but note there are differences in rt.jar which you'll need to separate, eg into rt32.jar and rt64.jar then use -Xbootclasspath for the appropriate one.

## Reducing Size ##

There are a number of files and classes that can be removed from the JRE to reduce the size. Below is a list of files to delete from the Windows JRE. Other platforms are very similar, though you may need classes on some platforms but not others (eg, xml classes are needed on Linux to use java.util.preferences). This list leaves Swing intact, if you don't need Swing the size could be reduced further.

```
**.diz
**.exe except javaw.exe
bin\client\
lib\applet\
lib\charsets.jar
lib\ext\localedata.jar
lib\management\
lib\management-agent.jar
lib\zi\
lib\rt.jar\com\sun\org\
lib\rt.jar\com\sun\xml\
lib\rt.jar\com\sun\corba\
lib\rt.jar\com\sun\media\
lib\rt.jar\com\sun\jndi\
lib\rt.jar\com\sun\imageio\
lib\rt.jar\com\sun\jmx\
lib\rt.jar\com\sun\rowset\
lib\rt.jar\com\sun\java\util\
lib\rt.jar\javax\imageio\
lib\rt.jar\javax\management\
lib\rt.jar\javax\print\
lib\rt.jar\javax\naming\
lib\rt.jar\javax\sound\
lib\rt.jar\javax\sql\
lib\rt.jar\javax\xml\
lib\rt.jar\javax\swing\plaf\nimbus\
lib\rt.jar\javax\swing\text\html\
lib\rt.jar\org\
lib\rt.jar\sun\applet\
lib\rt.jar\sun\management\
lib\rt.jar\sun\rmi\
lib\rt.jar\sun\security\jgss\
lib\rt.jar\sun\security\krb5\
lib\rt.jar\sun\security\tools\
lib\resources.jar\com\sun\corba\
lib\resources.jar\com\sun\imageio\
lib\resources.jar\com\sun\jndi\
lib\resources.jar\com\sun\org\
lib\resources.jar\com\sun\rowset\
lib\resources.jar\com\sun\servicetag\
lib\resources.jar\com\sun\xml\
lib\jsse.jar\sun\security\ssl\
```

To make this list I went through the files and JARs sorting by largest size first. I then deleted the largest files that looked like that were not needed and ran my app to make sure everything still works.

This list reduces the JRE size to about 36MB. Note that for faster start up the JRE JARs are not compressed. After zipping the entire JRE, the size is reduced to about 13.5MB. If Swing packages are also removed from rt.jar, the zipped size goes down to about 9.8MB.
