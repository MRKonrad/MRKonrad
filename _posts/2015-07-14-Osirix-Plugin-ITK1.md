---
layout: post
title: "How to Devolop an Osirix Plugin - part 2: ITK"
date:   2015-07-14
categories: osirix_plugin
tags: tutorial
---

This time we want to use basic ITK filtering in our plugin.

1. For testing purposes use the code from the bottom of the page. File has to have ``.mm`` extention (Typical error: ``Lexical or Preprocessor Issue 'iostream' file not found``).
2. Get ITK. We are going to use ITK version used in Osirix and provided in Osirix code.
  * Get Osirix code from [Osirix github repository](https://github.com/pixmeo/osirix) to ``<your_osirix_path>``
  * In Osirix xcode project select **Unzip Binaries** as a target to build. Run it. In ``<your_osirix_path>``. ``ITK180`` dir should appear.
  * In terminal go to ``<your_plugin_dir>``. create a symbolic link to ``ITK180`` dir:  
  `ln -s <your_osirix_path>/ITK180`
3. Confiugere your project
  * in **Build Settings** fill **Header Search Paths** with the paths below. (Typical errors: ``Lexical or Preprocessor Issue 'itkImage.h' file not found``).
  ``ITK180/Modules/Core/Common``    
   ``ITK180/Modules/Core/Common/include``  
   ``ITK180/Modules/ThirdParty/VNL/src/vxl/vcl``   
   ``ITK180/Modules/ThirdParty/VNL/src/vxl/core``   
   ``ITK180/Modules/Filtering/Thresholding/include``  
  * in **Build Settings** change **C++ Language Dialect** to **C++14** (otherwise you will get ``error: unknown type name 'constexpr'``).


## Code 
``myitkplugin5Filter.mm``

{% highlight objective-c %}
#import "myitkplugin5Filter.h"

#include "itkImage.h"
#include "itkImportImageFilter.h"
#include "itkBinaryThresholdImageFilter.h"

typedef     float itkPixelType;
typedef     itk::Image< itkPixelType, 2 > ImageType;
typedef     itk::ImportImageFilter< itkPixelType, 2 > ImportFilterType;
typedef     itk::BinaryThresholdImageFilter<ImageType, ImageType>  BinaryThresholdImageFilterType;

@implementation myitkplugin5Filter

- (void) initPlugin
{
}

- (long) filterImage:(NSString*) menuName
{
    // we want to desplay results in new viewer, so we duplicate current
    ViewerController *new2DViewer = [self duplicateCurrent2DViewerWindow];
    // get first Pix object in series, it will be used in import filter configuration
    DCMPix      *firstPix = [[viewerController pixList] objectAtIndex:0];
    // we will loop over slice and time dimention (in 4d viewer)
    int         times= [[viewerController pixList] count];
    int         slices = [viewerController maxMovieIndex];
    int         lowerThreshold = 60, upperThreshold = 1000;
    
    // ITK initialization
    BinaryThresholdImageFilterType::Pointer thresholdFilter = BinaryThresholdImageFilterType::New();
    ImportFilterType::Pointer importFilter = ImportFilterType::New();
    
    // settings needed in ITK import filter
    ImportFilterType::IndexType start;
    start[0] = 0;
    start[0] = 0;
    
    ImportFilterType::SizeType size;
    size[0] = [firstPix pwidth];
    size[1] = [firstPix pheight];
    long bufferSize = size[0] * size[1];
    
    ImportFilterType::RegionType region;
    double  origin[2];
    double  voxelSpacing[2];
    
    origin[0] = [firstPix originX];
    origin[1] = [firstPix originY];
    
    voxelSpacing[0] = [firstPix pixelSpacingX];
    voxelSpacing[1] = [firstPix pixelSpacingY];
    
    region.SetIndex(start);
    region.SetSize(size);
    
    // apply settings to ITK import filter
    importFilter->SetRegion(region);
    importFilter->SetOrigin(origin);
    importFilter->SetSpacing(voxelSpacing);
    
    // apply settings to ITK threshold filter
    thresholdFilter->SetLowerThreshold(lowerThreshold);
    thresholdFilter->SetUpperThreshold(upperThreshold);
    thresholdFilter->SetInsideValue(1000);
    thresholdFilter->SetOutsideValue(0);
    
    // loop over all images
    for (int s=0; s<slices; s++)
    {
        NSMutableArray *PixList = [viewerController pixList:s];
        
        for (int t=0; t<times; t++){
            
            // get image from viewer
            DCMPix *pix = [PixList objectAtIndex:t];
            
            // pass it to import filter
            importFilter->SetImportPointer([pix fImage], bufferSize, false);
            
            // use threshold filter
            thresholdFilter->SetInput(importFilter->GetOutput());
            thresholdFilter->Update();
            
            // get result buffer
            float* resultBuff = thresholdFilter->GetOutput()->GetBufferPointer();
            
            long mem = bufferSize * sizeof(float);
            
            // copy result to current pix
            memcpy( [[[new2DViewer pixList:s] objectAtIndex:t] fImage], resultBuff, mem);
        }
    }
    [new2DViewer needsDisplayUpdate];
    return 0;
}

@end
{% endhighlight %}
