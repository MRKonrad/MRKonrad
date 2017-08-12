---
layout: post
title: "How to develop a Horos/Osirix Plugin - new tutorial"
date:   2017-08-12
categories: osirix_plugin, horos_plugin, ITK, CorePlot
tags: Tutorial
---

You want to write an Horos/Osirix plugin and other basic tutorial are confusing? Try this one!

# Useful resources
[Osirix plugins Github repository](https://github.com/pixmeo/osirixplugins)  
[OsiriX website](http://www.osirix-viewer.com/Documentation/Guides/Development/)  
[OsiriX plugin basics website](https://osirixpluginbasics.wordpress.com)  
[OsiriX plugin by osirixnewby](http://myfirstosirixplugin.blogspot.com/)  
[OsiriX plugin development tutorial by Kyung Hyun Sung](http://kyungs.bol.ucla.edu/Site/Software.html)  
[OsiriX ITK plugin development tutorial by Brian Jensen](http://campar.in.tum.de/Students/SepOsiriXSegmentation)

# Basic configuration
1. Get a copy of Plugin Template from [osirix plugin github repository](https://github.com/pixmeo/osirixplugins).
2. (Optional) Rename the plugin in XCode:
  * Project name on in the left panel.
  * Target name in the left panel of general setting of the projects
  * In Build settings of the Target change product name.
  * Select class name in .h file, Edit->Refactor the name.
  * Replace all occurrences in xxx_Prefix.pch file and folder names
  * Change name is Mange schemes
  * Verify everything is working
3. In XCode click **Edit Scheme**. As **executable** choose Horos/Osirix (you can choose the one in Application directory).
4. Go to **Arguments**, **Arguments Passed on Launch** and add     
  `--LoadPlugin $(BUILT_PRODUCTS_DIR)/$(PRODUCT_NAME).$(WRAPPER_EXTENSION)`
5. Verify everything is working

# ITK configuration
1. (If you do not have [ITK](http://itk.org) installed already).  
  * In terminal navigate to the directory where you want to have ITK code, for example you can use `~` (your home directory) as `<myCodeDir>`.  

    ```
    cd <myCodeDir>  
    git clone git://itk.org/ITK.git  
    cd ITK  
    git checkout --track -b release origin/release
    ```

  * In CMake use source code directory `<myCodeDir>/ITK` and build the binaries directory you can use for example `<myCodeDir>/ITK_bin`. Configure. As generator for the project select `Unix Makefiles`. Uncheck `Build_Tests`. Select `Module_ITKVtkGlue` if you have VTK. Select `Module_Revision` if you want to use [Elastix](http://elastix.isi.uu.nl/). Generate.
    ```
    cd <myCodeDir>/ITK_bin  
    make
    sudo make install
    ```
2. In your project in XCode rename your filter file extension to `.mm`.
3. Create a `symbolList` file, and fill it with `.objc_class_name_<myPluginFilter> `.
Replace `<myPluginFilter>` with your filter class name (see also [OsirixPluginBasics](https://osirixpluginbasics.wordpress.com/2015/09/29/osirix-plugin-with-itk4/)).
4. In build settings:
  * Set `Other Linker Flags` to `-undefined dynamic_lookup -exported_symbols_list symbolList` (see also [OsirixPluginBasics](https://osirixpluginbasics.wordpress.com/2015/09/29/osirix-plugin-with-itk4/)).
  * Add ITK header paths. Check your ITK installation directory. If you did not change it during the ITK installation, it should be `/usr/local`. Go there to confirm (`Command Shift G` in Finder). Set `Header Search Paths` to `/usr/local/include/ITK-4.12` or similar.
  * Add ITK library paths. Set `Library Search Path` to `/usr/local/lib/`. Add ITK library files (`.a` extension) to your project for example by dragging and dropping it from Finder.
5. Add some ITK code to verify if everything works. [For example code from here]({{ site.baseurl }}{% post_url 2015-07-17-Osirix-Plugin3 %}).
