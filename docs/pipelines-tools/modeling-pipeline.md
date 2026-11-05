<sub><sup>Tested with: **KSA Version 2026.10.25.5707**, Blender 5.2</sup></sub>

# Modeling Pipeline

This page describes the path of a part mesh from Blender to the game: units, axes, naming, the export settings, what the game reads from a GLB file, and what it costs to draw. The XML that uses the meshes is in the [Parts guide](../modding-guides/parts.md). Textures are in the [Texture Pipeline](../pipelines-tools/texture-pipeline.md).

!!! note "Build"
    Checked against KSA 2026.10.25.5707 with the game code and the stock files. A part made with these settings loaded and rendered in the game with no error in the log, and the axes and the normal map convention below were confirmed by that test. Radial parts and the editor behavior were not tested. The developers announced a large change of the part XML for the next build (`Ref` / `KeyRef` data references, `Modify="Patch|Replace|Remove"`, part variants by A/B/C name suffix, module templates in `Components`). The GLB rules on this page may also change then.

## Steps

1. Set up the scene: metric units, unit scale 1.0.
2. Model one object per subpart, with the stack axis on Blender X.
3. Name objects and mesh data, apply transforms, add view meshes and markers.
4. Unwrap one UV map and texture it.
5. Export a mesh atlas GLB (and optionally a part layout GLB).
6. Write or generate the XML and put everything into a mod folder.

## Units and axes

- One glTF unit is one metre. Set the Blender scene to Metric with Unit Scale 1.0. The stock tank skin `CoreFuelTankA_Subpart_Skin3W6HA` measures 6 x 3 x 3 units for a part 3 m wide and 6 m long.
- The game does no axis conversion. The glTF positions are the part positions, and the node transforms of a part layout GLB go into the XML unchanged.
- The part stack axis is +X. The nose points to +X. Engines fire toward -X. Round parts have their symmetry axis on X.
- Export with `+Y Up` off. Then Blender (x, y, z) is the part (x, y, z), and Blender locations and XYZ Euler angles equal the XML `Position` and `Rotation` values. With `+Y Up` on, Blender (x, y, z) becomes (x, z, -y) and a half turn about Blender Z becomes a turn about the part Y axis. Use the same setting for all files of one mod.
- Use the stock diameters if the part must fit stock parts: 0.5, 1, 2 and 3 m (and a 2.5 m capsule). There is no 4 m part in this build.
- Round skins of the standard sizes use 48 sides in the stock game. It is a convention, not a rule of the engine, and not every stock cylinder follows it.

## Subparts, names and transforms

A part is a list of subparts, and a subpart is one mesh with one material. Model every piece that repeats once and place it many times with `InstanceOf` in the XML.

| Rule | Reason |
|---|---|
| One Blender object per subpart, named `<Prefix>_Subpart_<Name>`. | The generated XML uses the node name as the SubPart Id and the Mesh Id. |
| Rename the mesh data block to the object name. | The game registers a mesh by the glTF mesh name, which is the data name. The import code uses the object name. They must be equal. |
| Put a mod prefix on every name. Do not use an underscore inside the prefix. | Ids are global across all mods and the first registered one wins silently. The import code takes the set name from the file name up to the first underscore. |
| A subpart name must not end in a digit or a dot. | The import code removes trailing digits and dots from an instance name to find the subpart (see below). |
| Apply all transforms (Object > Apply > All Transforms) in the mesh atlas. | The game ignores node transforms in an atlas, so the mesh origin is the pivot. The import code warns about unapplied transforms. |
| Use one material slot and export with no materials. | Only primitive 0 of a mesh is drawn, and the material comes from the XML. |
| Keep the origin near the geometry. | The bounding sphere is measured from the origin. It is used for light shadow culling and picking. |

Use sharp edges where the surface has a hard edge and smooth shading elsewhere. Do not mirror an instance with a negative scale: back faces are culled, so a mirrored copy can show its inside.

## The view mesh

For each visible subpart add a low-polygon mesh named `<Subpart>_VM`: a closed shape with the same bounds and the same frame, transforms applied. The game never draws it. It uses it for mouse picking in the editor, for part bounds, for tank particle spawn points and for debris. A subpart without it renders but cannot be picked. The stock `Skin3W6HA` has 1344 triangles and a view mesh with 384. Give the view mesh a UV map (any content), and do not texture it. It is uploaded to the GPU like every mesh in an atlas, so keep it simple.

