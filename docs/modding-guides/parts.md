<sub><sup>Tested with: **KSA Version 2026.10.25.5707**</sup></sub>

# Parts Modding Guide

This guide explains how to add a new part to Kitten Space Agency (KSA) as a content mod: the mod folder, the XML files that describe the part, and how the names in the XML connect to the meshes in your GLB file. A minimal complete example is at the end.

For the Blender side see the [Modeling Pipeline](../pipelines-tools/modeling-pipeline.md) and the [Texture Pipeline](../pipelines-tools/texture-pipeline.md).

!!! warning "The part XML changes in the next build"
    The developers announced a large change of the part XML for the build after 2026.10.25.5707: data references with `Ref` and `KeyRef`, a `Modify="Patch|Replace|Remove"` attribute on elements in `<Assets>`, part variants grouped by an A/B/C suffix in the part name, and module templates moved into a `Components` collection. This page describes build 2026.10.25.5707. The XML details will need an update when the next build is out.

## Overview

A part mod is a folder with a `mod.toml`, one or more XML files, a GLB file with the meshes and the texture files. The game loads it directly. A mod without code needs no DLL and no StarMap.

A part has five levels:

| Level | XML element | Meaning |
|---|---|---|
| Mesh | glTF mesh inside a `MeshAtlas` GLB | The triangles, registered by the glTF mesh name. |
| Material | `PbrMaterial` | The texture set. Usually one per mesh atlas. |
| SubPart | `SubPart` with a `PartModel` | A reusable piece: one mesh with one material. |
| Part | `Part` | The thing the player picks. It lists SubPart instances with a transform, and connectors. |
| Game data | `PartGameData` | What the part does: editor tag, diameter, mass, tank, colliders, connector flags. |

## Mod folder and mod.toml

A mod is a folder with a `mod.toml`. The folder name is the mod Id. The game searches two places:

- `Documents/My Games/Kitten Space Agency/mods/<ModId>/` (this path survives game updates)
- `<game folder>/Content/<ModId>/`

```text
MyMod/
  mod.toml
  MyModAssets.xml
  MyModGameData.xml
  Meshes/MyMod_MeshAtlas.glb
  Textures/MyMod_TextureAtlas_Diffuse.ktx2
  Textures/MyMod_TextureAtlas_Normal.ktx2
  Textures/MyMod_TextureAtlas_PBR.ktx2
```

```toml
name = "MyMod"
assets = [
    "MyModAssets.xml",
    "MyModGameData.xml",
]
```

For a part mod only `name` and `assets` matter. `assets` lists the XML files of the mod, relative to the mod folder. All `Path` values inside the XML are relative to the mod folder too.

Things that fail without a message:

- A file in `assets` that does not exist is skipped.
- An element name that the game does not know is ignored. A misspelled element gives a subpart without a model, or a material without a texture.
- A new mod is added to `Documents/My Games/Kitten Space Agency/manifest.toml` as `enabled = false`, and the game asks the player to confirm it. Enable it in the dialog, or add `[[mods]]`, `id = "MyMod"`, `enabled = true` by hand. Mods load in the order of the manifest, Core first.

An XML syntax error is not silent: the whole file is dropped and the game logs the exception.

## Assets files and GameData files

The split into two kinds of files is a convention of the stock game. The parser accepts every element in both.

| | Assets files (`Core*Assets.xml`) | GameData files (`Core*GameData.xml`) |
|---|---|---|
| Says | What the part looks like | What the part does |
| Content | `MeshAtlas`, `PbrMaterial`, `SubPart`, `Part` with instances and connector transforms | `PartGameData`: editor tag, diameter, mass, tank, engine, colliders, connector flags and capabilities |
| Written | Generated from the GLB files (see below) | By hand |

A `PartGameData` element is merged into the `Part` with the same Id after all files are read. Lists are appended. A `Connector` with the Id of a part connector is merged into it, and Flags and Capabilities are combined. A `PartGameData` whose Id matches no `Part` is ignored without a message, so a typo in the Id is a silent failure.

## MeshAtlas

```xml
<MeshAtlas Path="Meshes/MyMod_MeshAtlas.glb" />
```

