---
layout: page
title: Other Projects
permalink: /other-projects/
---

# OpenGL GPU Particle System

This small C++ project was created as a learning exercise for compute shaders. 
It's build on a small OpenGL wrapper. The GPU particle system uses the traditional 
double buffered design, with a compute shader for emission and a shader for simulation. 
The simulation shader supports collision with the scene's depth buffer.

<a href="/images/OtherProjects/GPUparticles.gif" target="_blank">
  <img src="/images/OtherProjects/GPUparticles.gif">
</a>

# Animation and Simulation University Project

This project was a three person team effort and was our submission for the animation and simulation course. 
The main features are skeletal animations, a particle system and rigid body physics.
These features are tied together by a small game like scene, with an animated controllable character.
Spheres are shot by 'enemies' and can be held and thrown with telekinesis, with the particles used as a visual flourish.  

My contribution was writing the rigid body system, terrain, player control and the logic components, like telekinesis and enemy spawns and behaviour.
The rigid body system supports sphere colliders and collision with the height based terrain.
In terms of visuals I implemented TAA, per object motion blur and the raymarched sky.

<a href="/images/OtherProjects/AnSim.gif" target="_blank">
  <img src="/images/OtherProjects/AnSim.gif">
</a>

# Augmented Reality Shader Cube

The AR Shader Cube was our project for the Augmented and Virtual Reality bachelor module.
The assignment was to implement marker tracking, using a framework for image processing and rendering developed by students, which leveraged OpenCV. 
We were a three person team and decided to create a physical cube with markers on each side and replace it with a virtual cube.
Each face of the virtual cube is a window into a small scene, demonstrating a shader effect. 

My main contribution was to come up with cool shader effects and implement them.

<div>
    An interview (in german) demonstrating the finished project can be found <a href="https://www.youtube.com/watch?v=-4J7I_7gFh0" class="link-visible">here</a>.
</div>
<br/>

<div style="display:flex;flex-wrap:wrap;">
    <div class="image_tripple">
        <figure>
            <a href="/images/OtherProjects/ShaderCube/DepthPeeling.PNG" target="_blank">
              <img src="/images/OtherProjects/ShaderCube/DepthPeeling.jpg">
            </a>
            <figcaption> Order independent transparency, using depth peeling </figcaption>
        </figure>
    </div>
    
    <div class="image_tripple">
        <figure>
            <a href="/images/OtherProjects/ShaderCube/Disco.PNG" target="_blank">
              <img src="/images/OtherProjects/ShaderCube/Disco.jpg">
            </a>
            <figcaption> Many lights, using deferred shading and spherical light proxies </figcaption>
        </figure>
    </div>
    
    <div class="image_tripple">
        <figure>
            <a href="/images/OtherProjects/ShaderCube/IBL.PNG" target="_blank">
              <img src="/images/OtherProjects/ShaderCube/IBL.jpg">
            </a>
            <figcaption> Diffuse image based lighting </figcaption>
        </figure>
    </div>

    <div class="image_tripple">
        <figure>
            <a href="/images/OtherProjects/ShaderCube/SphereTracing.PNG" target="_blank">
              <img src="/images/OtherProjects/ShaderCube/SphereTracing.jpg">
            </a>
            <figcaption> Sphere tracing on an implicit surface, defined by a signed distance field </figcaption>
        </figure>
    </div>
    
    <div class="image_tripple">
        <figure>
            <a href="/images/OtherProjects/ShaderCube/Translucency.PNG" target="_blank">
              <img src="/images/OtherProjects/ShaderCube/Translucency.jpg">
            </a>
            <figcaption> Translucency </figcaption>
        </figure>
    </div>
    
    <div class="image_tripple">
        <figure>
            <a href="/images/OtherProjects/ShaderCube/Volumetric.PNG" target="_blank">
              <img src="/images/OtherProjects/ShaderCube/Volumetric.jpg">
            </a>
            <figcaption> Volumetric shadowed spotlight </figcaption>
        </figure>
    </div>
</div>