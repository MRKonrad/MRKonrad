---
layout: post
title: "How to Devolop an Osirix Plugin - part 2: ITK"
date:   2015-07-14
categories: osirix_plugin
tags: tutorial
---

This time we want to use basic ITK filtering in our plugin.

## Part 1
1. For testing purposes use the code from the bottom of the page. The file has to have ``.mm`` extention (Typical error: ``Lexical or Preprocessor Issue 'iostream' file not found``). 
2. Get ITK. This time we are going to use ITK version used in Osirix and provided in Osirix code.
  * Get Osirix code from [Osirix github repository](https://github.com/pixmeo/osirix) to ``<your_osirix_path>``
  * In Osirix xcode project select **Unzip Binaries** as a target to build. Run it. In ``<your_osirix_path>``. ``ITK180`` dir should appear.
  * In terminal go to ``<your_plugin_dir>``. create a symbolic link to ``ITK180`` dir:  
  `ln -s <your_osirix_path>/ITK180`
3. Confiugere your project
  * in **Build Settings** fill **Header Search Paths** with the paths below. (otherwise you will get ``Lexical or Preprocessor Issue 'itkImage.h' file not found`` is the error you might get).    
  ``ITK180/Modules/Core/Common``    
   ``ITK180/Modules/Core/Common/include``  
   ``ITK180/Modules/ThirdParty/VNL/src/vxl/vcl``   
   ``ITK180/Modules/ThirdParty/VNL/src/vxl/core``   
   ``ITK180/Modules/Filtering/Thresholding/include``  
  * in **Build Settings** change **C++ Language Dialect** to **C++14** (otherwise you will get ``error: unknown type name 'constexpr'``).
4. You should be able to compile it. It should run correctly.

## Part 2
As you work with this configuration, you might get to a problem, for example if you add ``'ITK180/Modules/Numerics/Optimizers/include'`` to **Header search path** and you use code like one below:
{% highlight objective-c %}
#include "itkLBFGSOptimizer.h"
typedef itk::LBFGSOptimizer OptimizerType;
...
OptimizerType::Pointer optimizer = OptimizerType::New();
{% endhighlight %}
Plugin will compile but we get ``Symbol not found: itk14LBFGSOptimizer`` error. To solve it:

1. In **Build Phases** add ITK libraries from <ITK180> folder (see picture). You can move them to a suitable group in XCode navigator. 
2. In **Build Settings** remove ``-undefined dynamic lookup`` from other linker flags
3. In **Build Settings** add Osirix binary path (mine is ``/Applications/OsiriX Lite.app/Contents/MacOS/OsiriX``) to let the linker know where it should look for the Osirix symbols.v

To better understand the problem, see the [linker manual](http://www.manpages.info/macosx/ld.1.html) and [Brian's explanation](http://campar.in.tum.de/Students/SepOsiriXSegmentation) in ProjectConfiguration.pdf.

<figure>
  <a href="{{ site.url }}/images/tutorial/PhaseSettings.png"><img src="{{ site.url }}/images/tutorial/PhaseSettings.png"></a>
</figure>

## Code 

{% gist MRKonrad/8732d8ba2b14c8a07851 %}

