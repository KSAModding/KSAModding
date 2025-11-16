# Kitten Space Agency - Parts Modding Guide

## Overview
This guide explains how to add custom 3D models as vehicles/parts in Kitten Space Agency (KSA).

## Requirements
- Blender (for 3D model preparation)
- Text editor for XML editing
- Your 3D model in a format Blender can import (FBX, OBJ, etc.)

## File Locations
- **Game Directory**: `C:\Users\[YourName]\AppData\Local\Programs\Kitten Space Agency\`
- **Meshes**: `Content\Core\Meshes\`
- **Textures**: `Content\Core\Textures\`
- **Part Definitions**: `Content\Core\PartAssets.xml`
- **Vehicle Definitions**: `Content\Core\Astronomicals.xml`
- **System Files**: `Content\Core\EarthOnly.xml`, `Content\Core\SolSystem.xml`

## Step 1: Prepare Your 3D Model

### 1.1 Export to GLB Format

KSA uses GLB (GLTF 2.0 binary) format for meshes.

**Model Orientation:**

Before exporting, orient your model according to KSA's coordinate system:

- **+X = Forward** (nose, front of vehicle)
- **-X = Back** (engine nozzles, rear)
- **+Y = Right**
- **-Y = Left**
- **+Z = Up** (top of vehicle, doors on capsules)
- **-Z = Down** (radial parts attach here, bottom of vehicle)

Examples:

- Capsule: doors face up (+Z), nose forward (+X), base back (-X)
- Engine: nozzle points back (-X), attachment point forward (+X)
- Radial parts: attachment face down (-Z), outer face up (+Z)

**In Blender:**

1. Import your model (File > Import)
2. Rotate model to match KSA orientation (see above)
3. Apply all transforms (Ctrl+A > All Transforms)
4. Select all objects (A key)
5. Apply all modifiers if needed
6. **Important**: Check vertex attributes - model MUST have:
    - POSITION
    - NORMAL
    - TEXCOORD_0 (UV coordinates)
7. File > Export > glTF 2.0 (.glb)
8. Export settings:
    - Format: **GLB (binary)**
    - Include: Selected Objects only (or everything if you want)
    - Transform: +Y Up
    - **DO NOT** embed textures
9. Save as `your_model.glb`

### 1.2 Optimize File Size (Optional but Recommended)

1. In Blender Edit Mode, select all (A)
2. Add Decimate Modifier
3. Set Ratio: 0.2-0.3 (reduces polygons by 70-80%)
4. Apply modifier
5. Re-export

### 1.3 Split Multi-Part Models

KSA only renders the **first primitive per mesh**. If your model has multiple parts, you need to split them into separate GLB files.

**Split in Blender:**

1. Select your model
2. Edit Mode (Tab)
3. Select faces for one part (L key over connected faces)
4. Separate (P > Selection)
5. Repeat for each part
6. Export each part as separate GLB file

## Step 2: Prepare Textures

### 2.1 Required Texture Types
KSA uses PBR (Physically Based Rendering) materials with 3 texture types:

1. **Diffuse** (Base Color/Albedo) - Required
  
   - The main color/texture map
   - Format: PNG or DDS
   - RGB channels

2. **Normal** (Tangent Space) - Required
 
   - Height/bump information
   - Format: PNG or DDS
   - RGB: (128, 128, 255) = flat normal (no bumps)

3. **RoughMetalAo** - Required
 
   - Combined texture with 3 properties:
   - Red channel: Roughness (0=smooth, 255=rough)
   - Green channel: Metallic (0=non-metal, 255=metal)
   - Blue channel: Ambient Occlusion (shadows)
   - Format: PNG or DDS

### 2.2 Create Placeholder Textures

If you don't have normal or RoughMetalAo textures, you need to create placeholder ones.

**Flat Normal Map:**

- Create a 512x512 PNG image filled with RGB color (128, 128, 255) - this is a flat blue color that represents no bumps
- Save as `your_model_normal.png`

**Neutral RoughMetalAo Map:**

- Create a 512x512 PNG image filled with RGB color (128, 0, 255)
- Red channel (128) = medium roughness
- Green channel (0) = not metallic
- Blue channel (255) = full ambient occlusion
- Save as `your_model_roughmetalao.png`

You can create these in Photoshop, GIMP, or any image editor.

### 2.3 Copy Textures to Game
Copy all texture files (diffuse, normal, and RoughMetalAo) to:

`C:\Users\[YourName]\AppData\Local\Programs\Kitten Space Agency\Content\Core\Textures\`

## Step 3: Register Mesh Files

Edit `PartAssets.xml` (lines 1-10 area):

```xml
<MeshFile Id="YourModelMesh" Path="Meshes/your_model.glb" Category="Vessel"/>
```

If you have multiple parts:
```xml
<MeshFile Id="YourBodyMesh" Path="Meshes/your_body.glb" Category="Vessel"/>
<MeshFile Id="YourHeadMesh" Path="Meshes/your_head.glb" Category="Vessel"/>
<MeshFile Id="YourLegsMesh" Path="Meshes/your_legs.glb" Category="Vessel"/>
```

## Step 4: Create Materials

In `PartAssets.xml` (after mesh definitions):

```xml
<PbrMaterial Id="YourModelMat">
    <Diffuse Path="Textures/your_model_diffuse.png" Category="Vessel"/>
    <Normal Path="Textures/your_model_normal.png" Category="Vessel"/>
    <RoughMetalAo Path="Textures/your_model_roughmetalao.png" Category="Vessel"/>
