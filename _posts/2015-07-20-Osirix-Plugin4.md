---
layout: post
title: "How to develop an Osirix Plugin - part 4: Plot"
date:   2015-07-20
categories: osirix_plugin, CorePlot
tags: Tutorial
---

[Core Plot](https://github.com/core-plot/core-plot) is a popular open source plotting framework in Objective C. It was used by [osirixnewby](http://myfirstosirixplugin.blogspot.com/).

If one wants to use its new version in osirixplugin, there is a problem - this framework is written for 64bit architecture and uses (ARC) Automatic Reference Counting. It cannot be used in code written for 32 bit architecture.

Older version of [Core Plot - 1.5.1](https://github.com/core-plot/core-plot/releases/tag/release_1.5.1) is available. I compiled the source code for 32bit architecture overcoming some issues:

* problem with ``Synthesized property 'areaBorderLineStyle' must be name the same as a compatible instance variable ...`` - I added ``CPTLineStyle *areaBorderLineStyle;`` line to ``@interface CPTRangePlot : CPTPlot `` in file ``CPTRangePlotDelegate.h``.
* problem with garbage collector - remove ``GCC_ENABLE_OBJC_GC`` flag from build settings
* problem with testing ``OCUnit deprecated``- I tried to switch to XCTest, but in the end I removed all file groups named 'Tests'

To run Core Plot in an Osirix plugin i looked at the DatePlot example.

* I added CorePlot framework:
   * in **Build Phases** using + I added it to **Copy files**
   * I dragged CorePlot to **Link with libraries**
* I copied ``Controller.h`` and ``Controller.m`` to plugin project from DatePlot example.
* I created new window xib. There I added custom view. I set custom view class to ``CPTGraphHostingView``.
* Still in xib editor, I added new object and set its class to ``Controller``.
* In connections inspector I connected hostView outlet to the view.
* Voila!
