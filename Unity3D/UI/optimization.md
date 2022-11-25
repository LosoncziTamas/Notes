### How to optimize your UI in Unity for better performance

- UI happens to be one of the main performance bottlenecks.
- Canvas
    - Root of all UI elements, owns the meshes that are generated from all elements it contains.
    - Sends the meshes to the GPU.
    - Performs rengeration of meshes anytime something changes on canvas.
        - Mesh generation can be quite expensive.
- By breaking up the UI to static (non-changing) and dirty (changable) UI, you can save some unnecessary mesh generation.
    - For example if you have static buttons in your canvas, then move them to a new sub-canvas.
- Layout groups should be avoided in general.
    - Use them to precalculate the positions and then disable the layout group component.
        - This only possible if the layout group is no longer needed.
    - They are more expensive.
        - When something gets dirty, then the whole canvas gets rengenerated.
        - Layout groups also recalculate what's going on.
        - Additionally it makes dirty the parent layout group (many `GetComponent()` calls)
- Turn off raycast target for images that don't need UI events.
- Use canvas for modals.
    - When you turn a GO on/off that's in a canvas you'll regenerate the mesh.
    - But with canvas, it's not going to regenerate.
- Object pooling
    - Objects are created upfront.
    - Reparenting & removing will dirty the canvas.
    - Disable the object in question first, then parent it to want you want and then re-enable. And do the reverse for removal.
- Screen Space camera
    - If the render camera is empty it will perform a camera look-up each time.
- World Space
    - Added cost for raycast, 7-10 times more.


### Reducing draw calls
- To render multiple images in one draw call, they need:
    - to be on the same canvas
    - to share the same material
    - to use the same sprite asset
    - to be clipped by the same mask
    - to have RectTransform that share the same Z position.
- Unity Sprite atlas
    - `Objects for packing`, assign the images 
    - disable "allow rotation" if contains Canvas UI element Textures, as when Unity rotates the Textures in the Sprite Atlas during packing, it rotates their orientation in the Scene as well.
    - use crunch compression
- In profiler see UI module and `Batch Breaking Reason`


