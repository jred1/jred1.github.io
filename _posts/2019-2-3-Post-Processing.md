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
Before I go into specifics of the custom effects, I will explain the basics of how post processing works. A post processing effect is programmed in a shader langage, in this case HLSL, and defines how a single pixel should be displayed. These programs are reffered to as shaders. The main input for this program is the main texture that is rendered from the main camera in the scene. This is the texture that is eventually displayed to the screen. The shader can access parameters set by the programmer, such as textures, colors, floating-point numbers and integers. Since the output of the shader is just the color for a single pixel, the code must be ran for every pixel on the screen. This means that for a standard 1080p monitor, the shader must be run 2,073,600 times each frame. I am developing for VR, so the frames per second is 90. Meaning the program must run 186,624,000 times each second. This is possible thanks to the power of graphics cards, but the performance of a post processing effect can quickly get out of hand.
