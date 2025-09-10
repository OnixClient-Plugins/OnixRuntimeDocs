# Changelog 5
[Previous Changelog](https://plugin.onixclient.com/docs/4/guide/changelog.html)

## Introduction
This changelog documents what changed in the fifth update of the Plugins API.<br>
Since this is very early-access and the API is still being developed, there will be a lot of changes and breaking changes.<br>
There will be a clear changelog for each update, so you can easily see what changed and what you need to do to adapt your plugin.<br>
This update is pretty much the whole package, you need to update the client first, that should take care of the new runtime.<br>
With this update, the PluginManager UI should update automatically, so you can just update your plugins from there.<br>

---

## Breaking Changes
Nothing that won't be fixed by recompiling your plugin.


## Runtime Changes
- The plugin manager UI will not always reinstall itself over and over again.

<br>

### New Features
- You can render blocks.
  - This is a single small entry but is huge and required a lot of work so please enjoy it.
  - You can find example in the documentation for [RenderBlock()](xref:OnixRuntime.Api.Rendering.GameBlockRenderer.RenderBlock(OnixRuntime.Api.Rendering.GameMeshBuilder,OnixRuntime.Api.Maths.BlockPos,OnixRuntime.Api.World.Block)).
- There is now a system for commands (supports both the client side and the local server side.)
  - This is also a very big feature that took forever to implement, so please enjoy it.
  - You can find the guide for commands at [Command Guide](./creating-commands.md).
- [Onix.Game.ExecuteCommand](xref:OnixRuntime.Api.OnixGame.ExecuteCommand*) now returns an int representing the command request ID.
- With this there is also the [Onix.Events.Session.Chat.CommandResponse](xref:OnixRuntime.Api.Events.Session.OnixEventSessionChat.CommandResponse) for getting the response of a command.
  - Note: You can also cancel it at this higher level, not sure if it would help when spamming commands for lag but you can cancel it here.
  - Note: Not every server honors the command response, so if you don't get the responses you expect, it might be the server.
  - This will work on vanilla servers where command feedback is enabled.

<br>

- [ObjectTag](xref:OnixRuntime.Api.NBT.ObjectTag) now has a [MergeWith](xref:OnixRuntime.Api.NBT.ObjectTag.MergeWith*).
- [WorldBlocks](xref:OnixRuntime.Api.World.WorldBlocks) now has a [GetExtraBlock](xref:OnixRuntime.Api.World.WorldBlocks.GetExtraBlock*) method.
  - The extra block (layer 1) is used for things like snow layers that are on top of vegetation. Or waterlogged blocks.
- Added [Angles.WithYaw](xref:OnixRuntime.Api.Maths.Angles.WithYaw*) and [Angles.WithPitch](xref:OnixRuntime.Api.Maths.Angles.WithPitch*), letting you keep one angle and change the other.
- Added [Angles3.WithYaw](xref:OnixRuntime.Api.Maths.Angles3.WithYaw*), [Angles3.WithPitch](xref:OnixRuntime.Api.Maths.Angles3.WithPitch*) and [Angles3.WithPitch](xref:OnixRuntime.Api.Maths.Angles3.WithPitch*), letting you keep one angle and change the other.
- Added [Vec3.Floor()](xref:OnixRuntime.Api.Maths.Vec3.Floor) and [Vec3.Ceil()](xref:OnixRuntime.Api.Maths.Vec3.Ceil) and [Vec3.Round()](xref:OnixRuntime.Api.Maths.Vec3.Round) methods.
- Added [Vec3.DistanceSqr()](xref:OnixRuntime.Api.Maths.Vec3.DistanceSqr*).
- Added [Vec2.Floor()](xref:OnixRuntime.Api.Maths.Vec2.Floor) and [Vec2.Ceil()](xref:OnixRuntime.Api.Maths.Vec2.Ceil) and [Vec2.Round()](xref:OnixRuntime.Api.Maths.Vec2.Round) methods.
- Added [Vec2.DistanceSqr()](xref:OnixRuntime.Api.Maths.Vec2.DistanceSqr*).
- Added [Entity.IsAlive](xref:OnixRuntime.Api.Entities.Entity.IsAlive).
- Added [Logger.GetForPlugin()](xref:OnixRuntime.Api.Logger.GetForPlugin) which lets you get a logger for your plugin easier.
- Added [GameViewport](xref:OnixRuntime.Api.Rendering.GameViewport) in `Onix.Render.GameViewport` which gives you information about the current viewport, or lets you be naughty.
<br>

### Bug Fixes
- Notifications sent from a plugin without a callback won't crash when clicked anymore.
- The [ItemStack.GetHoverText](xref:OnixRuntime.Api.Items.ItemStack.GetHoverText*) method no longer crashes on recent versions.
- Players will not show up twice in the list of all entities.
<br>



### Version Updates
- Now expects runtime version 5.



