# Tools Index

This page lists community and official tools that help with KSA modding. Where possible it links to the official repository or download page of a tool and does not host the tool itself.

## Parts and 3D content

| Tool | Use | Tested with |
|---|---|---|
| [Blender](https://www.blender.org/download/) | Modeling, UV unwrapping, baking and glTF export of parts. Use the glTF 2.0 exporter with the settings on the [Modeling Pipeline](modeling-pipeline.md) page. | Blender 5.2, KSA 2026.10.25.5707 |
| [KTX-Software](https://github.com/KhronosGroup/KTX-Software/releases) | The `ktx` command line tool converts PNG images to the KTX2 files (BC7, BC5, BC4 with Zstd) that the stock parts use. The commands are on the [Texture Pipeline](texture-pipeline.md) page. | KTX-Software 4.4.2, KSA 2026.10.25.5707 |
| [flexo](https://github.com/meow-sci/flexo) | A community browser tool for parts: it imports a Blender GLB and exports a mod folder with the XML. Its documents describe the KSA file formats. Parts exported with it use uncompressed KTX2 textures, which need more video memory than the stock BC7 files. Check its output against the [Parts guide](../modding-guides/parts.md) before you release. | Not tested against build 2026.10.25.5707 |

## Mod loading

| Tool | Use |
|---|---|
| [StarMap](https://github.com/StarMapLoader/StarMap/releases/latest) | The code loader for mods that contain a DLL. See [Installing Tools](../getting-started/install-tools.md). A part mod with only XML, meshes and textures does not need it. |

## Notes

- The developers' own GLB to XML code is part of `KSA.dll` and is described on the [Modeling Pipeline](modeling-pipeline.md) page.
- Tools for the part XML will need a recheck when the next build changes the XML (see the [Parts guide](../modding-guides/parts.md)).
