### The modules API

The modules API is similar to the one found in the Lua API, but it has some extra features.

### The Basics

Just like in the Lua API, you start by getting a list of all modules. This one is located in [Onix.Client.Modules](xref:OnixRuntime.Api.OnixClientThings.Modules).

```cs
var mods = Onix.Client.Modules;
```

You can iterate over the modules with a simple `foreach` loop:

```cs
foreach (OnixModule mod in Onix.Client.Modules) {
    Console.WriteLine($"Module: {mod.Name}, Enabled: {mod.Enabled}");      
}
```

That's about where the similarities end, this being C#, you can use LINQ to filter, sort and manipulate the modules list in various ways. For example, to get all enabled modules:

```cs
foreach (OnixModule mod in Onix.Client.Modules.Where(m => m.Enabled)) {
    Console.WriteLine($"Enabled Module: {mod.Name}");
}
```

We've also added [GetModule](xref:OnixRuntime.Api.OnixClient.OnixModuleList.GetModule) which allows you to get a module by its save name:

```cs
var globalSettings = Onix.Client.Modules.GetModule("module.global_settings.name");
if (globalSettings is not null) {
    Console.WriteLine($"Global settings module found: {globalSettings.Name}");
}
```

Or if you want to be a fancypants.
<br>(Note: this uses C#'s pattern matching feature, the {} means that the returned value is not null and if you put a variable name after it, it will create and assign a variable for you.)

```cs
if (Onix.Client.Modules.GetModule("module.global_settings.name") is {} globalSettings) {
    Console.WriteLine($"Global settings module found: {globalSettings.Name}");
}
```

When you have an [OnixModule](xref:OnixRuntime.Api.OnixClient.OnixModule), things should vaguely look like the Lua API, but you may notice some things missing.
That's because now the different types of modules are more separated, we have classes for each type of module, so you can use the specific properties and methods for that type in a safe way.

### Types of Modules

- [OnixModule](xref:OnixRuntime.Api.OnixClient.OnixModule) The most basic type of module, it has the most common properties and methods that all modules have.
- [OnixModuleVisual](xref:OnixRuntime.Api.OnixClient.OnixModuleVisual) The base type for mods that can be moved around the HUD Editor. It has things like position & size.
- [OnixModuleTextual](xref:OnixRuntime.Api.OnixClient.OnixModuleTextual) A visual module that only specifies text, you can get its text that way.

Example for OnixModuleVisual. This gets the `Reach Display` module as a visual module, checks if its center position is within the top left corner of the screen and either judges the user or compliments them based on that.

```cs
if (mods.GetModule("module.reach_display.name") is OnixModuleVisual reachDisplayModule) {
    if (reachDisplayModule.Position + reachDisplayModule.Size / 2 < Onix.Gui.ScreenSize / 2) {
        Console.WriteLine($"Wow really? You put {reachDisplayModule.Name} in the top left corner? That's so... original!");
    }
    else {
        Console.WriteLine($"Wow that placement for {reachDisplayModule.Name} is awesome! I love it!");
    }
}
```

Example for OnixModuleTextual. This gets the `Reach Display` module as a textual module, then prints its text in chat.

```cs
if (mods.GetModule("module.reach_display.name") is OnixModuleTextual reachDisplayModule) {
    Console.WriteLine($"Current reach display text: {reachDisplayModule.Text}");
}
// Note that if you do want the reach of the last hit, you can do this instead of abusing the Reach Display module.
// You would put this event listener in the OnLoaded method.
// If you have more complex logic, you can make your IDE create a class method for you aswell.
Onix.Events.Player.Attack += (player, entity) => {
    Console.WriteLine($"Player attacked with a reach of {player.Raycast.Origin.Distance(player.Raycast.HitPosition)}");
    return false;
};
```

Of course, you can do all those in your foreach loop as well, if you decide to act on all modules.

### Settings

Just like before, you access a mod's settings with [OnixModule.Settings](xref:OnixRuntime.Api.OnixClient.OnixModule.Settings).
The setting has two GetSetting methods, one with a template and one without.
[The one without template](OnixRuntime.Api.OnixClient.OnixSettingsList.GetSetting) returns the base [OnixSetting](OnixRuntime.Api.OnixClient.OnixSetting).
[The one with template](OnixRuntime.Api.OnixClient.OnixSettingsList.GetSetting[T]) lets you specify a specific OnixSetting type for convenience.
<br>
Now you may notice that unlike scripting, settings don't have a Value property, since C# is strongly typed, the settings all have their own types.

### Types of Settings

- [OnixSetting](xref:OnixRuntime.Api.OnixClient.OnixSetting) The base type for all settings, it has the most common properties and methods that all settings have.
- [OnixSettingBool](xref:OnixRuntime.Api.OnixClient.Settings.OnixSettingBool) A boolean setting, it has a Value property that returns a bool.
- [OnixSettingAir](xref:OnixRuntime.Api.OnixClient.Settings.OnixSettingAir) an air setting, it does nothing, it has an Air property and will move down the settings below it in the settings list.
- [OnixSettingButton](xref:OnixRuntime.Api.OnixClient.Settings.OnixSettingButton) A button in the UI, it has a Click method and a ButtonText method.
- [OnixSettingCategory](xref:OnixRuntime.Api.OnixClient.Settings.OnixSettingCategory) A category setting, it has a IncludedCount property that returns the number of settings in the category, as well as a IncludesEverythingUntilNextCategory
  property.
- [OnixSettingColor](xref:OnixRuntime.Api.OnixClient.Settings.OnixSettingColor) A color setting, it has a Value property that returns a [Color](xref:OnixRuntime.Api.OnixClient.Settings.OnixSettingColor.ValueType) object.
- [OnixSettingCustom](xref:OnixRuntime.Api.OnixClient.Settings.OnixSettingCustom) A custom setting, doesn't really contain much since it's per setting type which are not known in advance.
- [OnixSettingEnum](xref:OnixRuntime.Api.OnixClient.Settings.OnixSettingEnum) An enum setting, containing a value as integer or as a type for convenience and an Options property with the possible values.
- [OnixSettingFloat](xref:OnixRuntime.Api.OnixClient.Settings.OnixSettingFloat) A float setting, it has a Value property that returns a float, as well as a min/max property.
- [OnixSettingGamepadKeybind](xref:OnixRuntime.Api.OnixClient.Settings.OnixSettingGamepadKeybind) A collection of keys from a Gamepad that must all be pressed to trigger.
- [OnixSettingInfo](xref:OnixRuntime.Api.OnixClient.Settings.OnixSettingInfo) Basically just some text in the settings list.
- [OnixSettingInt](xref:OnixRuntime.Api.OnixClient.Settings.OnixSettingInt) An integer setting, it has a Value property that returns an int, as well as a min/max property.
- [OnixSettingKeybind](xref:OnixRuntime.Api.OnixClient.Settings.OnixSettingKeybind) A single keybind setting, it has a value property of [InputKey](OnixRuntime.Api.Inputs.InputKey) which can be any button from the mouse, keyboard or
  gamepad.
- [OnixSettingLocalizedInfo](xref:OnixRuntime.Api.OnixClient.Settings.OnixSettingLocalizedInfo) Basically just some text in the settings list, this is more important from the client where everything can be localized but for you it's the
  same.
- [OnixSettingModuleHeader](xref:OnixRuntime.Api.OnixClient.Settings.OnixSettingModuleHeader) A setting used to display a module's name, enable & favorite button at the top of the settings list.
- [OnixSettingTextbox](xref:OnixRuntime.Api.OnixClient.Settings.OnixSettingTextbox) A [Textbox](xref:OnixRuntime.Api.Inputs.OnixTextbox) to let the user enter text, you can also provide validity feedback to it, highlighting it red or green.

This example shows getting `Global Settings`, then getting the `Use New Renderer` setting from it and judging the user whether it's enabled or not.

```cs
if (Onix.Client.Modules.GetModule("module.global_settings.name") is {} globalSettings) {
    if (globalSettings.Settings.GetSetting("module.global_settings.setting.use_new_renderer.name") is OnixSettingBool useNewRendererSetting) {
        if (!useNewRendererSetting.Value) {
            Console.WriteLine("You're cringe for not using the new renderer!");
        }
    }
}
```

<br>

Generally exploring the API in the IDE pressing . on things is not a bad idea, it shows you the options and their types, you can always print all settings and their types when you are unsure.
The following example shows how to print all settings and their types for a module:

```cs
if (Onix.Client.Modules.GetModule("module.global_settings.name") is {} globalSettings) {
    foreach (OnixSetting setting in globalSettings.Settings) {
        Console.WriteLine($"Setting: {setting.Name}, SaveName: {setting.SaveName} Type: {setting.Type}");
    }
}
```

### New Features

#### There are of course, new features and things you can do that the Lua API does not have. Those include.

- Gamepad Keybinds: You can now create and edit keybinds that require multiple gamepad buttons to be pressed at the same time.
- Creating Settings that aren't part of a Module.
- Change Callbacks: Get notified when the value of your setting changes.
- Save Names: Have a different save name than display name, allowing you to painlessly rename settings or modules.
- Creating Modules: You can additionally to your plugin, create new modules that show up in the client's UI.
- Inserting Settings in ANY Modules: You can now extend any Module you can get your hands on by inserting settings in them.
- Custom Settings Types: You can create your own settings types containing their own data and UI.
    - Technically, you could also do that before, it just wasn't too clear or really documented how you would do that.
    - You also needed to handle saving all on your own, externally
    - And the data of the settings was not actually contained in the setting.
    - Custom settings have [their own guide](./creating-custom-setting-types.md).


### Moduleless Settings

You can now create any setting types, passing null for the parent module.
This isn't too useful on its own but it allows you to insert it into other modules.<br>
That's also a great way to render a setting without having to implement everything yourself.
You would use [RendererDirect2D.RenderSetting](xref:OnixRuntime.Api.Rendering.RendererDirect2D.RenderSetting) or [RendererDirect2D.RenderSubSetting](xref:OnixRuntime.Api.Rendering.RendererDirect2D.RenderSubSetting) if inside a custom
setting.<br>
Please note that your plugin is still the real owner of the setting and that's even when you add it to a module, if you keep no references to it, it will get garbage collected and removed.

```cs
// You would store that in your class or a class, a List, whatever you want really, you wouldn't let it just die unless you didn't need it after that function.
var myNewBool = new OnixSettingBool(null, "My Real Bool setting!", "my_real_bool_setting", false, "This is just my setting, nothing crazy here :D");
```


### Setting Change Callbacks

You can now be notified whenever the value of your setting changes, this is useful when you don't poll the value all the time.<br>
[The callback documentation can be found here.](xref:OnixRuntime.Api.OnixClient.OnixSetting.SettingChangedDelegate)<br>
It gives you the current parent Module, the Setting that has changed containing its new value and a bool defining if the change is from loading the saved configuration or the user changing it.<br>
It's generally the last value of the constructor of any setting and for [OnixSettingButton](xref:OnixRuntime.Api.OnixClient.Settings.OnixSettingButton) it gets called when the user clicks the button.
You have to assume this can and will be called from any thread, so you should keep it thread-safe in there.
For default settings, setting the value will trigger call it. If you need to call it with an isInit value of true, most of them have a ValueInit property.
When making a UI you should always use Value so the new value is saved.
<br>

In this example, we create a new bool setting and give it a setting change callback.
I'm casting it to an OnixSettingBool as we get it as a generic OnixSetting.
This will throw an exception if the setting is not a bool but since I only added it to a bool setting, it will not be an issue.
I then print the new value of the setting to the console whenever it changes.

```cs
// You would put the setting in a module or keep it somewhere safe.
 var myNewBool = new OnixSettingBool(null, "My Real Bool setting!", "my_real_bool_setting", false, "This is just my setting, nothing crazy here :D",
    (mod, setting, init) => {
        Console.WriteLine($"Setting changed: {setting.Name} to {((OnixSettingBool)setting).Value}");
    });
```

For custom settings, you are responsible for calling
it. [We provide a function called CallValueChangedCallbackfor you to do that in the OnixCustomSetting class.](xref:OnixRuntime.Api.OnixClient.Settings.OnixSettingCustom.CallValueChangedCallback)<br>
[More on that in the custom settings guide.](./creating-custom-setting-types.md)

---
### Save Names
Though you could do that in the Lua API, it was a lot more annoying to work with, hence why nameless settings were introduced.
You would often end up with sync issues and other problems. <br>
This is useful in case you want to rename or localize a setting or module without breaking existing user data.
You could also do the opposite, rename the save name without changing the display name, putting the user's value back to the default value.
Taking advantage of it is quite simple, most settings optionally take a save name right after their name parameter in their constructor.
If you want to publish your plugin on the Onix Client Plugin Repository you should always use save names for your settings.

```cs
// You would put the setting in a module or keep it somewhere safe. Right below this text is the save name,
 var myNewBool = new OnixSettingBool(null, "My Real Bool setting!", "my_real_bool_setting", false, "This is just my setting, nothing crazy here :D");
```

### Creating Modules
You can now create additional onix modules that show up in the client's UI.
This is not often required as you have the plugin manager to change the settings of your plugin.
This is useful when you want to create a visual or textual module that can be moved around the screen by the user.


## I hate this. This will be redone in runtime version 2. Don't do this 😭.
```cs

        public OnixModuleTextual? OtherTextHudElement = null; 
        public OnixModuleVisual? HudElement = null; 
        protected override void OnEnabled() {
            // Only add the visual modules when the plugin is enabled.
            // Doing it on loaded would force the user to unload your plugin to not have the useless visual modules.
            // We have to register them (the true parameter at the very end) so they are recognized by the client.
            HudElement = new OnixModuleVisual($"{CurrentPluginManifest.Name}'s Hud Element", "hud_element", 
                Vec2.Zero, OnixModuleVisual.VisualAnchor.TopLeft, new Vec2(20, 20),
                $"This is the hud element part of {CurrentPluginManifest.Name}", true);
            
            OtherTextHudElement = new OnixModuleTextual($"{CurrentPluginManifest.Name}'s Text Hud Element", "text_hud_element", 
                new Vec2(20, 0), OnixModuleVisual.VisualAnchor.TopLeft, new Vec2(20, 12),
                $"This is the text hud element part of {CurrentPluginManifest.Name}", true);
            
            // We hide this one because there isn't much you can really do as a user with an empty visual module.
            // We don't touch the textual module since the whole point is to display text according to user preferences.
            // Hiding it only hides it in the Onix Client UI and NOT from the hud.
            // Note: you could leave them and let the user decide to turn them on or off that way.
            HudElement.IsHidden = true;
            
        }

        protected override void OnDisabled() {
            // Remove the mods when we get disabled, this is not necessary if you have your plugin unload when disabled.
            // The ! tells the compiler that the variables will not be null here, suppressing the nullability warning.
            // This function is only called if OnEnabled was called before, so the variables will not be null.
            HudElement!.Dispose();
            HudElement = null;
            OtherTextHudElement!.Dispose();
            OtherTextHudElement = null;
        }
        
        private void OnHudRender(RendererGame gfx, float delta) {
            // This function only gets called when we are enabled and OnLoaded & OnEnabled were called.
            
            // We assume the user wants both of them since they enabled the plugin.
            OtherTextHudElement!.Enabled = true;
            HudElement!.Enabled = true;
            
            // Set the text of our text hud element.
            OtherTextHudElement!.Text = $"{Onix.LocalPlayer!.AliveDuration:g}";
            
            // Render a nice red square where the hud element is.
            gfx.FillRectangle(Rect.FromSize(HudElement!.Position, HudElement.Size), ColorF.Red);
            
        }

        protected override void OnUnloaded() {
            // Ensure every task or thread is stopped when this function returns.
            // You can give them base.PluginEjectionCancellationToken which will be cancelled when this function returns. 
            Console.WriteLine($"Plugin {CurrentPluginManifest.Name} unloaded!");
            Onix.Events.Common.Tick -= OnTick;
            Onix.Events.Common.HudRender -= OnHudRender;
        }
```

---
### Inserting Settings in Other Modules
You're now able to insert settings into any module you can get your hands on.
This is useful when you want to extend a module with your own settings, for example, adding a setting to the `Global Settings` module.
(You should *probably* not do that specifically unless you're sure it makes sense to have it there as it wont necessarily be obvious for the user.)<br>
There are two ways you can insert a setting.
- [With the index.](xref:OnixRuntime.Api.OnixClient.OnixModule.InsertSetting)
- [After another setting.]()

With the index, you insert it wherever you want, make sure to set the origin where you want it.<br>
With a setting, you place it at an offset from that setting, wherever it may be.
```cs
if (Onix.Client.Modules.GetModule("module.global_settings.name") is {} globalSettings) {
    if (globalSettings.Settings.GetSetting("module.global_settings.setting.use_new_renderer.name") is OnixSettingBool useNewRendererSetting) {
        // You need to store that in a variable that will stay alive somewhere, otherwise it will be garbage collected and removed.
        var myNewBool = new OnixSettingBool(null, "My Real Bool setting!", "my_real_bool_setting", false, "This is just my setting, nothing crazy here :D");
        // Insert the setting right after the useNewRendererSetting.
        globalSettings.InsertSetting(myNewBool, useNewRendererSetting, OnixModule.SettingInsertionPosition.After);
    }
}
```

---
## That's Great! But How do I Save?
When you create additional modules or settings that aren't part of a module you made that gets saved, they will not be saved automatically.
If you want your module to be saved, you need to call [SaveManager.TrackModule](OnixRuntime.Plugin.OnixPluginSaveManager.TrackModule) with your module.<br>
This will also load and save any settings you have in it at this time.<br>
```cs
HudElement = new OnixModuleVisual($"{CurrentPluginManifest.Name}'s Hud Element", "hud_element", 
    Vec2.Zero, OnixModuleVisual.VisualAnchor.TopLeft, new Vec2(20, 20),
    $"This is the hud element part of {CurrentPluginManifest.Name}", true);
SaveManager.TrackModule(HudElement);
```
This will load and save your module whenever your plugin is saved and loaded.<br>
You can also do it on settings, in the example below, I add a setting to the global settings.
See how I started tracking the setting after I inserted it into the global settings module.
That's because it will use the global settings module's name to uniquify the setting's save name.
```cs
if (Onix.Client.Modules.GetModule("module.global_settings.name") is {} globalSettings) {
    if (globalSettings.Settings.GetSetting("module.global_settings.setting.use_new_renderer.name") is OnixSettingBool useNewRendererSetting) {
        // You need to store that in a variable that will stay alive somewhere, otherwise it will be garbage collected and removed.
        var myNewBool = new OnixSettingBool(null, "My Real Bool setting!", "my_real_bool_setting", false, "This is just my setting, nothing crazy here :D");
        // Insert the setting right after the useNewRendererSetting.
        globalSettings.InsertSetting(myNewBool, useNewRendererSetting, OnixModule.SettingInsertionPosition.After);
        SaveManager.TrackSetting(myNewBool);
    }
}
```
