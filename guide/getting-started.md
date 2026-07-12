## 🚀 Getting Started

---
### 🛠️ Requirements
- To start developing plugins for Onix Client, you will need the following tools and software. Each section below includes a link to the official download page and a tutorial to help you get set up.<br>
  <br>
#### 💎 Onix Client Premium
- <a href="https://onixclient.com/buy" target="_blank"><strong>Get Onix Client Premium</strong></a><br>
- Required to use the Onix Client Plugin API.<br>
  <br>
---
#### 🎮 Minecraft: Bedrock Edition
- The game you will be modding so you need to have it installed.<br>
  <br>
---
### 💻 IDE (Integrated Development Environment)
To develop plugins for Onix Client, you will need an IDE to write and compile your code. I would personally recommend using Visual Studio or JetBrains Rider as they are both good IDEs, but it always comes down to personal preference. If your PC struggles to run either, then feel free to use Visual Studio Code, as it's lightweight and only needs a few extensions to work well with C#.<br>
- <strong>Note:</strong> JetBrains Rider's debugger does **not** work in this environment, you will **have** to use Visual Studio or Visual Studio Code for debugging.<br>
- <strong>Note:</strong> Make sure to install the C# extension for Visual Studio Code if you choose to use it.<br>
  <br>
---
#### 🏢 Visual Studio
- <a href="https://visualstudio.microsoft.com/" target="_blank"><strong>Download Visual Studio</strong></a> ([Tutorial](https://learn.microsoft.com/en-us/visualstudio/install/install-visual-studio?view=vs-2022) <a href="https://learn.microsoft.com/en-us/visualstudio/install/install-visual-studio?view=vs-2022" target="_blank">here</a>)<br>
- A powerful IDE for C# development made by Microsoft. One of their last good products.<br>
  <br>
---
#### 🦄 JetBrains Rider
- <a href="https://www.jetbrains.com/rider/download/" target="_blank"><strong>Download JetBrains Rider</strong> here</a><br>
- A cross-platform C# IDE from JetBrains. Pretty good IDE, I personally use it. It has a lot of features and is very powerful.<br>
  <br>
---
#### 📝 Visual Studio Code
- <a href="https://code.visualstudio.com/" target="_blank"><strong>Download Visual Studio Code</strong></a> ([Tutorial](https://code.visualstudio.com/docs/setup/setup-overview) <a href="https://code.visualstudio.com/docs/setup/setup-overview" target="_blank">here</a>)<br>
- A lightweight, cross-platform code editor. Make sure to install the C# extension. I don't personally use it, but it's a good option if you have a low-end PC.<br>
  <br>
---
### 📥 Getting The Runtime Ready
- Before you start developing or using plugins, you need to get the .NET runtime and the OnixRuntime placed correctly.
  - Thankfully, it's very easy to do! Simply run the following command in the game chat and wait for the setup to complete.
```
.plugin setup
```
This command will download everything you need to run a plugin.
The correct OnixRuntime version will be automatically downloaded to match the client's desired version so you don't have to worry about it for the future.
<br>
---
### 🧩 Creating Your First Plugin
- To get started with your first plugin, head over to the [Creating Your First Plugin](./creating-your-first-plugin.md) page. It will guide you through the process of creating a simple plugin and give you an overview of the Onix Client Plugin API.

### 🌐 Adding NuGet Source (Optional)
- If you want the Onix Plugin NuGet packages, simply run `dotnet nuget add source https://plugin-nuget.onixclient.com/ -n "Onix Plugins"` in your terminal.
  * If you don't have the .NET SDK installed, download both [.NET SDK 10.0](https://dotnet.microsoft.com/en-us/download/dotnet/10.0) from these links.