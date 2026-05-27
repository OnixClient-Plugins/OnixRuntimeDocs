# Changelog 8
[Previous Changelog](https://plugin.onixclient.com/docs/7/guide/changelog.html)

## Introduction
This changelog documents what changed in the eith update of the Plugins API.<br>
Since this is very early-access and the API is still being developed, there will be a lot of changes and breaking changes.<br>
There will be a clear changelog for each update, so you can easily see what changed and what you need to do to adapt your plugin.<br>
This update is pretty much the whole package, you need to update the client first, that should take care of the new runtime.<br>
With this update, the PluginManager UI should update automatically, so you can just update your plugins from there.<br>

---

## Breaking Changes
- dotnet10 support! You will need to set your projects's .net version to 10.0 and update the language version to 14 for the new runtime.
- Block.States are now [Block.RawStates](xref:OnixRuntime.Api.World.Block.RawStates) and Block.GetState is now replaced with [Block.GetStateValue](xref:OnixRuntime.Api.World.Block.GetStateValue*)

## Runtime Changes
- Server commands and soft enumeration values autocompletion is sent to other clients with plugins enabled.

<br>

### New Features
- You can now control/override the player's text shadow setting in [RendererCommon.FontShadowType](xref:OnixRuntime.Api.Rendering.RendererCommon.FontShadowType) with the options from [FontShadowType](xref:OnixRuntime.Api.Rendering.FontShadowType).
- [Onix.DebugDraw](xref:OnixRuntime.Api.Rendering.OnixDebugDraw) has been added! You can now draw various shapes in the world from any thread for quick debugging only purposes.
- You can now access module aliases with [OnixModule.Aliases](xref:OnixRuntime.Api.OnixClient.OnixModule.Aliases). Which is useful when making your own UI and just brings new client additions to C#.
- Similarly, you can now access [OnixModule.IsNew](xref:OnixRuntime.Api.OnixClient.OnixModule.IsNew) to check if a module is considered new or not. This shows up in the UI as the NEW bubble.
- Added the [SmallBubble](xref:OnixRuntime.Api.OnixClient.OnixClientThemeV3.SmallBubble*) color to [OnixClientThemeV3](xref:OnixRuntime.Api.OnixClient.OnixClientThemeV3) for the background of that NEW bubble and other things in the future.
- Added [ItemStack.GetIcon](xref:OnixRuntime.Api.Items.ItemStack.GetIcon*) which gets the item's texture coordinates in the item atlas texture.
- Added [PackManager.LoadContentString](xref:OnixRuntime.Api.ResourcePacks.PackManager.LoadContentString*) and [PackManager.LoadContentOrNullString](xref:OnixRuntime.Api.ResourcePacks.PackManager.LoadContentOrNullString*) functions to get the content as a UTF-8 string.
  - Also added them in [PackAssetLoader.GetContentString](xref:OnixRuntime.Api.ResourcePacks.PackAssetLoader.GetContentString*) and [PackAssetLoader.GetContentOrNullString](xref:OnixRuntime.Api.ResourcePacks.PackAssetLoader.GetContentOrNullString*).
- [OnixSettingReorderableEnum](xref:OnixRuntime.Api.OnixClient.Settings.OnixSettingReorderableEnum) has been added, it lets the user enable/disable entries and reorganize them in any order. It's very versatile and customizable.
- [Onix.Paths](xref:OnixRuntime.Api.OnixClientPaths) is now available, allowing you to easily get to various game paths that GDK made annoying to get. It's good for UWP, GDK and the changes that happened in GDK for these.
- The [RectI](xref:OnixRuntime.Api.Maths.RectI) struct has been added to allow integer based rectangles which were useful for new features.
- [BlockPos](xref:OnixRuntime.Api.Maths.BlockPos) now has an [Offset](xref:OnixRuntime.Api.Maths.BlockPos.Offset*) function taking a [BlockFace](xref:OnixRuntime.Api.World.BlockFace) to offset towards any direction easily.
- Added matrix handness functions to allow making left/right handed matrices with [TransformationMatrix](xref:OnixRuntime.Api.Maths.TransformationMatrix)
- Added [TransformationMatrixAura](xref:OnixRuntime.Api.Maths.TransformationMatrixAura) which is a 4x4 matrix struct taking a regular matrix. The normal one includes a direct 2d matrix and is not suitable for Direct X buffers as a result.à
- All math types have their WithX/Y/Z functions now.
- Plugin Message Connection! You can now as the host send arbitrary data to another client running plugins.
  - This is probably going to be broken in a few ways at first, but it should get there if someone wants to use it.
  - You can receive messages from just your own plugin with matching version by default or interop with other plugins/older versions.
  - The client events (client side receiving from the server) are in [Onix.Events.Session.PluginMessageConnection](xref:OnixRuntime.Api.Events.OnixEventsSession.PluginMessageConnection)
  - The server events (client sending message to the server) are in [Onix.Events.LocalServer.PluginMessageConnection](xref:OnixRuntime.Api.Events.LocalServer.OnixEventLocalServer.PluginMessageConnection)
  - You can send a message to the server with [Onix.Client.SendPluginMessage()](xref:OnixRuntime.Api.OnixClientThings.SendPluginMessage*) when [Onix.Client.PluginConnectionEstablished](xref:OnixRuntime.Api.OnixClientThings.PluginConnectionEstablished) is true.
  - You can send a message to a client/player with [ServerPlayer.SendPluginMessage()](xref:OnixRuntime.Api.Entities.ServerPlayer.SendPluginMessage*) when [ServerPlayer.PluginConnectionEstablished](xref:OnixRuntime.Api.Entities.ServerPlayer.PluginConnectionEstablished)
  - Bytes or string, it's up to you! But you must know what you're expecting to receive on the other end.
- Added [LocalPlayer.OpenInventory](xref:OnixRuntime.Api.Entities.LocalPlayer.OpenInventory) which will open the inventory eventually. Don't spam it though, with server ping it could be some time before the screen updates/opens.
- Added Modal Forms! The raw events are still there, you have a generic event and you also have typed events for each form type.
  - Generic:
    - Receive and Response Events: [Onix.Events.Session.ModalForm.AnyRequest](xref:OnixRuntime.Api.Events.Session.OnixEventSessionModalForms.AnyRequest) and [Onix.Events.Session.ModalForm.AnyResponse](xref:OnixRuntime.Api.Events.Session.OnixEventSessionModalForms.AnyResponse)
    - Parsed Class and Replier: [ModalFormAny](xref:OnixRuntime.Api.Events.Session.ModalFormAny) and [ModalFormReplierAny](xref:OnixRuntime.Api.Events.Session.ModalFormReplierAny)
  - Modal:
    - Receive and Response Events: [Onix.Events.Session.ModalForm.ModalRequest](xref:OnixRuntime.Api.Events.Session.OnixEventSessionModalForms.ModalRequest) and [Onix.Events.Session.ModalForm.ModalResponse](xref:OnixRuntime.Api.Events.Session.OnixEventSessionModalForms.ModalResponse)
    - Parsed Class and Replier: [ModalFormModal](xref:OnixRuntime.Api.Events.Session.ModalFormModal) and [ModalFormModalReplier](xref:OnixRuntime.Api.Events.Session.ModalFormModalReplier)
  - Form:
    - Receive and Response Events: [Onix.Events.Session.ModalForm.FormRequest](xref:OnixRuntime.Api.Events.Session.OnixEventSessionModalForms.FormRequest) and [Onix.Events.Session.ModalForm.FormResponse](xref:OnixRuntime.Api.Events.Session.OnixEventSessionModalForms.FormResponse)
    - Parsed Class and Replier: [ModalFormForm](xref:OnixRuntime.Api.Events.Session.ModalFormForm) and [ModalFormFormReplier](xref:OnixRuntime.Api.Events.Session.ModalFormFormReplier)
  - CustomForm:
    - Receive and Response Events: [Onix.Events.Session.ModalForm.CustomRequest](xref:OnixRuntime.Api.Events.Session.OnixEventSessionModalForms.CustomRequest) and [Onix.Events.Session.ModalForm.CustomResponse](xref:OnixRuntime.Api.Events.Session.OnixEventSessionModalForms.CustomResponse)
    - Parsed Class and Replier: [ModalFormCustom](xref:OnixRuntime.Api.Events.Session.ModalFormCustom) and [ModalFormCustomReplier](xref:OnixRuntime.Api.Events.Session.ModalFormCustomReplier)
  - Let me know if there is interest into receiving/sending them to local server players.
  - As it is, it allows editing the forms and making your own ones. Would just need a player.SendModalForm() and an event for the response.
- Added the [Scheduler](xref:OnixRuntime.Api.Events.OnixEventScheduler)! This is the best thing ever and will save you decades and a lot of ugly code. It lets you interact with the game in an async context.
  - Check it out, it might just be the second best thing from this release. Note: you can schedule events even when your plugin isn't enabled so be careful what you do without an onEnabled call.
- Added AURA. Go crazy! Aura is a DirectX wrapper that allows you to use the GPU in a low level way on all supported game backends.
- You will need these examples at https://github.com/OnixClient-Plugins/AuraExamplePlugin
- Some DirectX 11 and above experience would be beneficial.
  <br>


### Bug Fixes
- SetHardcore now refreshes the heart containers for the local player instead of needing a relog.
- Fix an issue with NBT from the client to C# sometimes crashing. That's with any NBT ever, yet it was rare for it to trigger.
- Fix WorldBlocks.CreateForServer creating a client sided version.
- RaycastResult.Clone now doesn't require a logic thread and can be called from any thread. Allowing raycasts to happen on background threads.
- Fix requiring no .dll extensions for external assemblies. Now you can include .dll in your [DllImport] calls. (Or more likely the library you're importing can.)
  - Also now releasing references to imported native DLLs when a plugin is unloaded.
- Fix [PackAssetLoader.GetPathList()](xref:OnixRuntime.Api.ResourcePacks.PackAssetLoader.GetPathList*) crashing.
- Fix [PackAssetLoader.GetPathList()](xref:OnixRuntime.Api.ResourcePacks.PackAssetLoader.GetPathList*) sometimes returning the input path which would lead you to a stack overflow when recursively iterating.
- Fix [WorldChunk.GetBlock](xref:OnixRuntime.Api.World.WorldChunk.GetBlock*) crashing.
- Fix [WorldChunk.LoadState](xref:OnixRuntime.Api.World.WorldChunk.LoadState) not being the right value.
- Fix [Dimension.Id](xref:OnixRuntime.Api.World.Dimension.Id) always being overworld.
- Fix a bug in the runtime loader that caused the runtime to still fail to load after an update.
  <br>



### Version Updates
- Now expects runtime version 8.



