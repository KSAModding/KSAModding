<sub><sup>Tested with: **KSA Version 2025.11.8.2847**</sup></sub>

# Namespace Brutal.ImGuiAPI

> **Namespace:** `Brutal.ImGuiAPI`  
> **Assembly:** `Brutal.ImGui.dll`

This page will be the next example and explanation of the KSA/Brutal ImGuiAPI. The topics of this page is text elements.
!!! warning "Documentation Incomplete"
    This documentation page is not finished yet. Some sections or method pages may  be missing, incomplete, or incorrect.

## Basic Text

### Prerequisites

To begin you will need the basic setup from the overview page, or if you have your own code, a ImGui window created with `ImGui.Begin()` and `ImGui.End()`. We are not going to use any `ImGuiWindowFlags` to start.

The code below will be used as the starting point for this page.

```csharp
using StarMap.API;
using Brutal.ImGuiAPI;

namespace ImGuiExamples
{
    [StarMapMod]
    public class Examples
    {
        private bool ShowWindow = true;

        [StarMapAfterGui]
        public void AfterGUI(double dt)
        {
            if (!ShowWindow) return;

            if(ImGui.Begin("Text Window", ref ShowWindow))
            {
                // New ImGui code will go here
            }
            ImGui.End();
        }
    }
}
```

This code will create a window looking like this.

![alt text](<Example Window.png>)

### Methods

* `void ImGui.Text(ImString text)`
* `void ImGui.TextColored(in float4 col, ImString text)`

### Adding Text

To add text to you window, all you need is is `ImGui.Text()` inside the if-statement where your `ImGui.Begin()` is. Inside the `()` you put the text string you wish to display.

```csharp
if(ImGui.Begin("Text Window", ref ShowWindow))
{
    ImGui.Text("Hello World!");
}
ImGui.End();
```

The above code will now display a line of text saying `Hello World!`.

![alt text](<Hello World.png>)

Note that the text will be cutoff if the window is resized to no longer fit the text.

![alt text](<Text Cutoff.png>)

With `ImGui.Text()` you can also use formatted strings to display information as the code is executed every frame, so the formatted string values will also be updated.

```csharp
using KSA;

if(ImGui.Begin("Text Window", ref ShowWindow))
{
    ImGui.Text("Hello World!");
    ImGui.Text($"Current Vehicle: {Program.ControlledVehicle.Id}");
}
ImGui.End();
```

> `Program.ControlledVehicle.Id` will return the Id (name) of the currently controlled vehicle.

> To use `Program.ControlledVehicle.Id` you need to put `using KSA` at the top of your file.

This will now show a second line of text saying `Current Vehicle: [Vehicle Name]`, and this will update in real time if you switch between vehicles.

![alt text](<Rocket.png>)

![alt text](<Hunter.png>)

Because you can use formatted strings, it is entirely possible to use loops to create set amounts of text without hardcoding each value. 

### Colored Text

To add a little bit of style to your text, you can add color to it. This is possible by changing the `ImGui.Text()` statements to `ImGui.TextColored()`, and then putting in the color you wish the text to be.

```csharp
using Brutal.Numerics;

if(ImGui.Begin("Text Window", ref ShowWindow))
{
    ImGui.TextColored(Color.Blue, "Hello World!");
    ImGui.TextColored(Color.Green, $"Current Vehicle: {Program.ControlledVehicle.Id}");
}
ImGui.End();
```

> `Color.Red` and `Color.Blue` are both `float4` enum values provided by `Brutal.Numerics` for color.

This will make `Hello World!` Blue and `Current Vehicle [Vehicle Name]` Green.

![alt text](<Colored Text.png>)

### Conclusion

Now that you know how to use some basic text, I would recommend trying out using logic to dynmically change what text is being shown and/or what color the text is.

The next section will go into more specalized styling of text.

## Stylized Text

### Prerequisites

