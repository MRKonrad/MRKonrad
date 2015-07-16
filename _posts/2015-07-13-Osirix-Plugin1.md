---
layout: post
title: "How to Devolop an Osirix Plugin - part 1: basics"
date:   2015-07-13
categories: osirix_plugin
tags: tutorial
---

It's not easy to find documentation or examples on how to develop one's own Osirix plugin. I gathered here some materials and my experience.

# Useful websites

[OsiriX website](http://www.osirix-viewer.com/Documentation/Guides/Development/)  
[OsiriX plugin basics website](https://osirixpluginbasics.wordpress.com)  
[OsiriX plugin by osirixnewby](http://myfirstosirixplugin.blogspot.com/)  
[OsiriX plugin development tutorial by Kyung Hyun Sung](http://kyungs.bol.ucla.edu/Site/Software.html)  
[OsiriX ITK plugin development tutorial by Brian Jensen](http://campar.in.tum.de/Students/SepOsiriXSegmentation)

# My how-to

1. Get available plugins code from [github repository](https://github.com/pixmeo/osirixplugins) to your ``<osirx_plugins_dir>``
2. Use **Osirix Plugin Generator.app** from ``<osirx_plugins_dir>/_help``. Move it to ``<your_plugin_dir>``
3. Configure build settings -Change ``Base SDK`` to ``Latest OSX``
4. Build and ... Voila! - you should be able to install your plugin, restart osirix and run your plugin.

It works, but it would be nice to have debugger before we start messing with the code. 

1. Click **Edit Scheme**. As executable choose Osirix (one installed in Application is OK). 
2. Go to **Arguments**, **Arguments Passed on Launch** and add     
  `--LoadPlugin $(BUILT_PRODUCTS_DIR)/$(PRODUCT_NAME).$(WRAPPER_EXTENSION)`
3. Change architecture to one corresponding to your Osirix version.
4. Build and ... Voila! Osirix should open, your plugin should be in plugins menu and you should be able to play with breakpoints in the plugin code.

<figure>
  <a href="{{ site.url }}/images/Tutorial/BuildSettings.png"><img src="{{ site.url }}/images/Tutorial/BuildSettings.png" alt="Build Settings"></a>
  <a href="{{ site.url }}/images/Tutorial/EditScheme.png"><img src="{{ site.url }}/images/Tutorial/EditScheme.png" alt="Edit Scheme"></a>
  <figcaption></figcaption>
</figure>