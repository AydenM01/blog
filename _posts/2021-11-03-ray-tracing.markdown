---
layout: post
title:  "Distribution Ray Tracing"
date:   2021-11-08 18:20:00
categories: School
---

School can actually be interesting?

I just finished working on one of the most interesting assignments during my time at Georgia Tech. The topic is Distribution Ray Tracing, where we got to implement some ideas from Robert L Cook's paper titled ['Distributed Ray Tracing'](https://dl.acm.org/doi/pdf/10.1145/964965.808590). Robert L Cook is the co-creator of RenderMan, and the ideas in this paper have been used by Pixar for some really cool effects.

The first half of the assignment was creating a simple Ray Tracer that only accounted for point lights. The shading model we used was based on Phong shading, which accounts for an ambient light component, a diffuse light component, and a specular light component. We then summed the contributions of the various components for all of the point lights in the scene.

The second half of the assignment consisted of adding area lights, shadowing (hard and soft), anti-aliasing, and reflection.

Distribution ray tracing is really interesting because you can simulate complex light sources by choosing random points on the light surface. Note that these points are not completely random, we can divide our area into a grid and choose a random point in each square of the grid, called jittering.

Anti-aliasing works similarly to the area light technique. For every pixel in our viewport, instead of tracing to the exact pixel location (like a simple ray tracer), we can make the pixel color an average of the results of jittered points. This leads to very smooth lines when you zoom into an image.

Reflection was surprisingly simple to implement, all we had to do was make our trace ray function recursive, and add the color of the reflected ray to the color we were going to return up to a maximum depth.

For shadowing, before setting the color of a pixel, we would check to see if any rays from light sources to the pixel are blocked, and if they are set the color to black. This resulted in soft shadows in the case of area light since only some portions of the light could be blocked.

Overall, I feel like I definitely have a better understanding of Ray Tracing from this assignment and have developed a greater appreciation for the advances in real-time ray tracing we now see in the RTX Nvidia GPUs.

Anyways, here was the final result of my custom raytracer.

![ray-tracing-image](/assets/images/ray-tracing.png)