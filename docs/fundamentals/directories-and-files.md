# Game Directories and File Layout

Understanding where KSA stores its files helps you mod safely and debug issues.

## Installation Directory

**Location:** `C:\Users\[YourName]\AppData\Local\Programs\Kitten Space Agency\`

This is where the game executable and core content live.

### Key Folders

**`Content\Core\`** - Core game assets and configuration
- `Meshes\` - All GLB 3D model files
- `Textures\` - All PNG/DDS texture files
- `PartAssets.xml` - Part and material definitions
- `Astronomicals.xml` - Vehicle and celestial body definitions
- `EarthOnly.xml` - Earth-only system definition
- `SolSystem.xml` - Full solar system definition

**`KSA.dll`** - Main game code (C# assembly)
- Can be decompiled with ILSpy or dnSpy
- Contains game logic, rendering, physics
 
**`Brutal.dll`** - Engine/framework code
- Underlying game engine
- Rendering, UI, input handling

### Modifying Installation Files

**⚠️ Important:**
- Always back up files before editing
- Game updates will overwrite your changes
- Consider version control (Git) for your modifications

**Safe to Edit:**
- XML files (`PartAssets.xml`, `Astronomicals.xml`, etc.)
- Add new GLB files to `Meshes\`
- Add new textures to `Textures\`

**Don't Edit:**
- EXE and DLL files (unless you know what you're doing)
- Files in system directories

## User Data Directory

**Location:** `C:\Users\[YourName]\Documents\My Games\Kitten Space Agency\`

This is where saves, logs, and user preferences are stored.

### Key Folders

**`Saves\`** - Save game files
- Player progress
- Vehicle configurations
- Mission states
- Format: Binary or serialized data

**`Logs\`** - Debug and error logs
- `Brutal.log` - Current session log
- `Archives\Brutal.YYYYMMDD.HH.log` - Previous session logs
- Check here first when debugging issues

**`Screenshots\`** (if exists) - Screenshots taken in-game

### User Settings

**`imgui.ini`** - UI preferences (may be in install or user directory)
- Window positions
- UI scale
- Last opened menus
- Delete to reset UI layout

**`Settings.xml`** (if exists) - Game settings
- Graphics options
- Audio settings
- Control bindings

## File Structure Overview

```
C:\Users\[YourName]\AppData\Local\Programs\Kitten Space Agency\
│
├── KSA.exe                      # Game executable
├── KSA.dll                      # Main game code
├── Brutal.dll                   # Engine code
│
└── Content\Core\
    ├── Meshes\
    │   ├── MediumCapsule.glb
    │   ├── Engine_A.glb
    │   ├── FuelTanks_SetA.glb
    │   └── ... (all 3D models)
    │
    ├── Textures\
    │   ├── Capsule_Diffuse.png
    │   ├── Capsule_Normal.png
    │   └── ... (all texture files)
    │
    ├── PartAssets.xml           # Parts, meshes, materials
    ├── Astronomicals.xml        # Vehicles, celestial bodies
    ├── EarthOnly.xml            # Earth system
    └── SolSystem.xml            # Solar system

C:\Users\[YourName]\Documents\My Games\Kitten Space Agency\
│
├── Saves\                       # Save game files
├── Logs\
│   ├── Brutal.log              # Current log
│   └── Archives\               # Old logs
│
└── imgui.ini                    # UI preferences (maybe)
```

## Backup Strategy

Before modding, back up these files:

**Critical Files:**
```
Content\Core\PartAssets.xml
Content\Core\Astronomicals.xml
Content\Core\EarthOnly.xml
Content\Core\SolSystem.xml
```

**Quick Backup:**
1. Copy the entire `Content\Core\` folder
2. Rename to `Content\Core_Backup`
3. Or use version control (Git)

**Restore if Something Breaks:**
1. Delete modified files
2. Copy files from `Content\Core_Backup`
3. Or verify/repair installation (if available)
4. Or reinstall game

## Modding Best Practices

**Use Version Control:**
- Track changes with Git
- Easy to revert mistakes
- Compare before/after

**Test Incrementally:**
- Make one change at a time
- Test in-game after each change
- Check logs for errors

**Document Your Changes:**
- Keep notes on what you modified
- Include XML line numbers
- Note which game version you're modding

**Share Mods Safely:**
- Only distribute files you created or modified
- Don't share entire game directories
- Include installation instructions
- Specify compatible game version
