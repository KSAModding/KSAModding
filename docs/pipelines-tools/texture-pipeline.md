<sub><sup>Tested with: **KSA Version 2026.10.25.5707**, Blender 5.2, KTX-Software 4.4.2</sup></sub>

# Texture Pipeline

This page describes the texture set of a part, the file formats the game reads, the channel order, how to make the files with KTX-Software, and how to preview the game look in Blender. The XML that connects the textures to a part is in the [Parts guide](../modding-guides/parts.md).

!!! note "Build"
    Checked against KSA 2026.10.25.5707 with the shaders and the stock files. KTX2 files made with the commands below (BC7, BC5, Zstd, linear) loaded in the game and showed the right colors. The normal map convention was confirmed in the game. The game changes with every build, so check the shader files under `Content/Core/Shaders` if something looks wrong.

## The texture set

A part material (`PbrMaterial`) needs three textures. All three are required. A material without one throws a `NullReferenceException` when the part is drawn.

| XML element | Content | How the shader reads it |
|---|---|---|
| `Diffuse` | Base color, no baked lighting and no AO | RGB, converted with `pow(x, 2.2)` in the shader. Alpha is not read. |
| `Normal` | Tangent-space normal map | Only R and G are read. The shader rebuilds Z. |
| `AoRoughMetal` | Packed AO, roughness, metalness | R = ambient occlusion, G = roughness, B = metalness |
| `Emissive` (optional) | Glow mask | One channel, multiplied by 1.25, white only |
| `ThinFilm` (optional) | R = thin film thickness, G = heat gradient, B = frost gradient | Heat and frost need `PartModelDynamic`. Frost shows only with the graphics setting `VesselFrost`, which is off by default. |

The channel order of the packed map is R = AO, G = roughness, B = metalness. Older community notes and the developers' first guideline list it as roughness, metalness, AO. That is outdated for this build, and the "neutral" value (128, 0, 255) of the old wiki page gives a mirror. A neutral dielectric is about (255, 128, 0).

Metalness is nearly binary in the stock maps (about 6 percent of the pixels near 0 and 93 percent near 255 in the capsule atlas). Roughness uses the full gray scale.

## Formats

The stock part textures are KTX2 files written by KTX-Software 4.4.2:

| Slot | Format | Size |
|---|---|---|
| Diffuse | BC7 UNORM | 2048 x 2048 |
| Normal | BC5 UNORM (two channels) | 2048 x 2048 |
| AoRoughMetal | BC7 UNORM | 2048 x 2048 |
| Emissive | BC7 or BC4 | 128 to 512 |
| ThinFilm | BC7 UNORM | 2048 x 2048 |

All of them use Zstd supercompression, a linear transfer function and a full mip chain (12 levels for 2048). They are UNORM, not sRGB, for every slot including Diffuse. The shader applies the 2.2 curve itself, so an sRGB-tagged diffuse would probably be decoded twice (not tested). Build 2026.8.5.5168 made the developers' tool treat all textures as linear data.

The loader chooses by file extension:

| Extension | Result |
|---|---|
| `.ktx2`, `.ktx` | Block-compressed data is uploaded as stored. Basis UASTC or ETC1S data is transcoded to uncompressed RGBA8. |
| `.png`, `.jpg`, `.tga`, `.bmp` and more | Loaded as uncompressed RGBA8 (4 bytes per texel, about 21 MiB with mips at 2048 x 2048, four times a BC7 file). Mips are generated. |
| `.dds` | Loaded as stored. BC7 inside DDS was not tested. |

A PNG path in the XML is the shortest way for a first test, because it needs no conversion. Convert to KTX2 for a release to save video memory. Vessel textures never stream and stay fully resident, and the player option `VesselTextureCap` can drop the top mips of a texture with mips. Use `Category="Vessel"` on every part texture.

A sidecar file `<texture>.toml` may set `max`, `mipMaps` and `format`. The stock part textures have none.

## The normal map

