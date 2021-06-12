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
- Every surface is considered to be visible to a light unless it faces away from that light source. No objects cast shadows.
- Simpler rendering: without lights and just draw each object with a solid color.

#### Teapot
- A model created by Martin Newell. Who is also creator of the painter's algortihm.
- The curves are represented by cubic Bézier Splines.

#### A Jog down the pipeline
- GPU uses rasterization or scan conversion.
- A rendering pipeline treats each object separately.
	- First, the application sends objects to the GPU.
		- 3D triangles, each triangle is defined by the full locations of it's 3 points.
		- An application converts a cube into a few triangles.
	- Second, the triangles are modified by the camera's view of the world along with whatever modelling transform is applied.
		- A modelling transform is way to modify the location, orientation and size of the part.
		- After the object is moved to it's location it is checked whether it is inside the view frustum or not.
			- Clipping
		-  The camera and modelling transforms compute the location of each triangle on the screen.
		- If the triangle is partially or fully inside the frustum, the thrree points of the triangle on the screen are then used in a process called rasterization.	
	- Rasterization identifies all the pixels whose centers are inside the triangle (fills in the triangle).

#### Pipeline parallelism
- The idea of a pipeline is that every object is dealt with once.
- The advantage of a pipeline is that each part of the pipeline can be worked on as a separate task at the same time.
- GPUs are designed to use pipelines and parallelism to get extremely fast rendering speeds.
- The bottleneck (slowest stage) determines how fast anything is going to come out of the pipeline.
- The bottleneck can change over time. 
- GPU designers use different techniques to perform load balancing (ex.: FIFO, unified shaders).
- 

#### Painter's algorithm
- What happens if two triangles overlap on the screen? Which one is visible?
- Draw each objects on top of the other. Sort the objects based on their distance from the camera back to front. And render the most distant first, next the next closest object and so on.

#### Z-Buffer
- The GPU solves the visibility problem by using Z-Buffer.
- Z stands for distance from the camera. Buffer means data array (ex.: image)
- For the image in addition to storing a color at each pixel, we also store a distance (z-depth).
	- float ranging from 0 (close to the eye) to 1 (maximum distance)
- At each pixel the distance of the object to the camera is used to compute it's z depth (a value from 0 to 1).
	- The computed value is checked against the value stored in the Z-Buffer (which is all initialized to 0).
	- If the object's distance is lower then the z depth value stored the object is closer to the camera. 
	- We'll use the object's color at this pixel.

#### Z-Buffer optimization
- Assume it's 1 cycle to read, write to the Z-Buffer. And 1 cycle to write a color.
-  The new value coming in is called source and the old value called destination.
- When the source is smaller than the destination it's 3 cycles (read, save, store)
- When the source is greater then the destination it's only 1 cycle (read)
- Drawing object from back to front is the worst case.
- Drawing objects from front to back is the most optimal way.

#### WebGL
- API Based on OpenGL ES
- State based API
	- Setup how you want the GPU to do something, then send a geometry to render.
	- Fine grained control

### Problem set

#### Frame skipping
- Assume a frame is not computed until the previous frame is displayed.
- If you don't get the work done in time to catch the current frame, you have to wait until the next one comes.

#### FPS vs. milliseconds
- Using miliseconds per frame can be easier to work with mathematically.
- Can be used to determine a budget.

#### Light Field Dimensions
- Our eyes detect radiance coming from each direction.
- Brain figures out distances from visual cues.
- Light field is that for any point in the universe and for all directions from that point there is an incoming radiance value that we can record.
- We record a radiance for each different, specific direction we look. To form an image we find the radiance coming through each pixel.
- At least 5 dimensions are needed to capture the radiance from any location and direction in the universe: 3 dimensions for the locations and 2 for the direction (altitude and swivel angle).
- Our eyes just detect radiance not distance.
- Put enough radiance readings for a given location and set of directions and you get an image.

### Points, vectors and meshes
#### Coordinate system
- Point is a location in space.
- A 3D vector defines a direction in space.
- A location in space has to be defined with respect to something else.
- Cartesian coordinate system has an origin and three direction vectors called x, y, z.
- The origin is some fixed point in space.
- The three direction vectors define the axes of the coordinate system. They are normally each perpendicular to the other.
- It's possible to define a coordinate system with axes that are not perpendicular.
-  With a given origin and 3 vectors we can define the location of any point in space.
- 3D vectors are described by a similar coordinate system, except that the origin is not needed. A vector describes a motion.
- Time analogy: a specific time is like a point and an amount of time is like a vector.

### Left-handed vs. right handed
- Coordinate systems have an orientation to them. 
- Right handed coordinate system: z-axis coming towards the viewer, x-axis going right, y-axis going up.
	- Thumb along the x-axis, index finger along the y-axis, mideel finger is the z-axis.
	- You can only do this with your right hand.
- Both are equally valid.
- To convert between these systems is simply a matter of negating the z-coordinate value.


https://classroom.udacity.com/courses/cs291/lessons/90856897/concepts/968210180923