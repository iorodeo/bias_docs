*********************************************
Setting up a build environment on windows 7&8
*********************************************

In this section we describe how to set up a build environment for bias
on window (7 & 8).  this process consists of the following steps:

 #. :ref:`section_win_install_gcc` 
 #. :ref:`section_win_install_perl` (required for qt build)
 #. :ref:`section_win_install_qt` 
 #. :ref:`section_win_install_cmake`
 #. :ref:`section_win_install_opencv`
 #. :ref:`section_win_install_hg`
 #. :ref:`section_win_install_bias`

Which we will describe in more detail below.  Note, it is good idea to use the
the versions of gcc, qt and opencv which are suggested. 

Useful tools
============

to do ... 


.. _section_win_install_gcc:

Download and install gcc 
========================

Bias is currently built with version 4.7.1 of the `tdm-gcc
<http://tdm-gcc.tdragon.net/>`_ compiler suite. Note, this is a slightly older
version of the compiler suite than what is currently available on the tdm-gcc web site.
Older versions of tdm-gcc can be found on source forge `here
<http://sourceforge.net/projects/tdm-gcc/files/tdm-gcc%20installer/previous/1.1006.0>`_.
in particular the recommended versions tdm-gcc for 32 and 64 bit windows are as follows: 

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


.. _section_win_install_perl:

download and install activestate perl
======================================

Perl is used when building the qt library. I recommend using activestate's
version of perl.  For my latest build I used version 5.16.3.1604. Note, I'm not
sure if the verision matters - e.g. version 5.18.2.1802 might work fine. In any
case activestate's perl can be downloaded from here http://www.activestate.com/activeperl/downloads 

Make sure that perl is added to the windows path. For example, if perl was installed
in the c:\\perl64 directory then the following two items should be on your windows path
c:\\perl64\\bin and  c:\\perl64\\site\\bin. 


.. _section_win_install_qt:

download, configure and build qt
================================

Bias currently uses qt version 4.8.5.  This can be downloaded from here
https://download.qt-project.org/official_releases/qt/4.8/4.8.5/ in particular
you will want to download the file `qt-everywhere-opensource-src-4.8.5.zip
<https://download.qt-project.org/official_releases/qt/4.8/4.8.5/qt-everywhere-opensource-src-4.8.5.zip>`_.

Unpack the .zip archive into the directory of your choice. Note, the actual
location isn't actually important - we will the appropriate directories to the
window path later in the install process.

From inside the qt directory run configure.exe with the
options -opensource, -no-webkit, -no-script, -qt-zlib  as follows

.. code-block:: none

  .\configure.exe -opensource -no-webkit -no-script -qt-zlib

 
Note, if you've made a mistake and need to reconfigure run 

.. code-block:: none

  mingw32-make confclean

you can then re-run configure.exe with the desired options.


Once the configuration has finished the next step is to build qt using

.. code-block:: none

  mingw32-make

This will take awhile. After the build is finished you will need to qt's bin
directory to the windows path. for example, if qt is installed in c:\\qt\qt-4.8.5
then you would want to add c:\\qt\\qt-4.8.5\\bin to the windows path.

Finally, test that qt is working.  In the qt directory you will find a
directory called "examples". Try running several of the examples in this
directory. Most should work - all except those depending on webkit, script,
etc. which we excluded during the build. The executables will be found in the
"debug" sub-folder of the example. An example, which should work is
"examples\\dialog\\tabdialog".  Note, before running the examples you may want to
open a new command window to ensure that the additions to the windows PATH
(above) have been applied. 


.. _section_win_install_cmake:

Download and install CMake
==========================

To do ..


.. _section_win_install_opencv:

Download Configure, and build OpenCV
====================================

To do ..

.. _section_win_install_hg:

Install mercurial
=================

To do ..

.. _section_win_install_bias:

Download and build the latest version of BIAS
=============================================


To do ...