</PbrMaterial>
```

For multiple parts with different textures, create multiple materials:
```xml
<PbrMaterial Id="YourBodyMat">
    <Diffuse Path="Textures/your_body_diffuse.png" Category="Vessel"/>
    <Normal Path="Textures/flat_normal.png" Category="Vessel"/>
    <RoughMetalAo Path="Textures/neutral_roughmetalao.png" Category="Vessel"/>
</PbrMaterial>

<PbrMaterial Id="YourHeadMat">
    <Diffuse Path="Textures/your_head_diffuse.png" Category="Vessel"/>
    <Normal Path="Textures/flat_normal.png" Category="Vessel"/>
    <RoughMetalAo Path="Textures/neutral_roughmetalao.png" Category="Vessel"/>
</PbrMaterial>
```

## Step 5: Define Parts

In `PartAssets.xml` (search for other `<Part Id=` definitions as reference):

### Simple Single-Part Vehicle
```xml
<Part Id="YourModelPart">
    <SubPart Id="YourModelSubPart">
        <SubPartModel Id="YourModelSubPartModel">
            <Mesh Id="YourModelMesh"/>
            <Material Id="YourModelMat"/>
        </SubPartModel>
    </SubPart>
</Part>
```

### Multi-Part Vehicle (Body + Head + Legs example)
```xml
<Part Id="YourBodyPart">
    <SubPart Id="YourBodySubPart">
        <SubPartModel Id="YourBodyModel">
            <Mesh Id="YourBodyMesh"/>
            <Material Id="YourBodyMat"/>
        </SubPartModel>
    </SubPart>
</Part>

<Part Id="YourHeadPart">
    <SubPart Id="YourHeadSubPart">
        <SubPartModel Id="YourHeadModel">
            <Mesh Id="YourHeadMesh"/>
            <Material Id="YourHeadMat"/>
        </SubPartModel>
    </SubPart>
</Part>

<Part Id="YourLegsPart">
    <SubPart Id="YourLegsSubPart">
        <SubPartModel Id="YourLegsModel">
            <Mesh Id="YourLegsMesh"/>
            <Material Id="YourLegsMat"/>
        </SubPartModel>
    </SubPart>
</Part>
```

## Step 6: Add RCS Thrusters (Optional)

Add thrusters to any `<SubPart>` section for attitude control:

```xml
<Part Id="YourModelPart">
    <SubPart Id="YourModelSubPart">
        <SubPartModel Id="YourModelSubPartModel">
            <Mesh Id="YourModelMesh"/>
            <Material Id="YourModelMat"/>
        </SubPartModel>
        
        <!-- Rotation Control -->
        <Thruster Id="YawLeft">
            <Location X="-0.2" Y="-0.15" Z="0.0" />
            <ExhaustDirection X="0.0" Y="1.0" Z="0.0" />
            <ControlMap CSV="YawLeft" />
            <Thrust N="150" />
            <SpecificImpulse Seconds="220" />
            <MinimumPulseTime Seconds="0.008" />
            <VolumetricExhaust Id="MmuRcsVac" />
            <SoundEvent Action="On" SoundId="DefaultRcsThruster"/>
            <ThrusterLight Value="false"/>
        </Thruster>
        
        <!-- Add more thrusters for full 6-DOF control -->
        <!-- See PartAssets.xml KSPBackpackUpPart for complete example -->
    </SubPart>
