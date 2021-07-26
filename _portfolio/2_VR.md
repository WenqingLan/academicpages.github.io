---
title: "VR Headset"
excerpt: "A fully functional Virtual Reality headset with stereoscopic rendering and body movement tracking. <br/><img src='/images/portfolio/headset.jpg'>"
collection: portfolio
---


## Graphics Pipeline

To render objects on the display, the complete graphics pipeline is implemented. 

Through Model Transform, the teapots are scalable and can be moved and rotated along x, y, x axes. After the teapots are placed in world space, View Transform can be applied to show how they are viewed when the camera moves. Next, when the camera is fixed to a position, Projection Transform is used to show how the teapots are projected from 3D world to a 2D plane based on parameters of the camera's lens.

In addition, lighting and shading using GLSL shaders are added to each teapot. 

![teapots](/images/portfolio/render_objects.gif)
<figcaption> Button Model Control, Viewer Position, Viewer Target showed the Model Transform, View Transform, and Projection Transform perspectively. Shading models from left to right. 1. Ambient Light 2. Ambient Light + Diffuse Term 3. Gouraud Shading (previous two + Specular Term) 4. Phong Shading. 5. Phong with Directional Light. </figcaption>


## Stereo Rendering
details coming soon.

## Inertial Measurement Units (IMU) and Pose Tracking

