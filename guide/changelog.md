# Changelog 2
[You can't find the previous changelog here](https://plugin.onixclient.com/docs/1/guide/changelog.html).

## Introduction
This changelog documents what changed in the second update of the Plugins API.<br>
Since this is very early-access and the API is still being developed, there will a lot of changes and breaking changes.<br>
There will be a clear changelog for each update, so you can easily see what changed and what you need to do to adapt your plugin.<br>
This update is pretty much the whole package, you need to update the client first, that should take care of the new runtime.<br>
Update the UI from the chat AFTER the client and runtime since if you update it before updating the client you will likely crash as there was a bug with unloaded plugins.
```
.plugin install uuid 50b8338a-1cc4-44ca-81ef-5ecf1062c37c
```
That will install the latest version of the Plugin Manager UI.<br>

---

## Breaking Changes
- [Onix.Events.Common](xref:OnixRuntime.Api.Events.OnixEvents.Common)
  - [HudRender](xref:OnixRuntime.Api.Events.OnixEventCommon.HudRender) was renamed into [HudRenderGame](xref:OnixRuntime.Api.Events.OnixEventCommon.HudRenderGame)
  - [HudRender](xref:OnixRuntime.Api.Events.OnixEventCommon.HudRender) is now called with a [RendererCommon2D](xref:OnixRuntime.Api.Rendering.RendererCommon2D) which will use the user's preferred renderer.
- [Onix.Events.Rendering](xref:OnixRuntime.Api.Events.OnixEvents.Rendering)
  - [HudRender](xref:OnixRuntime.Api.Events.OnixEventRendering.HudRender) was renamed into [HudRenderGame](xref:OnixRuntime.Api.Events.OnixEventRendering.HudRenderGame)
  - [HudRender](xref:OnixRuntime.Api.Events.OnixEventRendering.HudRender) is now called with a [RendererCommon2D](xref:OnixRuntime.Api.Rendering.RendererCommon2D) which will use the user's preferred renderer.
  - [RenderScreen](xref:OnixRuntime.Api.Events.OnixEventRendering.RenderScreen) was renamed into [RenderScreenGame](xref:OnixRuntime.Api.Events.OnixEventRendering.RenderScreenGame)
  - [RenderScreen](xref:OnixRuntime.Api.Events.OnixEventRendering.RenderScreen) is now called with a [RendererCommon2D](xref:OnixRuntime.Api.Rendering.RendererCommon2D) which will use the user's preferred renderer.
  - `PreRenderScreen` was renamed into [PreRenderScreenGame](xref:OnixRuntime.Api.Events.OnixEventRendering.PreRenderScreenGame)
  - There is no longer a `PreRenderScreen` event, since it means different things depending on which renderer is used.
- [OnixSettingCategory](xref:OnixRuntime.Api.OnixClient.Settings.OnixSettingCategory)'s `IncludesEverytingUntilNextCategory` typo has been fixed [IncludesEverythingUntilNextCategory](xref:OnixRuntime.Api.OnixClient.Settings.OnixSettingCategory.IncludesEverythingUntilNextCategory)
- [SettingChangedDelegate](xref:OnixRuntime.Api.OnixClient.OnixSetting.SettingChangedDelegate) now gives the [OnixModule](xref:OnixRuntime.Api.OnixClient.OnixModule) as nullable since not every setting has a parent module.
- [operator==](xref:OnixRuntime.Api.OnixClient.OnixModule.op_Equality) on [OnixModule](xref:OnixRuntime.Api.OnixClient.OnixModule) is now more extensive checks related to nullability. This shouldn't break much but since it could, it's on this list.
- `RendererTwoDimentional` was renamed to [RendererCommon2D](xref:OnixRuntime.Api.Rendering.RendererCommon2D) to better reflect its purpose and be consistent with [RendererCommon](xref:OnixRuntime.Api.Rendering.RendererCommon2D).
- The `Onix.Client.SetTooltipText` taking a Vec2 has been removed as it would not work unless you matched the client's mouse position, which is not possible in plugins. 
  - It has been replaced with a [SetTooltipText(Rect)](xref:OnixRuntime.Api.OnixClientThings.SetTooltipText) overload.
  - That convenience overload checks if the mouse is inside the rectangle before the tooltip is set.

## Runtime Changes
- The runtime now has [OnixRendererType](xref:OnixRuntime.Api.Rendering.OnixRendererType) to determine which renderer the user chose to use.
  - That's in [Onix.Render.MainRenderer](xref:OnixRuntime.Api.Rendering.RenderContexes.MainRenderer) and it's not too useful to you, but we use it to determine which renderer to use for the new generic render events.
- [OnixModuleVisual](xref:OnixRuntime.Api.OnixClient.OnixModuleVisual) now has a [Render](xref:OnixRuntime.Api.OnixClient.OnixModuleVisual.Render) event you can subscribe to.
  - This is useful when rendering hud elements as just rendering where the module is located was very bug-prone and was bound to have edge-cases.
  - This even works with the preview in the client's UI.
- [OnixModuleVisual](xref:OnixRuntime.Api.OnixClient.OnixModuleVisual) now has a [Area](xref:OnixRuntime.Api.OnixClient.OnixModuleVisual.Area) property that returns a [Rect](xref:OnixRuntime.Api.Maths.Rect) representing the module's position and size.
  - Because dealing with a rectangle is so much more convenient than dealing with a position and size separately.
- All the [VertexBatch](xref:OnixRuntime.Api.Rendering.GameMeshBuilder.VertexBatch) variants taking a `List<VertexType>` no longer causes an exception.
- The `.plugin` command now has a `ui` subcommand to open the Plugin Manager UI.
  - You can also specify a plugin UUID to open a specific plugin's settings.
  - `.plugin ui [uuid]`
- Fixed a freeze that occurred when loading an incompatible plugin.
- Fixed all the bounding box rendering functions the incorrect front/back faces.
- Added BoundingBox rendering functions that take a normal but no UVs.
- The [OnixSetting.SettingChanged](xref:OnixRuntime.Api.OnixClient.OnixSetting.SettingChangedDelegate) callbacks no longer hard crash the runtime when invoked.
- The [OnixModules](xref:OnixRuntime.Api.OnixClient.OnixModule) that have been created by a plugin now have an `Open in Plugin Manager` button.


## Client Changes
- Now expects runtime version 2.
- Fixed a freeze when too many notifications were sent from another thread than the main thread.
- Modules opened from the hud editor now re-open correctly when they disappear for a brief moment.
- Added a `Never Trusted` option in `Developer Settings`, which is enabled by default.
  - This will help you remember to check for trust when you create plugins, so you don't forget to do it later.
  - Since most testing happens in singleplayer where you are trusted, you might never realize you need to check for trust on some functions.
  - I would recommend having this on and testing with on before you release your plugin.
---

## Plugin Manager Changes
- Uninstalling a local plugin no longer causes multiple exceptions.
- Implemented the ability to open a plugin by its UUID.
- Incompatible plugins will now have a warning displayed on their card.
  - Details view still has nothing as it is unfinished at this time.
---

## Plugin API Changes
- Hopefully fixed the infinite update task.
- Allowed the browser to cache some docs content.
  - That fixes the light mode blinding you for a split second on every single page load.
  - It also speeds up the loading of the docs for the next 30 minutes after visiting that page.
- There is an endpoint to update the documentation from the [Documentation Website Repository](https://github.com/OnixClient-Plugins/OnixRuntimeDocs)
---

## Plugin Generator Changes
- Fixed the OnHudInput event not returning false.
- Fixed the OnHudInput not using the OnixRuntime.Api.Inputs namespace.
- Fixed the OnChatMessage event not returning false.
- Fixed the OnChatMessage not importing the OnixRuntime.Api.UI namespace.
- Added the `.editorconfig` file to the generated project.
- Renamed the `Hudender` event to `HudRenderGame` to better reflect its purpose and make it consistent with `HudRenderDirect2D`.
- Added the new `HudRender` event to the plugin generator.
  - This one is a generic event that uses the user's preferred renderer.
  - You should prefer this for rendering unless you have a good reason to specialize.
- Added an OS check for the `rm`/`del` commands to get rid of two warnings when building.
- Ignored the return value of the asset copy step in the plugin generator.
  - This is to prevent the build from failing when the asset copy fails, which is not critical for the plugin to work.
- Added a `README.md` file to the generated project.
