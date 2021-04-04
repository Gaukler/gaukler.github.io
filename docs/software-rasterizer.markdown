---
layout: page
title: Software Rasterizer
permalink: /software-rasterizer/
---

I initially started work on the software rasterizer to improve my understanding of the graphics pipeline and details like perspective correct interpolation and 
rasterization algorithms.  
After I implemented the basics I decided to use it as an exercise in CPU optimization. For that I profiled the project, optimized algorithms and restructured data layouts for
improved cache hit rates. Additionally I used SIMD and multithreading to speed up the computationally most expensive parts of the rendering.

<div style="display:flex">
   <img src="/images/SoftwareRasterizer/ukulele.png" alt="Ukulele Render" class="image-side-by-side"/>
   <img src="/images/SoftwareRasterizer/avocado.png" alt="Avocado Render" class="image-side-by-side"/>
</div>