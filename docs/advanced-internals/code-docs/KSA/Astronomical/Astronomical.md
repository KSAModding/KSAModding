<sub><sup>Tested with: **KSA Version 2025.11.6.2829**</sup></sub>
# class KSA.Astronomical

> **Namespace:** `KSA`  
> **Assembly:** `KSA.dll`

The `Astronomical` class represents any orbiting or flying object in the Universe include the star.
> examples: Stars, Planets, Moons, Asteriods, and Vehicles.

It integrates orbital properties, physical properties, rendering, and more into one high-level object.

An `Astronomical` has its own **Children Astronomicals**, **Class**, **orbit**, **MeshData**, **Id**, **SphereOfInfluence**, and **Physical Properties**. 
It has methods for Rendering builtin as well as methods to get the different refernce frames. 
The **Class** attribute for the Astronomical tells you which type of Astronomical it is.

!!! warning "Documentation Incomplete"
    This documentation page is not finished yet. Some sections or method pages may  be missing, incomplete, or incorrect.

## Code Example
```csharp
// Gets the current CelestialSystem (Star System)
var celestialSystem = KSA.Universe.CelestialSystem;

// Gets a list of all the Astronomicals in the CelestialSystem
var astronomicalList = celestialSystem.All.GetList();

// Prints out the Id (name) of each Astronomical
foreach(var astro in astronomicalList) 
{
    Console.WriteLine($"Astronomical Name: {astro.Id}");
}
```

## Constructors

### Astronomical(CelestialSystem system, AstronomicalTemplate bodyTemplate, string id = "")

Creates a new Astronomical in the given CelestialSystem with the name given by `id`, in the format given by the AstronomicalTemplate.
The Constructor Initalizes:

* Id (name) from the given `id`
* SphereOfInfluence from the `BodyTemplate`
* **byte4** color value from the `BodyTemplate`
* The CelestialSystem the Astronomical is in from `system`
* OrbitView

## Public Members

> **Note:** This section lists only the major public members. Many additional fields and properties exist; method-level documentation will link to them individually.

This section will assume all members are public. Any additional add-ons like **readonly** or **protected** will be included in the list entry. Any member that has an inital value that doesn't change will also be shown.

### Render Properties

* `bool ShowAxes`
* `CelestialRenderData RenderData`
* `ProceduralModifiersRenderData? TerrainModifiersRenderData`
* `byte4 OrbitColor => bodyTemplate.OrbitColor`
* `VkDescriptorSet TextureSet`
* `VkDescriptorSet[] MeshSets = Array.Empty<VkDescriptorSet>()`
* `bool RenderDataLoaded`
* `const double MINIMUM_FOLLOWING_DISTANCE = 500000.0`
* `static float SpriteDistanceScale = 1f`
* `protected const double NearSurfaceOffset = 1.0`
* `byte4 SoiColor`

### ImGui and UI 

* `const ImGuiWindowFlags INFOBOX_FLAGS`
* `bool ShowCelestialWindow`
* `readonly struct UiContext`
> **Note:** `UiContext` should contain its own subpage as it contains a lot of useful information.

### Physical Attributes

* `virtual string Id`
* `virtual uint Hash`
* `virtual string Class`
* `CelestialSystem System`
* `bool Rotates => bodyTemplate.RotationPeriod.Seconds > 0.0`
* `double MeanRadius => bodyTemplate.MeanRadiusRef`
* `double Mass => bodyTemplate.Mass`
* `double TerrainScale => bodyTemplate.TerrainScale`
* `double SphereOfInfluence`

### Miscellaneous

* `readonly List<Astronomical> Children = new List<Astronomical>()`
* `protected readonly AstronomicalTemplate bodyTemplate`
* `OrbitView OrbitView`
* `abstract AstronomicalTemplate BodyTemplate`


## Method Pages (Overview)

This is a list of some of the public methods in Astronomical.

### Physical Properties

* `StateVectors GetStateVectorsAt(SimTime gameTime)`
* `double DistanceTo(Astronomical body)`
* `bool IsMoon()`
* `bool IsStar()`
* `bool HasOrbit()`
* ` double GetNearSurfaceRadius()`
* `double GetAngularVelocity()`
* `bool HasChildren()`


### String Formating

* `void ToConsole();`
* `TableString.Row ToRow()`

### Rendering

* `void UpdatePerFrameData()`
* `void UpdatePerFrameDataTree()`
* `bool DrawUi(Viewport inViewport)`
* `void DrawCelestialWindow(Viewport inViewPort)`
* `void DrawCelestialWindowData()`
* `void SetPbrMaterial()`

### BodyTemplate

* `void AppendAttributesTo(TreeString tree)`
* `void AppendTo(TreeString tree)`

## Operators

This class does **not** define any custom operators.