## Collider and flag markers

Colliders are XML shapes at run time. The import code builds them from child objects of a subpart. Use Empties for primitives.

| Child name starts with | Result | Size from the object scale |
|---|---|---|
| `_ColPrim_Box` | Box | Edge lengths = scale X, Y, Z. The marker is a unit cube. |
| `_ColPrim_Sphere` | Sphere | Radius = 0.5 * scale X |
| `_ColPrim_Cylinder` | Cylinder | Length (on Y) = scale Y, radius = 0.5 * scale X |
| `_ColPrim_Capsule` | Capsule | Like the cylinder. The caps extend beyond each end by the radius. |
| `_ColPrim_Hull` | Convex hull | Needs a closed, convex, smooth-shaded mesh, merged by distance (0.002 m), with mesh data named like the object. |

- Do not apply the scale of a marker, because its scale is its size.
- The object translation is the collider location and the rotation is the collider rotation.
- A cylinder or capsule is long along its local Y axis, while round part geometry is long along X.
- A stretched cross-section (scale X and Z differ by more than 1 percent) is skipped with a warning.
- Hull meshes are copied into a separate `<Stem>_CollisionAtlas.glb` and loaded with `HostOnly="true"`, so they take no GPU memory. A hull that has no volume throws when the game loads it.
- Prefer boxes, cylinders and capsules. Hulls are for shapes that primitives cannot fit.

Other child markers (Empties) set flags of the subpart: `_glass` makes the model a glass model, `_raytrace` and `_raytraceShadowProxy` set the ray tracing mode.

## Exporter settings

Use File > Export > glTF 2.0 (.glb). Save the settings as an operator preset so that every export uses them. The names are those of the Blender 5.2 export dialog.

| Panel and label | Setting | Why |
|---|---|---|
| Format | glTF Binary (.glb) | One binary file. |
| Include > Limit to: Selected Objects | on | Keeps other scene content out. |
| Transform > +Y Up | off | The game does no axis conversion. |
| Data > Mesh > Apply Modifiers | on | The game reads final geometry only. |
| Data > Mesh > UVs | on | TEXCOORD_0. |
| Data > Mesh > Normals | on | NORMAL, also on view meshes. |
| Data > Mesh > Tangents | off | Not read. The shader derives tangents. |
| Data > Mesh > Use Vertex Color | None | Not read. |
| Data > Mesh > Attributes, Loose Edges, Loose Points | off | Not needed. |
| Data > Material > Materials | No Export | One primitive per mesh. The other settings keep one primitive per material slot, and only the first is drawn. |
| Data > Material > Images | None | Images are never read for parts. An embedded texture without a name crashed the game in an earlier build. |
| Data > Compression (Draco) | off | Not supported. |
| Data > Include > GPU Instances | off | Not supported. |
| Data > Include > Custom Properties | off | Not read for parts. |
| Animation | off for atlas and layout files | Ignored. |
| Data > Skinning, Shape Keys, Cameras, Punctual Lights | off | Not read. |

## What the loader reads

The game reads from each glTF mesh:

- `POSITION`, as float VEC3, with the accessor `min` and `max` set (the Blender exporter writes them).
- `NORMAL` and `TEXCOORD_0`, as float.
- The index buffer. A mesh without indices does not load.

Not read: tangents, vertex colors, a second UV set, materials, images, skins, cameras. Draco and meshopt files fail. Quantized, sparse or interleaved accessors are not supported. A missing `NORMAL` or `TEXCOORD_0` is filled with zeros: the mesh loads, but lights and textures wrongly. Scenes, nodes and node transforms of an atlas are not read. The game draws only primitive 0 of a mesh.

Every mesh in the atlas, including view meshes, is stored in one shared GPU vertex buffer (32 bytes per vertex) and one index buffer for the whole session.

## The part layout GLB

The Assets XML can be written by hand. If you generate it with the developers' import code, you also export a second GLB per part, named `<Prefix>_Prefab_<Part>.glb`. It is only a layout. The game never reads it, and the mesh data inside it is not used. Only names and node transforms count.

