# Getting started with writing Code for KSA

!!! warning "Unofficial project"
    There is currently no official way to add custom code to Kitten Space Agency. The tools used in this guide are developed by the community. They are not affiliated with or endorsed by the KSA developers.

## Overview
This guide explains how to add custom code execution to Kitten Space Agency.

## Prerequisites
To develop mods using visual studio you need the following prerequisites

- Any [Visual Studio](https://visualstudio.microsoft.com/downloads/) version with c# .NET 10 development tools installed. A fresh install of Visual Studio 2026 includes .NET 10 by default. Visual Studio Community edition is free.
- A github account
- [A github access token with access to package registries](https://docs.github.com/en/packages/learn-github-packages/about-permissions-for-github-packages#about-scopes-and-permissions-for-package-registries)


## Step 1: Setting up the development environment

Currently the most popular way to create mods is by using [StarMap](https://github.com/StarMapLoader/StarMap). 

Before starting this guide it is recommended to set up the example mod and running it to verify StarMap is working. You can find a guide [here](/KSAModding/getting-started/installing-mods/) and an example mod [here](https://github.com/StarMapLoader/StarMap-ExampleMods/releases/tag/0.1.4)

*THIS GUIDE USES STARMAP 0.3.x MAKE SURE YOU HAVE THE CORRECT VERSION*

### Creating the Visual Studio project
1. Create a new visual studio project by pressing `Create a new project`.
![Press create project](/assets/images/modding-guides/code/create-project.png)

1. Filter for the C# language
2. Select `Class Library`
3. Click next
![Select Class Library under C#](/assets/images/modding-guides/code/project-type.png)

1. Enter `FirstMod` as the name of the project
2. Select a location
3. Press next
![Enter FirstMod as the project name and chose a project location](/assets/images/modding-guides/code/project-details.png)

1. Select .NET version 10 (Long Term Support)
2. Press create
![Select .NET version 10 (Long Term Support)](/assets/images/modding-guides/code/select-dotnet-version.png)

### Setting up StarMap
Now that we have a working Visual Studio project it is time to setup StarMap. For this step you will need the [previously mentioned](#prerequisites) github access key so make sure you have that ready.

We will have to add a custom Package Source to nuget to be able to use StarMap

1. Right click the project
2. Select `Manage NuGet Packages`
![Right click the project and select Manage NuGet Packages](/assets/images/modding-guides/code/manage-nugetpackages.png)

3. Click the cogwheel
![Select to cogwheel](/assets/images/modding-guides/code/edit-NuGet-settings.png)

4. Under `NuGet Package Manager` Select `Package Sources`
5. Press `Add`
6. Under Name write `StarMap`
5. Under Source write `https://nuget.pkg.github.com/StarMapLoader/index.json`
6. Press Save
7. Close the options tab
![Add new Package Source](/assets/images/modding-guides/code/add-package-source.png)

8. Under Package Source Select `StarMap`
9. Open the Browse Tab
10. A dialog box will open
11. Under username Select `Personal Access Token`
12. Instead of using your github password under password enter your Personal Access token.
13. Press `Ok`
![Browse StarMap Source](/assets/images/modding-guides/code/browsing-starmap.png)


14. Select `StarMap.API`
15. Press `Install`
16. Complete the installation process
![Install StarMap.API](/assets/images/modding-guides/code/install-starmap.png)
17. StarMap should be set up

### Setting up Harmony

1. Under Package Source select `nuget.org` again
2. Go to the `Browse` tab
3. Search for `harmony`
4. Select `Lib.Harmony`
5. Press `Install`
6. Wait for the instal to finish
![Install Lib.Harmony](/assets/images/modding-guides/code/install-harmony.png)

## Step 2: Adding dependencies

When using StarMap to mod the game you will eventually need external resources from the game itself called assemblies or .dll files. For a basic mod you only need one dependency: KSA.dll. So this first-steps guide will do just that.

1. Right click on dependencies
2. Press Add Project Reference
![add project reference](/assets/images/modding-guides/code/add-project-reference.png)

1. Press Browse
2. Now go to the KSA install directory (default: C:\Program Files\Kitten Space Agency) and find KSA.dll, Select it and click add
3. Press OK
![alt text](/assets/images/modding-guides/code/browse-for-reference.png)

## Step 3: Creating a Mod class

StarMap automatically loads the class with the same name as the .dll file and that has the `[StarMapMod]` attribute

Our first step will be creating this class. Our example will be using the name `FirstMod` for the mod.

### Creating the class

Create a class under the namespace FirstMod with the name FirstMod. And let it inherit from IStarMapMod

```c#
using StarMap.API;

namespace FirstMod
{
    [StarMapMod]
    public class FirstMod
    {

    }
}

```

The api exposes a couple of attributes but for this simple hello world example we only need one:
`[StarMapImmediateLoad]`. The function marked with this attribute will be run when the mod is loaded. The function this attribute is assigned to does need to take in `Mod definingMod` as a parameter.

The `Mod` class needs the `KSA` namespace (found in KSA.dll) so we will add `using KSA` on the top of the file.


```c#
using StarMap.API;
using KSA;

namespace FirstMod
{
    [StarMapMod]
    public class FirstMod
    {
        [StarMapImmediateLoad]
        public void Init(Mod definingMod)
        {

        }
    }
}

```

Lets make a simple hello world program now.

Just add a `Console.WriteLine("Hello World!")`

Inside FirstMod:
```c#
[StarMapImmediateLoad]
public void Init(Mod definingMod)
{   
    Console.WriteLine("Hello World!");
}
```

## Step 4: Creating mod.toml

A mod should always have a mod.toml file so it can be identified by StarMap. Lets Create that file.

1. Create a new file in Visual Studio and call it `mod.toml`.
2. Edit the file and add this inside, name should always be the name of both the dll and the class that implements IStarMapMod.
```toml
name = "FirstMod"
```

3. Now right click on mod.toml and select `Properties`.
4. Under properties there is a field called `Copy To Output Directory` set it to `Copy always`.


## Step 5: Compiling

If you did all steps correctly simply right clicking the project and selecting `Build` should build the files.

The compiled files will now be in the following directory: `projectdir\FirstMod\bin\Debug\net9.0`.
Now follow the [installation guide](/KSAModding/getting-started/installing-mods/) to test your brand new mod. If you did all the steps correctly you should see `Hello World!` in the console output of StarMap.exe.
