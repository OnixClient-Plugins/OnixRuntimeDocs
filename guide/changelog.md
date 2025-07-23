# Changelog 3
[Previous Changelog](https://plugin.onixclient.com/docs/2/guide/changelog.html)

## Introduction
This changelog documents what changed in the third update of the Plugins API.<br>
Since this is very early-access and the API is still being developed, there will a lot of changes and breaking changes.<br>
There will be a clear changelog for each update, so you can easily see what changed and what you need to do to adapt your plugin.<br>
This update is pretty much the whole package, you need to update the client first, that should take care of the new runtime.<br>
```
.plugin install uuid 50b8338a-1cc4-44ca-81ef-5ecf1062c37c
```
That will install the latest version of the Plugin Manager UI.<br>

---

## Breaking Changes

### Rendering Changes
- `RendererWorld.EnableLights` has been removed.
  - You should now use [IGameRenderer.SetMaterialParameters](xref:OnixRuntime.Api.Rendering.IGameRenderer.SetMaterialParameters) with a material that has the [Light](xref:OnixRuntime.Api.Rendering.GameMaterialParameters.Light) property set to true.
  - You can also toggle just that with [IGameRenderer.SetMaterialParameter](xref:OnixRuntime.Api.Rendering.IGameRenderer.SetMaterialParameter) using [Light](xref:OnixRuntime.Api.Rendering.GameMaterialParameterType.Light) and true.
- `RendererWorld.PushWorldRenderSettings` has been removed.
  - You should now use [IGameRenderer.SetMaterialParameters](xref:OnixRuntime.Api.Rendering.IGameRenderer.SetMaterialParameters) with a [Material](xref:OnixRuntime.Api.Rendering.GameMaterialParameters) containing the settings you're looking for.
  - You can also toggle a specific setting with [IGameRenderer.SetMaterialParameter](xref:OnixRuntime.Api.Rendering.IGameRenderer.SetMaterialParameter) specifying which [parameter](xref:OnixRuntime.Api.Rendering.GameMaterialParameterType) to change.
  - UI simulation mode can be changed using [RendererWorld.IsSimulatingUi](xref:OnixRuntime.Api.Rendering.RendererWorld.IsSimulatingUi) but as before, you shouldn't touch that yourself.

<br>

### Settings Changes
- The gfx in [OnixSettingRenderer](xref:OnixRuntime.Api.OnixClient.OnixSettingRenderer)'s [Render](xref:OnixRuntime.Api.OnixClient.OnixSettingRenderer.Render) function will have its Width/Height/Size match the size parameter.

<br>

### Block Changes
- `Block.States` has been renamed to [Block.RawStates](xref:OnixRuntime.Api.World.Block.RawStates).
  - You should likely avoid using that in 99.99% of cases as the information isn't necessarily accurate on all versions.
  - I would trust nothing but the [Name](xref:OnixRuntime.Api.World.BlockState.Name) and the [StateId](xref:OnixRuntime.Api.World.BlockState.StateId).
- `Block.GetState` has been renamed to [Block.GetStateValue](xref:OnixRuntime.Api.World.Block.GetStateValue).
  - It has the same behavior as before, you give it a name or id and it returns the value for that state.

## Runtime Changes

### Material System
- [Materials](xref:OnixRuntime.Api.Rendering.GameMaterialParameters) have been introduced.
  - This lets you set specific parameters for rendering, such as light, inputs like gradient or uv, if textures are linear, culling modes.
  - Those are fine-tuned materials that should respect most of the combinations of parameters.
  - Some parameters kill or break some behavior, but if you wish to suggest a fix by all means send us the material's JSON.
  - You can set individual parameters using [IGameRenderer.SetMaterialParameter](xref:OnixRuntime.Api.Rendering.IGameRenderer.SetMaterialParameter) or set a whole material using [IGameRenderer.SetMaterialParameters](xref:OnixRuntime.Api.Rendering.IGameRenderer.SetMaterialParameters).
  - There are getter functions for the parameters, such as [IGameRenderer.GetMaterialParameter](xref:OnixRuntime.Api.Rendering.IGameRenderer.GetMaterialParameter) and [IGameRenderer.GetMaterialParameters](xref:OnixRuntime.Api.Rendering.IGameRenderer.GetMaterialParameters).
  - Note that the individual parameter setter returns the previous value, so you can restore it if you're doing a one off.
  - You can also set any material from the game or a pack using [IGameRenderer.SetMaterialCustom](xref:OnixRuntime.Api.Rendering.IGameRenderer.SetMaterialCustom) specifying its name and if it is in the common.json or fancy/sad.json.
  - [RendererCommon.SetDefaultState](xref:OnixRuntime.Api.Rendering.RendererCommon.SetDefaultState) will bind the default material as well as all default settings if you ever need it.
  - The material system is available in [IGameRenderer](xref:OnixRuntime.Api.Rendering.IGameRenderer) so you get the advantages of materials for UI too.
    - Note that for UI, some parameters are not supported like light, and some should always be enabled.
    - You can use [IGameRenderer.GetMaterialParameters](xref:OnixRuntime.Api.Rendering.IGameRenderer.GetMaterialParameters) to see the default parameters.

<br>

### New Features
- [BlockGraphics](xref:OnixRuntime.Api.Rendering.BlockGraphics) has been added, giving you information like block shape and texture.
  - You can access it using the [Block.Graphics](xref:OnixRuntime.Api.World.Block.Graphics) property.
- Added `With` methods for most math types.
  - [Vec3.WithX](xref:OnixRuntime.Api.Maths.Vec3.WithX) - [Vec3.WithY](xref:OnixRuntime.Api.Maths.Vec3.WithY) - [Vec3.WithZ](xref:OnixRuntime.Api.Maths.Vec3.WithZ)
  - [Vec2.WithX](xref:OnixRuntime.Api.Maths.Vec2.WithX) - [Vec2.WithY](xref:OnixRuntime.Api.Maths.Vec2.WithY)
  - [BlockPos.WithX](xref:OnixRuntime.Api.World.BlockPos.WithX) - [BlockPos.WithY](xref:OnixRuntime.Api.World.BlockPos.WithY) - [BlockPos.WithZ](xref:OnixRuntime.Api.World.BlockPos.WithZ)
  - [Vec2i.WithX](xref:OnixRuntime.Api.Maths.Vec2i.WithX) - [Vec2i.WithY](xref:OnixRuntime.Api.Maths.Vec2i.WithY)
  - [Rect.WithX](xref:OnixRuntime.Api.Maths.Rect.WithX) - [Rect.WithY](xref:OnixRuntime.Api.Maths.Rect.WithY) - [Rect.WithZ](xref:OnixRuntime.Api.Maths.Rect.WithZ) - [Rect.WithW](xref:OnixRuntime.Api.Maths.Rect.WithW)
  - [Rect.WithWidth](xref:OnixRuntime.Api.Maths.Rect.WithWidth) - [Rect.WithHeight](xref:OnixRuntime.Api.Maths.Rect.WithHeight) - [Rect.WithSize](xref:OnixRuntime.Api.Maths.Rect.WithSize)
- [MightOwnMemoryAddressContainer](xref:OnixRuntime.Api.Internal.MightOwnMemoryAddressContainer) and [MemoryAddressContainer](xref:OnixRuntime.Api.Internal.MemoryAddressContainer) now have an `operator==` and a `GetHashCode()`.
- [ItemStack](xref:OnixRuntime.Api.Items.ItemStack) now has an [HasEnchantOverlay](xref:OnixRuntime.Api.Items.ItemStack.HasEnchantOverlay) property.

<br>

### Bug Fixes
- Fixed [WorldChunk](OnixRuntime.Api.World.WorldChunk)'s [LoadState](xref:OnixRuntime.Api.World.WorldChunk.LoadState) property always being [Unloaded](xref:OnixRuntime.Api.World.WorldChunk.ChunkState.Unloaded).
- Fixed [Onix.Game.InvokeUri](xref:OnixRuntime.Api.OnixGame.InvokeUri) forcing you to be in a world and throwing an exception.
- Fixed [Dimension](xref:OnixRuntime.Api.World.Dimension)'s [Id](xref:OnixRuntime.Api.World.Dimension.Id) property always being [Overworld](xref:OnixRuntime.Api.World.DimensionType.Overworld).
- Fixed [WorldChunk.GetBlock](xref:OnixRuntime.Api.World.WorldChunk.GetBlock) always returning null.
- Fixed [WorldChunk.GetRainHeightAt](xref:OnixRuntime.Api.World.WorldChunk.GetRainHeightAt) always returning 0.
- Fixed [WorldChunk.GetHeightAt](xref:OnixRuntime.Api.World.WorldChunk.GetHeightAt) always returning 0.
- Fixed [BlockRegistry.GetBlock](xref:OnixRuntime.Api.World.BlockRegistry.GetBlock) never finding blocks on 1.12.
- Fixed [InsufficientTrustException](xref:OnixRuntime.Api.Errors.InsufficientTrustException) not being thrown/reported and silently failing.
- Fixed [SettingChangedDelegate](xref:OnixRuntime.Api.OnixClient.OnixSetting.SettingChangedDelegate) getting garbage collected and crashing the dotnet runtime when invoked.
- Fixed [MemoryHelpers.ScanSignature](xref:OnixRuntime.Api.Utils.MemoryHelpers.ScanSignature) now returns the correct address of the signature it finds.

## Client Changes

### Version Updates
- Now expects runtime version 3.

<br>

### Bug Fixes
- Notifications happening in weird orders should no longer freeze the game.
- Input events are no longer duplicated.
- Fixed changing skins from the client on 1.21.20 to 1.21.60.
