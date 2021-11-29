## Real-time path tracing using a voxel representation of the scene:

# **Implementation details**:

So this project started as simply learning OpenGL and one of the things I wanted to try implementing was VXGI. Essentially just doing real-time global illumination using a voxel representation of the scene! The traversal algorithm is simply ray-marching, which is inaccurate due to the step size being either too small(taking way too many cycles to reach a filled voxel), or too big, which causes it to miss some voxels that it would've otherwise hit.

The voxelization is done at run-time, during the rasterization process, by simply checking the current world position of the fragment in a 3d texture and if it's empty, we set the alpha to one of two choices. If it's set to 0.45, then the voxel is emissive and if it's set to 0.95, then the voxel is just opaque. I voxelize the scene both from the perspective of the camera and the perspective of the sun light.

-First I render the scene from the point of view of the sun light, writing to a 3d texture for the voxels and to a depth buffer texture.
-Second thing I do is create the G-Buffer that holds information like world position, normals, depth and etc... 
-After which in a new shader I compute a single sample of path tracing with 5 bounces and using the sun's depth texture to do NEE(next event estimation).
I use temporal reprojection to accumulate multiple samples across multiple frames. 

Currently I have code for denoisement that works, however I've decided that it needs a major rewrite to make it better.

A lot of progress can be seen on my channel:
[Link]https://www.youtube.com/channel/UC43Y2pvLs3G60Zqsj7p2D0w/videos


![Octocat](https://github.com/NamelessCoding/NamelessCoding.github.io/blob/main/assets/images/ConsoleApp1_jlnU8eAfnQ.png?raw=true)
![Octocat](https://github.com/NamelessCoding/NamelessCoding.github.io/blob/main/assets/images/ConsoleApp1_OE1TYePKfz.jpg?raw=true)
![Octocat](https://github.com/NamelessCoding/NamelessCoding.github.io/blob/main/assets/images/ConsoleApp1_jlzpTlKV9W.jpg?raw=true)

[back](./)
