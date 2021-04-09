---
layout: page
title: Plain Renderer
permalink: /plain-renderer/
---

The Plain Renderer is a C++ Vulkan renderer I wrote to learn about lower level APIs and their more advanced features,
such as multithreaded command recording and explicit CPU/GPU synchronization.  
Additionally my goal was to build a framework that avoided as much boilerplate code as possible, 
so that I could focus on implementing rendering techniques without being bogged down by tedious details, like image layout transitions or memory barriers.  

<figure>
  <img src="/images/Plain/sponza01.png" alt="Sponza Screenshot" />
  <figcaption> The classic Sponza scene rendered in Plain </figcaption>
</figure>

In terms of visuals my goal was to achieve natural looking results.  
To achieve this I decided to go for a physically based approach.
Not only using the corresponging BRDF models, but also using physical light units, exposure values with the proper equations for the camera 
and following physically motivated techniques for volumetric and sky rendering.

<div>
The project can be found on <a href="https://github.com/Gaukler/PlainRenderer" class="link-visible">GitHub</a>.
</div> 
<br/>

<div class="image-text-container">

    <div class="image-text-text-container">
        <h1>Feature list</h1>
        <ul>
        <li>Real time diffuse GI, by tracing SDF representation of scene and denoising</li>
        <li>Cook-Torrance BRDF using GGX-distribution, correlated smith geometry term and multiscattering</li>
        <li>Temporal Anti Aliasing using an exponential history buffer and bicubic sampling</li>
        <li>Physically based sky rendering with multiscattering approximation</li>
        <li>Physically based light and camera units and histogram based automatic exposure</li>
        <li>Volumetric lighting using froxels</li>  
        <li>Bloom and tonemapping</li>
        <li>Single pass min/max hierarchical depth buffer generation</li>
        <li>Cascaded shadow maps for the sun, tightly fitted to depth buffer</li>
        <li>Blue noise generation using the void and cluster method</li>
        <li>3D perlin noise generation</li>
        <li>Simple job system for multithreading, used for accelerating SDF generation, texture loading and multithreaded drawcall recording</li>
        <li>Custom Vulkan memory allocator</li>
        <li>Separate asset pipeline producing mesh data in a binary format for efficient loading and SDF generation</li>
        <li>Shader hot reloading</li>
        </ul>
    </div>
    
    <figure class="image-text-image-container">
      <img src="/images/Plain/indirectLighting.gif" alt="Indiret lighting" />
      <figcaption> Dynamic indirect lighting </figcaption>
    </figure>
    
</div>

<figure>
  <img src="/images/Plain/sky.png" alt="Sky" />
  <figcaption> Sky rendering using earth's atmosphere values </figcaption>
</figure>