# 🐞 Using a Debugger

---

One of the best tools you can use when developing **Onix Client Plugins** is a debugger.
Instead of spamming `Console.WriteLine` and hoping for the best, you can:
- Set **breakpoints** and step through your code.
- **Inspect values** at runtime.
- Quickly figure out what's going wrong.

---

### ⚠️ Note For *JetBrains Rider* Users
If you're using **JetBrains Rider**, you won't be able to attach directly to the game as a .NET process.
To use a debugger, you'll need to open your project in **Visual Studio** or **Visual Studio Code** instead.

---

## 💻 Using Visual Studio
- You will need Visual Studio installed with the `.NET desktop development` workload selected.
- Open your plugin's solution in Visual Studio for the best experience.
- Then in the `Debug` menubar option, click on `Attach to Process...`.
  - The default keybind is `Ctrl+Alt+P`. 
  - You can use the keybind to re-attach quickly when you have already attached once, the default keybind is `Shift+Alt+P`.
- This should bring up the `Attach to Process` window.
- Before you do anything, make sure at the bottom right that `Code Type: ` is set to `Managed (.NET Core, .NET 5+) code`
  - Make sure to select specifically that one and not another option that looks "close enough". 
  - If `Native` or any other options are selected, make sure to uncheck them.
  - If you don't uncheck `Native`, the debugger will be very slow and will crash the game when you stop debugging.
- Then find (or search for) the `Minecraft.Windows.exe` process in the list.
  - If you don't see it, open the game.
- You can double-click the process or select it and click `Attach`.
- You're done! You can now set breakpoints and debug your plugin.

---

## 📝 Using Visual Studio Code
- For Visual Studio Code, you will need to install the `C# Dev Kit` extension if you haven't already.
- I'm not sure how well it will work if you don't load the solution, so you should load your plugin's solution in Visual Studio Code.
- Then you could do it manually every time by doing the following steps:
  - Open the command palette with `Ctrl+Shift+P`.
  - Type `Debug: Attach to .NET Core` and select it.
  - This should bring up a list of processes, select `Minecraft.Windows.exe`.
- Iff you want to set up keybinds, you can do the following:
  - In your project's root (in the folder view, not the Solution Explorer), create a folder named `.vscode` if it doesn't already exist.
  - Create a file named `launch.json` in the `.vscode` folder.
```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Build & Attach Debugger",
      "type": "coreclr",
      "request": "attach",
      "processName": "Minecraft.Windows.exe",
      "justMyCode": false
    }
  ]
}
```
- With this, you should be able to build and attach the debugger by pressing `F5` or going to the Run and Debug view and clicking the green play button.
  - Note: If you don't have a `tasks.json` or a task named `dotnet build`, it will only attach the debugger without building.
  - Here is a simple `tasks.json` that you can use to build your project:
```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "dotnet build",
      "type": "shell",
      "command": "dotnet",
      "args": [
        "build",
        "${workspaceFolder}"
      ],
      "problemMatcher": "$msCompile",
      "group": {
        "kind": "build",
        "isDefault": true
      },
      "presentation": {
        "reveal": "always",
        "panel": "shared"
      }
    }
  ]
}

```

## 🔍 Using The Debugger
It's quite simple to use, it's useful when:
- You want to inspect variables.
- You want to step through your code line by line to see where it all went wrong.
- You want to set breakpoints to pause execution at a specific line of code.
- You want to see the call stack to understand how you got to a certain point in your code.
- You can use the icons in Visual Studio or Visual Studio Code to step over, step into, step out, continue, and stop debugging.
- Or learn the shortcuts, the defaults are:
  - `F10` to step over. 
    - Step over means it will execute the function without going into it.
  - `F11` to step into.
    - Step into means it will go into the function and pause at the first line of it.
  - `Shift+F11` to step out. 
    - Step out means it will execute the rest of the function and pause at the line after the function call.
  - `F5` to continue.
    - Continue means it will resume execution until the next breakpoint or the end of the program.
  - `Shift+F5` to stop debugging. 
    - Stop debugging means it will detach the debugger and resume normal execution.
- [Here is a tutorial showing the basics of using the Visual Studio debugger.](https://youtu.be/ntOpMI7rmJM?si=-DquI7Omx8j_FxOK)
- [Or if you prefer the written format, here is a guide from Microsoft.](https://learn.microsoft.com/en-us/visualstudio/debugger/debugger-feature-tour?view=vs-2022)