- Use the OpenGL convention: red = right, green = up in the image. This is the Blender default for a baked tangent-space normal map. Set the image color space to Non-Color. This was confirmed in the game: a raised plate with ramps is lit on the side that faces the light.
- The stock files store two channels (BC5). A normal map with three channels works too, because blue is ignored. A flat normal is (128, 128, 255).
- The mesh needs no tangents. The shader builds the tangent frame from screen-space derivatives of the position and the UV. Bake or paint the map for the UV layout you export, and do not flip single UV islands by hand, because a mirrored island gets a mirrored frame.
- A normal map shows slopes only. A raised detail with vertical sides and a height of a few millimetres is invisible at 256 pixels per metre. A ramp of 1 to 2 cm shows.
- The `Power` attribute of the `Normal` element is not read for parts. It does not scale the strength.

## Baking AO into the R channel

Ambient occlusion goes into the red channel of the `AoRoughMetal` map and never into the diffuse. The shader multiplies the stored AO with a screen-space AO and applies it to the ambient light only. AO baked into the diffuse would darken the direct sunlight too.

1. Unwrap the part on the atlas layout.
2. Bake with Cycles, bake type Ambient Occlusion, to a 2048 x 2048 image set to Non-Color. Bake with the subparts in their part positions (an endcap on its tube), so that the contact darkening is right, and move them back to the origin before the export.
3. Bake or paint roughness. Metalness has no direct bake type. Plug the value into an Emission shader and bake Emit, or paint it. Keep metalness black or white.
4. Pack the three gray images into one RGB image with Separate/Combine Color nodes or a short Pillow script, and save it as PNG in linear data.

The DIFFUSE color bake of Cycles is black on metallic surfaces. Bake the base color through an Emission shader instead. An open tube has no inside, so "Recalculate Outside" can turn it inside out. Set the normal direction per face, because the game culls back faces.

## Texel density and trim sheets

Texel density is the number of atlas pixels per metre of surface at 2048 x 2048. The stock values, measured from the UVs of the stock meshes:

| Kind of part | Typical density |
|---|---|
| Most small parts (electrical, coupling, structural, utility, landing) | about 350 pixels per metre |
| Large tanks and fairings (2 m skins 213 to 240, 3 m skins 162 to 216) | 160 to 250 |
| Capsule | about 400 |
| RCS | about 470 |
| Hatches | about 520 |
| Interior props | 690 to 760 |

The UV test grid in the stock capsule atlas is labeled "350px / meter". Use about 350 for ordinary detail, about 200 for large smooth bodies and 500 or more for small parts the player clicks. Keep the density equal on all faces of one part. At 350 pixels per metre one 2048 atlas covers about 34 square metres of surface.

The sampler repeats, so UVs outside 0 to 1 tile. The stock fuel tank skins do not unwrap each skin on its own. They map their faces onto a few shared strips of the atlas (a trim sheet), and the main panel strip repeats around the tank. That is how 33 subpart meshes share one 2048 atlas. For a single new part a unique unwrap is simpler. For a family of parts a trim sheet saves atlas space.

Stock parts share one texture set per family. One material means one set of three bindless texture slots, and the table has 1024 slots for the whole game. Share an atlas between your subparts.

## Making KTX2 files with KTX-Software 4.4.2

Download KTX-Software 4.4.2 from the releases of the KhronosGroup/KTX-Software repository on GitHub and make sure `ktx` is on your path. The stock files carry the writer string `ktx transcode v4.4.2` and Zstd level 20. Each map takes two commands: encode the PNG to UASTC, then transcode to a BC format.

Diffuse (BC7):

```bash
ktx create --format R8G8B8A8_UNORM --assign-tf linear --generate-mipmap --encode uastc --uastc-quality 2 MyMod_TextureAtlas_Diffuse.png tmp_diffuse.ktx2
ktx transcode --target bc7 --zstd 20 tmp_diffuse.ktx2 MyMod_TextureAtlas_Diffuse.ktx2
```

AoRoughMetal (BC7). Add `--input-swizzle rgb1`:

```bash
ktx create --format R8G8B8A8_UNORM --assign-tf linear --generate-mipmap --encode uastc --uastc-quality 2 --input-swizzle rgb1 MyMod_TextureAtlas_PBR.png tmp_pbr.ktx2
ktx transcode --target bc7 --zstd 20 tmp_pbr.ktx2 MyMod_TextureAtlas_PBR.ktx2
```

Normal (BC5). The swizzle `rrrg` is required:

```bash
ktx create --format R8G8B8A8_UNORM --assign-tf linear --generate-mipmap --encode uastc --uastc-quality 2 --input-swizzle rrrg MyMod_TextureAtlas_Normal.png tmp_normal.ktx2
ktx transcode --target bc5 --zstd 20 tmp_normal.ktx2 MyMod_TextureAtlas_Normal.ktx2
```

The BC5 transcoder takes its two channels from R and A of the UASTC data. Without the swizzle the second channel comes out as a constant 255, and the normal map is broken without any error message.

Emissive or alpha mask (BC4, one channel):

```bash
ktx create --format R8_UNORM --assign-tf linear --generate-mipmap --encode uastc tmp_in.png tmp_mask.ktx2
ktx transcode --target bc4 --zstd 20 tmp_mask.ktx2 MyMod_TextureAtlas_Emissive.ktx2
```

Check a result with `ktx info <file.ktx2>`. It should show the BC format, Zstd supercompression, the linear transfer function and the full list of mip levels. A round trip of the stock capsule maps through these commands gave a mean error below 0.1 of 255 per used channel.

## Previewing the game look in Blender

The game does not use the Principled BSDF, so the match is close and not exact. Lighting and color management change the look more than the material does.

Material nodes for one atlas:

| Game slot | Blender nodes |
|---|---|
| Diffuse | Image Texture, color space sRGB, to Base Color. |
| AoRoughMetal | Image Texture, Non-Color, to Separate Color. G to Roughness, B to Metallic. R is the AO. |
| Normal | Image Texture, Non-Color, to a Normal Map node (space Tangent, the atlas UV map), to the Normal input. No green flip. |
| Emissive | Image Texture to Emission Color, Emission Strength 1.25. |

Settings that bring the viewport close to the game:

- **AO only on ambient light.** The game multiplies AO into the ambient light and never into the sun. Plug the AO into a Multiply node on the base color with a factor you can set, and keep the factor at 0 for a sunlit preview. A factor of 1 darkens the lit side and makes the part look duller than in the game.
- **A dark world and a hard sun.** In space a part is lit by one hard sun and little ambient light. Use a nearly black world and a sun light with a strength of about 6 and an angle of about 0.5 degrees. A light gray world flattens the shading.
- **Rendered mode, not Material Preview.** Material Preview uses a studio HDRI and ignores the scene lights, so everything looks soft and matte.
- **View transform Khronos PBR Neutral.** The default AgX desaturates more than the game. The game tone maps with Hable by default, and Blender has no Hable transform. Khronos PBR Neutral is the closest choice to judge texture colors.
- **EEVEE ray tracing** (Render properties > Raytracing) gives reflections on metal parts. Without it low-roughness metal looks dull.
- **Workbench** shows the diffuse texture only. It ignores normal maps, roughness and metalness, so use it for shape and UV checks. EEVEE with ray tracing and Cycles use the full material.
- In Local View only the isolated objects take part, so the sun is missing unless it is in the local view too.

The game BRDF is GGX with alpha = roughness squared and F0 = 0.04 for dielectrics, the same mapping as the Principled BSDF at the default Specular IOR Level of 0.5, so the roughness and metalness values mean the same in both. Since build 2026.10.13.5602 the game adds specular anti-aliasing, which makes smooth curved metal a little rougher at grazing angles than in Blender. Since build 2026.9.22.5482 the roughness is read correctly, so older screenshots show a different look.

Textures sometimes look different in the game than in Blender, and the developers' artist checks the final look in the game. Plan to iterate there.
