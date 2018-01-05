---
layout: post
title: "My Osirixplugin - getDicomClut"
date:   2018-01-05
categories: osirix_plugin, horos_plugin
tags: soft
---

Simple Horos/Osirix plugin to add colormap (CLUT Color Look Up Table) from the currently displayed dicom. The CLUT from the last time you run the plugin is stored and kept in the CLUT menu in Horos/Osirix.
[ShMOLLI article](http://www.jcmr-online.com/content/12/1/69).

<div markdown="0">
<a href="https://bitbucket.org/kwerys/getdicomclut/downloads/getDicomClut.osirixplugin.zip" class="btn btn-success">Download .osirixplugin</a>
<a href="https://bitbucket.org/kwerys/getdicomclut.git" class="btn btn-info">Code on Bitbucket</a>
</div>

## Comments
* Original DICOM maps often have more than Horos/Osirix 256 colur, for example they could have. 4096 color levels. I am taking every 16th value from original map.