</Part>
```

**Thruster Coordinate System:**

- X = Forward/Back (+X forward, -X back)
- Y = Left/Right (+Y right, -Y left)
- Z = Up/Down (+Z up, -Z down)

**ExhaustDirection:** Points opposite to thrust direction

- To thrust forward: ExhaustDirection X="-1.0"
- To thrust back: ExhaustDirection X="1.0"
- To thrust up: ExhaustDirection Z="-1.0"
- To thrust down: ExhaustDirection Z="1.0"

**Control Maps Available:**

- Rotation: `YawLeft`, `YawRight`, `PitchUp`, `PitchDown`, `RollLeft`, `RollRight`
- Translation: `TranslateForward`, `TranslateBack`, `TranslateLeft`, `TranslateRight`, `TranslateUp`, `TranslateDown`

## Step 7: Create Vehicle Definition

Edit `Astronomicals.xml` (find similar vehicle definitions as reference):

### Single-Part Vehicle
```xml
<Vehicle Id="YourVehicle" Parent="Earth">
    <Canvas Id="Navball"/>
    <Canvas Id="Altitude"/>
    <Canvas Id="GameTime"/>
    <Canvas Id="PitchYawRoll"/>
    <Canvas Id="AutopilotSettings"/>
    <Canvas Id="BurnControl"/>
    <Canvas Id="RendezvousControl"/>
    <WindSound SoundId="AtmosphericMovement"/>

    <RootPart Id="YourModelPart"/>

    <!-- Orbital Parameters -->
    <SemiMajorAxis Km="6713" />
    <Inclination Degrees="28.87" />
    <Eccentricity Value="0.0077" />
    <LongitudeOfAscendingNode Degrees="65" />
    <ArgumentOfPeriapsis Degrees="35" />
    
    <!-- Physical Properties -->
    <MeanRadius M="2.0" />
    <MassSpecificPMI X="1.5" Y="3.0" Z="3.0" />
    <Mass Kg="5000" />
    <PropellantMass Kg="500" />
    <Color R="0.95" G="0.33" B="0.33" />
    
    <!-- RCS Sound (if you added thrusters) -->
    <SoundEvent Action="Rcs" SoundId="DefaultRcsThruster"/>
</Vehicle>
```

### Multi-Part Vehicle
```xml
<Vehicle Id="YourVehicle" Parent="Earth">
    <!-- ... same canvases and settings ... -->
    
    <RootPart Id="YourBodyPart">
        <Part Id="YourHeadPart"/>
        <Part Id="YourLegsPart"/>
    </RootPart>
    
    <!-- ... orbital and physical parameters ... -->
</Vehicle>
```

## Step 8: Add Vehicle to System

### For EarthOnly System
Edit `EarthOnly.xml`, add before `</System>`:
```xml
<LoadFromLibrary Id="YourVehicle" Parent="Earth" />
```

### For Solar System
Edit `SolSystem.xml`, add near other Earth vehicles:
```xml
<LoadFromLibrary Id="YourVehicle" Parent="Earth" />
```

## Step 9: Test

1. Save all XML files
2. Launch Kitten Space Agency
3. Check Tracking Station - your vehicle should appear in orbit
4. Select it and switch to it
5. Test RCS controls (N to toggle RCS, Q/E for roll, W/S for pitch, A/D for yaw)

## Troubleshooting

### Game Crashes on Load

- **Missing Normal/RoughMetalAo textures**: All three texture types are REQUIRED
- **Invalid GLB file**: Check that your GLB has proper vertex attributes (POSITION, NORMAL, TEXCOORD_0)
- **XML syntax error**: Validate your XML changes

### Model Not Visible

- **Scene node issue**: GLB scene must reference the correct node
- **Scale too small/large**: Check your model scale in Blender before export
- **Wrong mesh path**: Verify `Path="Meshes/your_model.glb"` matches actual filename

### Only Part of Model Shows

- **Multi-primitive mesh**: KSA only renders first primitive - split your model into separate files
- **Child parts not attached**: Check RootPart hierarchy in Astronomicals.xml

### Black/Missing Textures

- **Inverted normals**: In Blender, select all faces > Shift+N (recalculate normals)
- **Wrong texture path**: Verify texture filenames match exactly in PbrMaterial definitions
- **Missing textures**: Check all three texture files are in Textures folder

### RCS Not Working

- **Wrong exhaust directions**: Check ExhaustDirection vectors are correct
- **Insufficient thrust**: Increase `Thrust N="150"` value
- **No propellant**: Check `<PropellantMass Kg="500" />` is set

## Advanced: Fixing GLB Scene References

If your model doesn't appear in-game, the GLB file's scene might be referencing the wrong node. This happens when Blender exports the scene pointing to an empty node instead of the node containing your mesh.

The scene reference needs to point to the node that has the mesh attached. You can use tools like glTF Viewer or custom scripts to inspect and fix the scene node references in your GLB file.