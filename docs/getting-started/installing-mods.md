# Installing mods with StarMap

This page covers how to install mods that work with StarMap.

First make sure you installed StarMap via this guide: [StarMap installation guide](/KSAModding/getting-started/install-tools/#starmap-ksa-code-loader)

## How to install mods

## Method 1: KSA Mod Loader
1. Download the zip file for the mod.
2. Copy and paste the zip file into the `ModSetup` folder of KSA Mod Loader
3. Run KSA Mod Loader and select option "2" to install.

## Method 2: Manual
1.  Download the mod's zip that contains the class library dll as well as any dependencies.
2.  Extract the zip folder so that the .dll file is inside a folder with the same nam as the .dll file.
    ```
        StarMap.ExampleMod
        └── StarMap.ExampleMod.dll
        └── ...
    ```
3. Put this folder in one of the two supported mod locations:
    - **Local mods folder (recommended):** `Documents\My Games\Kitten Space Agency\mods\` - Mods here persist across game updates.
    - **Game content folder:** `KSAInstallLocation\Content\` - This folder also contains the `Core` mod folder. Mods here may be lost when the game updates.
4. The game auto-discovers new mods and prompts you to enable them on startup. Alternatively, you can manually edit the `manifest.toml` in `Documents\My Games\Kitten Space Agency\` to enable or disable mods. The id must match the folder name:
```
[[mods]]
id = "[mod folder name]"
enabled = true
```
5. When now loading the game via `StarMap.exe` or `StarMapLoader.exe`, the mod should be loaded and run.
