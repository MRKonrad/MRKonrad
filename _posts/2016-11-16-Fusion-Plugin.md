---
layout: post
title: "How to develop a fusion plugin"
date:   2016-11-16
categories: osirix_plugin, horos_plugin, CorePlot
tags: soft, Tutorial
---

Simple fusion plugin generating Bland-Altman plot to compare every pixel on two images.

<div markdown="0">
<a href="https://github.com/MRKonrad/BlandAltmanFusionPlugin/raw/master/BlandAltman.osirixplugin.zip" class="btn btn-success">Download .osirixplugin</a>
<a href="https://github.com/MRKonrad/BlandAltmanFusionPlugin" class="btn btn-info">Code on GitHub</a>
</div>

## What is it?

Bland-Altman plot to compare every pixel on two images. Sample use:
<iframe width="560" height="315" src="https://www.youtube.com/embed/tgbMoc92FPA" frameborder="0" allowfullscreen></iframe>

## Some remarks on plugin development and use of CorePlot framework

1. There is a new plugin template project in ``_help`` folder at the [OsiriX plugins repository](https://github.com/pixmeo/osirixplugins)
2. Change ``plugin type`` to ``fusionFiler`` in the ``Info.plist`` file.
3. In ``Edit scheme`` add executable (Horos or Osirix) in the Info tab. In the Arguments tab add ``--LoadPlugin $(BUILT_PRODUCTS_DIR)/$(PRODUCT_NAME).$(WRAPPER_EXTENSION)``.
4. Follow the directions to add [CoreData Framework](https://github.com/core-plot/core-plot/wiki/Using-Core-Plot-in-an-Application)
For some reason the plugin does not see the CoreData framework file. In the ``Build Settings/Framework search paths`` I added the path to the compiled CoreData framework (``<your CorePlot directory>/core-plot/build/Debug``).
5. File -> new file -> user Interface -> empty -> add new HUD Panel.pl
6. Rename:
* the file names in finder
* the file names and class names in the xcode project
* all occurrences of the 'plugin Template' in the Build Settings
* Target name
* ``Scheme name`` in ``Manage Schemes``. ``Target`` name in ``Build`` tab in the scheme window. ``Expand Variables based on`` in ``Run`` tab in scheme window.
7. Write the code, see the repository.
