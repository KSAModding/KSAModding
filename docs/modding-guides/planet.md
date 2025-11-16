# Planets

## Overview
This guide explains how to add custom planets to the game.

## Requirements
Requirements:
- GIMP or any other image editing tool that can export .dds files in BC5 format

## File Locations
- **Game Directory**: `C:\Users\[YourName]\AppData\Local\Programs\Kitten Space Agency\`
- **Textures**: `Content/Core/Textures`
- **Planet Definitions**: `Content/Core/Astronomicals.xml`
- **System Files**: `Content\Core\EarthOnly.xml`, `Content\Core\SolSystem.xml`

## Step 1: Making a Basic Planet

### Choosing a template
Making a planet from scratch can be very tedious, so I recommend using a template.

Earth Template:
'''xml
    <AtmosphericBody Id="Earth" Parent="Sol">
        <MeshCollection Id="EarthScale"/>
        <SemiMajorAxis Au="1" />
        <Inclination Degrees="0.00005" />
        <Eccentricity Value="0.0167" />
        <LongitudeOfAscendingNode Degrees="-11.261" />
        <ArgumentOfPeriapsis Degrees="114.207" />
        <Diffuse Path="Textures/Earth_Diffuse.dds" Category="Terrain"/>
        <Normal Path="Textures/Earth_Normal.dds" Category="Terrain"/>
        <Height Path="Textures/Earth_Height.ktx2" Category="Terrain">
            <Minimum Km="-10.930"/>
            <Maximum Km="8.627"/>
        </Height>
        <Terrain>
            <BiomeIDMap Id="EarthBiomeID" Path="Textures/Earth_Biome_ID.png"/>
            <BiomeControlMap Id="EarthBiomeControl" Path="Textures/Earth_Biome_Control.png"/>
        </Terrain>
        <MeanAnomalyAtEpoch Degrees="100" />
        <MeanRadius Km="6371" />
        <Mass Earths="1" />
        <Color R="0.11" G="0.31" B="0.415" />
        <Period Days="1"/>
        <AxialTilt Degrees="23.44" />
        <AxialAlignment Degrees="0" />
        <Atmosphere>
            <Visual>
                <RayleighScattering>
                    <Coefficients R="0.0058" G="0.0135" B="0.0331" />
                    <ScaleHeight Km="8" />
                </RayleighScattering>
                <MieScattering>
                    <Coefficients R="0.0015761662727847277" G="0.0015761662727847277" B="0.0015761662727847277" />
                    <ScaleHeight Km="1.8" />
                    <PhaseFunctionAsymmetry X="0.83" Y="0.83" Z="0.83" />
                    <AbsorptionMultiplier Value="1.1111" />
                </MieScattering>
                <Ozone>
                    <Coefficients R="0.0004766464336060959" G="0.0011544693829723834" B="5.008543960834634E-05" />
                    <Altitude Km="20" />
                    <Extent Km="10" />
                </Ozone>
                <StartHeight Km="0" />
            </Visual>
            <Physical>
                <SeaLevelPressure Atm="1.0"/>
                <SeaLevelDensity KgPerM3="1.225"/>
                <ScaleHeight Km="8"/>
            </Physical>
        </Atmosphere>
        <Ocean>
            <Level Km="0" />
            <MeshCollection Id="OceanEarth"/>
            <WindWaves>
                <!-- The WindTexture is used to adjust wind speed and fetch locally, lerping between min and max-->
                <WindTexture Path="Textures/Earth_Ocean_Wind.png" Category="Terrain"/>
                <MinWindSpeed Value="2.0" />
                <MinWindFetch Km="30.0"/>
                <MaxWindSpeed Value="15.0" />
                <MaxWindFetch Km="800.0"/>
                <Directionality Value="1.0"/>
                <FoamAmount Value="0.5"/>
                <Depth Km="0.2"/>
            </WindWaves>
            <GerstnerWaves> <!-- leaving this in just for the cool factor of combining big gerstner waves with FFT -->
                <Count Value="0" /> <!-- change to 1 for tsunami waves -->
                <Length M="10000" />
                <Amplitude Value="0.75" />
                <Speed Value="1.0" />
                <LengthPersistence Value="0.93" />
                <AmplitudePersistence Value="0.975" />
                <SpeedPersistence Value="1.01" />
            </GerstnerWaves>
            <!-- This color value and the colormap's content get multiplied -->
            <Color R="0.588" G="0.588" B="0.588" />
            <ColorTexture Path="Textures/Earth_Ocean_Color_4096.dds" Category="Terrain"/>
            <TransparencyDepth M="100" />
        </Ocean>
        <Clouds>
            <OrbitTransitionStartAltitude Km="500.0"/>
            <OrbitTransitionEndAltitude Km="1500.0"/>
            <VolumetricsFlickerReductionDistance Km="100.0"/>
            <Layer Id="EarthClouds">
                <RotationSpeed X="0" Y="0" Z="-20" />
                <Texture Id="Textures/Clouds/compressed/Earth_clouds_tile_mask.dds" Path="Textures/Clouds/compressed/Earth_clouds_tile_mask.dds" Category="Terrain"/>
                <Detail>
                    <Texture Id="CloudDetail2048" Path="Textures/Clouds/CloudDetail2048.png" Category="Terrain"/>
                    <Size Km ="400"/>
                </Detail>
            <TwoDimensionalCloud>
                <Color R="1.0" G="1.0" B="1.0" />
                <Lambertian Value="0.65" />
                <UseAlphaOnly Value="true" />
            </TwoDimensionalCloud>
            <Raymarching>
                <Step Scale="0.006">
                    <Size Km="0.07"/>
                    <MaxSize Km="1.5"/>
                </Step>
                <LightDistance Km="1.8"/>
                <LightSamples Value="6"/>
            </Raymarching>
            <Noise>
                <ScrollSpeed Value="5"/>
            </Noise>
            <CloudType Name="Stratus">
                <StartAltitude Km="2.0"/>
                <Height Km="1.2"/>
                <Density Value="0.002"/>
                <NoiseScale Km="2.3"/>
                <EdgeSharpness Value="0.87"/>
                <MultipleScatteringBrightness Value="0.7"/>
                <CloudShape InterpolateShapes="true">
                    <ShapeCurve>
                        <SplinePoint>
                            <Key Value="0.0"/>
                            <Value Value="0.0"/>
                            <InTangent Value="1.633945"/>
                            <OutTangent Value="1.633945"/>
                        </SplinePoint>
                        <SplinePoint>
                            <Key Value="0.1140957"/>
                            <Value Value="0.8506892"/>
                            <InTangent Value="0.06669102"/>
                            <OutTangent Value="0.261398"/>
                        </SplinePoint>
                        <SplinePoint>
                            <Key Value="0.3686106"/>
                            <Value Value="0.9575245"/>
                            <InTangent Value="0.0"/>
                            <OutTangent Value="0.0"/>
                        </SplinePoint>
                        <SplinePoint>
                            <Key Value="1.0"/>
                            <Value Value="0.0"/>
                            <InTangent Value="-0.369256"/>
                            <OutTangent Value="-0.5550035"/>
                        </SplinePoint>
                    </ShapeCurve>
                </CloudShape>
            </CloudType>
            <CloudType Name="Cumulus">
                <StartAltitude Km="2.0"/>
                <Height Km="2.7"/>
                <Density Value="0.012"/>
                <NoiseScale Km="2.3"/>
                <EdgeSharpness Value="0.96"/>
                <MultipleScatteringBrightness Value="0.8"/>
                <CloudShape InterpolateShapes="true">
                    <ShapeCurve>
                        <SplinePoint>
                            <Key Value="0.0"/>
                            <Value Value="0.0"/>
                            <InTangent Value="1.633945"/>
                            <OutTangent Value="1.633945"/>
                        </SplinePoint>
                        <SplinePoint>
                            <Key Value="0.08695792"/>
                            <Value Value="0.976166"/>
                            <InTangent Value="0.06669102"/>
                            <OutTangent Value="0.04846065"/>
                        </SplinePoint>
                        <SplinePoint>
                            <Key Value="1.0"/>
                            <Value Value="0.0"/>
                            <InTangent Value="-0.7966601"/>
                            <OutTangent Value="-0.5550035"/>
                        </SplinePoint>
                    </ShapeCurve>
                </CloudShape>
            </CloudType>
            <CloudType Name="Cumulus Congestus 1">
                <StartAltitude Km="2.0"/>
                <Height Km="4.7"/>
                <Density Value="0.012"/>
                <NoiseScale Km="2.3"/>
                <EdgeSharpness Value="0.96"/>
                <CloudShape InterpolateShapes="true">
                    <ShapeCurve>
                        <SplinePoint>
                            <Key Value="0.0"/>
                            <Value Value="0.0"/>
                            <InTangent Value="1.633945"/>
                            <OutTangent Value="1.633945"/>
                        </SplinePoint>
                        <SplinePoint>
                            <Key Value="0.08695792"/>
                            <Value Value="0.976166"/>
                            <InTangent Value="0.06669102"/>
                            <OutTangent Value="0.04846065"/>
                        </SplinePoint>
                        <SplinePoint>
                            <Key Value="0.4343985"/>
                            <Value Value="0.6765829"/>
                            <InTangent Value="-1.224023"/>
                            <OutTangent Value="-1.224023"/>
                        </SplinePoint>
                        <SplinePoint>
                            <Key Value="0.8816288"/>
                            <Value Value="0.3897246"/>
                            <InTangent Value="-1.164106"/>
                            <OutTangent Value="-1.164106"/>
                        </SplinePoint>
                        <SplinePoint>
                            <Key Value="1.0"/>
                            <Value Value="0.0"/>
                            <InTangent Value="-0.7966601"/>
                            <OutTangent Value="-0.5550035"/>
                        </SplinePoint>
                    </ShapeCurve>
                </CloudShape>
            </CloudType>
            <CloudType Name="Cumulus Congestus 2">
                <StartAltitude Km="2.0"/>
                <Height Km="6.7"/>
                <Density Value="0.006"/>
                <NoiseScale Km="2.3"/>
                <EdgeSharpness Value="0.96"/>
                <CloudShape InterpolateShapes="false">
                    <ShapeCurve>
                        <SplinePoint>
                            <Key Value="0.0"/>
                            <Value Value="0.0"/>
                            <InTangent Value="1.633945"/>
                            <OutTangent Value="1.633945"/>
                        </SplinePoint>
                        <SplinePoint>
                            <Key Value="0.09431168"/>
                            <Value Value="0.9640306"/>
                            <InTangent Value="0.06669102"/>
                            <OutTangent Value="-0.8954304"/>
                        </SplinePoint>
                        <SplinePoint>
                            <Key Value="0.2904161"/>
                            <Value Value="0.8003764"/>
                            <InTangent Value="0.2357683"/>
                            <OutTangent Value="0.2357683"/>
                        </SplinePoint>
                        <SplinePoint>
                            <Key Value="0.59456"/>
                            <Value Value="0.7429472"/>
                            <InTangent Value="-0.001872113"/>
                            <OutTangent Value="-0.001872113"/>
                        </SplinePoint>
                        <SplinePoint>
                            <Key Value="0.734686"/>
                            <Value Value="0.7428927"/>
                            <InTangent Value="-3.4198"/>
                            <OutTangent Value="-1.176937"/>
                        </SplinePoint>
                        <SplinePoint>
                            <Key Value="1.0"/>
                            <Value Value="0.0"/>
                            <InTangent Value="-0.8304251"/>
                            <OutTangent Value="-0.5550035"/>
                        </SplinePoint>
                    </ShapeCurve>
                </CloudShape>
            </CloudType>
            <CloudType Name="Cumulus Congestus 3">
                <StartAltitude Km="2.0"/>
                <Height Km="6.7"/>
                <Density Value="0.006"/>
                <NoiseScale Km="2.3"/>
                <EdgeSharpness Value="0.96"/>
                <CloudShape InterpolateShapes="false">
                    <ShapeCurve>
                        <SplinePoint>
                            <Key Value="0.0"/>
                            <Value Value="0.0"/>
                            <InTangent Value="1.633945"/>
                            <OutTangent Value="1.633945"/>
                        </SplinePoint>
                        <SplinePoint>
                            <Key Value="0.09431168"/>
                            <Value Value="0.9640306"/>
                            <InTangent Value="0.06669102"/>
                            <OutTangent Value="-0.8954304"/>
                        </SplinePoint>
                        <SplinePoint>
                            <Key Value="0.2904161"/>
                            <Value Value="0.8003764"/>
                            <InTangent Value="0.2357683"/>
                            <OutTangent Value="0.2357683"/>
                        </SplinePoint>
                        <SplinePoint>
                            <Key Value="0.734686"/>
                            <Value Value="0.7428927"/>
                            <InTangent Value="-3.4198"/>
                            <OutTangent Value="-1.176937"/>
                        </SplinePoint>
                        <SplinePoint>
                            <Key Value="1.0"/>
                            <Value Value="0.0"/>
                            <InTangent Value="-0.8304251"/>
                            <OutTangent Value="-0.5550035"/>
                        </SplinePoint>
                    </ShapeCurve>
                </CloudShape>
            </CloudType>
        </Layer>
        <Layer Id="EarthClouds2">
            <RotationSpeed X="0" Y="0" Z="-30" />
            <Texture Id="Textures/Clouds/compressed/Earth_clouds_placeholder_top_layer.dds" Path="Textures/Clouds/compressed/Earth_clouds_placeholder_top_layer.dds" Category="Terrain"/>
                <Detail>
                    <Texture Path="Textures/Clouds/compressed/CirroStratusDetail.dds" Category="Terrain"/>
                    <Size Km ="400"/>
                </Detail>
                <Color R="1.0" G="1.0" B="1.0" />
                <TwoDimensionalCloud>
                    <Color R="1.0" G="1.0" B="1.0" />
                    <Lambertian Value="0.65" />
                    <UseAlphaOnly Value="true" />
                </TwoDimensionalCloud>
                <Raymarching>
                    <Step Scale="0.0044">
                        <Size Km="0.11"/>
                        <MaxSize Km="4.5"/>
                    </Step>
                    <LightDistance Km="2.8"/>
                    <LightSamples Value="4"/>
                </Raymarching>
                <Noise>
                    <ScrollSpeed Value="5"/>
                </Noise>
                <CloudType Name="CirroStratus">
                    <StartAltitude Km="11.0"/>
                    <Height Km="1.2"/>
                    <Density Value="0.001"/>
                    <NoiseScale Km="4.0"/>
                    <EdgeSharpness Value="0.5"/>
                    <MultipleScatteringBrightness Value="0.7"/>
                    <CloudShape InterpolateShapes="true">
                        <ShapeCurve>
                            <SplinePoint>
                                <Key Value="0.0"/>
                                <Value Value="0.0"/>
                                <InTangent Value="2.0"/>
                                <OutTangent Value="2.0"/>
                            </SplinePoint>
                            <SplinePoint>
                                <Key Value="0.5"/>
                                <Value Value="1.0"/>
                                <InTangent Value="0.0"/>
                                <OutTangent Value="0.0"/>
                            </SplinePoint>
                            <SplinePoint>
                                <Key Value="1.0"/>
                                <Value Value="0.0"/>
                                <InTangent Value="-2.0"/>
                                <OutTangent Value="-2.0"/>
                            </SplinePoint>
                        </ShapeCurve>
                    </CloudShape>
                </CloudType>
            </Layer>
        </Clouds>
    </AtmosphericBody>
'''