`MeshAtlas` loads a GLB file and registers every glTF mesh in it under its glTF mesh name (`meshes[i].name`). Scenes, nodes and node transforms are not read. Every mesh is uploaded to the GPU, including the view meshes that are never drawn. For meshes that the game only needs on the CPU (convex hull colliders) add `HostOnly="true"`.

The old `MeshFile` element loads only the first mesh and gives a part model no GPU data. Use `MeshAtlas` for parts. It has no `Category` attribute.

Which attributes the loader reads and how to export them is on the [Modeling Pipeline](../pipelines-tools/modeling-pipeline.md) page.

## PbrMaterial

```xml
<PbrMaterial Id="MyMod_Material">
  <Diffuse      Path="Textures/MyMod_TextureAtlas_Diffuse.ktx2" Category="Vessel" />
  <Normal       Path="Textures/MyMod_TextureAtlas_Normal.ktx2"  Category="Vessel" />
  <AoRoughMetal Path="Textures/MyMod_TextureAtlas_PBR.ktx2"     Category="Vessel" />
</PbrMaterial>
```

`Diffuse`, `Normal` and `AoRoughMetal` are all required. The game reads them without a null check, so a material without one of them throws a `NullReferenceException` when it is drawn. Optional slots are `Emissive` and `ThinFilm`.

The `AoRoughMetal` image packs three values in the glTF "ORM" order:

| Channel | Content |
|---|---|
| R | Ambient occlusion |
| G | Roughness |
| B | Metalness |

A neutral placeholder is about (255, 128, 0): no occlusion, medium roughness, no metal. Older community notes list the order as roughness, metalness, AO and a placeholder of (128, 0, 255). That is wrong for this build and gives a mirror-like metal.

Use `Category="Vessel"` on every part texture. Formats, color spaces and file creation are on the [Texture Pipeline](../pipelines-tools/texture-pipeline.md) page.

A `PbrMaterial` with an Id that already exists becomes a reference to the existing material. A mod material called like a stock material shows the stock textures.

## SubPart

```xml
<SubPart Id="MyMod_Subpart_Tube">
  <PartModel Id="MyMod_Subpart_Tube_Model">
    <Mesh Id="MyMod_Subpart_Tube" />
    <Material Id="MyMod_Material" />
  </PartModel>
  <MeshView>
    <Mesh Id="MyMod_Subpart_Tube_VM" />
  </MeshView>
</SubPart>
```

A subpart has one model element. `Mesh Id` is the glTF mesh name of a mesh in a loaded atlas. `Material Id` is a `PbrMaterial` Id. The old name `SubPartModel` does not exist in this build.

| Model element | Use |
|---|---|
| `PartModel` | The default. |
| `PartModelDynamic` | For parts that show heat glow or frost, and for engines that gimbal. It cannot show the emissive map. |
| `PartModelGlass` | For glass, drawn in its own pass. |

The Id of a `PartModel` must be unique. The renderer draws all instances of one `PartModel` Id with one instanced draw call, and a second `PartModel` with the Id of an existing one draws the existing mesh. The stock tool names it `<SubPart>_Model`.

Optional children of `PartModel`: `Internal` (drawn only in the interior view), `ShadowCaster`, and `RayTracing`.

### MeshView

`MeshView` names a low-polygon mesh, by convention `<SubPart>_VM`. The game never draws it. It uses it on the CPU for mouse picking in the editor, for bounds, for tank particle spawn points and for debris. A subpart without a `MeshView` renders, but the editor cannot pick it with the mouse. Give every visible subpart a view mesh. It must be a closed shape with the same bounds as the visible mesh.

## Part

```xml
<Part Id="MyMod_Prefab_TankA">
  <SubPart Id="MyMod_Subpart_Tube" InstanceOf="MyMod_Subpart_Tube" />
  <SubPart Id="MyMod_Subpart_Cap" InstanceOf="MyMod_Subpart_Cap">
    <Transform><Position X="0.4" /></Transform>
  </SubPart>
  <SubPart Id="MyMod_Subpart_Cap1" InstanceOf="MyMod_Subpart_Cap">
    <Transform><Position X="-0.4" /><Rotation Z="3.1416" /></Transform>
  </SubPart>
  <Connector Id="_connector1"><Transform><Position X="0.5" /></Transform></Connector>
  <Connector Id="_connector2"><Transform><Position X="-0.5" /><Rotation Z="3.1416" /></Transform></Connector>
</Part>
```

