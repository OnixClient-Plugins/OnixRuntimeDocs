# Changelog 7
[Previous Changelog](https://plugin.onixclient.com/docs/5/guide/changelog.html)

## Introduction
This changelog documents what changed in the seventh update of the Plugins API.<br>
Since this is very early-access and the API is still being developed, there will be a lot of changes and breaking changes.<br>
There will be a clear changelog for each update, so you can easily see what changed and what you need to do to adapt your plugin.<br>
This update is pretty much the whole package, you need to update the client first, that should take care of the new runtime.<br>
With this update, the PluginManager UI should update automatically, so you can just update your plugins from there.<br>

---

## Breaking Changes
- There are no breaking changes, just new features.

## Runtime Changes
- There were no runtime changes this update.

<br>

### New Features
- There are new [TransformationMatrix](xref:OnixRuntime.Api.Maths.TransformationMatrix) functions now.
  - [TransformationMatrix.Orthographic()](xref:OnixRuntime.Api.Maths.TransformationMatrix.Orthographic*) creates an orthographic projection matrix.
  - [TransformationMatrix.PerspectiveFov()](xref:OnixRuntime.Api.Maths.TransformationMatrix.PerspectiveFov*) creates a perspective projection matrix based on a field of view.
  - [TransformationMatrix.PerspectiveSize()](xref:OnixRuntime.Api.Maths.TransformationMatrix.PerspectiveSize*) same as Fov but gets the aspect ration from the provided width/height.
  - [TransformationMatrix.LookAt()](xref:OnixRuntime.Api.Maths.TransformationMatrix.LookAt*) creates a view matrix looking from one point to another.
  - [TransformationMatrix.Frustum()](xref:OnixRuntime.Api.Maths.TransformationMatrix.Frustum*) creates a perspective projection matrix based on the frustum parameters.
  - [TransformationMatrix.Invert()](xref:OnixRuntime.Api.Maths.TransformationMatrix.Invert*) creates an inverted version of the provided matrix.
  - [TransformationMatrix.Transpose()](xref:OnixRuntime.Api.Maths.TransformationMatrix.Transpose*) creates a transposed version of the provided matrix.
- You can now get the client version with [Onix.Client.Version](xref:OnixRuntime.Api.OnixClientThings.Version)!
- You can now open an [OnixModule](xref:OnixRuntime.Api.OnixClient.OnixModule)'s settings page with [OnixModule.OpenSettingsUi()](xref:OnixRuntime.Api.OnixClient.OnixModule.OpenSettingsUi*).
<br>


### Bug Fixes
- Fixed a bug where [IGameRenderer.RenderMesh(...)](xref:OnixRuntime.Api.Rendering.IGameRenderer.RenderMesh*) would not work on 1.21.111+.
- Fixed a crash that could occur when using [WorldBlocks.GetCollisions(...)](xref:OnixRuntime.Api.World.WorldBlocks.GetCollisions*).
- Fixed a bug where getting some [GameOptions](xref:OnixRuntime.Api.Options.GameOption) would always be null when it happened to happen too early.
<br>



### Version Updates
- Now expects runtime version 7.



