# Data Formats Overview

KSA uses several file formats for configuration, assets, and game state.

## Configuration Files (XML)

KSA uses XML extensively for defining parts, vehicles, celestial bodies, and systems.

**File Examples:**

- `PartAssets.xml` - Mesh definitions, materials, and part specifications
- `Astronomicals.xml` - Vehicles, celestial bodies, and orbital parameters
- `EarthOnly.xml` / `SolSystem.xml` - Star system definitions
- `Settings.xml` - Game settings and preferences

**Example Structure:**
```xml
<Part Id="YourPart">
    <SubPart Id="YourSubPart">
        <SubPartModel Id="YourModel">
            <Mesh Id="YourMesh"/>
            <Material Id="YourMaterial"/>
        </SubPartModel>
    </SubPart>
</Part>
```

## 3D Models (GLB)

KSA uses **GLB** (GLTF 2.0 binary) format for all 3D meshes.

**Required Vertex Attributes:**

- `POSITION` - 3D coordinates
- `NORMAL` - Surface normals for lighting
- `TEXCOORD_0` - UV coordinates for textures

**Important Limitations:**

- KSA only renders the **first primitive per mesh**
- Multi-part models must be split into separate GLB files

## Textures (PNG/DDS)

KSA uses **PBR (Physically Based Rendering)** materials with three texture types.

**Required Textures:**

1. **Diffuse** (Base Color/Albedo)
2. **Normal** (Tangent Space)
3. **RoughMetalAo** (Combined PBR)

**Important:** All three textures MUST be present for each material, or the game will crash with `NullReferenceException`. Use placeholder textures if you don't have real ones.

## Save Files

Save files store game state, vehicle configurations, and orbital data.

**Location:** `Documents\My Games\Kitten Space Agency\Saves\`

## Log Files

Logs provide debugging information and error messages.

**Location:** `Documents\My Games\Kitten Space Agency\Logs\`

**Files:**

- `Brutal.log` - Current session log
- `Archives\Brutal.YYYYMMDD.HH.log` - Archived logs from previous sessions

**Format:** Plain text with timestamps

**Common Errors:**

- `NullReferenceException` - Missing textures or invalid references
- `XmlException` - Malformed XML syntax
- `FileNotFoundException` - Missing mesh or texture files

## Audio Files

KSA uses audio files for sound effects and music.

**Format:** .OGG

