---
layout: post
title: CUDA Cloth Simulation
excerpt_separator: <!--more-->
---
****
<div align="center" >
A CUDA-based GPU cloth simulation (mass-spring system) with self collision, object collision, particle pinning, gravity, and wind forces
  <video width="600" autoplay controls muted loop playsinline>
    <source src="/videos/Cloth Sim Video.mp4" type="video/mp4">
    Your browser does not support the video tag.
  </video>
</div>
<!--more-->

## Results
> ### Performance metrics were taken with a 	AMD Ryzen 7 3750H and NVIDIA GTX 1660 Ti<br/>
> ### The Binned Neighnors implementation runs at ~4 ms frame time (~240 fps) using a RTX 4070<br/>
### Speedup Over CPU at 25.6k particles
Each Technique can be tested by changing to the corresponding index in the runSim() call
<hr>
<img src="performance graph.png" width="500px">

|Index  |Technique|Speedup over CPU|
|--|---------|----------------|
|0 |CPU      |1.00x           |
|1 |Naive GPU|108.51x         |
|2 |Constants|110.88x         |
|3 |Math     |154.90x         |
|4 |Buffer| 154.68x           |
|5 |Binned Refresh|156.70x    |
|6 |Binned Neighbors|1,635.58x|

<hr>
