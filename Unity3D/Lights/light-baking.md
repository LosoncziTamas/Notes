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
- Lightmapper settings
    - Fiddle around with it:
        - Padding to prevent bleeding
        - Ambient occlusion adds a little bit extra detail
        - Indirect intensity to control overall lightness
        - Lightmap parameters (use high resolution if you need fine detailed shadows)
- Once the baking is done the directional light has no effect regarding shadows.
- Directional mode is good if you use normal maps or specularity.
    - It can take into accoount the baked lighting information.
- Larger items are good candidates for light baking. And lightprobe groups are better fit for small objects.
- Keep in mind the number of lightmaps.
    - Saves time at runtime but you'll need to load more textures.
- Lower the `Scale In Lightmap` (`Mesh Renderer/Lightmap Settings`) if you have objects that do not need that much shadow detail.


