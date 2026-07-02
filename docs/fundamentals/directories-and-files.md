# Game Directories and File Layout

## Installation Directory

**Location:** `C:\Users\[YourName]\AppData\Local\Programs\Kitten Space Agency\`

### Core Directory

Where all the game assets live:

- **Meshes** - GLB 3D model files
- **Textures** - PNG/DDS texture files
- **PartAssets.xml** - Parts, meshes, materials
- **Astronomicals.xml** - Vehicles and celestial bodies
- **EarthOnly.xml** - Earth-only system
- **SolSystem.xml** - Solar system

### Game Code

- **KSA.dll** - Main game code (decompile with ILSpy)
- **Brutal.dll** - Engine code

## User Data Directory

**Location:** `C:\Users\[YourName]\Documents\My Games\Kitten Space Agency\`

- **Logs** - `Brutal.log` and archived logs
- **Saves** - Save game files
- **mods/** - Local mods folder. Mods placed here are auto-discovered by the game and persist across game updates. See [Installing Mods](/KSAModding/getting-started/installing-mods/) for details.
- **manifest.toml** - The local mod manifest that controls which mods are enabled.
 