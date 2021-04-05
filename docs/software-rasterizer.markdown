---
layout: page
title: Software Rasterizer
permalink: /software-rasterizer/
---

I initially started work on the software rasterizer to improve my understanding of the graphics pipeline and details like perspective correct interpolation and 
rasterization algorithms.  
After I implemented the basics I decided to use it as an exercise in CPU optimization. For that I profiled the project, optimized algorithms and restructured data layouts for
improved cache hit rates. Additionally I learned about SIMD and multithreading and used it to speed up the computationally most expensive parts of the rendering.

<img src="/images/SoftwareRasterizer/ukulele.png" alt="Ukulele Render"/>

The project is only using the C++ standard library, no external libraries have been used. Meshes are loaded as .obj files with textures in the .tga format. 
The rendering is structured with functions akin to shaders, with a function for vertex transformation and a function for pixel shading being passed to the renderer.  
When profiling the software the majority of the rendering time was spent on rasterization and shading, so these are the parts that I focused on optimizing.
For multithreading the image is split into tiles. Every triangle writes it's index into all tiles that intersect with it's bounding box. 
The threads work on different tiles, this way they never collide and no locking for pixel access is needed.  
Within each thread four pixels are processed at a time using SIMD. This proved to be a more consistent and easier way to use all lanes than using SIMD for a 
single pixel. Besides that, masking lanes are used to avoid branches and allowing the data to stay in the SIMD registers for as long as possible. 

<img src="/images/SoftwareRasterizer/avocado.png" alt="Avocado Render">

<div>
The project can be found on <a href="https://github.com/Gaukler/Software-Rasterizer" class="link-visible">GitHub</a>.
</div>