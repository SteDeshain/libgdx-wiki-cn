This document is just a draft with some instructions to build libgdx's native libraries.

The following steps are based on Fedora 27.


Step 1: Installing needed packages
```bash
sudo dnf install glibc-devel.i686 mingw32-gcc-c++ mingw64-gcc-c++ mingw32-winpthreads-static mingw64-winpthreads-static
```

Step 2: Clone libgdx repository
```bash
git clone https://github.com/libgdx/libgdx.git
```

Step 3: Run ant script
```bash
cd libgdx/gdx/jni
ant
```

The jar will be generated at libgdx/gdx/libs.

To only generate .C/.H files, use:

```
mvn install

cd gdx
mvn clean compile exec:java -Dexec.mainClass="com.badlogic.gdx.utils.GdxBuild"
```