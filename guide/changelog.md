# Changelog 4
[Previous Changelog](https://plugin.onixclient.com/docs/3/guide/changelog.html)

## Introduction
This changelog documents what changed in the fourth update of the Plugins API.<br>
Since this is very early-access and the API is still being developed, there will be a lot of changes and breaking changes.<br>
There will be a clear changelog for each update, so you can easily see what changed and what you need to do to adapt your plugin.<br>
This update is pretty much the whole package, you need to update the client first, that should take care of the new runtime.<br>
With this update, the PluginManager UI should update automatically, so you can just update your plugins from there.<br>

---

## Breaking Changes
- [ContainerScreen](xref:OnixRuntime.Api.UI.ContainerScreen)'s [GetItem](xref:OnixRuntime.Api.UI.ContainerScreen.GetItem*) method will now return a null item stack on failure instead of a disposed one.
- [ContainerScreen](xref:OnixRuntime.Api.UI.ContainerScreen)'s `HoveringItem` property was renamed to [IsHoveringItem](xref:OnixRuntime.Api.UI.ContainerScreen.IsHoveringItem) for clarity.
- The event `Onix.Events.Input.OnPlayerInputTick` was renamed to [Onix.Events.Input.PlayerInputTick](xref:OnixRuntime.Api.Events.OnixEventInputs.PlayerInputTick) for the IDE to not generate `OnOnPlayerInputTick`.
- [GameInputHandler](xref:OnixRuntime.Api.Inputs.GameInputHandler)'s `IsAAllDown` was renamed to [IsAllDown](xref:OnixRuntime.Api.Inputs.GameInputHandler.IsAllDown*) as it clearly contained a typo.
- The constructor of [OnixModuleSettingRedirector](xref:OnixRuntime.Api.OnixClient.OnixModuleSettingRedirector) taking an [OnixModule](xref:OnixRuntime.Api.OnixClient.OnixModule) now also takes a bool.
  - This breaks every single plugin with a config.
  - To fix it, simply add `, true` in your [OnLoaded](xref:OnixRuntime.Plugin.OnixPluginBase.OnLoaded) method where you create your [OnixModuleSettingRedirector](xref:OnixRuntime.Api.OnixClient.OnixModuleSettingRedirector).
  - It should look something like this: `Config = new(PluginDisplayModule, true);`
  - The bool is for whether you want missing settings to be added to your module automatically.
  - This is important because if you start wrapping base Onix Client modules in a class like that, you don't want a deleted setting to be re-added automatically linking to nothing, as this will cause serious problems.
- [TransformationMatrix](xref:OnixRuntime.Api.Maths.TransformationMatrix) fields are now public. (And for the sillyheads that were using reflection, the field 11 now exists.)
- The [Logger](xref:OnixRuntime.Api.Logger) constructor no longer takes a [Logger.Level](xref:OnixRuntime.Api.Logger.Level). It is now entirely decided by the `runtime/config.json`.
  - We still have a `.plugin loglevel` to change the chat and log file levels.
- `OnixModuleList` was renamed to [OnixModulesList](xref:OnixRuntime.Api.OnixClient.OnixModulesList) for consistency.


## Runtime Changes
- Outdated/incompatible plugins will not try to load over and over again, they will be skipped the next time.
- A plugin can now mark itself as a plugin manager UI. This will be used by the UI someday if you want to select a different UI to use, not letting you select ones that aren't UIs.
- You can now specify multiple plugin repository sources in the `runtime/config.json`.
  - The default one is always there but you can add more.
  - This can be useful for early access plugins or private plugins (still have the user provide any API secrets as someone can just leak or stumble upon that repo URL).
  - For now you can change them in the `runtime/config.json` or from C# using [RemoteSourceUrls](xref:OnixRuntime.Plugin.OnixPluginInstaller.RemoteSourceUrls) and [SaveRemoteSourceUrls()](xref:OnixRuntime.Plugin.OnixPluginInstaller.SaveRemoteSourceUrls) from the [OnixPluginInstaller](xref:OnixRuntime.Plugin.OnixPluginInstaller).
- You can now select a different plugin manager UI from the `runtime/config.json`. The default one is still the Onix Client one.
  - This plugin is now treated differently too.
    - When it can't load because it's outdated or incompatible, it will try to find and install a compatible version to load instead. (From the plugin sources.)
    - When it's disabled or not marked as should load, it will load and enable it automatically.
    - It will be installed if not present. (From the plugin sources.)

<br>

### New Features
- [Item](xref:OnixRuntime.Api.Items.Item) now has a [DescriptionIdentifier](xref:OnixRuntime.Api.Items.Item.DescriptionIdentifier) property. This is used in the game's json and lang files.
- [Block](xref:OnixRuntime.Api.World.Block) now has a [DescriptionIdentifier](xref:OnixRuntime.Api.World.Block.DescriptionIdentifier) property. This is used in the game's json and lang files.
- [ContainerScreen](xref:OnixRuntime.Api.UI.ContainerScreen) now has [GetVisualItem](xref:OnixRuntime.Api.UI.ContainerScreen.GetVisualItem*) method which returns some items that [GetItem](xref:OnixRuntime.Api.UI.ContainerScreen.GetItem*) does not.
- [ItemStack](xref:OnixRuntime.Api.Items.ItemStack) now has a [GetHoverText](xref:OnixRuntime.Api.Items.ItemStack.GetHoverText*) method which returns the hover text of that item, before or after client modifications.
- [EnchantType](xref:OnixRuntime.Api.Items.EnchantType) now has a [DisplayName](xref:OnixRuntime.Api.Items.EnchantmentTypesExtensions.DisplayName*) extension which returns the translated name or name and level for that enchantment type.
- [InputMappingLayout](xref:OnixRuntime.Api.Inputs.InputMappingLayout) now has a [SetKeys](xref:OnixRuntime.Api.Inputs.InputMappingLayout.SetKeys*) method.
  - The properties that wrap the keys also now all have setters. The default keys are still read-only.
- Added [GameOptions](xref:OnixRuntime.Api.OnixGame.Options)! No need to parse options.txt anymore guys please. Here are a few useful links.
  - [GameOptionInputBool](xref:OnixRuntime.Api.Options.GameOptionInputBool) - Boolean game option for various [InputMode](xref:OnixRuntime.Api.Inputs.InputMode).
  - [GameOptionFloat](xref:OnixRuntime.Api.Options.GameOptionFloat) - Float game option.
  - [GameOptionInputFloat](xref:OnixRuntime.Api.Options.GameOptionInputFloat) - Float game option for various [InputMode](xref:OnixRuntime.Api.Inputs.InputMode).
  - [GameOptionInt](xref:OnixRuntime.Api.Options.GameOptionInt) - Integer game option.
  - [GameOptionString](xref:OnixRuntime.Api.Options.GameOptionString) - String/Textbox game option.
  - [GameOptionEnum](xref:OnixRuntime.Api.Options.GameOptionEnum) - Enum game option.
  - [GameOptionStringList](xref:OnixRuntime.Api.Options.GameOptionStringList) - List of string game option.
  - [GameOptionVec3](xref:OnixRuntime.Api.Options.GameOptionVec3) - Unused in the game's options.txt but I presume it may be used in things like the structure block internally?
  - [GameOptionInt64](xref:OnixRuntime.Api.Options.GameOptionInt64) - Unused in the game's options.txt, not sure where or what they use it for.
  - [GameOptionUInt64](xref:OnixRuntime.Api.Options.GameOptionUInt64) - Unused in the game's options.txt, not sure where or what they use it for.
- [RuntimeWorld](xref:OnixRuntime.Api.World.RuntimeWorld) now has [IsHardcore](xref:OnixRuntime.Api.World.RuntimeWorld.IsHardcore), which you can get AND set :D. 
  - Setting it on the client will require trust and probably not do much other than change the client-side UI (hearts).
  - Setting it on the server won't immediately do anything but after a relog it should apply.
- All Onix Client Settings and Modules have a `ToString()` method now.
    - This mostly helps in the debugger to see what you're working with at a glance.
- [BlockPos](xref:OnixRuntime.Api.Maths.BlockPos) now has a [FromString](xref:OnixRuntime.Api.Maths.BlockPos.FromString*) static method to parse a block position from a string.
  - I got annoyed when testing having to add , so I added this. `var pos = BlockPos.FromString("451 74 17");` is now a thing.
- A few of the [OnixModuleSettingRedirector](xref:OnixRuntime.Api.OnixClient.OnixModuleSettingRedirector)'s now have an `AfterSetting` bool you can set to have them take effect after the setting.
    - [CategoryAttribute](xref:OnixRuntime.Api.OnixClient.OnixModuleSettingRedirector.CategoryAttribute) has is.
    - [CategoryStopAttribute](xref:OnixRuntime.Api.OnixClient.OnixModuleSettingRedirector.CategoryStopAttribute) has is.
    - [GapAttribute](xref:OnixRuntime.Api.OnixClient.OnixModuleSettingRedirector.GapAttribute) has is.
    - [AirAttribute](xref:OnixRuntime.Api.OnixClient.OnixModuleSettingRedirector.AirAttribute) has is.
- [OnixModule](xref:OnixRuntime.Api.OnixClient.OnixModule) now has [IsBlockedEnabled](xref:OnixRuntime.Api.OnixClient.OnixModule.IsBlockedEnabled) to reflect recent changes in the blocking system.
- [WorldChunks](xref:OnixRuntime.Api.World.WorldChunks) now has a [LoadedChunks](xref:OnixRuntime.Api.World.WorldChunks.LoadedChunks) property and an [AllChunks](xref:OnixRuntime.Api.World.WorldChunks.AllChunks) property. [LoadedChunks](xref:OnixRuntime.Api.World.WorldChunks.LoadedChunks) does the same as iterating over the [WorldChunks](xref:OnixRuntime.Api.World.WorldChunks) instance but is more explicit.
- NEW [World](xref:OnixRuntime.Api.Events.OnixEvents.World) EVENTS!
  - [BlockChanged](xref:OnixRuntime.Api.Events.OnixEventWorld.BlockChanged) for when a block changes, you don't need to enable it like in the legacy lua scripting.
    - [Here is a link to the BlockChanged Delegate.](xref:OnixRuntime.Api.Events.OnixEventWorld.OnBlockChangedDelegate) You can see what you receive in the event here.
  - [AreaChanged](xref:OnixRuntime.Api.Events.OnixEventWorld.AreaChanged) for when an area of blocks changes.
    - [Here is a link to the AreaChanged Delegate.](xref:OnixRuntime.Api.Events.OnixEventWorld.OnAreaChangedDelegate) You can see what you receive in the event here.
- You can now create your own [WorldBlocks](xref:OnixRuntime.Api.World.WorldBlocks) instances!
  - You should create it on the thread it will be used on. This lets you use it on other threads. (AKA async [GetBlock]())
  - I can't guarantee the absolute stability if not used exactly how the game wants you to.
  - Here are the different 'constructors'.
    - [WorldBlocks.CreateForClient](xref:OnixRuntime.Api.World.WorldBlocks.CreateForClient*) (Recommended for most use cases)
    - [WorldBlocks.CreateForServer](xref:OnixRuntime.Api.World.WorldBlocks.CreateForServer*) (Recommended for server-side cases)
    - [WorldBlocks.CreateFromDimension](xref:OnixRuntime.Api.World.WorldBlocks.CreateFromDimension*) (Not recommended unless you know what you're doing, since you can't get those instances from another thread.)
    - [WorldBlocks.CreateFromOther](xref:OnixRuntime.Api.World.WorldBlocks.CreateFromOther*) (Not recommended unless you know what you're doing, since you can't get those instances from another thread.)
    - [WorldBlocks.CreateFromWorldChunks](xref:OnixRuntime.Api.World.WorldBlocks.CreateFromWorldChunks*) (Not recommended unless you know what you're doing, since you can't get those instances from another thread.)
- Added [BoundingBoxI](xref:OnixRuntime.Api.Maths.BoundingBoxI) which is an integer version of an axis-aligned [BoundingBox](xref:OnixRuntime.Api.Maths.BoundingBox).
  - It is inclusive by default, meaning the [Maximum](xref:OnixRuntime.Api.Maths.BoundingBoxI.Maximum) values are included in the box.
  - You can do a for loop over it to iterate all the [BlockPos](xref:OnixRuntime.Api.Maths.BlockPos) in the box.
  - You also get a [ToFloat()](xref:OnixRuntime.Api.Maths.BoundingBoxI.ToFloat*) which will produce a [BoundingBox](xref:OnixRuntime.Api.Maths.BoundingBox) from it.
    - That bounding box will be a visually correct outline of the integer box (e,g. when you render it).
    - A [ToInteger()](xref:OnixRuntime.Api.Maths.BoundingBox.ToInteger*) was also added to [BoundingBox](xref:OnixRuntime.Api.Maths.BoundingBox) to get the integer version of it.
- `Onix.Client.` [Notify](xref:OnixRuntime.Api.OnixClientThings.Notify*), [NotifyBanner](xref:OnixRuntime.Api.OnixClientThings.NotifyBanner*) and [NotifyTray](xref:OnixRuntime.Api.OnixClientThings.NotifyTray*) now all take an onClick parameter.
- The [OnixPluginInstaller](xref:OnixRuntime.Plugin.OnixPluginInstaller) now has a few more functions to help with the multiple plugin sources.
  - The static [OnixPluginInstaller.OfficialRepositoryUrl](xref:OnixRuntime.Plugin.OnixPluginInstaller.OfficialRepositoryUrl) property lets you get the official repository URL.
  - [GetCuratedRemoteSourceUrls()](xref:OnixRuntime.Plugin.OnixPluginInstaller.GetCuratedRemoteSourceUrls) gets a list of URLs for the sources. (Fixes formatting and makes sure the official one is always there and first in the list. It never has a trailing /.)
  - [SaveRemoteSourceUrls()](xref:OnixRuntime.Plugin.OnixPluginInstaller.SaveRemoteSourceUrls) for when you change the [RemoteSourceUrls](xref:OnixRuntime.Plugin.OnixPluginInstaller.RemoteSourceUrls) property.
  - [GetRemotePluginFromSources()](xref:OnixRuntime.Plugin.OnixPluginInstaller.GetRemotePluginFromSources*) gets all versions of a plugin from all sources. Note that there is a bool to get only the compatible ones. The list it returns is sorted from best to worst.
  - [GetRemotePluginsFromSources()](xref:OnixRuntime.Plugin.OnixPluginInstaller.GetRemotePluginsFromSources*) Gets all plugins from all sources regardless of compatibility or duplicates. The list is unsorted.
- More types now have an `IsClientSide` property to help with generic code.
  - [WorldBlocks.IsClientSide](xref:OnixRuntime.Api.World.WorldBlocks.IsClientSide)
  - [RuntimeWorld.IsClientSide](xref:OnixRuntime.Api.World.RuntimeWorld.IsClientSide)
  - [WorldChunk.IsClientSide](xref:OnixRuntime.Api.World.WorldChunk.IsClientSide)
  - [WorldChunks.IsClientSide](xref:OnixRuntime.Api.World.WorldChunks.IsClientSide)
<br>

### Bug Fixes
- Fixed [OnixSettingFloat](xref:OnixRuntime.Api.OnixClient.Settings.OnixSettingFloat)'s [Value](xref:OnixRuntime.Api.OnixClient.Settings.OnixSettingFloat.Value) property giving strange results.
- Fixed buttons not linking to the function in the [OnixModuleSettingRedirector](xref:OnixRuntime.Api.OnixClient.OnixModuleSettingRedirector).
    - On another note, I really need to write a guide for this since other than the few easy cases, it gets difficult if you don't know what you're doing.
- Settings you add in your plugins using the default saving system now load and save correctly.
- Fixed [OnixModuleSettingRedirector](xref:OnixRuntime.Api.OnixClient.OnixModuleSettingRedirector)'s [ConvertModuleToRedirectorClass](xref:OnixRuntime.Api.OnixClient.OnixModuleSettingRedirector.ConvertModuleToRedirectorClass*) method not producing valid code with + and &.
- Enum setting overlays will now correctly use the preferred UI font.
- The Modules API now uses shared memory address containers for all modules, fixing a lot of issues with the previous runtime.
  - (1k changes moment...) 
  - This fixes `AddSetting` but also anything else that was broken that I was or was not aware of.
  - This also ensures that you don't get your setting taken away from you randomly.
  - Now when that happens you'll get a [ObjectDisposedException](https://learn.microsoft.com/en-us/dotnet/api/system.objectdisposedexception?view=net-8.0) instead of a crash.
<br>

## Plugin Manager Changes
- The plugin manager UI now reports itself as a UI plugin.
- You can no longer disable, unload or uninstall the plugin manager UI from the UI to avoid getting stranded.
- The different sources you add in the `runtime/config.json` will now be shown in the plugin manager UI.
  - Even if there is no way to manage them yet. 

## Client Changes

- `Plugin Developer is Never Trusted` in `Developer Settings` is now off by default.

### Version Updates
- Now expects runtime version 4.


### Client Bug Fixes
- Rendering with Vibrant Visuals now works as you'd expect.
- Notifications now stack properly again. (You could only ever have one notification at a time before).