The prerequisites are the same as the above prerequisites. [Prerequisites](#prerequisites)

### Methods

* `void ImGui.TextWrapped(ImString text)`
* `void ImGui.TextDisabled(ImString text)`
* `void ImGui.BulletText(ImString text)`
* `void ImGui.LabelText(ImString label, ImString text)`

### Wrapping Text

To start, we will add a wrapping text line. This line will wrap around the edge and start on the next line if the window is resized to no longer fix the text.

```csharp
if(ImGui.Begin("Text Window", ref ShowWindow))
{
    ImGui.TextWrapped("This is a long text string to test TextWrapped.");
    ImGui.Text("Hello World!");
}
ImGui.End();
```

The code above will display this when the window size fits.

![alt text](<No Wrap.png>)

and this when the window no longer fits.

![alt text](<Wrapped.png>)

### Disabled Text

There is also a unique looking text called disabled text. It is a text style that is meant to show disabled options through text or subtext.

```csharp
if(ImGui.Begin("Text Window", ref ShowWindow))
{
    ImGui.TextDisabled("You can't do this");
    ImGui.Text("Hello World!");
}
ImGui.End();
```

The code will display this,

![alt text](<Disabled Text.png>)

The text appears a dark shade of grey but has not other special properties like this.

### Bullet Text

There is also a way to make bullet points using text. These will automatically handle bullet point formating.

```csharp
if(ImGui.Begin("Text Window", ref ShowWindow))
{
    ImGui.BulletText("Do this 1st");
    ImGui.BulletText("Do this 2nd");
    ImGui.Text("Hello World!");
}
ImGui.End();
```

The code will display this,

![alt text](<Bullet Text.png>)

You can now make To-Do list and much more.

### Labeled Text

You can also have text have a labeled put before the text. By default, this will have the label locked to the right side of the window or container it is in, and the text locked to the left side. The middle will be empty space.

```csharp
if(ImGui.Begin("Text Window", ref ShowWindow))
{
    ImGui.LabelText("Universe", KSA.Universe.CurrentSystem.Id);
    ImGui.Text("Hello World!");
}
ImGui.End();
```

The code displays

![alt text](<Label One.png>)

> Note that the `Universe` label is on the right acting as supporting text for the current Celestial System.

![alt text](<Label Two.png>)

This type of text would be useful if you have two different lines of text that are likely to have similar results but are fundamentally different. You can use the label to differentiate them.

## Text Links

### Prerequisites

The prerequisites are the same as the first prerequisites. [Prerequisites](#prerequisites)

### Methods

* `bool ImGui.TextLink(ImString label)`
* `bool ImGui.TextLinkOpenURL(ImString label, [ImString url = default])`

### Link Text

This type of text just looks like a link, however clicking on the text will not take you to a website as thetext doesn't take in a URL. However, it does return `true` when clicked so you can use it as a psuedo button.

```csharp
private bool ShowHiddenText = false;

if(ImGui.Begin("Text Window", ref ShowWindow))
{
    if(ImGui.TextLink("This just looks like a link"))
    {
        ShowHiddenText = true;
    }
    if (ShowHiddenText) ImGui.Text("I was hidden");
    ImGui.Text("Hello World!");
}
ImGui.End();
```

This code will show a hidden piece of text when the TextLink is clicked.

![alt text](<Text Link.png>)

and when the button is clicked,

![alt text](<Hidden Text.png>)

### Actual Links

This next text element will let you put actual URL links into your window. The link does return `true` when clicked so you can also use it in logic one again.

```csharp
private bool ShowHiddenText = false;

if(ImGui.Begin("Text Window", ref ShowWindow))
{
    if(ImGui.TextLinkOpenURL("GitHub", "https://github.com/KSAModding"))
    {
        ShowHiddenText = true;
    }
    if (ShowHiddenText) ImGui.Text("I was hidden");
    ImGui.Text("Hello World!");
}
ImGui.End();
```

The above code will create an actual link the to KSAModding GitHub page, the host of this Wiki.

![alt text](<Actual Link.png>)

And when clicked,

![alt text](<KSAModding GitHub.png>)

![alt text](<Hidden Text Two.png>)

### Conclusion

You now know more ways to format you text and can start creating unique GUIs. The next section will talk about how to organize your text more.

## Seperators

### Prerequisites

The prerequisites are the same as the first prerequisites. [Prerequisites](#prerequisites)

### Methods

* `void ImGui.Seperator()`
* `void ImGui.SeperatorText(ImString label)`

### Basic Seperator

This seperator just creates a thin horizontal line across the window or container.

```csharp
private bool ShowHiddenText = false;

if(ImGui.Begin("Text Window", ref ShowWindow))
{
    ImGui.Text("Above the Seperator");
    ImGui.Separator();
    ImGui.Text("Below the Seperator");
}
ImGui.End();
```

This code displays,

![alt text](<Seperator.png>)

This is useful for making certain sections of your text as well as making areas easier to read.

### Text Seperator

This seperator is similar to the basic seperator as it has a line across the middle. However, in the line there is text added to the seperator.

```csharp
private bool ShowHiddenText = false;

if(ImGui.Begin("Text Window", ref ShowWindow))
{
    ImGui.Text("Above the Seperator");
    ImGui.SeparatorText("You Shall Not Pass!!");
    ImGui.Text("Below the Seperator");
}
ImGui.End();
```

The code displays,

![alt text](<Text Seperator.png>)

This can be used to add more style to your seperators.

### Conclusion

You now know how to organize your text neatly. However this isn't every option available. To see more options, check out the `TreeNode` and `Collapsing Header` pages.
!!! warning "Page Not Created"
    As of 11/22/2025, the time this page was created, the `TreeNode` and `Collapsing Header` pages do not exist. Sorry about that.

## Contributors

* MrJeranimo: 11/22/2025, Created the ImGuiAPI `Text Elements` page.