---
layout: post
title: "Osirixplugin - Shmolli Color Look Up Table"
date:   2014-06-20
categories: osirix_plugin
---

Simple Osirix plugin to add Color Look Up Table similar to T1 map ShMOLLI color table used on Siemens scanners
[ShMOLLI article](http://www.jcmr-online.com/content/12/1/69).

<div markdown="0">
<a href="https://bitbucket.org/kwerys/shmolli_clut/downloads/ShMolli_CLUT.osirixplugin.zip" class="btn btn-success">Download .osirixplugin</a>
<a href="https://bitbucket.org/kwerys/shmolli_clut.git" class="btn btn-info">Code on Bitbucket</a>
</div>


##How to use it?

<ul>
  <li>Install</li>
  <li>Run</li>
  <li>Select CLUT</li>
  <li>Enjoy</li>
</ul>

<figure class="third">
  <a href="{{ site.url }}/images/ShMolli_CLUT/install.png"><img src="{{ site.url }}/images/ShMolli_CLUT/install.png" alt="Install"></a>
  <a href="{{ site.url }}/images/ShMolli_CLUT/run.png"><img src="{{ site.url }}/images/ShMolli_CLUT/run.png" alt="Run"></a>
  <a href="{{ site.url }}/images/ShMolli_CLUT/selectCLUT.png"><img src="{{ site.url }}/images/ShMolli_CLUT/selectCLUT.png" alt="Select CLUT"/></a>
  <figcaption>Install, run, select CLUT.</figcaption>
</figure>

<figure >
  <a href="{{ site.url }}/images/ShMolli_CLUT/enjoy.png"><img src="{{ site.url }}/images/ShMolli_CLUT/enjoy.png" alt="Enjoy"></a>
  <figcaption>Enjoy.</figcaption>
</figure>


##Comments
* I could not find how to add custom CLUT not using OsiriX CLUT editor. So I hardcoded CLUT in a plugin. Feel free to sugest a better solution =]
* Original map has 4096 color levels, Osirix uses only 256, so I took every 16th value from original map.
