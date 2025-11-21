<sub><sup>Tested with: **KSA Version 2025.11.7.2844**</sup></sub>
# Void Brutal.ImGuiApi.ImGui.PushStyleColor

> **Namespace:** `Brutal.ImGuiApi`  
> **Assembly:** `Brutal.ImGui.dll`

The void `ImGui.PushStyleColor` allows modification of the color for each element of an ImGui window.

The current page will go over the different aspects of `PushStyleColor` including different elements that can be changed, how to call PushStyleColor, and other requirements to allow it to work.

!!! warning "Documentation Incomplete"
    This documentation page is not finished yet. Some sections or method pages may still be missing.

## Structuring your Call

### **Prerequisites**
It is reccomended to be familiar with creating a window in ImGui before styling it. Please go read [Brutal.ImGui Overview](https://ksamodding.github.io/KSAModding/advanced-internals/code-docs/Brutal/ImGuiAPI/Overview/ImGuiAPI/).

Styling within ImGui for modding is applied in a stack. This means that you set a temporary overide for rendering ImGui and then reset it once finished. This is done with `ImGui.PushStyleColor();` and `ImGui.PopStyleColor();`. In addition, each PushStyleColor must be paired with its own unique PopStyleColor.

For StarMap and example window that uses PushStyleColor would be,
```csharp
using Brutal.ImGuiAPI;
using StarMap.API;

[StarMapMod]
public class PushStyleColorExample
{
    [StarMapAfterGui]
    public void AfterGUI()
    {
        ImGui.PushStyleColor(ImGuiCol.TitleBgActive, new byte4(255, 255, 255, 255));
        ImGui.PushStyleColor(ImGuiCol.TitleBgCollapsed, new byte4(255, 255, 255, 255));
        // ImGui code or function calls go here
        ImGui.PopStyleColor();
        ImGui.PopStyleColor();
    }
}
```
To learn more about the specifics on how StarMap handles drawing the GUI or making a window in general, check out the StarMap GitHub page or join the KSA Discord to ask questions.
