---
layout: post
title: "Siemens DICOM trick"
date:   2016-03-30
categories: 
tags: 
---

Sometimes information in DICOM header is not enough. What is the echo spacing that was used in this image? What was the GRAPPA acceleration factor? All of the fields from Siemens scanner GUI can be found in the DICOM file itself. You just have to open the file in a text editor, like WordPad or Atom and find the fields interesting for you. 

For example, GRAPPA acceleration factor is equal to ``sPat.lAccelFactPE``. If the reviewers do not like 'apparent TR' reported in DICOM header, you can calculate 'real TR' using following equation:
``TR = alTR[0]/lSegments``

<figure>
  <a href="{{ site.url }}/images/SiemensDicom.png"><img src="{{ site.url }}/images/SiemensDicom.png" alt="Build Settings"></a>
  <figcaption></figcaption>
</figure>