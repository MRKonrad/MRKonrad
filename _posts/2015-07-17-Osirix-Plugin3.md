---
layout: post
title: "How to Devolop an Osirix Plugin - part 3: ITK"
date:   2015-07-17
categories: osirix_plugin
tags: Tutorial
---

You want to use other version of ITK?  Here is a solution that worked for me.  It is based on [a Tutorial by Brian Jensen](http://campar.in.tum.de/Students/SepOsiriXSegmentation).

## ITK
1. Get [ITK](http://www.itk.org/) source code. Lets name path of the directory with source ``<ITK_src>`` and one with binaries ``<ITK_bin>``.
2. Run [Cmake](www.cmake.org). 
3. Configure with ``Unix makefiles``.
4. Change ``CMAKE_OSX_ARCHITECTURES`` to ``i386`` (see pic below).
5. Generate.
6. Refactor namespace using ``refactor_namespace.sh`` from NMSegmentation or PetSpectFusion directory. You have it in your ``<osirix_plugins_dir>`` (same where ``_help/Osirix Plugin Generator.app`` is). To do so start terminal, go to ``<osirix_plugins_dir>/NMSegmentation`` directory and type:
   ``sh refactor_namespace.sh <ITK_src> nmITK``  
   ``sh refactor_namespace.sh <ITK_bin> nmITK``  
   or change nmITK to any other namespace name you like.
7. In terminal go to ``<ITK_bin>`` and type``make``.
8. You are going to encounter some errors (see pics below), here is how I dealt with them:

**Error1:** ``fatal errror: 'nmITKsys/SystemTools.hxx' file not found``  
**Solution1:** Using [Sublime text editor](http://www.sublimetext.com/) I found all files with ``nmITKsys`` string and replaced it with ``itksys``.

**Error2:** ``error: unterminated /* `` in ``<ITK_src>/Module/ThirdParty/GDCM``  
**Solution2:** copy whole GDCM folder from ``<ITK_src>`` with no refactor namespace

**Error3:** ``error: use of undeclared identifier 'itkv3'``  
**Solution3:** Replace ``itkv3::`` with ``nmITKv3::`` similarly to Solution1.

**Error4:** some errors in ``itkTestDriverAfterTest.inc `` 
**Solution4:** none yet, but not necessary :)

## Osirix Plugin

1. Generate new plugin, copy the code from the botttom of the post (sligtly different from the one in previous post). Remember to change the file to ``.mm``. 
2. In ``<your_plugin_dir>``, using terminal ``ln -s``, create symbolic links to ITK source code (here ``ITK48_src``) and binaries folders (here ``ITK48_bin``). 
3. Go to **Build Settings**, change **Base SDK** to **Latest OSX**.
4. In **Build Settings**, change **OS X Deployment Targer** to **Compiler Default**.
5. In **Build Settings** fill **Header search paths** with:  
   ``ITK48_bin/Modules/ThirdParty/VNL/src/vxl/vcl``  
   ``ITK48_bin/Modules/ThirdParty/VNL/src/vxl/core``  
   ``ITK48_bin/Modules/Core/Common/**``   
   ``ITK48_src/Modules/ThirdParty/VNL/src/vxl/vcl``     
   ``ITK48_src/Modules/ThirdParty/VNL/src/vxl/core``  
   ``ITK48_src/Modules/Core/Common/**``  
   ``ITK48_src/Modules/Filtering/**`` 
5. In **Build Phases** add ITK libraries in **Link binary with libraries** (see figure below). You can move them to suitable group in XCode navigator. 
6. It complies, you can run it ([how to turn on debugging]({% post_url 2015-07-13-Osirix-Plugin1 %})). But the plugin won't load. I get ``nmITK10DataObject`` error: {% gist MRKonrad/e51bead7f4695ba13b29 %}
6. There is a problem with the linker and dynamic lookup it performes. To solve it:
* in **Build Settings** remove ``-undefined dynamic lookup`` from other linker flags
* in **Build Settings** add Osirix binary path (mine is ``/Applications/OsiriX Lite.app/Contents/MacOS/OsiriX``) in **Bundle loader**to let the linker know where it should look for the Osirix symbols.

## Pics 

<figure>
  <a href="{{ site.url }}/images/Tutorial/cmakeSettings.png"><img src="{{ site.url }}/images/Tutorial/cmakeSettings.png"></a>
</figure>
<figure>
  <a href="{{ site.url }}/images/Tutorial/error_nmITKsys.png"><img src="{{ site.url }}/images/Tutorial/error_nmITKsys.png"></a>
</figure>
<figure>
  <a href="{{ site.url }}/images/Tutorial/error_GDCM.png"><img src="{{ site.url }}/images/Tutorial/error_GDCM.png"></a>
</figure>
<figure>
  <a href="{{ site.url }}/images/Tutorial/error_itkv3.png"><img src="{{ site.url }}/images/Tutorial/error_itkv3.png"></a>
</figure>
<figure>
  <a href="{{ site.url }}/images/Tutorial/error_itkTestDriver.png"><img src="{{ site.url }}/images/Tutorial/error_itkTestDriver.png"></a>
</figure>
<figure>
  <a href="{{ site.url }}/images/Tutorial/PhaseSettings.png"><img src="{{ site.url }}/images/Tutorial/PhaseSettings.png"></a>
</figure>

## Code

{% gist MRKonrad/d9d3668768a3ce5c7a03 %}

