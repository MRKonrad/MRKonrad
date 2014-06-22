---
layout: post
title: Osirixplugin - Shmolli Color Look Up Table
description: "How to get ShMolli colors in Osirix"
modified: 2014-06-22
tags: [osirix plugin]
image:
  feature: abstract-3.jpg
  credit: dargadgetz
  creditlink: http://www.dargadgetz.com/ios-7-abstract-wallpaper-pack-for-iphone-5-and-ipod-touch-retina/
comments: true
share: true
---

Simple Osirix plugin to add Color Look Up Table similar to T1 map ShMolli color table used on Siemens scanners - [ShMolli article](http://www.jcmr-online.com/content/12/1/69).

<div markdown="0">
<a href="https://bitbucket.org/kwerys/shmolli_clut/downloads/ShMolli_CLUT.osirixplugin.zip" class="btn btn-success">Download .osirixplugin</a>
<a href="https://bitbucket.org/kwerys/shmolli_clut.git" class="btn btn-info">Code on Bitbucket</a>
</div>


##How to use it?

<ul>
  <li>Install
  <br/><a href="{{ site.url }}/images/ShMolli_CLUT/install.png"><img src="{{ site.url }}/images/ShMolli_CLUT/install.png" alt="Install" style="width: 300px"/></a><br/>
  </li>

  <li>Run
  <br/><a href="{{ site.url }}/images/ShMolli_CLUT/run.png"><img src="{{ site.url }}/images/ShMolli_CLUT/run.png" alt="Run" style="width: 300px"/></a><br/>
  </li>

  <li>Select CLUT
  <br/><a href="{{ site.url }}/images/ShMolli_CLUT/selectCLUT.png"><img src="{{ site.url }}/images/ShMolli_CLUT/selectCLUT.png" alt="Select CLUT" style="width: 300px"/></a><br/>
  </li>

  <li>Enjoy
  <br/><a href="{{ site.url }}/images/ShMolli_CLUT/enjoy.png"><img src="{{ site.url }}/images/ShMolli_CLUT/enjoy.png" alt="Enjoy" style="width: 300px"/></a><br/>
  </li>
</ul>

##Comments
* I could not find how to add custom CLUT not using OsiriX CLUT editor. So I hardcoded CLUT in a plugin. Feel free to sugest a better solution =]
* Original map has 4096 color levels, Osirix uses only 256, so I took every 16th value from original map.
