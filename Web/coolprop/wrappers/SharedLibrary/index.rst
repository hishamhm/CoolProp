
.. _shared_library:

********************
Shared Library (DLL)
********************

General Information
===================

Shared libraries are compiled code that can be accessed by other programs.  On windows, they are DLL files, on other platforms .os (linux) or .dylib (OSX)

There are a few things that need to be considered when determining what shared library you should build/use:

* Calling convention (`http://en.wikipedia.org/wiki/Calling_convention <wikipedia>`_: ``__stdcall`` or ``__cdecl`` - only a consideration on 32-bit windows
* Architecture: 32-bit or 64-bit
* Compiler: Visual Studio, Mingw, GCC, clang

Pre-Compiled Binaries
======================

Download the appropriate shared library for your architecture from from :sfdownloads:`shared_library`, or the development versions from the buildbot server at :sfnightly:`shared_library`.

User-Compiled Binaries
======================

Common Requirements
-------------------
Compilation of a shared library requires a few :ref:`common wrapper pre-requisites <wrapper_common_prereqs>`

Windows
-------
On windows, the greatest amount of complexity is experienced.

Your compiler options are:

* MinGW (a windows port of GCC)
* Visual Studio, multiple versions can be installed in parallel

By default, cmake will use your most up to date version of visual studio it finds.

Your calling convention options are:

* 32-bit __stdcall (``-DCOOLPROP_32BIT_STDCALL_SHARED_LIBRARY=ON``)
* 32-bit __cdecl (``-DCOOLPROP_32BIT_CDECL_SHARED_LIBRARY=ON``)
* 64-bit (``-DCOOLPROP_64BIT_SHARED_LIBRARY=ON``)

You can select the compiler in the call to cmake below.

1. Check out sources and go into the build directory::

    # Check out the sources for CoolProp
    git clone https://github.com/CoolProp/CoolProp --recursive
    # Move into the folder you just created
    cd CoolProp
    # Make a build folder
    mkdir build && cd build

2. Generate the build file.  Here is where it gets complicated.

    A. If you use MinGW, these are your options:

    For 64-bit DLL::

        cmake .. -DCOOLPROP_64BIT_SHARED_LIBRARY=ON -G "MinGW Makefiles"

    For 32-bit __stdcall DLL::

        cmake .. -DCOOLPROP_32BIT_STDCALL_SHARED_LIBRARY=ON -G "MinGW Makefiles"

    For 32-bit __cdecl DLL::

        cmake .. -DCOOLPROP_32BIT_CDECL_SHARED_LIBRARY=ON -G "MinGW Makefiles"

    B. If you use Visual Studio, you will need to replace the visual studio version with the version that you are using.  Running the command ``cmake`` at the command prompt will tell you what generators are supported

    For 64-bit DLL (Watch out for the 64-bit flag with Win64)::

        cmake .. -DCOOLPROP_64BIT_SHARED_LIBRARY=ON -G "Visual Studio 10 2010 Win64"

    For 32-bit __stdcall DLL::

        cmake .. -DCOOLPROP_32BIT_STDCALL_SHARED_LIBRARY=ON -G "Visual Studio 10 2010"

    For 32-bit __cdecl DLL::

        cmake .. -DCOOLPROP_32BIT_CDECL_SHARED_LIBRARY=ON -G "Visual Studio 10 2010"

3. Do the build::

    cmake --build . --config Release

If you are using MinGW, you can leave off the ``--config Release``, the default build configuration is release

Linux & OSX
-----------

On linux and OSX there is no calling convention to worry about, only options are 32-bit and 64-bit compilation.  If you give one of the 32-bit flags, the calling convention will be ignored, so just do that.

For 32-bit compilation::

    # Check out the sources for CoolProp
    git clone https://github.com/CoolProp/CoolProp --recursive
    # Move into the folder you just created
    cd CoolProp
    # Make a build folder
    mkdir build && cd build
    # Generate builder
    cmake .. -DCOOLPROP_32BIT_SHARED_LIBRARY_LINUX_MODULE=ON
    # Build
    cmake --build .

For 64-bit compilation::

    # Check out the sources for CoolProp
    git clone https://github.com/CoolProp/CoolProp --recursive
    # Move into the folder you just created
    cd CoolProp
    # Make a build folder
    mkdir build && cd build
    # Generate builder
    cmake .. -DCOOLPROP_64BIT_SHARED_LIBRARY=ON
    # Build
    cmake --build .

On Linux, installation could be done by::

    # Change "32" to match your system bitness
    sudo cp libCoolProp.so /usr/local/lib/libCoolProp.so.32.:version: 
    pushd /usr/local/lib
    sudo ln -sf libCoolProp.so.32.:version: libCoolProp.so.5
    sudo ln -sf libCoolProp.so.5 libCoolProp.so
    popd
