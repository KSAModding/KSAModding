# Code

!!! warning "Unofficial project"
    There is currently no official way to add custom code to Kitten Space Agency. The tools used in this guide are developed by the community. They are not affiliated with or endorsed by the KSA developers.

## Overview
This guide explains how to add custom code execution to Kitten Space Agency.

## Prerequisites
To develop mods using visual studio you need the following prerequisites

- Any [Visual Studio](https://visualstudio.microsoft.com/downloads/) version with c# .NET 9 development tools installed.
- A github account
- [A github access token with access to package registries](https://docs.github.com/en/packages/learn-github-packages/about-permissions-for-github-packages#about-scopes-and-permissions-for-package-registries)


## Step 1: Setting up the development environment

Currently the most popular way to create mods is by using [StarMap](https://github.com/StarMapLoader/StarMap). 

---

Before starting this guide it is recommended to set up the example mod and running it to verify StarMap is working. You can find a guide [here](https://github.com/StarMapLoader/StarMap/wiki/Modding#example-mods-and-tutorial-on-how-to-mod) and an example mod [here](https://github.com/StarMapLoader/StarMap-ExampleMods/blob/main/StarMap.SimpleMod/SimpleMod.cs)
<!-- TODO: add custom mod install tutorial -->

### Creating the Visual Studio project
1. Create a new visual studio project by pressing `Create a new project`.
![Press create project](/KSAModding/assets/images/modding-guides/code/create-project.png)

1. Filter for the C# language
2. Select `Class Library`
3. Click next
![Select Class Library under C#](/KSAModding/assets/images/modding-guides/code/project-type.png)

1. Enter `FirstMod` as the name of the project
2. Select a location
3. Press next
![Enter FirstMod as the project name and chose a project location](/KSAModding/assets/images/modding-guides/code/project-details.png)

1. Select .NET version 9 (Standard Term Support)
2. Press create
![Select .NET version 9 (Standard Term Support)](/KSAModding/assets/images/modding-guides/code/select-dotnet-version.png)

### Setting up StarMap
Now that we have a working Visual Studio project it is time to setup StarMap. For this step you will need the [previously mentioned](#prerequisites) github access key so make sure you have that ready.

We will have to add a custom Package Source to nuget to be able to use StarMap

1. Right click the project
2. Select `Manage NuGet Packages`
![Right click the project and select Manage NuGet Packages](/KSAModding/assets/images/modding-guides/code/manage-nugetpackages.png)

3. Click the cogwheel
![Select to cogwheel](/KSAModding/assets/images/modding-guides/code/edit-NuGet-settings.png)

4. Under `NuGet Package Manager` Select `Package Sources`
5. Press `Add`
6. Under Name write `StarMap`
5. Under Source write `https://nuget.pkg.github.com/StarMapLoader/index.json`
6. Press Save
7. Close the options tab
![Add new Package Source](/KSAModding/assets/images/modding-guides/code/add-package-source.png)

8. Under Package Source Select `StarMap`
9. Open the Browse Tab
10. A dialog box will open
11. Under username Select `Personal Access Token`
12. Instead of using your github password under password enter your Personal Access token.
13. Press `Ok`
![Browse StarMap Source](/KSAModding/assets/images/modding-guides/code/browsing-starmap.png)


14. Select `StarMap.API`
15. Press `Install`
16. Complete the installation process
![Install StarMap.API](/KSAModding/assets/images/modding-guides/code/install-starmap.png)
17. StarMap should be set up

### Setting up Harmony

1. Under Package Source select `nuget.org` again
2. Go to the `Browse` tab
3. Search for `harmony`
4. Select `Lib.Harmony`
5. Press `Install`
6. Wait for the instal to finish
![Install Lib.Harmony](/KSAModding/assets/images/modding-guides/code/install-harmony.png)



## Step 2: Creating a IStarMapMod class

StarMap automatically loads the first class that implements IStarMapMod and has the same name as its dll. For more info about the dll see [The compilation section]() <!-- TODO: add section -->

Our first step will be creating this class. Our example will be using the name `FirstMod` for the mod.

### Creating the class

Create a class under the namespace FirstMod with the name FirstMod. And let it inherit from IStarMapMod

```c#
using StarMap.API;

namespace FirstMod
{
    public class FirstMod : IStarMapMod
    {

    }
}

```