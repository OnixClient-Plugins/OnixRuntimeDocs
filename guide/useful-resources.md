## 🛠️ Useful Resources

The Onix Client Plugin API is quite extensive, so it may be overwhelming at first. To help you get started, here are some useful resources that you can use to learn more about the API and how to use it effectively:

### 🧑 LocalPlayer
1. [LocalPlayer](xref:OnixRuntime.Api.Entities.LocalPlayer)
    - The LocalPlayer API provides access to the local player entity, which represents your own player, you. It is useful for getting information about your player, such as your position, rotation, gamertag, inventory, etc.

### 🎨 Rendering
2. [RenderContexes](xref:OnixRuntime.Api.Rendering.RenderContexes)
    - The Rendering API provides access to the rendering system, which allows you to create and manipulate 2D or 3D graphics in the game. It is useful for creating custom user interfaces, overlays, and other visual elements. Requires the rendering events. See events below.
    - The link provided leads to the a class that will give you leads to specific renderers. You should always get your those renderers from one of the events. 

### ⚡ Events
3. [Onix.Events](xref:OnixRuntime.Api.Onix.Events)
    - The Events API provides access to the event system, which allows you to listen for and respond to various events that occur in the game. It contains events such as chat events, input events, rendering events, and more. It is useful for creating custom game mechanics, responding to player actions, and other interactive elements.