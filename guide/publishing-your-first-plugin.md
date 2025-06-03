### 📦 Publishing Your First Plugin

Ready to share your plugin with the world? Before you upload your plugin, you'll want to make sure your manifest is properly configured. Here's everything you need to know about preparing your plugin for publication.

<br>

### 📋 Plugin Manifest Setup

The plugin manifest (`manifest.json`) contains all the essential metadata for your plugin. Here's the JSON structure:

```json
{
  "uuid": "9938f109-df76-4cce-aebd-533f39e777f8",
  "plugin_name": "Plugin Name",
  "plugin_author": "Author",
  "plugin_description": "My amazing plugin description that I forgot to change for some reason.",
  "plugin_version": "1.0.0",
  "game_version": "1.21.80",
  "supported_game_version_ranges": [
    "0.0.0-9.99.9"
  ],
  "categories": [],
  "runtime_version": 1,
  "target_assembly": "PluginName.dll"
}
```

<br>

### ⚙️ Manual Configuration Required

The plugin generator will handle most of the manifest setup for you, but there are a few important fields you'll need to configure manually:

- **supported_game_version_ranges**: This field determines which versions of Minecraft: Bedrock Edition your plugin will work with. If your plugin only uses the safe, official methods provided by the Onix Client Plugin API, you typically don't need to restrict the version range. However, if you're using "unsafe" features like:
  - Direct memory reading or writing
  - Raw packet manipulation
  - Anything that doesn't use the official Onix Client Plugin API methods
  
  Then you **must** specify the exact game version ranges your plugin supports, as these features can break between game updates, and using them without proper version checks can lead to crashes or unexpected behavior.

- **categories**: (Optional), You can specify the appropriate categories that best describe your plugin's functionality.

<br>

### 🏷️ Available Plugin Categories

Choose from the following categories to help users discover your plugin:

- **Utility** - General utility tools and helpers
- **Library** - Code libraries for other plugins to use
- **Client Side** - Features that enhance the client experience
- **Server Side** - Server-related functionality and tools
- **Social** - Social features and community tools

Make sure to select the categories that accurately represent your plugin's purpose and functionality. This helps users find plugins that match their needs.

<br>

### 📤 Uploading Your Plugin

Once your manifest is properly configured, you can upload your plugin to the Onix Client Plugin Repository. Follow these steps:
and then steps will go here at some point