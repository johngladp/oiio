Installation instructions for OpenImageIO
=========================================

# Table of Contents
1. [OpenImageIO's Dependences](#dependencies)
2. [Installing binaries from package managers](#installingfrompackagemanagers)
3. [Building OIIO from source](#buildingfromsource)


Dependencies
------------

NEW or CHANGED MINIMUM dependencies since the last major release are **bold**.

### Required dependencies -- OIIO will not build at all without these

 * C++11 (also builds with C++14 and C++17)
 * Compilers: gcc 4.8.2 - 10.1, clang 3.3 - 10.0, MSVS 2015 - 2019,
   icc version 13 or higher
 * CMake >= 3.12 (tested up through 3.17)
 * OpenEXR >= 2.0 (recommended: 2.2 or higher; tested through 2.5)
 * libTIFF >= 3.9 (recommended: 4.0+; tested through 4.1.0)

### Optional dependencies -- features may be disabled if not found
 * If you are building the `iv` viewer (which will be disabled if any of
   these are not found):
     * Qt >= 5.6 (tested through 5.14)
     * OpenGL
 * If you are building the Python bindings or running the testsuite:
     * Python >= 2.7 (tested against 2.7, 3.6, 3.7)
     * pybind11 >= 2.4.2 (Tested through 2.4.3.)
     * NumPy
 * If you want support for camera "RAW" formats:
     * LibRaw >= 0.15 (tested 0.15 - 0.20; LibRaw >= 0.18 is necessary for
       ACES support and much better recognition of camera metadata)
 * If you want support for a wide variety of video formats:
     * ffmpeg >= 2.6 (tested through 4.2)
 * If you want support for jpeg 2000 images:
     * **OpenJpeg >= 2.0** (tested through 2.3)
 * If you want support for Field3D files:
     * Field3D
 * If you want support for OpenVDB files:
     * OpenVDB >= 5.0 (tested through 7.0) and Intel TBB >= 2018 (tested
       through 2020_U1)
 * If you want support for converting to and from OpenCV data structures,
   or for capturing images from a camera:
     * OpenCV 2.x, 3.x, or 4.x (tested through 4.3)
 * If you want support for GIF images:
     * giflib >= 4.1 (tested through 5.2; 5.0+ is strongly recommended for
       stability and thread safety)
 * If you want support for HEIF/HEIC images:
     * libheif >= 1.3 (tested through 1.6; older versions may also work, we
       haven't tested)
* If you want support for DDS files:
     * libsquish >= 1.13 (tested through 1.15)
     * But... if not found on the system, an embedded version will be used.
* If you want support for DICOM medical image files:
     * DCMTK >= 3.6.1 (tested through 3.6.5)



Supported platforms at present include Linux (32 and 64 bit),
Mac OS X, and Windows.

Our build system is based upon 'CMake'.  If you don't already have it
installed on your system, you can get it from http://www.cmake.org

After you build OpenImageIO, if you compiled with the `EMBEDPLUGINS=0` flag
you will need to set the environment variable `OIIO_LIBRARY_PATH` to point
to the 'lib' directory where OpenImageIO is installed, or else it will
not be able to find the plugins.


Installing from package managers
================================

If all you want to do is install the OIIO libraries, headers, and command
line tools as quickly as possible (don't need OIIO source or any custom
build options), maybe one of these packages managers will do it for you:

* vckpg (https://github.com/Microsoft/vcpkg)
    * https://github.com/Microsoft/vcpkg/tree/master/ports/openimageio
    * `.\vcpkg install openimageio [tools]`
* homebrew (https://github.com/Homebrew/brew)
    * https://formulae.brew.sh/formula/openimageio
    * `brew install openimageio`
* macports (https://github.com/macports/macports-ports)
    * https://www.macports.org/ports.php?by=name&substr=openimageio
    * `port install openimageio`
* fink (https://github.com/fink/fink)
    * http://pdb.finkproject.org/pdb/package.php/libopenimageio2.1-shlibs
    * `fink install openimageio`
* conan (https://github.com/conan-io/conan)

If these work for you and it's all you need, bingo! You are done.



Building from source
====================


Dependency control and disabling components
-------------------------------------------

**Hints for finding dependencies**

For each external dependency PkgName, our CMake build system will recognize
the following optional variable:

    PkgName_ROOT=...

to specify a hint about where the package is installed. It can either be
a CMake variable (set by `-DPkgName_ROOT=...` on the CMake command line),
or an environment variable of the same name, or a variable setting on the
Make wrapper (`make PkgName_ROOT=...`).

**Disabling optional dependencies and individual components**

`USE_PYTHON=0` : Omits building the Python bindings.

`OIIO_BUILD_TESTS=0` : Omits building tests (you probably don't need them
unless you are a developer of OIIO or want to verify that your build
passes all tests).

`OIIO_BUILD_TOOLS=0` : Disables building all the command line tools (such
as iinfo, oiiotool, maketx, etc.).

`ENABLE_toolname=0` : Disables building the named command line tool (iinfo,
oiiotool, etc.). This works both as a CMake variable and also as an
environment variable.

`ENABLE_formatname=0` : Disables building support for the particular named
file format (jpeg, fits, png, etc.). This works both as a CMake variable and
also as an environment variable.

`ENABLE_PkgName=0` : Disables use of an *optional* dependency (such as
FFmpeg, Field3D, Webp, etc.) -- even if the dependency is found on the
system. This will obviously disable any functionality that requires the
dependency. This works both as a CMake variable and
also as an environment variable.



Building OpenImageIO on Linux or OS X
-------------------------------------

The following dependencies must be installed to build the core of
OpenImageIO:
* Boost
* libjpeg
* libtiff
* libpng
* OpenEXR.

These can be installed using the standard package managers on your system.
Optionally, to build the image viewing tools, you will need Qt and OpenGL.

On OS X, these dependencies can be installed using Fink, MacPorts or
Homebrew.  After installation of any of these package installers, use
the "fink", "port" or "brew" commands (respectively) to install the
dependencies (e.g. "fink install libpng16", "port install qt4-mac" or "brew
update; brew doctor; brew install qt") before invoking make as described
below.

On OS X, Fink can also be used to directly compile and install the OpenImageIO
tools directly with the command "fink install openimageio-tools".  On OS X
releases where Fink has a binary distribution (10.8, 10.9, and 10.10 as
of 2015), the command "apt-get install openimageio-tools" will fetch
prebuilt binaries.

Dependent libraries can be installed in either the system default
locations or in custom directories.  Libraries installed in custom
directories must notify the CMake system using environment variables.
For example, set QTDIR to point at the root of the Qt library location
so that CMake can find it (see CMake configuration output).

**On Linux and OS X, you can build from source from the top-level
directory by just typing 'make'.  (Yes, we have a 'make' wrapper around
our CMake build, it simplifies things.)**

During the make, various temporary files (object files, etc.) will
be put in build/PLATFORM, where 'PLATFORM' will be the name of the
platform you are building for (e.g., linux, linux64, macosx).

The result of the make will be a full binary distribution in
dist/PLATFORM.

Make targets you should know about:

|  Target           |  Command                                           |
| ----------------- | -------------------------------------------------- |
|  make             |  Build an optimized distro in dist/PLATFORM, with temp files created while building in build/PLATFORM. |
|  make debug       |  Build a debugging (symbols, not stripped) distro, will end up in dist/PLATFORM.debug |
|  make clean       |  Get rid of all the temporary files in build/PLATFORM |
|  make realclean   |  Get rid of both build/PLATFORM and dist/PLATFORM     |
|  make nuke        |  Get rid of all build/ and dist/, for all platforms   |
|  make profile     |  Build a profilable version dist/PLATFORM.profile     |
|  make help        |  Print all the make options                           |

Additionally, a few helpful modifiers alter some build-time options:

|  Target                   |  Command                                       |
| ------------------------- | ---------------------------------------------- |
| make VERBOSE=1 ...        |  Show all compilation commands                 |
| make STOP_ON_WARNING=0    |  Do not stop building if compiler warns        |
| make EMBEDPLUGINS=0 ...   |  Don't compile the plugins into libOpenImageIO |
| make USE_OPENGL=0 ...     |  Skip anything that needs OpenGL               |
| make USE_QT=0 ...         |  Skip anything that needs Qt                   |
| make MYCC=xx MYCXX=yy ... |  Use custom compilers                          |
| make USE_PYTHON=0 ...     |  Don't build the Python binding                |
| make BUILD_SHARED_LIBS=0  |  Build static library instead of shared        |
| make LINKSTATIC=1 ...     |  Link with static external libraries when possible |
| make SOVERSION=nn ...     |  Include the specifed major version number in the shared object metadata |
| make NAMESPACE=name       |   Wrap everything in another namespace         |

The command 'make help' will list all possible options.

You can also ignore the top level Makefile wrapper, and instead use
CMake directly:

    mkdir build
    cd build
    cmake ..

If the compile stops because of warnings, try again with

    make nuke
    make STOP_ON_WARNING=0

or, if you are using CMake directly,

    cd build
    cmake -DSTOP_ON_WARNING=0 ..




Building on Windows
-------------------

**Method 1 - Default**

1. Check out the trunk or a branch of your choice.  The remainder of
   these instructions assume that you checked out the trunk to the
   D:\OIIO\trunk directory.

2. Download the package with precompiled external libraries from
http://www.openimageio.org/external.zip

   Next, unpack it. The directory with downloaded code from the repository
   and the directory with unpacked libraries should be siblings. For example,

   ```
   D:
       \OIIO
           \trunk          // this is my tree
               \src        // directory with src files for OIIO
               \build      // directory that is created by cmake
           \external       // this is extracted external package
               \dist
                   \windows
                       \glew-1.5.1
                       \ilmbase-1.0.1
                       \jpeg-6b
                       \libpng-1.2.3
                       \openexr-1.6.1
                       \tbb-21_20080605oss
                       \zlib-1.2.3
                       \tiff-3.8.2
   ```

3. Download precompiled Qt4 binaries for Windows from here:
   http://qt.windows.binaries.googlepages.com/index.html

   Unpack it (it doesn't matter where). After unpacking, add the path to
   the Qt bin directory to the PATH variable. For example, if you unpacked
   this package to the D:\qt-win directory, you should add D:\qt-win\bin to
   your PATH. It's important to add the Qt bin directory to PATH because
   the FindQt4 module uses it to search for qmake applications.

4. Also, just to be safe, add QTDIR to the environment variables. It
   should point to directory where you unpacked Qt.

5. Download precompiled BOOST 1.53 or newer libraries from
   http://www.boostpro.com/download

   Install it on your system. Choose two versions: Multithread Debug, DLL
   and Multithread, DLL for Your Visual Studio version.

6. Download precompiled BOOST 1.53 or newer libraries from here (unfficial
   mirror) or from here (unofficial mirror, registration required). Install
   it on Your system. Choose two versions: Multithread Debug, DLL and
   Multithread, DLL for Your Visual Studio version.

7. Install cmake. You can download precompiled binaries from here:
   http://www.cmake.org/cmake/resources/software.html
   After installing, run cmake-gui. Set the field that specifies the source
   code location (for example, to D:\OIIO\trunk\src). Then set the field
   that specifies where to build binaries to the directory you want to
   build project for OIIO (for example, D:\OIIO\trunk\build).

8. Set the THIRD_PARTY_TOOLS_HOME environment variable to the directory
   where are stored unpacked external libraries (for example,
   D:\OIIO\external\dist\windows). You can add variables by clicking Add
   entry button.

9. Hit the Configure button. Cmake should automatically find externals
   libraries and prepare the environment for creating the OIIO project. If
   the configuration process ends without errors go to next step. If not,
   read the instructions from the end of this tutorial.

10. Hit the Generate button. Cmake will build Visual Studio a solution in
the build directory.

11. That's all. You can open the OpenImageIO solution and start developing
OIIO! Potential problems:


It may happen that cmake won't find zlib, png, tiff or jpeg
libraries. If so you have to set CMAKE_PREFIX_PATH to point to the
directory where the missing libraries are stored. For example, if cmake
can't find ZLIB, add to CMAKE_PREFIX_PATH the
D:\OIIO\external\dist\windows\zlib-1.2.3 directory. If it can't find
ZLIB and PNG, add
D:\OIIO\external\dist\windows\zlib-1.2.3;D:\OIIO\external\dist\windows\libpng-1.2.3.

**Method 2 - Using vcpkg**

1. Visit Microsoft's vcpkg GitHub page: https://github.com/Microsoft/vcpkg. Also note that the openimageio package is located here: https://github.com/Microsoft/vcpkg/tree/master/ports/openimageio

2. Follow vcpkg installation instructions and complete the install. Please note vcpkg has its own list of prerequisites listed on their page.

3. Execute the PowerShell command from where vcpkg is located in directory. ``.\vcpkg install openimageio``

Test Images
-----------

We have yet another project containing a set of sample images for testing
OpenImageIO. We split test images into a separate project in order to make
the main source code tree smaller and simpler for people who don't need the
test suite.

    git clone https://github.com/OpenImageIO/oiio-images.git

Also, there are collections of images for some of the file formats we
support, and make test expects them to also be present. To run full tests,
you will need to download and unpack the test image collections from:

* http://www.simplesystems.org/libtiff/images.html
* http://www.openexr.com/downloads.html
* http://www.itu.int/net/ITU-T/sigdb/speimage/ImageForm-s.aspx?val=10100803
* http://www.cv.nrao.edu/fits/data/tests/

These images should be placed in a sibling directory to the OpenImageIO
repository named oiio-testimages.

You do not need any of these packages in order to build or use
OpenImageIO. But if you are going to contribute to OpenImageIO
development, you probably want them, since it is required for executing
OpenImageIO's test suite (when you run "make test").