- Make one object per subpart instance, at the root of the scene, placed where it belongs in the part.
- Give every instance its own mesh data, named `<Subpart>1`, `<Subpart>2` and so on. Use real copies (Shift+D), not linked duplicates (Alt+D), because linked duplicates share one mesh data block and get the same Id. The import code reads the instance Id from the mesh data name.
- The subpart is found by removing trailing digits and dots from that name (regex `[.\d]+$`). `Cap1` and `Cap.001` both map to `Cap`. This is why a real subpart name must not end in a digit.
- Instances use their local transform only. Parent transforms are not composed, so keep everything at the root.
- Add `_connector<Name>` Empties at the root for the connectors. The local +X axis of the Empty points out of the part. Keep the local Z axis of a connector on the center line along the part Z axis, or all attached parts roll.
- A child named `_pivot*` under an engine instance gives the instance a gimbal pivot.

## Instancing and performance

How the renderer treats parts decides which modeling choices cost something.

- All instances of one `PartModel` Id are drawn with one instanced indirect draw call, in every vehicle. Reusing a subpart saves memory and CPU time. It does not save vertex work, because every instance runs the vertex shader in every pass.
- There is no part LOD and no per-part or per-vehicle culling. Every triangle draws at every distance until the whole vehicle is smaller than one pixel. An instance is drawn in the depth pre-pass, the color pass and every sun shadow cascade (up to 6).
- Count triangles x instances x passes. A 5000-triangle piece used 20 times is 100000 triangles per pass. Spend triangles on the silhouette and use the normal map for flat detail.
- Do not merge repeated pieces to save draw calls, because there is no draw call per subpart. Do not split a part into many tiny unique subparts either: every unique template adds a draw record and per-frame CPU work.
- Mark interior-only subparts `Internal` in the XML. They are skipped in exterior views.
- Use `PartModelDynamic` only for subparts that need heat, frost or a gimbal.

Triangle numbers of the stock parts (472 subpart meshes in 19 mesh atlases, without view meshes and markers):

| Measure | Triangles |
|---|---|
| Median subpart | 316 (384 for exterior parts) |
| 90th percentile | 1664 |
| Largest exterior subpart | 7450 (`CorePropulsionA_Subpart_EngineA1WBase`) |
| Tank skin, 1 m / 2 m / 3 m (median) | 672 / 1248 / 1344 |
| Tank endcap (median) | 1920 |
| RCS subparts (median) | 229 |
| Finished part (all instances), median / 90th percentile / maximum | 2632 / 7840 / 35846 |

A new part of 2000 to 8000 triangles in total, with single pieces of a few thousand at most, is in line with the stock parts. This is a guide taken from the stock numbers, not a limit of the game.

Memory notes: vessel textures do not stream, so every texture stays in video memory. The bindless texture table has 1024 slots for all textures of all mods and cannot grow, so share one atlas between your subparts.

## The developers' import code

The developers' tool that turns Blender GLB files into the Assets XML is called `GlbToXmlUtility`. Since build 2026.7.10.5056 its code is inside `KSA.dll` in the namespace `KSA.GlbImport`. The classes are public, but the game does not call them and no tool program is shipped. A small console program that references `KSA.dll` can call `PartInputSet.FromDirectory`, `PartAssetBundler.Build` and `ToolXml.Write` to produce the XML without starting the game. The code is not documented and can change with each build. Read every warning it prints. They point at problems in the Blender file, such as unapplied transforms, rolled connectors or flat shading on a hull.

The naming rules above come from this code. In short:

| Name | Result |
|---|---|
| `<Stem>_MeshAtlas.glb` | The atlas. One per set. |
| `<Stem>_Prefab*.glb` | One Part per file, named like the file. |
| Node in the atlas without leading `_` and without `_VM` suffix | One SubPart. The SubPart Id, the Mesh Id and the node name are equal. The model Id is `<name>_Model`. |
| Node `<name>_VM` | The `MeshView` of `<name>`. |
| Node with a leading `_` | Not a subpart. |
| Prefab node with a mesh | A SubPart instance. |
| Prefab node `_connector*` | A connector. |

The tool writes the XML for meshes, materials, subparts, parts, connector transforms and colliders. It does not write the `PartGameData`, `mod.toml`, or convert images to KTX2.