- A child `SubPart` of a `Part` is an instance. `InstanceOf` names the SubPart to place, `Id` is the name of the instance and must be unique inside the part. Place one subpart many times instead of copying its mesh.
- `Transform` has `Position` (metres), `Rotation` (Euler X, Y, Z in radians) and `Scale`. All attributes are optional and default to 0 (Scale: 1). The rotation turns about X first, then Y, then Z, about fixed axes. This is the same as the XYZ Euler mode of Blender.
- If you write a `Scale` element, write all three axes. A missing axis reads as 0, not 1.
- Keep instance scale uniform.
- A Part and a SubPart share one Id registry. They must not share an Id.
- Because Ids are global, `InstanceOf` can name a stock subpart, for example one from `Content/Core/CoreStructuralAAssets.xml`. The stock content then must be loaded, and `mod.toml` has no dependency field. This follows from the code and was not run in the game.

## Axes and units

Units are metres and radians. The game does no axis conversion between the GLB file and the part, so the glTF frame is the part frame.

- +X is the stack axis. The nose points to +X. Engines fire toward -X. A round part has its symmetry axis along X.
- The crew side of the stock capsule is -Z (doors and windows at negative Z).
- Which side of a radial part (landing leg, RCS block) faces the vehicle is not settled. Build one test part and look at it in the editor before you build a set.

## Connectors

- A connector is a `Connector` element in the `Part` with an Id and a `Transform`. The generated XML takes the Id and Transform from nodes named `_connector*` in a part layout GLB.
- The local +X axis of a connector points out of the part. Two connectors meet with opposite +X directions. A tank has one connector at X = +0.5 without rotation and one at X = -0.5 with `Rotation Z="3.1416"`.
- Keep the local Z axis of a connector on the part axis for a connector on the center line. A roll about X rolls every attached part by that angle.
- Flags and Capabilities go into the `PartGameData`, not into the generated XML.

| Flag | Meaning |
|---|---|
| `Internal` | Two `Internal` connectors never connect. |
| `ToSurface`, `FromSurface` | Surface connectors, not a target for a normal connection. |

| Capability | Meaning |
|---|---|
| (none) | The connector carries electricity and service fluid. |
| `BulkFluid` | Fuel flows through it. |
| `DecouplerJoint`, `SolidMotorCase` | Used by decouplers and solid motors. |
| `NoElectricity`, `NoServiceFluid` | Remove those defaults. |

Write the tokens separated by spaces. Whether a comma-separated list works was not tested.

Each stack end of a stock 2 m tank has two connectors: an outer one with `Scale` 2 for the 2 m stack, and an inner one with no `Scale` (1 m) and the flag `Internal`. The game data puts both in one `Aligned` group, and the game joins an attached part to the connector whose size fits. A 1 m engine such as `CorePropulsionA_Prefab_EngineA3` therefore joins the inner connector `_connector46` of `CoreFuelTankA_Prefab_LF2W2HA` and nests 0.082 m inside the skirt. A tank without the inner connector puts the same engine on its outer rim. This was checked in the game with build 2026.10.25.5707. The pattern from `CoreFuelTankAGameData.xml`:

```xml
<Connector Id="_connector46">
    <Capabilities>BulkFluid</Capabilities>
    <Flags>Internal</Flags>
</Connector>
<Aligned>
    <ConnectorRef Id="_connector22"/>
    <ConnectorRef Id="_connector46"/>
</Aligned>
```

The `Scale` of a connector is its size class, the stack diameter in metres: 2.5 on the medium capsule, 2 on a 2 m tank, and 1 (no `Scale` element) on a 1 m engine.

Saved vehicles store connectors by an index (`PartConnectorRef Index`), which is the position of the connector in the Part connector list. Do not reorder or delete the connectors of a released part, or old saves join the wrong connectors.

## Colliders

Colliders are XML shapes in a `Collider` element. They may sit in a SubPart, a Part or the game data.

```xml
<Collider Id="Collider1">
  <Cylinder Id="CylinderCollider1">
    <LocationAsmb X="0" Y="0" Z="0" />
    <Collider2Asmb X="0" Y="0" Z="-1.5708" />
    <LengthY M="1" />
    <Radius M="0.5" />
  </Cylinder>
</Collider>
```

