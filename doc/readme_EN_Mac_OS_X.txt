﻿///////////////////////////////////////////////////////////////////////////
// Author: Martial TOLA
// Year: 2010-2011-2012
// CNRS-Lyon, LIRIS UMR 5205
///////////////////////////////////////////////////////////////////////////

How-to for Mepp under Mac OS X ('10.5 Leopard'*, '10.6 Snow Leopard', '10.7 Lion' or '10.8 Mountain Lion') :
------------------------------------------------------------------------------------------------------------

1a) starting from Mac OS X '10.8 Mountain Lion', X11 is no longer part of the system, you must install in this case XQuartz:
http://xquartz.macosforge.org/

1b) install Xcode:
http://developer.apple.com/xcode/

---> with recent version of Xcode (for example > 4.3.x) you need then to install the 'Command Line Tools' for Xcode.
You can install them from inside Xcode's Download preferences. Finally, you also need to do that command:
sudo xcode-select -switch /Applications/Xcode.app/Contents/Developer

2) install Java Developer Package for Mac OS X
https://connect.apple.com/

///////////////////////////////////////////////////////////////////////////

3) use Homebrew (3a) or MacPorts (3b)
I highly recommend Homebrew, stronger and faster during the installation because there is sometimes "Bottle" (binary package)

3a) install Homebrew (http://mxcl.github.com/homebrew/):
ruby -e "$(curl -fsSkL raw.github.com/mxcl/homebrew/go)"

---> update Homebrew: 
brew update

---> install this packages with Homebrew:
brew install cgal
brew install qt
brew install libqglviewer
brew install glew
brew install doxygen graphviz xerces-c
brew install ffmpeg

---> it may be necessary to create this link for libQGLViewer (here 2.3.15 is an example...)
sudo ln -s "/usr/local/Cellar/libqglviewer/2.3.15/QGLViewer.framework" "/Library/Frameworks/QGLViewer.framework"

or

3b) install MacPorts:
http://www.macports.org/install.php

---> update MacPorts: 
sudo port -v selfupdate

---> install this packages with MacPorts:
sudo port install cgal
sudo port install qt4-mac
sudo port install libQGLViewer
sudo port install glew
sudo port install doxygen graphviz xercesc3
sudo port install ffmpeg

---> then:
sudo port install python_select
sudo python_select pythonXX (ex.: python27)

///////////////////////////////////////////////////////////////////////////

4a) compile Mepp with CMake and Makefile:
go into MEPP.git/MEPP folder, then,
 - mkdir build; cd build
 - for a Release version: "cmake .." then make
 - for a Debug version: "cmake .. -DCMAKE_BUILD_TYPE=Debug" (2 times for taking into account of debug mode) then make
note: you can also use the "graphics" version of CMake: cmake-gui

 - Mepp documentation can be generated with: make mepp_DOC
 - your component documentation in Doxygen format can be generated with: make component_componentname_DOC (example: make component_CGAL_Example_DOC)

or

4b) compile Mepp with CMake and Qt Creator:
CMake (or CMake-gui) is able to generate (right now) Unix Makefiles or Code::Blocks and KDevelop projects, but unfortunately not Qt Creator projects (.pro).
But fortunately Qt Creator can import directly CMakeLists.txt to generate Qt Creator projects (.pro).

So, in order to generate Qt Creator projects (.pro), here's the how-to under Qt Creator:

Open a file and choose at the bottom 'CMake project file'
then choose CMakeLists.txt in MEPP.git/MEPP folder (warning, not MEPP.git/MEPP/src folder !) then click on 'next'
then in 'Arguments' field put -DCMAKE_BUILD_TYPE=Debug for generate Qt Creator projects (.pro) in Debug mode
or nothing for generate Qt Creator projects (.pro) in Release mode.
If need set the CMake path: /opt/local/bin/cmake
Click then on 'Run CMake', then, after the execution of CMake, click on 'Terminate' and the project (.pro) loads into the IDE.

-------------------------------------------------------
Note on components activation/deactivation with CMake :
-------------------------------------------------------
- components activation: add -DBUILD_component_componentname=ON to CMake (example: -DBUILD_component_Curvature=ON)
- components deactivation: add -DBUILD_component_componentname=OFF to CMake (example: -DBUILD_component_Curvature=OFF)

--------------------------------------------------
Note on the generation of a Mac bundle (mepp.app):
--------------------------------------------------
"sudo make install" let you deploy a binary bundle to another Mac, the generated bundle is in mepp_deploy folder



* Under 'Mac OS X 10.5 Leopard', gcc 4 contains a bug when using optimized compilation for an application using CGAL library,
so use gcc 4.2 instead:
cmake .. -DCMAKE_CXX_COMPILER=/usr/bin/g++-4.2 -DCMAKE_C_COMPILER=/usr/bin/gcc-4.2

Also, if your processor does not support 64-bit, you must also pass -DCMAKE_C_FLAGS='-arch i386'
