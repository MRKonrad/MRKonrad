---
layout: post
title: "How to develop a Horos/Osirix Plugin - new tutorial"
date:   2017-08-12
categories: osirix_plugin, horos_plugin, ITK, CorePlot
tags: Tutorial
---

Do you want to write an Horos/Osirix plugin? Basic tutorials you have found so far in the internet are confusing? Try this one!

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
  * Target name in the left panel of general setting of the projects.
  * In Build settings of the Target change product name.
  * Select class name in .h file, Edit->Refactor the name.
  * Replace all occurrences in `xxx_Prefix.pch` file and folder names.
  * Change name is Mange schemes.
  * Verify everything is working.
3. In XCode click **Edit Scheme**. As **executable** choose Horos/Osirix (you can choose the one in Application directory).
4. Go to **Arguments**, **Arguments Passed on Launch** and add     
  `--LoadPlugin $(BUILT_PRODUCTS_DIR)/$(PRODUCT_NAME).$(WRAPPER_EXTENSION)`
5. Verify that everything is working.

# ITK configuration
1. (If you do not have [ITK](http://itk.org) installed already).  
  * In terminal navigate to the directory where you want to have ITK code, for example you can use `~` (your home directory) as `<myCodeDir>`.  
    ```bash
    cd <myCodeDir>  
    git clone git://itk.org/ITK.git  
    cd ITK  
    git checkout --track -b release origin/release
    ```
  * In CMake use source code directory `<myCodeDir>/ITK` and build the binaries directory you can use for example `<myCodeDir>/ITK_bin`. Configure. As generator for the project select `Unix Makefiles`. Uncheck `Build_Tests`. Select `Module_ITKVtkGlue` if you have VTK. Select `Module_Revision` if you want to use [Elastix](http://elastix.isi.uu.nl/). Generate.
    ```sh
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

# CorePlot

1. (If you do not have [CorePlot](https://github.com/core-plot/core-plot)).
  * In terminal navigate to the directory where you want to have CorePlot code, for example you can use `~` (your home directory) as `<myCodeDir>`.  
    ```sh
    cd <myCodeDir>  
    git clone git@github.com:core-plot/core-plot.git  
    cd ITK  
    git checkout --track -b release origin/release-2.3  
    ```
2. Annoyingly, following [CorePlot instructions](https://github.com/core-plot/core-plot/wiki/Using-Core-Plot-in-an-Application) does not do the job (XCode cannot localize `CorePlot.h`) as far as I tried it.To make CorePlot work you can try the following steps:
  * Open CorePlot project and build it. You can build Release version by Edit Scheme -> Build Configuration -> Deployment.
  * CorePlot is build to <myCodeDir>/core-plot/build/ folder. Drag and drop CorePlot.framework to your project opened in XCode. Select 'Copy files if needed'.
  * Add `#import <CorePlot/CorePlot.h>` in your code to verify everything works.
  * It is time to display something with CorePlot. Copy `Controller.h` and `Controller.m` from MinorTickLabels example in CorePlot.
  * If you are getting error messages about the code, update project setting to recommended by XCode and in Build settings set `macos deployment target` to a current one (for example `10.12`).
  * Create a new window xib, call it `Window.xib`. Add a `Custom View`. Set the `Custom View` class to `CPTGraphHostingView`.
  * Add following lines to your plugin:
  ```objective-c
  NSWindowController *mywindow = [[NSWindowController alloc] initWithWindowNibName:@"Window" owner:self];
  [mywindow showWindow:self];
  ```
  * Still in xib editor, add a new object and set its class to `Controller`.
  * In connections inspector connect `hostView` outlet to the `Custom View`.
  * Everything should compile but you can run into a problem with loading your plugin `Library not loaded`, `Reason: image not found`. In build settings set
  `Runpath search paths` to `LD_RUNPATH_SEARCH_PATHS = @loader_path/../Frameworks`. In build phases add `New copy files phase` using `+` at the top of the screen. Change `Destination` to `Frameworks` and drag and drop `CorePlot.framework` from the left panel.

# Troubleshooting

## Typeinfo file not found
Error: ` /usr/local/include/ITK-4.12/itkMacro.h:45:10: 'typeinfo' file not found `
Solution: change file extension from `.m` to `.mm`