- Shapes: `Box` (`LengthX`, `LengthY`, `LengthZ`), `Sphere` (`Radius`), `Cylinder` and `Capsule` (`LengthY`, `Radius`), `ConvexHull` (a `Mesh Id` from a `HostOnly` atlas).
- `LocationAsmb` is the center in the part frame in metres. `Collider2Asmb` is the Euler rotation in radians.
- A cylinder or capsule has its length along its local Y axis, while a round part has its axis on X. To lay a cylinder along the part, rotate it by -1.5708 about Z, as in the example.
- The generated XML builds colliders from marker nodes in the GLB (see the [Modeling Pipeline](../pipelines-tools/modeling-pipeline.md)).

Give each part colliders. The build 2026.10.25.5707 places vehicles by their colliders.

## EditorTag and Diameter

```xml
<PartGameData Id="MyMod_Prefab_TankA" DisplayName="Example Tank A">
  <EditorTag Value="Fuel Tanks" />
  <Diameter M="1" />
</PartGameData>
```

- A part shows in the editor part list when it is not a SubPart, is not tagged `Hidden` and has an editor tag. The game renders the thumbnail from the models, so you provide no icon.
- The stock tags are Booster, Capsules, Engines, RCS, Fuel Tanks, Electrical, Coupling, Structural, Landing, Interstage, Passage, Cargo, Lights, Interstellar, Radial, NoFaceSnapping, All and Hidden. They are defined by `EditorTagDef` elements in `CoreEditorTagsGameData.xml`. A tag without a definition gives only a warning.
- `Diameter M` feeds the diameter filter of the editor. The stock values are 0.125, 0.25, 0.5, 1, 2 and 3. Use them if the part must fit stock parts. Whether a part without `Diameter` shows in the editor was not tested.
- Without `DisplayName` the game shows the part Id.

## How Ids resolve

Ids are global across all mods, not per mod. The first item registered with an Id wins, and a later duplicate is dropped without a log line.

| XML reference | Resolves to |
|---|---|
| `MeshAtlas Path` | A GLB file. Every glTF mesh in it is registered by its glTF mesh name. |
| `PartModel/Mesh Id`, `MeshView/Mesh Id` | A registered glTF mesh name from any loaded atlas. |
| `PartModel/Material Id` | A `PbrMaterial` Id. |
| `SubPart InstanceOf` | A SubPart Id. |
| `PartGameData Id` | A `Part` Id. |

The consequences:

- Give every Id a mod prefix: parts, subparts, part models, materials, tags and glTF mesh names. A mesh called like a stock mesh silently draws the stock mesh.
- The Mesh Id is the glTF mesh name, which is the mesh data name in Blender, not the object name. Keep both equal.
- A glTF mesh without a name cannot be registered.
- A `SubPart` that names a Mesh Id which no atlas registered throws a `NullReferenceException` ("MeshReference is null") in the startup loop over parts. This stops the whole game load, not only your mod. Check the mesh names first.

## Generating the Assets XML

You can write the Assets XML by hand, as in the example below. The developers' import tool does it from the GLB files: it creates the `MeshAtlas`, the `PbrMaterial`, one `SubPart` per mesh with its `MeshView` and colliders, and one `Part` per layout GLB. Its code is part of `KSA.dll` (namespace `KSA.GlbImport`). The tool program itself is not shipped. The details are on the [Modeling Pipeline](../pipelines-tools/modeling-pipeline.md) page. The tool never writes the `PartGameData`, the texture files or `mod.toml`.

## Minimal complete example

A tank made of one tube and two endcaps. The GLB `Meshes/MyMod_MeshAtlas.glb` holds four meshes with applied transforms and the long axis on X: `MyMod_Subpart_Tube`, `MyMod_Subpart_Tube_VM`, `MyMod_Subpart_Cap` and `MyMod_Subpart_Cap_VM`.

`mod.toml`:

```toml
name = "MyMod"
assets = [
    "MyModAssets.xml",
    "MyModGameData.xml",
]
```

`MyModAssets.xml`:

