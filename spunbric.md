## Main capabilities
- Sponge Plugins are to be loaded as fabric's `ModContainers`.
  - Mainly for allowing `isModLoaded(String)` checks
  - Also exposes fabric mods and MC as Plugins to Sponge API.
    
- It must be possible to represent fabric's `ModContainer` as a `PluginContainer`
  - CANNOT USE MIXIN SINCE FABRIC LOADER EXISTS TOO EARLY TO DO THAT.
    - Idea: A sort of `ModContainer.ViewFactory<T>` which is a `Function<api.ModContainer, T>`, registered in `api.FabricLoader`
      - To convert to a view, `ModContainer#as(Class<T>) -> T throws IllegalArgumentException`.
       - These views are prebaked and cached.
       - It's one solution to representing fabric mods that already exist as `ModContainers` as Sponge's `Plugin Container`

- Plugin Loading:
  - We would init sponge's launch system within `preLoad` state.
  - We need to parse the `mcmod.info` Sponge Plugins are expected to have. This is how we detect sponge plugins.
      - We need to bind a ModContainer to a plugin's instance (which is an object) if it is a sponge plugin, this is for Sponge's PluginContainer for the `getInstance() -> Optional<?>` call
      - If any fabric mods bundle a sponge plugin for compat or hooks, they are considered as part of the `fabric` mod's mod container, we just need to run our `ClassAnnotationVisitor` across the entire classpath to find those straggling classes with a `@Plugin` annotation.

## Possible capabilities
  - Future Capabilities (these may actually never be implemented, so these shouldn't be prioritized):
    - Allow loading of plugins with existing Mixin Configs:
      - Some sponge plugins can contain mixins. However we need to remap the jar using TinyRemapper and the mixin refmaps to handle that case.
        - Would mean we need a way to intercept and prevent the direct raw loading of sponge jars, preferably to use the remapped jar.
