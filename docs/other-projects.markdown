---
layout: page
title: Other Projects
permalink: /other-projects/
---

# Vulkan Framework University Project

We developed a Vulkan framework with a 14 person team during a university research project.
After developing the core framework we split into smaller groups to develop small, independent projects using the framework to test and refine it.
 
Our first project was a rendering demo that used several different techniques.
The goal was to test a variety of API features, show the capabilities of the framework and to work on algorithms that we were personally interested in.
We worked on the demo as a two person team.
I implemented voxel cone tracing for diffuse and specular indirect illumination using real time scene voxelization, moment shadow mapping and MSAA with custom resolve.
In addition I added film grain, lens distortion and chromatic aberration as post processing effects.

<a href="/images/OtherProjects/VoxelConeTracing.PNG" target="_blank">
  <img src="/images/OtherProjects/VoxelConeTracing.jpg">
</a>

I wrote a small GPU Monte-Carlo path tracer that consisted of a single compute shader. It used an existing compute Whitted-style raytracer as it's basis.
It supports diffuse and specular light transport, uses importance sampling and runs in real time.
Tracing is performed at a single sample per pixel. The frame results are accumulated, as long as the scene is static.
Instead of triangle meshes it uses simple rectangle and sphere primitives.

<a href="/images/OtherProjects/PathTracer.PNG" target="_blank">
  <img src="/images/OtherProjects/PathTracer.jpg">
</a>

We wrote an application to experiment with mesh shaders in a three person team. It features meshlet frustum culling based on meshlet bounding spheres.
I was responsible for the meshlet GPU data structure and writing the mesh and task shaders.

<a href="/images/OtherProjects/MeshShader.PNG" target="_blank">
  <img src="/images/OtherProjects/MeshShader.jpg">
</a>

I wanted to test out Vulkan's indirect dispatch feature. I chose post-process motion blur as an application.
Indirect dispatch is used to dispatch a minimal compute shader for every part of the image. An initial shader classifies the different image parts, based on the motion. 
Static parts of the image do not need any blur, so they use a simple copy shader. Uniformly moving parts of the image use a simplified blur. 
The fully featured blur shader is only applied to the remaining areas.

<a href="/images/OtherProjects/MotionBlur.PNG" target="_blank">
  <img src="/images/OtherProjects/MotionBlur.jpg">
</a>

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