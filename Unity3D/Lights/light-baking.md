### Light Baking

- A way to take all the shadows/lighting data and baking them to texture maps that are applied to objects. So at runtime there is no need for light calculations. 
- Lighting/Settings
    - Scene tab:
        - Skybox material, Sun source
        - Environment lighting
    - Auto generate uncheck before all the lights are properly set in the scene.
    - Realtime Lighting should be checked
    - Baked Global illumination should be checked
    - Lightmapping Settings
        - Most of functions used for shadows are situated here.
- In order to apply lightmaps to objects
    - The mesh has to have a second UV channel, with unique UVs to each surface.
        - Generally they share the UVs
        - For lightmapping you'll need a separate UV channel.
            - In import settings check the `Generate Lightmap UV's` checkbox
- The object needs to be a static object.
    - Make sure it is also Lightmap static.
    - Also you need a lighting on them, which should be set to `Baked`
