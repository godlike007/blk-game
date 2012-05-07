ASAP
================================================================================

* prime sound system on start
    * generated empty buffer/playback node?

Before Release
================================================================================

* new block textures
* new sounds
* new font
    * http://www.dafont.com/volter-goldfish.font

M3: Infinite Maps
================================================================================

* io
    * prioritize reads (distance from view? min when multiple users?)

* programmable views
    * packets for create/delete per player, attach to entity
    * waitForLoaded or something (before entering player into the world)

M4: UI
================================================================================

* click on block types to switch

* settings ui:
    * escape to toggle + ui button?
    * user name -> packet for name change
    * mouse sensitivity
    * view distance -> send packets to adjust view

* better error propagation
    * error types?

* client dialogs
    * connecting...
    * disconnected

M5: Performance Tuning
================================================================================

* performance
    * forEachInViewport / viewport.containsBoundingBox is slowest method!
    * rotateCube_
    * BuildQueue sort optimization
    * SegmentCache list remove is expensive

* networking
    * override format with serializer, use UNCOMPRESSED when running localhost

* io
    * defer writes a bit (no write each modify)
    * gm.io updates for NODE:
        * concurrent writes
            * on HTML use a queue (chained deferreds)
        * optimized reader resets (just update extents)

M6: Physics
================================================================================

* fix entity system/generalize

* god mode toggle (fly, infinite placement distance, etc)

* move set block to command actions

* entity eye point/height/etc
* collision detect down needs to sweep (not eye center, but entire AABB base)
* jump latch

* collision detection
    * actor/block
    * actor/actor
        * r-tree? http://stackulator.com/rtree/
        * kd-tree (low memory)
        * nested spheres? (fast movement)

* gravity
    * jump command action
    * sound on landing
    * sound on moving on ground

M7: Weapons
================================================================================

* mesh/meshbuilder/etc to replace facebuffer for entities
* use vao
* entity rewrite

* actions in movement
* latency compensation on server (rewind players/previous states/etc)

Experiments
================================================================================

* draw distance adjust
    * client must tell server
* store block counts per segment in chunk so can fast skip build queue work

* fog rewrite
    * environment density setting
    * color based on whether above or below ground?
    * only take xz into effect (no fog when looking up/down)

* evaluate marching cubes for terrain - could have flag on blocks that indicate
  'natural' vs. things like brick
    * http://paulbourke.net/geometry/polygonise/

* lighting:
    * really needs off-thread chunk prep
    * http://dave.uesp.net/wiki/Block_Land_12

* environment
    * skybox (star field/etc)
    * time (sun/moon positioning, fog changes, etc)
    * display sun/moon

* ChunkBuilder enhancements
* simple cave terrain:
    * http://blog.zelimirfedoran.com/archives/cave-terrain-generator
* city world
    * http://dev.bukkit.org/server-mods/cityworld/
    * needs fill/populate model
* some grammar for map generation?
    * http://www.nuke24.net/projects/TMCMG/

* track cache/etc on server, don't send chunks if haven't changed
    * list of chunks sent this session?
    * client cache via mapstore
    * if setBlock on uncached chunk, load chunk, set
        * chunk should queue setBlocks when unloaded, apply when LOADED?