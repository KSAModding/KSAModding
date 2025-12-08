# Astronomicals.xml (&lt;Assets>)

This information is for version 2025.8.33.2091.

All tags use example data from the Astronomicals.xml file in Core. The examples provided are only for display purpose and are not exact. _If something is highlighted like this, (italics) that means the information is not confirmed and/or is unclear._

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
- `<SemiMajorAxis Km="6713" />`: Specifies the semi-major axis of the vessel's orbit. (farthest point of the orbit from the core of the planet)
- `<Inclination Degrees="28.87" />`: Specifies the inclination of the vessel's orbit. (degrees from the equator)
- `<Eccentricity Value="0.0077" />`: Specifies the eccentricity of the vessel's orbit. (0 - circular, 1 - line)
- `<LongitudeOfAscendingNode Degrees="60.48" />`: Specifies the longitude of ascending node of the vessel's orbit. (honestly no idea)
- `<ArgumentOfPeriapsis Degrees="30.0015" />`: Specifies the argument of periapsis of the vessel's orbit in degrees. (what?)
- `<MeanRadius M="1.5" />`: Used for specifying planet size. _Appears to be ignored for vessels, although I assume this is some kind of collider._
- `<MassSpecificPMI X="1.13" Y="2.83" Z="2.83" />`: "Principal moments of inertia divided by vehicle mass" - quote by gravhoek. Appears to be a 3D vector.
- `<Mass Kg="3396" />`: Specifies the mass of the vessel in kilograms.
- `<PropellantMass Kg="12220" />`: Specifies the starting mass of the vessel's propellant in kilograms.
- `<Body2Cce X="1.815" Y="0.0091" Z="1.9134" />`: "Vehicle rotation with respect to the CCE frame" - quote by gravhoek. Appears to be a 3D vector.
- `<BodyRates X="0" Y="0.0011657209075534262" Z="0" />`: "Vehicle roll/pitch/yaw rate" - quote by gravhoek. Appears to be a 3D vector.
- `<Color R="0.33" G="0.95" B="0" />`: Specifies the color for the vessel's orbit line.

## <KittenEva Id="Banjo" Parent="Earth">
Specifies an EVA kitten to be spawned. `Id` is the kitten's name, this is also shown in-game. `Parent` specifies the object the kitten is orbiting. KittenEva appears to have everything copied from `<Vehicle>` with a few more tags added, meaning that kittens are actually vessels, similarly to KSP.

Descendants:
- `<Character Id="BanjoKitten"/>`: Specifies the kitten model to use. Available stock kitten models are `"BanjoKitten"`, `"PolarisKitten"`, and `"HunterKitten"`.
- `<PartRef Id="KittenBackPackPart"/>`: This is always the same for all KittenEva, but this can be used to attach parts to kittens. Must also close with `</PartRef>` and _overall seems to work the same as a vessel part, however this is not confirmed._
- `<SemiMajorAxis Km="6713" />`: Specifies the semi-major axis of the kitten's orbit. (farthest point of the orbit from the core of the planet)
- `<Inclination Degrees="28.87" />`: Specifies the inclination of the kitten's orbit. (degrees from the equator)
- `<Eccentricity Value="0.0077" />`: Specifies the eccentricity of the kitten's orbit. (0 - circular, 1 - line)
- `<LongitudeOfAscendingNode Degrees="60.48" />`: Specifies the longitude of ascending node of the kitten's orbit. (honestly no idea)
- `<ArgumentOfPeriapsis Degrees="30.0015" />`: Specifies the argument of periapsis of the kitten's orbit in degrees. (what?)
- `<MeanRadius M="1.5" />`: Used for specifying planet size. _Appears to be ignored for kittens, although I assume this is some kind of collider._
- `<MassSpecificPMI X="1.13" Y="2.83" Z="2.83" />`: "Principal moments of inertia divided by vehicle mass" - quote by gravhoek. Appears to be a 3D vector.
- `<Mass Kg="3396" />`: Specifies the mass of the kitten in kilograms.
- `<PropellantMass Kg="12220" />`: Specifies the starting mass of the kitten's propellant in kilograms.
- `<Body2Cce X="1.815" Y="0.0091" Z="1.9134" />`: "Vehicle rotation with respect to the CCE frame" - quote by gravhoek. Appears to be a 3D vector.
- `<BodyRates X="0" Y="0.0011657209075534262" Z="0" />`: "Vehicle roll/pitch/yaw rate" - quote by gravhoek. Appears to be a 3D vector.
- `<Color R="0.33" G="0.95" B="0" />`: Specifies the color for the kitten's orbit line.
- `<SoundEvent Action="Rcs" SoundId="DefaultRcsThrusters"/>`: Specifies the sound to use for an event. All KittenEva have this exact tag, however `SoundId` can be changed to change the kitten's RCS sound.

## <AtmosphericBody Id="Earth" Parent="Sol">
Specifies a planet with atmosphere. `Id` is the planet's name, this is also shown in-game. `Parent` specifies the object the planet is orbiting.

Descendants:
- `<Orbit>`: Specifies the orbit of the planet.
  - `<SemiMajorAxis Km="1.495396277103892E+08" />`: Specifies the semi-major axis of the planet, appears to be able to use scientific notation.
  - `<Inclination Degrees="28.87" />`: Specifies the inclination of the planet's orbit. (degrees from the equator)
  - `<Eccentricity Value="0.0077" />`: Specifies the eccentricity of the planet's orbit. (0 - circular, 1 - line)
  - `<LongitudeOfAscendingNode Degrees="60.48" />`: Specifies the longitude of ascending node of the planet's orbit. (honestly no idea)
  - `<ArgumentOfPeriapsis Degrees="30.0015" />`: Specifies the argument of periapsis of the planet's orbit in degrees. (what?) 
