---
layout: post
title: Stylized Post-Processing
excerpt_separator: <!--more-->
---
****
<div align="center">
    <img src="/images/PostProcessComparisonMain.png" width="900">
</div>
The visual quality of a game is a major contributing factor to how emmersive the experience is. However, better graphical fidelity isn't always the answer to a more emmersive experience as most games that attempt realism get stuck in the uncanny valley. The other apporach is stylism. Instead of getting closer to realism, stylized games aim to exaggerate certain aspects and leave others up to interpretation. The purpose of this post processing is to minimise the over-precistion of 3D rendering to create a more stylized portayal of a game. The picture above compares a scene without and with post processing effects. The scene on the left has no post processing effects and the scene on the right has color grading, vignette, fog, and the custom effects that this post will discuss.
<!--more-->



## The Basics
Before I go into specifics of the custom effects, I will explain the basics of how post processing works. A post processing effect is programmed in a shader langage, in this case HLSL, and defines how a single pixel should be displayed. These programs are reffered to as shaders. The main input for this program is the main texture that is rendered from the main camera in the scene. This is the texture that is eventually displayed to the screen. The shader can access parameters set by the programmer, such as textures, colors, floating-point numbers and integers. 

Since the output of the shader is just the color for a single pixel, the code must be ran for every pixel on the screen. This means that for a standard 1080p monitor, the shader must be run 2,073,600 times each frame. I am developing these effects with an emphasis on performance, so I do not want any significant difference in performance between it being on and off even at 500+ frames per second(fps). If the game is running at 500fps then the program must run 1,036,800,000 times each second. This is possible thanks to the power of graphics cards, but the performance of a post processing effect can quickly get out of hand with complex effects.

If a shader is simple, such as multiplying the input pixel by a certain color to tint the image, then the performance degridation is negligible. On the other hand, if one run of a shader requres sampling several dozen pixels, and performing several dozen calculations to each, then the fps can drop massively. 


## Inspiration and Implementation
The idea for the custom effects started with the kurahara filter. The purpose of the kurahara filter is to reduce the noise in an image without blurring the image. During this process, it is able to smooth simplify forms. It accomplishes this by taking color samples of pixels in four regions surrounding the current pixel. The mean of the region with the lowest standard deviation becomes the new color for the current pixel. The number of samples used for each region is good for elimiating extreme noise, but it is overboard as a post processing effect. The image below shows the difference between the standard kuwahara and my custom implementation.

<div align="center">
    <img src="/images/PixelSampling.png" width="500">
</div>

## Features
Note: I suggest to open the images of the comparisons in a new tab to get a closer look.
### - Flow
<div align="center">
    <img src="/images/FlowComparisonWithOverlay.png" width="900">
</div>

### - Blending
<div align="center">
    <img src="/images/TreeBlend.png" width="900">
</div>

### - Close-Up Appearance
<div align="center">
    <img src="/images/CloseUpFidelityWithOverlay.png" width="900">
</div>

### - Hiding Texture Tiling





