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

### Foveated rendering

Foveated rendering is implemented to save computational time to render the scene. With our eyes focusing on a point, things far away form the focus are blurred out and we render the scene with advantage of it.

| Standard (no blur) | Foveated |
| :------: | :------: |
| ![standard](/images/portfolio/standard.png)| ![foveated_rendering](/images/portfolio/foveated.png)|

<figcaption> The black dot represents the focus. Things are blurred out in ranges of the circle.</figcaption>

### depth-of-field (dof) rendering

Depth-of-field rendering is implemented to create more realistic scenes based on the limited depth of field of human eyes. Every object is blurred differently based on its depth to our eye, so this technique could actually add computational time to the rendering process.

| Focus on the front | Focus on the back |
| :------: | :------: |
|![dof_front](/images/portfolio/dof_front.png)  | ![dof_back](/images/portfolio/dof_back.png)|

<figcaption>The black dot represents the focus. You can right click on the image and open it in a new tab to see the bigger picture. </figcaption>

### Anaglyph rendering

Here comes the first 3D rendering result. Different View and Projection Transforms are applied to the left and right eyes. The two renderings are finally rendered in red v.s. green + blue color channels and combined into this Anaglygh picture. Wearing the anaglyph glasses, you will be able to view the 3D result.

![anaglyphic](/images/portfolio/anaglyphic.png)


### HMD Stereo Rendering

Here we start to render the objects on display in the headset. The screen is separated into two parts side by side and each eye sees one scene through the headset's lenses. To make the objects align well in both eyes and looks realistically 3D, we render the scene based on computations over these parameters: focal length of lenses, lens diameter, interpupillary distance (IPD), distance between lenses and the display, display width, and eye relief.

Here is the effect on the display. 

![anaglyphic](/images/portfolio/without_correction.png)


However, due to the optical features of lenses, straight lines on the display will look bended through the lenses. To correct the distortion in our view, the opposite kind of distortion is applied to our rendering. Here is the updated effect.

![anaglyphic](/images/portfolio/with_correction.png)

Below shows the comparison between the distorted view and the corrected result.

| Pincushion distortion | Corrected |
| :------: | :------: |
| ![distorted](/images/portfolio/distorted.jpeg) | ![distorted](/images/portfolio/corrected.jpeg) | 

Here we created vivid teapots floating from different distances in front of us. Woohoo!

## Inertial Measurement Units (IMU) and Pose Tracking

### Gyroscope and Accelerometer based Orientation Tracking

Now we use the IMU and Teensy to sample gyroscope and accelerometer information to compute our head's rotation using quaternion algebra. Sampled data is subject to noise variance and bias due to the environment like temperature and mechanical stress, so we first estimate them and then subtract them from the system. We found that gyroscope values are more subject to variances, while accelerometer values are more subject to biases. To resolve this, we use a complementary filter to combine both gyroscope and accelerometer information to balance out their negative effects.

![imu](/images/portfolio/imu.gif)

You can see it's a little bit shaky. It's because the data is been collected at a high frequency and the transient acceleration contributes too much to the short-term orientation estimate. The performance can be refined by limiting the Teensy's output rate and adjusting the complementary filter's parameter.

### Marker-based Pose Tracking

Now, we will use the marker-based tracking provided by AR.js to collect position data. A marker tag is created and attached to the front of our AR headset. The position data is collected via the web camera, and the orientation data solely comes from the IMU. By fusing them into our quaternion calculation, here is the achieved result.

![marker](/images/portfolio/marker.gif)

