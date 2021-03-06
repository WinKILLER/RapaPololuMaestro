RapaPololuMaestro
=================
A cross-platform C++ library for controlling Pololu Maestro devices

![alt text](docs/RapaPololuMaestro1.jpg?raw=true "Pololu Maestro driving a servo")
![alt text](docs/RapaPololuMaestro2.jpg?raw=true "Pololu Maestro controlled by a Raspberry Pi")
![alt text](docs/RapaPololuMaestroViewer.png?raw=true "Pololu Maestro Viewer")

# Overview 
RapaPololuMaestro (formerly [Polstro](https://code.google.com/p/polstro/)) is a lightweight cross-platform C++ library that provides access, in an object-oriented fashion, to one or more [Pololu Maestro servo-controller devices](http://www.pololu.com/docs/0J40) connected to the computer.

The communication is based on the [Serial Interface protocol](http://www.pololu.com/docs/0J40/5.c Serial Interface), not the native USB interface (which offers more advanced functionalities).

With RapaPololuMaestro it's possible to:

* set/get the target position of any servo connected to the Maestro device (from 6 to 24 depending on the Maestro model)
* set the speed and acceleration at which the Maestro changes the position.

Note that by servo here we mean any RC component that is driven by a PWM signal. This can be for example an ESC (Electronic Speed Controller) that controls a brushless motor.

The code is based on the [cross-platform C](http://www.pololu.com/docs/0J40/5.h.2) and [Windows examples](http://www.pololu.com/docs/0J40/5.h.2 Windows) provided by Pololu.

# Example of use
```cpp
#include "RPMSerialInterface.h"

int main( int argc, char** argv )
{
    // Create the interface
    RPM::SerialInterface* serialInterface = RPM::SerialInterface::createSerialInterface( "COM4", 9600 );
    if ( !serialInterface->isOpen() )
        return -1;

    // Set the value of channel 0 to 6000 quarter-of-microseconds (i.e. 1.5 milliseconds)
    serialInterface->setTargetCP( 0, 6000 );

    // Delete the interface
    delete serialInterface;

    return 0;
}
```

# Compiling the code
RapaPololuMaestro uses CMake 3 to create the build system corresponding to the platform. You need to have it installed on your system, please visit [CMake](http://www.cmake.org/) for more information.

After downloading the source code and opening a console in the main source folder, creating the build system can be done like this:

```
mkdir _build_
cd _build_
cmake .. -DCMAKE_INSTALL_PREFIX=../_output_ 
```

You should then have a build system ready to use for your platform (for example, a Visual Studio solution on Windows, a makefile on Linux, etc...).

The build process generates a static library, and two samples:
* a command-line test program.
* a GUI  program to control a Maestro interactively

The GUI uses Qt as a dependency. If it can't be found on your system, the GUI program will simply be not built. 
Either Qt4 or Qt5 can be used. You can specify one or the other using the RAPA_USE_QT5 CMake variable. For example, to compile using QT4:

```
cmake .. -DCMAKE_INSTALL_PREFIX=../_output_ -DRAPA_USE_QT5=OFF
```

If CMake doesn't find Qt automatically, you can point it to the appropriate folder. In this example we also compile for Visual Studio 12 2013 in 64 bits:

```
cmake .. -DCMAKE_INSTALL_PREFIX=../_output_ -DCMAKE_PREFIX_PATH=C:\SomePath\Qt\5.5\msvc2013_64 -G "Visual Studio 12 2013 Win64" 
```

Once the compilation done, you can deploy/install RapaPololuMaestro as a standalone SDK, by building the INSTALL target/project generated by CMake. This will copy the headers, libs and binaries into the directory designated by the standard CMAKE_INSTALL_PREFIX variable.

