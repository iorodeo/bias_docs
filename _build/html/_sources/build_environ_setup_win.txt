*********************************************
Setting up a build environment on windows 7&8
*********************************************

In this section we describe how to set up a build environment for bias
on window (7 & 8).  this process consists of the following steps:

 #. :ref:`section_win_install_gcc` 
 #. :ref:`section_win_install_msys` (required for openssl build)
 #. :ref:`section_win_install_perl` (required for qt build)
 #. :ref:`section_win_install_openssl` (required for qt build)
 #. :ref:`section_win_install_qt` 
 #. :ref:`section_win_install_cmake`
 #. :ref:`section_win_install_eigen`
 #. :ref:`section_win_install_opencv`
 #. :ref:`section_win_install_flycapture2`
 #. :ref:`section_win_install_hg`
 #. :ref:`section_win_install_bias`

We will describe the steps in more detail below.  Note, for a trouble free
build, it is good idea to use the the versions of gcc, qt and opencv which are
suggested. 

The instructions assume that git is installed. https://git-scm.com/ 


.. _section_win_install_gcc:

Download and install gcc 
========================

Bias is currently built with the `tdm-gcc <http://tdm-gcc.tdragon.net/>`_
compiler suite.  Version 4.7.1 is know to work. Note, this is a slightly older version
of the compiler suite than what is currently available on the tdm-gcc web site.
Older versions of tdm-gcc can be found on source forge `here
<http://sourceforge.net/projects/tdm-gcc/files/tdm-gcc%20installer/previous/1.1006.0>`_.
in particular the recommended versions tdm-gcc for 32 and 64 bit windows are as
follows: 

* 32bit `tdm-gcc-4.7.1.exe <http://sourceforge.net/projects/tdm-gcc/files/tdm-gcc%20installer/previous/1.1006.0/tdm-gcc-4.7.1.exe/download>`_
* 64bit `tdm64-gcc-4.7.1-3.exe <http://sourceforge.net/projects/tdm-gcc/files/tdm-gcc%20installer/previous/1.1006.0/tdm64-gcc-4.7.1-3.exe/download>`_

The .exe downloaded will be an executable windows installer. Run the installer
to install the compiler suite. There will be some options during the
installation make sure the following items are checked

* gcc w/ the c++ sub-menu item
* binutils
* mingw64-runtime (or mingw-rutime if 32bit)
* mingw32-make 
* gdb (if you want a debugger)
* start menu items
* add to path

After running the installer double check the compiler suite has been added to
the windows path. For example, if the compiler suites is installed in the 
c:\\mingw62 directory therefore in order to use the compiler suite you should have
c:\\mingw64\\bin on my windows path. You can test that everything works by
opening command line and trying to run "g++ -v" from the command line.

Note, newer versions of gcc may work as well - I have successfully built BIAS
using version 4.8.2 on Linux. However the build has only been tested on windows
with version 4.7.1.


.. _section_win_install_msys:

Download and install MSYS
=========================

MSYS will be used to configure and build openssl. Download MSYS from here http://www.mingw.org/wiki/msys 
Run the .exe to install.

.. _section_win_install_perl:

Download and install ActiveState Perl
======================================

Perl is used when building the qt library. I recommend using activestate's
version of perl.  For my latest build I used version 5.16.3.1604. Note, I'm not
sure if the verision matters - e.g. version 5.18.2.1802 might work fine. In any
case activestate's perl can be downloaded from here http://www.activestate.com/activeperl/downloads 

Make sure that perl is added to the windows path. For example, if perl was installed
in the c:\\perl64 directory then the following two items should be on your windows path
c:\\perl64\\bin and  c:\\perl64\\site\\bin. 


.. _section_win_install_openssl:

Download and install Openssl
============================

Openssl can be downloaded from here here https://www.openssl.org/source/ Note, some minor edits are required for building on a 64.

First edit config and  add options="$options no-asm" into it (~15th line from bottom).

Using MSYS run ./config as follows:

.. code-block:: none

    ./config --prefix=<openssl install directory> --openssldir=<openssl source directory> no-asm

where <openssl install directory> and  <openssl source directory> are something like

.. code-block:: none

    <openssl install directory>  =  /c/Users/Will/work/openssl/openssl_install 
    <openssl source directory>   =  /c/Users/Will/work/openssl/openssl_files 

    
Next edit the generated  Makefile and change -march=i486 to -march=x86-64.
With openssl-1.0.1p I also needed to edit the files md5test.c, rc5test.c and
jpaketest.c in the test directory and change the line

.. code-block:: c

    dummytest.c

to 

.. code-block:: c

    #include "dummytest.c"


Finally build and install - using MSYS.  

.. code-block:: none

    ./mingw32-make.exe
    ./mingw32-make.exe install




.. _section_win_install_qt:

Download, configure and build Qt
================================

Bias currently uses qt version 5.3.2.  The repository can be download using git. 

.. code-block:: none

    git clone git://code.qt.io/qt/qt5.git

Using git bash checkout the 5.3.2 branch. 

.. code-block:: none

    git checkout 5.3.2

Temporarily add git to the windows PATH. Make sure to add it after Perl. Then
run the "init-repository" script to fetch the sub-repositories. This should be
run from either a window cmd shell or Powershell as running it from git bash
will fail.


.. code-block:: none

    perl init-repository -no-webkit

When finished remove git from windows PATH.  

Open a shell (cmd or Poweershell) for building Qt. Set the  QT_SRC_DIR
environment variable to the location of the Qt source files and QTMAKESPEC to
the appropriate spec file location.  For example setting the environment
variables can be done in Powershell as follows.

