# Astronomicals.xml (&lt;Assets>)

This information is for version 2025.8.33.2091.

All tags use example data from the Astronomicals.xml file in Core. _If something is highlighted like this, (italics) that means the information is not confirmed and/or is unclear._

This file adds planets, moons, comets, rockets, kitten EVAs, and the star of the system. This data can be loaded from a different file, see [SolSystem.xml (&lt;System>)](google.com). (link not added yet)

## <StellarBody Id="Sol">
Specifies a star. This is used as a base for a system. `Id` specifies the name of the star, this shows up in-game as well.

Descendants:
- `<MeanRadius Km="696342" />`: Used for the radius of the star.
- `<Mass Suns="1" />`: Used for the mass of the star.
- `<Diffuse Path="Textures/Sol_Diffuse.jpg" Category="Terrain"/>`: The diffuse texture map to use for the star. Shows up as the surface, however getting close to the sun breaks the game as of version 2025.8.11.2945. Category seems to always be "Terrain", however it would make sense for it to be different.
- `<Color R="1" G="1" B="1" />`: Overall color of the star when seen.
- `<Sunlight R="9" G="9" B="9" />`: Color of the sunlight the star produces, slightly modifying the surfaces it hits. This can be higher than the normal 0 to 1 range to make sunlight persist farther.

## <Vehicle Id="Gemini7" Parent="Earth">
Specifies a vessel to be spawned. `Id` specifies the name of the vessel, this shows up in-game as well. `Parent` specifies the object the vessel is orbiting.

Descendants:
- `<PartRef InstanceOf="Core_Command_A_Prefab_MediumCapsule_VariantA">`: Specifies a part and can include other `<PartRef>` tags inside. `InstanceOf` specifies the part object to load.
  - `<PartRef InstanceOf="Core_ServiceModule_A_Prefab_MediumServiceModuleA">`: Specifies a part and can include other `<PartRef>` tags inside. `InstanceOf` specifies the part object to load.
    - `<Transform>`: Specifies the location of the part.
      - `<Position X="-1.4197" Y="-1.3013" Z="0"/>`: Specifies the position of the part. The parameters can be omitted, but at least one must remain. _I assume the omitted parameters become "0", but it is unclear._
      - `<Rotation X="1.57" Z="-3.03687"/>`: Specifies the rotation of the part. The parameters can be omitted, but at least one must remain. _I assume the omitted parameters become "0", but it is unclear._ _All X, Y, and Z are seen to be used but never all three. This needs to be clarified._
