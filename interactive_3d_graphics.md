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