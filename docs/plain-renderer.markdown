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