```xml
<Assets>
  <MeshAtlas Path="Meshes/MyMod_MeshAtlas.glb" />
  <PbrMaterial Id="MyMod_Material">
    <Diffuse Path="Textures/MyMod_TextureAtlas_Diffuse.ktx2" Category="Vessel" />
    <Normal Path="Textures/MyMod_TextureAtlas_Normal.ktx2" Category="Vessel" />
    <AoRoughMetal Path="Textures/MyMod_TextureAtlas_PBR.ktx2" Category="Vessel" />
  </PbrMaterial>
  <SubPart Id="MyMod_Subpart_Tube">
    <PartModel Id="MyMod_Subpart_Tube_Model">
      <Mesh Id="MyMod_Subpart_Tube" />
      <Material Id="MyMod_Material" />
    </PartModel>
    <MeshView><Mesh Id="MyMod_Subpart_Tube_VM" /></MeshView>
  </SubPart>
  <SubPart Id="MyMod_Subpart_Cap">
    <PartModel Id="MyMod_Subpart_Cap_Model">
      <Mesh Id="MyMod_Subpart_Cap" />
      <Material Id="MyMod_Material" />
    </PartModel>
    <MeshView><Mesh Id="MyMod_Subpart_Cap_VM" /></MeshView>
  </SubPart>
  <Part Id="MyMod_Prefab_TankA">
    <SubPart Id="MyMod_Subpart_Tube" InstanceOf="MyMod_Subpart_Tube" />
    <SubPart Id="MyMod_Subpart_Cap" InstanceOf="MyMod_Subpart_Cap">
      <Transform><Position X="0.4" /></Transform>
    </SubPart>
    <SubPart Id="MyMod_Subpart_Cap1" InstanceOf="MyMod_Subpart_Cap">
      <Transform><Position X="-0.4" /><Rotation Z="3.1416" /></Transform>
    </SubPart>
    <Connector Id="_connector1"><Transform><Position X="0.5" /></Transform></Connector>
    <Connector Id="_connector2"><Transform><Position X="-0.5" /><Rotation Z="3.1416" /></Transform></Connector>
  </Part>
</Assets>
```

`MyModGameData.xml`:

```xml
<Assets>
  <PartGameData Id="MyMod_Prefab_TankA" DisplayName="Example Tank A">
    <EditorTag Value="Fuel Tanks" />
    <Diameter M="1" />
    <Collider Id="Collider1">
      <Cylinder Id="CylinderCollider1">
        <LocationAsmb X="0" Y="0" Z="0" />
        <Collider2Asmb X="0" Y="0" Z="-1.5708" />
        <LengthY M="1" />
        <Radius M="0.5" />
      </Cylinder>
    </Collider>
    <Connector Id="_connector1"><Capabilities>BulkFluid</Capabilities></Connector>
    <Connector Id="_connector2"><Capabilities>BulkFluid</Capabilities></Connector>
  </PartGameData>
</Assets>
```

This example has no tank, mass or engine data. The tank and mass elements (`Tank`, `CustomMass`, `SolidCylinderMass` and others) are in the stock game data files, for example `CoreFuelTankAGameData.xml`. Check their values against a stock part, because the volume depends on them.

## Testing and troubleshooting

The log folder is `Documents/My Games/Kitten Space Agency/logs/`. Search it for `ERROR`, `not found at`, `is null for`, `fails during construction` and your mod Id. The game builds every part once at load and logs `Part '<id>' fails during construction` with the exception, so a broken part shows its error at start.

| Symptom | Cause |
|---|---|
| Mod not found | No `mod.toml`, or the new mod is still disabled in `manifest.toml`. |
| Nothing from your XML | A wrong file name in `assets`. The file is skipped without a message. |
| `mod file '<Id>' ... not found at '<path>'` | A mesh or texture path is wrong. |
| `MeshReference is null for '<id>'` at start | A `Mesh Id` in a SubPart is in no loaded atlas. |
| Part is in no category | The `EditorTag` is missing, or the `PartGameData` Id has a typo (no message). |
| Stock mesh or stock textures show | Duplicate Id. No message. |
| Crash when the part is drawn | The material lacks `Diffuse`, `Normal` or `AoRoughMetal`. |
| Part cannot be clicked in the editor | The subpart has no `MeshView`. |

There is no hot reload for meshes or textures. Restart the game after every change of a file.

## What is not covered

This page does not cover engines, thrusters, power modules, part animations or docking ports. Read the stock `Core*GameData.xml` files for those. An older version of this guide described editing `PartAssets.xml` and `Astronomicals.xml` in the game folder and a `Thruster` element. None of that applies to this build: mods add their own XML through `mod.toml`.
