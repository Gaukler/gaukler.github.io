---
layout: page
title: Blender Plugin - Alamo Tools
permalink: /alamo-tools/
---

Blender is an open-source 3D modeling software. The Blender Alamo plugin allows import and export of ALAMO engine models and animations, specifically the format used in "Star Wars: Empire at War: Forces of Corruption". 

It was my first bigger software project. 
I initially started it because I wanted to create some custom models for the game. 
However the only available exporter was for an old version of 3ds Max, which is not available anymore. 
During my research on the file model I stumbled on a complete description of the file model from Mike Lankamp, who reverse engineered and documented it. 
So I decided to write an exporter myself.  

After I made some progress, I started to work together with some members of the game's modding community to troubleshoot issues and to get feedback on the usability of the plugin. 
According to the feedback I adjusted the UI, automated certain procedures where possible and rewrote error and warning messages to be as informative as possible.
 
The exported models and animations can be used in the game and the plugin is actively used by the modding community. 
Besides the mesh and animation data the exporter handles additional data, required by the game, such as collision trees, preparing shadow meshes for their use as shadow volumes and 
visibility data for attached particle effects.