.. code-block:: none

    $Env:QT_SRC_DIR = "<Path to qt source dir>"
    $Env:QMAKESPEC = "$Env:QT_SRC_DIR\qtbase\mkspecs\win32-g++"


Next, from within the qt5 source directory Configure Qt as follows:

.. code-block:: none

    ./configure  -developer-build  -opensource  -platform win32-g++ -c++11  -opengl desktop -openssl -I <path to openssl install>\include -L <apth to openssl install>\lib

Run mingw32-make to build and install (Do we need to do this for developer build??).

.. code-block:: none

    mingw32-make.exe -j4
    mingw32-make.exe intall


Next add the Qt bin directory to the windows PATH. For a developer build the
bin directory will in the qtbase sub-directory.  Finally, Create the QTDIR
environment variable and set it to the qtbase sub-directory.
    

Note, if you've made a mistake during configuration or building Qt and need to reconfigure run 

.. code-block:: none

  mingw32-make confclean

you can then re-run configure.exe with the desired options.


.. _section_win_install_cmake:

Download and install CMake
==========================

CMake is required for building both OpenCV and BIAS.  The  latest version of CMake can be found here 

http://www.cmake.org/cmake/resources/resources.html.

  * Download the Window (Win32 Installer) cmake-2.8.xx.x-win32-x86.exe and install
    the program as usual. 
  
  * After installing make sure that CMake is added to the windows PATH. You should
    see something like  "C:\Program Files(x86)\Cmake 2.8\bin" on your path. Also
    you should be able to run cmake from a command window. 
    


.. _section_win_install_eigen:

Downlad an unpack Eigen
=======================

Eigen is a C++ template library for linear algebra which is used by OpenCV.
Eigen isn't required, but it can speed up some computations.  You can download the latest 
version of eigen from here http://eigen.tuxfamily.org/index.php?title=Main_Page

Note, eigen is a  pure template library defined in the headers so we don't need
to build anything.  Just unpack the latest version of the library  to a
convenient directory.


.. _section_win_install_opencv:

Download Configure, and build OpenCV
====================================


Download
--------

Clone the latest version of OpenCV from GitHub here
https://github.com/Itseez/opencv.git.  

Note, as I haven't updated the library in a while (about a year) it is possible
that there have been sufficient changes to OpenCV such that the latest version
is no longer compatible BIAS. In which case in might be necessary to roll back
commit which is last know work -  which is 0e7ca71dcc1b53430893362faf302c05c8695524.
The following command should enable you to rollback the repository from this commit
in  tempory branch named 'old-build-temp'.

.. code-block:: bash

  git checkout -b old-build-temp 0e7ca71dcc1b53430893362faf302c05c8695524. 


Configure and Build
-------------------
 
Start by creating a build directory. Note, this can be anywhere and named
anything. However, I typically create a directory named 'build' in the parent
directory of the opencv's source directory - i.e., one directory up from the root
of the source tree.   
    
Configure OpenCV by running 

.. code-block:: bash

  cmake-gui.exe

In the configuration tool perform the following steps

  * Select the source and build directories 
  * Run configure
  * Set the build type to MinGW
  * Make sure that the following options are selected: WITH_QT, WITH_EIGEN
  * Set EIGEN_INCLUDE_PATH to point to the location of the Eigen library 
  * Re-run configure 
  * Run generate

Next, exit the configuration tool and  build open by running the following command

.. code-block:: bash

  mingw32-make


After, building the library and the 'build/bin' sub-directory to the windows path, e.g., 

.. code-block:: none

  C:\<PATH-TO-BUILD-DIRECTORY>\build\bin 



.. _section_win_install_flycapture2:

Install Point Grey's FlyCapture2 libraray, a camera, etc.
=========================================================

Links and instructions for downloading and installing the FlyCapture2 library from Point Grey can be found here

http://ww2.ptgrey.com/sdk/flycap

After installing the library use the FlyCap2 program to verify that the cameras are working.

Finally, add the FlyCapture2 library to the Windows PATH. Note, this may or may not be done automatically. Also, the path
will very depending on whether the host system is 32bit or 64bit.

On 64bit Windows

.. code-block:: none

  C:\Program Files\Point Grey Research\FlyCapture2\bin64

On 32bit Windows


.. code-block:: none

  C:\Program Files\Point Grey Research\FlyCapture2\bin


.. _section_win_install_hg:

Install mercurial
=================


BIAS uses the `mercurial <http://mercurial.selenic.com/>`_ revision control system.
Which can be downloaded from here

http://mercurial.selenic.com/downloads

Download and install mercurial.  Make sure that it is on the windows PATH  e.g., 

.. code-block:: none

  C:\Program Files\Mercurial\


and can be run from the command line using the 'hg' command.


.. _section_win_install_bias:

Download and build the latest version of BIAS
=============================================


Download the latest version of BIAS w/ mercurial using the following command

.. code-block:: none

  hg clone https://bitbucket.org/iorodeo/bias


Create a build directory.  Again this can be named anything and located
anywhere. However, I typically create a directory named 'build' in the root
directory of BIAS's source tree. This directory is the projects ".hgignore" file
so its contents won't be tracked by mercurial.

In the build directory run the following
command

.. code-block:: none

  cmake -G "MinGW Makefiles"  <path to root of bias source>

For example if the build directory is in the root directory of the source tree this command would be

.. code-block:: none

  cmake -G "MinGW Makefiles" ../


Next Build BIAS using the following command from inside the build directory

.. code-block:: none

  mingw32-make


 
