Goals:
- Forge mods are remapped, patched, and loaded as full-class Fabric mods.
- Forge mods can be placed in the "mods" folder directly without any issues.
- Forge mods are not replaced, but instead the patched mods are kept in a cache somewhere else.
        
What we need:
- Two new stages that happen before mixins are applied: "discovery" and "addition" (todo better name for the latter)
  - discovery:
    - Happens after all valid Fabric mods are put on the classpath, but before non-fabric mods are added to the classpath
    - Patchwork specifically needs an option to exclude certain jars from being added to the classpath.
    - Called "discovery" because this is the stage both Patchwork and Spungbric will use to find the jar files they need to do something with.
  - addition:
    - Happens before mixins are applied, and after the discovery entrypoint.
    - Patchwork will patch and cache its Forge mods in this stage. (flexible) 
    - Patchwork will present jars (preferrably through a FileSystem) to be loaded by Loader. These mods will be loaded just like any other mod with all features available (mixins, AWs, etc.) except the discovery and addition stages

Additional things:
  - JimFS support is not completely neccisary but allows us to work with the system currently in Patcher. Apparently though it need some hacks to work accross classloaders, and it might not be possible (see: Jumploader)
