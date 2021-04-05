---
layout: page
title: OpenGL Renderer
permalink: /opengl-renderer/
---

The OpenGL renderer was my first attempt at a bigger rendering project, which combines multiple techniques and wraps the underlying API for convenient use.
It was also where I began implementing papers, starting with simple ones like cascaded shadow mapping and working my way up to more advanced ones, like temporal anti aliasing.  
Furthermore it served as a starting point for my bachelor thesis about global illumination approximations, 
in which I implemented light propagation volumes(LPVs), screen space reflections(SSR) and screen space directional(SSDO) occlusion.

<figure>
  <img src="/images/GLRenderer/FlightHelmet.png" alt="Flight Helmet IBL" />
  <figcaption> Flight Helmet model, lit using image based lighting </figcaption>
</figure>

The project is written in C++ and renders in a deferred manner, altough forward materials are supported.  
Image based lighting(IBL) is implemented, using the traditional approach of
pre-convolving an HDRi cubemap and using the split-sum approximation for the specular BRDF.  

<figure>
  <img src="/images/GLRenderer/LPV.png" alt="Light propagation volumes" />
  <figcaption> A combination of LPV, SSDO and SSR is used for a coarse approximation of indirect lighting </figcaption>
</figure>

The sun light supports cascaded shadow mapping, with the option of texel-snapping and rotational invariant projection to avoid flickering under movement and rotation. 
Additionally shadowed point lights are supported.

<figure>
  <img src="/images/GLRenderer/PointLights.png" alt="Point Light demo" />
  <figcaption> Sponza scene lit by four point lights, two shadowed and two unshadowed </figcaption>
</figure>

SSR and SSDO are computed at half resolution to save on processing time. They both use a combination of spatial and temporal filtering for a smooth and stable appearance. 
Depending on the global illumination algorithm, reflections from the IBL or the LPV are used as a fallback, in case the reflection information is not available in screen space. 

<figure>
  <img src="/images/GLRenderer/SSR.png" alt="Screen Space reflections demo" />
  <figcaption> Screen space reflections, the floor uses a smooth metal material </figcaption>
</figure>