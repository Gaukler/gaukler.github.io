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

<figure>
  <img src="/images/Plain/sky.png" alt="Sky" />
  <figcaption> Sky rendering using earth's atmosphere values </figcaption>
</figure>

<div class="image-text-container">

    <div class="image-text-text-container">
        Of course one of the most importance factors in good looking rendering is indirect lighting. 
        Because of the dynamic sky model and the joy of moving the sun around in real time I decided that I wanted to implement a GI system that supports dynamic lighting. 
        Inspired by recent techniques that use signed-distance-fields (SDFs) for lighting I decided to precompute an SDF per mesh and trace the indirect lighting at runtime in an SDF representation of the scene.
    </div>
    
    <figure class="image-text-image-container">
      <img src="/images/Plain/indirectLighting.gif" alt="Indiret lighting" />
      <figcaption> Dynamic indirect lighting </figcaption>
    </figure>
    
</div>

# Feature list

* Real time diffuse GI, by tracing SDF representation of scene and denoising
* Cook-Torrance BRDF using GGX-distribution, correlated smith geometry term and multiscattering
* Temporal Anti Aliasing using an exponential history buffer and bicubic sampling
* Physically based sky rendering with multiscattering approximation
* Physically based light and camera units and histogram based automatic exposure
* Volumetric lighting using froxels     
* Bloom and tonemapping
* Single pass min/max hierarchical depth buffer generation
* Cascaded shadow maps for the sun, tightly fitted to depth buffer
* Blue noise generation using the void and cluster method
* 3D perlin noise generation
* Simple job system for multithreading, used for accelerating SDF generation, texture loading and multithreaded drawcall recording
* Custom Vulkan memory allocator
* Separate asset pipeline producing mesh data in a binary format for efficient loading and SDF generation 