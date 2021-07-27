# Tools, Tricks and Technologies for Reaching Stutter Free 60 FPS in INSIDE

## Technology invisible to the player
- Technology should never cause distraction from the game experience.
	- Game can be completed from start to end without loading screens.
	- No visual artifacts.
	- No framerate stutters.

### Splitting content spatially
- Split spatially, and then save parts into sub-scenes.
	- Take background and put in scene called Backdrop.
	- Split foreground to environment and gameplay scenes.
- Master scene for the global objects.
- Separate sub-scenes for other objects.
	- They can be loaded/unloaded in memory as the game progresses.

![alt text](./master_scene.png)


### Hierarchy of sub-scenes
![alt text](./subscenes.png)

https://www.youtube.com/watch?v=mQ2KTRn4BMI&list=WL&index=61&t=12s


## Scripting performance

### Performance budget
- 60 frames per second (16 ms per frame)
- Streaming & initialization takes 4.6 ms
- For rendering, fog & reflections the aim was 2 ms
- 1 ms for culling
- 9 ms for all general gameplay, particles

### Reduce vector operations
- Make sure all the floats are multiplied together (in compound math operations) before the vector multiplication. 

### Use cached Transforms
- When you call `gameobject.transform` it does a lot of safety check to make sure your object is not deleted, etc. Cache the transfrom at `Start()` and use the cached transform.

### Use localPosition (if possible)
- When the parent objects are located at 0, 0, 0 and the rotation is identity. Then you can use `transform.localPosition` instead of norma
- Unity stores all the data as localPosition, whenever you need a real position unity has to go all the way up to through the hierarchy and translate from local to world positions. And it doesn't chache it.

### Reduce engine calls
- You are working with managed code in C#. But unity is working with unmanaged code.
- At each engine call unity has to marshall/copy the managed code to unmanaged.

### Are getters and setters slow?
-  Replacing properties with public accessors may improve the performance when using mono script engine.
-  it's not the same with il2cpp.

### Don't use Vector math
- Inline the vector math code manually. 

### Time.deltaTime could be cached
- Store it globally

### Don't use foreach
- It creates an enumerator which creates garbage.

### Arrays! Not Lists.

https://youtu.be/mQ2KTRn4BMI?t=2563