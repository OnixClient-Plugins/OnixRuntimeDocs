### 🚀 Creating Your First Plugin

Getting started with your first plugin is simple with the easy-to-use Onix Client plugin project generator. Follow these steps to quickly set up a new project:

1. **Launch Minecraft: Bedrock Edition** with **Onix Client**.
2. In Minecraft's in-game chat, with Onix Client injected, use the **`.plugin create {name}`** command to start creating a new Onix Client Plugin.
3. Follow the plugin project generator prompts to configure your plugin settings. Once you're happy with your choices, click **Generate Project** to create the project files.
4. Open the generated solution in your preferred IDE (e.g., **Visual Studio**, **Visual Studio Code**, or **Rider**) and start coding!

With the project set up, you're ready to begin developing your very own plugin!

### 📝 Example Plugin
If you have followed the steps above, you should have a basic plugin project set up. Here's a simple example of what your plugin might look like:

```csharp
using OnixRuntime.Api;
using OnixRuntime.Plugin;

namespace ExamplePlugin {
    public class ExamplePlugin : OnixPluginBase {
        public static ExamplePlugin Instance { get; private set; } = null!;
        public static ExamplePluginConfig Config { get; private set; } = null!;

        public ExamplePlugin(OnixPluginInitInfo initInfo) : base(initInfo) {
            Instance = this;
            // If you can clean up what the plugin leaves behind manually, please do not unload the plugin when disabling.
            base.DisablingShouldUnloadPlugin = false;
#if DEBUG
           // base.WaitForDebuggerToBeAttached();
#endif
        }

        protected override void OnLoaded() {
            Console.WriteLine($"Plugin {CurrentPluginManifest.Name} loaded!");
            Config = new ExamplePluginConfig(PluginDisplayModule);
        }

        protected override void OnEnabled() {

        }

        protected override void OnDisabled() {

        }

        protected override void OnUnloaded() {
            // Ensure every task or thread is stopped when this function returns.
            // You can give them base.PluginEjectionCancellationToken which will be cancelled when this function returns. 
            Console.WriteLine($"Plugin {CurrentPluginManifest.Name} unloaded!");
        }
    }
}
```
This example plugin simply logs a message to the console when it is loaded and unloaded. You can modify this code to add your own functionality and features.
The console is redirected to the game chat, so you can see the output in-game.

### 📥 Importing the Plugin
- To import the plugin, you simply build it in your IDE.
- If you used the project generator, it will copy the necessary files over and it will be ready to use.
When your plugin is loaded, it will automatically unload and reload it when you build.
  - On Visual Studio Code, you can type dotnet build in the terminal to build the plugin or use Ctrl+B on Visual Studio Community or Rider.
  - If you get a build error talking about `/langVersion` install [.NET SDK 8.0](https://dotnet.microsoft.com/en-us/download/dotnet/8.0) and [.NET SDK 9.0](https://dotnet.microsoft.com/en-us/download/dotnet/9.0).
  - If you get it on Visual Studio, Update it and make sure to enable .NET desktop development in the Visual Studio Installer.

### ✏️ Changing the Plugin Details
If you already used the plugin generator and would like to change the name, description or other details of your plugin to something else, you can edit the `manifest.json` file located in the root of your plugin project.
This file contains all the metadata for your plugin, including its name, description, version, author, and more.

- We recommend leaving the `uuid` field, `runtime_version`, and `target_assembly` fields as they are, as they are automatically generated and managed by the Onix Client Plugin API.