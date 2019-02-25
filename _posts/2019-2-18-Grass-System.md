---
layout: post
title: Grass Generation and Rendering
excerpt_separator: <!--more-->
---
****
<div align="center">
    <img src="/images/GrassMain.png" width="900">
</div>

Grass is an important aspect of a nature scene. It adds depth to the surface of the terrain, and can add a layer of immersion if it reacts to real-time forces, such as wind or walking on it. However, its use is typically avoided as much as possible in video games mainly for how graphically expensive it is to render. Most grass systems in games have short draw distances, lack player feedback, and have sparsely placed clumps of grass. The purpose of my grass system is to be able to overcome these shortcomings without sacrificing performance.
<!--more-->

## Generating the grass
There are two major steps in the generation of a field of grass:
- placing points over the terrain
- using this point data to create batches of grass
##### Note: I will not be covering every single calculation, and avoid discussing the heavily nested data structure and custom classes used to store the information. I will discuss the generation process in a more abstract way to hopefully help understandability of the system.

### Placing points
First, the system needs a few basic parameters to determine where to check for possible grass. These parameters include the size of the terrain, the grass density, and the amount of randomized jitter to apply to each point check. To check for points, a ray is casted from above the terrain for each possible point of grass defined by the aforementioned parameters. For example, for a 1 km by 1 km terrain with a grass density of 3.5 points per meter, and a 70 percent jitter, there are 12,250,000 possible points for grass. The first check for each point is to determine what collider the ray is hitting by getting the tag of the object the collider is attached to. The tags that are checked include:
- Terrain 
  - Defines a terrain object. The texture layer at the collision point, and the blend cutoff for that texture will also be used to decide if grass should be placed.
- Grass Object 
  - Defines a non-terrain object that grass should be placed on.
- Pass 
  - Defines an object that the check should ignore.
- Blend 
  - Defines an object that prevents grass from being placed at the collision point and changes the terrain texture below the object.

This check defaults to preventing grass from being placed if the object has a different tag than listed above. If the tag is Grass Object or Terrain, then further calculations are needed. The next calculation involves the slope of the surface the ray hit. Grass will not be placed if the angle of the slope is larger than the slope limit value set in the parameters. If the angle was over the limit, and the object tag was Terrain, then the terrain texture is changed to a rock texture. If the angle is under the limit, then the point is stored with data of the collision point, collision normal, and a pseudo-random scale. Also, for objects tagged with Terrain, Grass Object, or Pass, the associated calculations are performed, then another ray is casted underneath the collision point and follows the same checks. This means that the system is capable of placing grass underneath overhanging ledges, trees, and etc. The system is also capable of calculating and storing the data for several different grass types at the same time.

### Batched Mesh Generation
After the point data has been created, a mesh preset, such as the one in the image above, can be placed, rotated, and scaled properly. However, some extra steps are needed before the grass can be rendered. The meshes need to be batched and culled properly for fluid visuals and frame-rate. 

Typical batching involves combining the meshes from several objects into a single mesh on a single object. Batching helps to cut down on the number of calculations for the CPU, in turn improving performance. The point data has been stored in a batched format to simplify the mesh generation process. A multi-threaded program runs through a batched set of points to create multiple vertices at each point with respect to the mesh preset. Other vertex data, such as normals, and vertex colors are also calculated in the multi-threaded program to ensure the mesh is rendered properly. The calculated data along with unchanged values such as the triangle indices and UV coordinates are used to create a batch of grass containing thousands of blades of grass. The mesh creation process is repeated until all of the batches are complete. 

Batching meshes is helpful, but only to an extent. The game would not perform well if the grass across the entire terrain was combined into a single mesh. This is due to the number of vertices that need to be rendered in each frame. If any part of a mesh is visible on screen, then the shader needs to run on every vertex in the mesh. So, the batch area is kept to a reasonable size (under 30 square meters). 


## Performance
<div align="center">
    <img src="/images/GrassPerf.png" width="500">
</div>

The most important aspect of this grass system is that it performs well while overcoming the faults of typical systems. The picture above shows a scene in three different states: no grass, the built-in Unity grass, and my custom grass system. To demonstrate my system’s performance at higher densities, the built-in grass system was set up with similar vertex count, the same batch size, and same draw distance. This scene without any grass was able to run at an average of 380 fps (frames per second) while the built-in system averaged 170 fps and the custom system ran at an average of 350 fps. The custom system is able to provide much better performance and with more features than the built-in system. A significant part of the performance difference between the built-in system and the custom system is due to the shader. I discussed image effect shaders in my previous post, but these shaders are different as they are used to determine how a mesh is rendered. Even though the shader for the custom system is overall more complex, it avoids one particular and very expensive effect. This effect is called alpha clipping and is demonstrated below.

<div align="center">
    <img src="/images/alphaClipEx.png" width="300">
</div>

The purpose of this effect is to make parts of a mesh completely transparent to fake more complex geometry. When applied to a quad mesh (which is what is used for the built-in system) it makes the quad appear as a plane of grass. However, this technique has two major flaws. On the performance side, the mesh is treated as transparent geometry, which is expensive to compute. On the appearance side, the grass is visibly flat, especially when seen up close which makes it unsuitable for most virtual reality or first-person games.



