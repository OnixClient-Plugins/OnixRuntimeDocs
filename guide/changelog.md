# Changelog 6
[Previous Changelog](https://plugin.onixclient.com/docs/5/guide/changelog.html)

## Introduction
This changelog documents what changed in the sixth update of the Plugins API.<br>
Since this is very early-access and the API is still being developed, there will be a lot of changes and breaking changes.<br>
There will be a clear changelog for each update, so you can easily see what changed and what you need to do to adapt your plugin.<br>
This update is pretty much the whole package, you need to update the client first, that should take care of the new runtime.<br>
With this update, the PluginManager UI should update automatically, so you can just update your plugins from there.<br>

---

## Breaking Changes
- The [Raycast](xref:OnixRuntime.Api.World.WorldBlocks.Raycast*) function in [WorldBlocks](xref:OnixRuntime.Api.World.WorldBlocks) now takes [BlockShapeType](xref:OnixRuntime.Api.World.BlockShapeType) instead of the `isSolid`.
- The [BlockEntity](xref:OnixRuntime.Api.World.BlockEntity) class no longer has a `Block` property, use [WorldBlocks.GetBlock](xref:OnixRuntime.Api.World.WorldBlocks.GetBlock*) with [BlockEntity.Position](xref:OnixRuntime.Api.World.BlockEntity.Position) instead.
  - It wasn't accurate to the current state either before so that's better anyways.


## Runtime Changes
- There were no runtime changes this update.

<br>

### New Features
- Nothing that hasn't been mentionned in the breaking changes.

<br>


### Bug Fixes
- There was no bugs, so there was nothing to fix (real)
<br>



### Version Updates
- Now expects runtime version 6.



