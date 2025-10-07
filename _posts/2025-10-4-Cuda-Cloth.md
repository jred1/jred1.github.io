---
layout: post
title: CUDA Cloth Simulation
excerpt_separator: <!--more-->
---
****
<img style="float: left;height: 250px;margin: 0px 10px 10px 0px;" src="/images/Cloth Sim Gif Square.gif">
A CUDA-based GPU cloth simulation (mass-spring system) with self collision, object collision, particle pinning, gravity, and wind forces.
<!--more-->

## Results
Performance metrics were taken with a AMD Ryzen 7 3750H and NVIDIA GTX 1660 Ti<br/>
The Binned Neighbors implementation runs at ~4 ms frame time (~240 fps) using a RTX 4070

### Speedup Over CPU at 25.6k particles
Each Technique can be tested by changing to the corresponding index in the runSim() call
<hr>
<img src="/images/performance graph.png" width="500px">

|Index  |Technique|Speedup over CPU|
|--|---------|----------------|
|0 |CPU      |1.00x           |
|1 |Naive GPU|108.51x         |
|2 |Constants|110.88x         |
|3 |Math     |154.90x         |
|4 |Buffer| 154.68x           |
|5 |Binned Refresh|156.70x    |
|6 |Binned Neighbors|1,635.58x|

## More Viewing Angles
  <video height="250px" autoplay controls muted loop playsinline>
    <source src="/videos/Cloth Sim Video.mp4" type="video/mp4">
    Your browser does not support the video tag.
  </video>

<hr>
