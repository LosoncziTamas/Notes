# Interactive 3D Graphics

## Concepts

- To render means to create an image.
- In 3D graphics you mathematically define objects, materials, lights and a camera. And use them together to render onto a 2D screen.
- Interactive means you can instantly influence what you see on the screen.

### Core ideas

#### FPS

- The minimum FPS for video games is usually 30 or 60 frames per second. The number of new images in a second is called _frame rate_.
- Most monitors refresh at 60 hertz. They refresh 60 times a second. This is called a _refresh rate_. It gives an upper limit on the frame rate.
- In the film industry the standard is 24 FPS. But the refresh rate can be 48 or 72 hertz. A film can have a low framerate because a film captures motion blur, which gives a smoother experience. 

#### CPU cycles

- If you have a CPU running at 2.8 GHZ it means you have 2.8 billion instructions per second. 56 million cycles for 20 milisec (assuming the 20 ms framerate). If there are 1 million pixels on the screen, this leaves 56 cycles per pixel per frame.
- The CPU usually takes care of creating/positioning objects in the scene and the objects sent to the GPU.

#### Pinhole camera

- Put together different light contributions and you get a picture.

#### Seeing is believing
- Our visual system is not a light meter. It tries to compensate for shadows and other distractions so that we can properly percieve the nature of the objects.

#### View frustum
- In CG we think of a camera with the image being formed in front of the viewer.
- The pyramid shaped view of the world is called the view frustum. Frustum means pyramid with it's top chopped off.
- We want to know how much light is coming from each direction.
- Each light emits photons which bounce around the world. In a camera we gather photons from the world and these create a photograph.
- In CG we try to determine how many photons are coming in each direction. Whatever amount of light we gather from a direction, we record a pixel in the screen.

#### Screen Door
- Screen as a window into a 3D world. 

#### 3D scene
- The computer renders a 3D scene by determining how much light comes from each direction.
- First we'll have to define a world. Geometry, material, animation, lights, camera.

#### How many pixels?
- GPU consists of specialized units working in parallel.
- For example 1920 x 1200 pixel monitor, 60 FPS frame rate. 138240000
- Pixel = picture element. The minimal number of pixels to compute.

#### Light and rendering
- In theory you could fully simulate the world around you. Photon starts from a light source, hit atoms, they are reflected, absorbed.
- You put a camera in the scene and whatever photons actually enter the camera's lens would determine the image.
- This would take a huge amount of computation.

#### Reversing the process
- It would be expensive to track all the photons in a scene. To avoid this level of computation. In CG make assumptions:
- Only the photons that reach the camera are the ones needed to make the image.
- Instead of sending photons from the the light, we essentially cast a ray from the eye through each pixel and see what's out there. When a surface is seen at a pixel we then compute the direct effect of each light on that surface. Add up all the light contributions and you have a reasonable proximation of what the surface looks like.

https://classroom.udacity.com/courses/cs291/lessons/68866048/concepts/964035330923