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

## Making a Basic Planet

### Choosing a template
Making a planet from scratch can be very tedious, so I recommend using a template.

Earth Template:
```xml
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
            <Texture Id="Textures/Clouds/compressed/Earth_clouds_placeholder_top_layer.dds" Path="Textures/Clouds/compressed/Earth_clouds_placeholder_top_layer.dds" Category="Terrain"/    >
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
```
Jupiter Template (for Gas Giants):
```xml
    <AtmosphericBody Id="Jupiter" Parent="Sol">	
        <MeshCollection Id="Default"/>
        <SemiMajorAxis Au="5.2044" />
        <Inclination Degrees="1.30530" />
        <Eccentricity Value="0.0489" />
        <LongitudeOfAscendingNode Degrees="100.464" />
        <ArgumentOfPeriapsis Degrees="273.867" />
        <MeanAnomalyAtEpoch Degrees="34.35152" />
        <MeanRadius Km="69911" />
        <Mass Jupiters="1" />
        <Diffuse Id="JupiterDiffuse" Path="Textures/Jupiter_Diffuse.dds" Category="Terrain"/>
        <Color R="1.0" G="0.69" B="0.36" />
        <Period Hours="9" Minutes="55" Seconds="30"/>
        <AxialTilt Degrees="3.13" />
        <AxialAlignment Degrees="0" />
        <Atmosphere>
            <Visual>
                <RayleighScattering>
                    <Coefficients R="1.8e-4" G="1.8e-4" B="1.1e-4" />
                    <ScaleHeight Km="650"/>
                </RayleighScattering>
                <MieScattering>
                    <Coefficients R="4e-4" G="4e-4" B="4e-4"/>
                    <ScaleHeight Km="100"/>
                    <PhaseFunctionAsymmetry X="0.1" Y="0.1" Z="0.1"/>
                </MieScattering>
            </Visual>
            <Physical>
                <SeaLevelPressure Atm="1.0"/>
                <SeaLevelDensity KgPerM3="0.16"/>
                <ScaleHeight Km="150" />
            </Physical>
        </Atmosphere>

        <Clouds>
            <OrbitTransitionStartAltitude Km="5000" />
            <OrbitTransitionEndAltitude Km="9000" />
            <MaxShadowsAltitude Km="9000" />
            <VolumetricsFlickerReductionDistance Km="10000.0"/>
            <Layer Id="JupiterLowerClouds">
                <RotationSpeed X="0" Y="0" Z="0" />

                <Texture Id="JupiterLowerCloudsMask" Path="Textures/Clouds/Compressed/JupiterLowerCloudsMask.dds" Category="Terrain">
                    <IsVirtual>false</IsVirtual>
                    <Manifest>
                        <MaxSize>0</MaxSize>
                        <MipMaps>false</MipMaps>
                    </Manifest>
                </Texture>

                <Detail>
                    <Texture Path="Textures/Clouds/Compressed/JupiterLowerLayerDetail.dds" Category="Terrain">
                        <IsVirtual>false</IsVirtual>
                        <Manifest>
                            <MaxSize>0</MaxSize>
                            <MipMaps>false</MipMaps>
                        </Manifest>
                    </Texture>
                    <Size Km ="40000"/>
                </Detail>

                <TwoDimensionalCloud>
                    <Texture Id="JupiterDiffuse"/>
                    <UseAlphaOnly Value="false" />
                    <Color R="1.0" G="1.0" B="1.0" />
                    <Lambertian Value="0.5" />
                </TwoDimensionalCloud>
                <Color R="0.29537368" G="0.29537368" B="0.29537368" />
                <MatchGroundColor Value="false" />
                <VolumetricsFlowMap>
                    <Texture Id="JupiterFlowmap" Category="Terrain"/>
                    <Displacement Km="1840.0"/>
                    <LoopDuration Hours="0.5"/>
                </VolumetricsFlowMap>
                <VolumetricsColorMap Id="JupiterDiffuse"/>
                <Raymarching>
                    <Step Scale="0.00275">
                        <Size M="10000" />
                        <MaxSize M="175000" />
                    </Step>
                    <LightDistance M="120000" />
                    <LightSamples Value="6" />
                </Raymarching>
                <Noise>
                    <ScrollSpeed Value="30" />
                </Noise>

                <CloudType Name="DiffuseCloud">
                    <StartAltitude M="0" />
                    <Height M="220000" />
                    <Density Value="0.0002800000074785203" />
                    <NoiseScale M="120000" />
                    <EdgeSharpness Value="0.9700000286102295" />
                    <MultipleScatteringBrightness Value="1" />
                    <CloudShape InterpolateShapes="true">
                        <ShapeCurve>
                            <SplinePoint>
                                <Key Value="0" />
                                <Value Value="1" />
                                <InTangent Value="0.01987062" />
                                <OutTangent Value="0.01987062" />
                            </SplinePoint>
                            <SplinePoint>
                                <Key Value="0.06069539" />
                                <Value Value="0.4903364" />
                                <InTangent Value="-1.326968" />
                                <OutTangent Value="-1.326968" />
                            </SplinePoint>
                            <SplinePoint>
                                <Key Value="0.2747427" />
                                <Value Value="-0.003249101" />
                                <InTangent Value="-1.609576" />
                                <OutTangent Value="-1.609576" />
                            </SplinePoint>
                            <SplinePoint>
                                <Key Value="0.4130899" />
                                <Value Value="0.003613152" />
                                <InTangent Value="1.113726" />
                                <OutTangent Value="1.113726" />
                            </SplinePoint>
                            <SplinePoint>
                                <Key Value="0.5144587755203247" />
                                <Value Value="0.4342148005962372" />
                                <InTangent Value="-0.8097732" />
                                <OutTangent Value="-0.8097732" />
                            </SplinePoint>
                            <SplinePoint>
                                <Key Value="0.5887667" />
                                <Value Value="0.008177351" />
                                <InTangent Value="-6.05793" />
                                <OutTangent Value="-6.05793" />
                            </SplinePoint>
                            <SplinePoint>
                                <Key Value="0.6785794" />
                                <Value Value="-0.06444599" />
                                <InTangent Value="0.0343262" />
                                <OutTangent Value="0.0343262" />
                            </SplinePoint>
                            <SplinePoint>
                                <Key Value="1" />
                                <Value Value="1" />
                                <InTangent Value="2.295903205871582" />
                                <OutTangent Value="0.06266714" />
                            </SplinePoint>
                        </ShapeCurve>
                        <DensityCurve>
                            <SplinePoint>
                                <Key Value="0" />
                                <Value Value="1" />
                                <InTangent Value="0" />
                                <OutTangent Value="0" />
                            </SplinePoint>
                            <SplinePoint>
                                <Key Value="0.07735103368759155" />
                                <Value Value="1.0004963874816895" />
                                <InTangent Value="0" />
                                <OutTangent Value="0" />
                            </SplinePoint>
                            <SplinePoint>
                                <Key Value="0.15438231825828552" />
                                <Value Value="0.28357529640197754" />
                                <InTangent Value="0" />
                                <OutTangent Value="0" />
                            </SplinePoint>
                            <SplinePoint>
                                <Key Value="0.5162501931190491" />
                                <Value Value="0.09440368413925171" />
                                <InTangent Value="0" />
                                <OutTangent Value="0" />
                            </SplinePoint>
                            <SplinePoint>
                                <Key Value="0.9031978845596313" />
                                <Value Value="0.25868427753448486" />
                                <InTangent Value="0" />
                                <OutTangent Value="0" />
                            </SplinePoint>
                            <SplinePoint>
                                <Key Value="0.9300693273544312" />
                                <Value Value="0.9839124083518982" />
                                <InTangent Value="0" />
                                <OutTangent Value="0" />
                            </SplinePoint>
                            <SplinePoint>
                                <Key Value="1" />
                                <Value Value="1" />
                                <InTangent Value="0" />
                                <OutTangent Value="0" />
                            </SplinePoint>
                        </DensityCurve>
                    </CloudShape>
                </CloudType>

                <CloudType Name="StormCenter">
                    <StartAltitude M="0" />
                    <Height M="220000" />
                    <Density Value="0.0007999999797903001" />
                    <NoiseScale M="120000" />
                    <EdgeSharpness Value="0.97" />
                    <MultipleScatteringBrightness Value="1" />
                    <CloudShape InterpolateShapes="true">
                        <ShapeCurve>
                            <SplinePoint>
                                <Key Value="0" />
                                <Value Value="1" />
                                <InTangent Value="-0.03297058" />
                                <OutTangent Value="19.57472" />
                            </SplinePoint>
                            <SplinePoint>
                                <Key Value="0.1271144" />
                                <Value Value="0.9584956" />
                                <InTangent Value="0.01987062" />
                                <OutTangent Value="0.01987062" />
                            </SplinePoint>
                            <SplinePoint>
                                <Key Value="0.5853482" />
                                <Value Value="2.2663" />
                                <InTangent Value="0.0343262" />
                                <OutTangent Value="0.0343262" />
                            </SplinePoint>
                            <SplinePoint>
                                <Key Value="0.9048536" />
                                <Value Value="1" />
                                <InTangent Value="0.06266714" />
                                <OutTangent Value="0.06266714" />
                            </SplinePoint>
                            <SplinePoint>
                                <Key Value="1" />
                                <Value Value="1" />
                                <InTangent Value="-14.69716" />
                                <OutTangent Value="-14.69716" />
                            </SplinePoint>
                        </ShapeCurve>
                        <DensityCurve>
                            <SplinePoint>
                                <Key Value="0" />
                                <Value Value="1" />
                                <InTangent Value="0" />
                                <OutTangent Value="0" />
                            </SplinePoint>
                            <SplinePoint>
                                <Key Value="1" />
                                <Value Value="1" />
                                <InTangent Value="0" />
                                <OutTangent Value="0" />
                            </SplinePoint>
                        </DensityCurve>
                    </CloudShape>
                </CloudType>

            </Layer>
            
            <Layer Id="JupiterClouds">
                <RotationSpeed X="0" Y="0" Z="0" />
                <Texture Id="JupiterMask" Path="Textures/Clouds/Compressed/JupiterMask.dds" Category="Terrain">
                    <IsVirtual>false</IsVirtual>
                    <Manifest>
                        <MaxSize>0</MaxSize>
                        <MipMaps>false</MipMaps>
                    </Manifest>
                </Texture>
                <Detail>
                    <!-- Jupiter detail texture is a "passthrough" to use tilemask diectly as cloud type map -->
                    <Texture Id="JupiterDetail" Path="Textures/Clouds/JupiterDetail.png" Category="Terrain">
                        <IsVirtual>false</IsVirtual>
                        <Manifest>
                            <MaxSize>0</MaxSize>
                            <MipMaps>false</MipMaps>
                        </Manifest>
                    </Texture>
                    <Size Km="400" />
                </Detail>
                <TwoDimensionalCloud>
                    <Texture Id="JupiterDiffuse"/>
                    <UseAlphaOnly Value="false" />
                    <Color R="1.0" G="1.0" B="1.0" />
                    <Lambertian Value="0.5" />
                    <FlowMap>
                        <Texture Id="Jupiter2DFlowmap" Path="Textures/Clouds/Compressed/Jupiter2DFlowmap.dds" Category="Terrain">
                            <IsVirtual>false</IsVirtual>
                            <Manifest>
                                <MaxSize>0</MaxSize>
                                <MipMaps>true</MipMaps>
                            </Manifest>
                        </Texture>
                        <!-- these values are ridiculous but will show the animation in a reasonable timeframe -->
                        <Displacement Km="21080.0"/>
                        <LoopDuration Hours="1.0"/>
                    </FlowMap>
                </TwoDimensionalCloud>
                <Color R="1.0" G="1.0" B="1.0" />
                <MatchGroundColor Value="false" />
                <VolumetricsFlowMap>
                    <Texture Id="JupiterFlowmap" Path="Textures/Clouds/Compressed/JupiterFlowmap.dds" Category="Terrain">
                        <IsVirtual>false</IsVirtual>
                        <Manifest>
                            <MaxSize>0</MaxSize>
                            <MipMaps>false</MipMaps>
                        </Manifest>
                    </Texture>
                    <Displacement Km="1240.0"/>
                    <LoopDuration Hours="1.0"/>
                </VolumetricsFlowMap>
                <VolumetricsColorMap Id="JupiterDiffuse"/>
                <Raymarching>
                    <Step Scale="0.00275">
                        <Size M="10000" />
                        <MaxSize M="175000" />
                    </Step>
                    <LightDistance M="120000" />
                    <LightSamples Value="6" />
                </Raymarching>
                <Noise>
                    <ScrollSpeed Value="30" />
                </Noise>
                
                <CloudType Name="StormCenter">
                    <StartAltitude M="220000" />
                    <Height M="55000" />
                    <Density Value="0.00018" />
                    <NoiseScale M="420000" />
                    <EdgeSharpness Value="0.97" />
                    <MultipleScatteringBrightness Value="1.0" />
                    <CloudShape InterpolateShapes="true">
                        <ShapeCurve>
                            <SplinePoint>
                                <Key Value="0" />
                                <Value Value="0" />
                                <InTangent Value="0" />
                                <OutTangent Value="0" />
                            </SplinePoint>
                            <SplinePoint>
                                <Key Value="0.02634327" />
                                <Value Value="1.0" />
                                <InTangent Value="0.111055" />
                                <OutTangent Value="0.111055" />
                            </SplinePoint>
                            <SplinePoint>
                                <Key Value="0.3164966" />
                                <Value Value="0.7820052" />
                                <InTangent Value="-1.475057" />
                                <OutTangent Value="-1.475057" />
                            </SplinePoint>
                            <SplinePoint>
                                <Key Value="1.0" />
                                <Value Value="0.0" />
                                <InTangent Value="-0.0226933" />
                                <OutTangent Value="-0.0226933" />
                            </SplinePoint>
                        </ShapeCurve>
                        <DensityCurve>
                            <SplinePoint>
                                <Key Value="0.0" />
                                <Value Value="1.0" />
                                <InTangent Value="0.0" />
                                <OutTangent Value="0.0" />
                            </SplinePoint>
                            <SplinePoint>
                                <Key Value="1.0" />
                                <Value Value="1.0" />
                                <InTangent Value="0.0" />
                                <OutTangent Value="0.0" />
                            </SplinePoint>
                        </DensityCurve>
                    </CloudShape>
                </CloudType>
                
                <CloudType Name="StormEdge">
                    <StartAltitude M="220000" />
                    <Height M="330000" />
                    <Density Value="0.0004" />
                    <NoiseScale M="420000" />
                    <EdgeSharpness Value="0.0" />
                    <MultipleScatteringBrightness Value="1.0" />
                    <CloudShape InterpolateShapes="true">
                        <ShapeCurve>
                            <SplinePoint>
                                <Key Value="0" />
                                <Value Value="0" />
                                <InTangent Value="0" />
                                <OutTangent Value="0" />
                            </SplinePoint>
                            <SplinePoint>
                                <Key Value="0.02634327" />
                                <Value Value="1.0" />
                                <InTangent Value="0.111055" />
                                <OutTangent Value="0.111055" />
                            </SplinePoint>
                            <SplinePoint>
                                <Key Value="0.3164966" />
                                <Value Value="0.7820052" />
                                <InTangent Value="-1.475057" />
                                <OutTangent Value="-1.475057" />
                            </SplinePoint>
                            <SplinePoint>
                                <Key Value="1.0" />
                                <Value Value="0.0" />
                                <InTangent Value="-0.0226933" />
                                <OutTangent Value="-0.0226933" />
                            </SplinePoint>
                        </ShapeCurve>
                        <DensityCurve>
                            <SplinePoint>
                                <Key Value="0.0" />
                                <Value Value="1.0" />
                                <InTangent Value="0.0" />
                                <OutTangent Value="0.0" />
                            </SplinePoint>
                            <SplinePoint>
                                <Key Value="1.0" />
                                <Value Value="1.0" />
                                <InTangent Value="0.0" />
                                <OutTangent Value="0.0" />
                            </SplinePoint>
                        </DensityCurve>
                    </CloudShape>
                </CloudType>
                
                <CloudType Name="LowDensityCloud">
                    <StartAltitude M="220000" />
                    <Height M="440000" />
                    <Density Value="0.0000081" />
                    <NoiseScale M="420000" />
                    <EdgeSharpness Value="0.97" />
                    <MultipleScatteringBrightness Value="1.0" />
                    <CloudShape InterpolateShapes="false">
                        <ShapeCurve>
                            <SplinePoint>
                                <Key Value="0" />
                                <Value Value="0" />
                                <InTangent Value="0" />
                                <OutTangent Value="0" />
                            </SplinePoint>
                            <SplinePoint>
                                <Key Value="0.02634327" />
                                <Value Value="1.0" />
                                <InTangent Value="0.111055" />
                                <OutTangent Value="0.111055" />
                            </SplinePoint>
                            <SplinePoint>
                                <Key Value="0.5562" />
                                <Value Value="1.0" />
                                <InTangent Value="0.0" />
                                <OutTangent Value="0.0" />
                            </SplinePoint>
                            <SplinePoint>
                                <Key Value="1.0" />
                                <Value Value="0.0" />
                                <InTangent Value="-0.0226933" />
                                <OutTangent Value="-0.0226933" />
                            </SplinePoint>
                        </ShapeCurve>
                        <DensityCurve>
                            <SplinePoint>
                                <Key Value="0.0" />
                                <Value Value="1.0" />
                                <InTangent Value="0.0" />
                                <OutTangent Value="0.0" />
                            </SplinePoint>
                            <SplinePoint>
                                <Key Value="1.0" />
                                <Value Value="1.0" />
                                <InTangent Value="0.0" />
                                <OutTangent Value="0.0" />
                            </SplinePoint>
                        </DensityCurve>
                    </CloudShape>
                </CloudType>
                
                <CloudType Name="HighDensityCloud">
                    <StartAltitude M="220000" />
                    <Height M="550000" />
                    <Density Value="0.00035" />
                    <NoiseScale M="420000" />
                    <EdgeSharpness Value="0.97" />
                    <MultipleScatteringBrightness Value="1.0" />
                    <CloudShape InterpolateShapes="false">
                        <ShapeCurve>
                            <SplinePoint>
                                <Key Value="0" />
                                <Value Value="0" />
                                <InTangent Value="0" />
                                <OutTangent Value="0" />
                            </SplinePoint>
                            <SplinePoint>
                                <Key Value="0.02634327" />
                                <Value Value="1.0" />
                                <InTangent Value="0.111055" />
                                <OutTangent Value="0.111055" />
                            </SplinePoint>
                            <SplinePoint>
                                <Key Value="0.5562" />
                                <Value Value="1.0" />
                                <InTangent Value="0.0" />
                                <OutTangent Value="0.0" />
                            </SplinePoint>
                            <SplinePoint>
                                <Key Value="1.0" />
                                <Value Value="0.0" />
                                <InTangent Value="-0.0226933" />
                                <OutTangent Value="-0.0226933" />
                            </SplinePoint>
                        </ShapeCurve>
                        <DensityCurve>
                            <SplinePoint>
                                <Key Value="0.0" />
                                <Value Value="1.0" />
                                <InTangent Value="0.0" />
                                <OutTangent Value="0.0" />
                            </SplinePoint>
                            <SplinePoint>
                                <Key Value="1.0" />
                                <Value Value="1.0" />
                                <InTangent Value="0.0" />
                                <OutTangent Value="0.0" />
                            </SplinePoint>
                        </DensityCurve>
                    </CloudShape>
                </CloudType>
            </Layer>
        </Clouds>
        
    </AtmosphericBody>
```
Pluto Template (for Small Terrestrail Bodies):
```xml
    <TerrestrialBody Id="Pluto" Parent="Sol">
        <MeshCollection Id="Asteroid"/>
        <SemiMajorAxis Km="	10000488.231" />
        <Inclination Degrees="1" />
        <Eccentricity Value="0" />
        <LongitudeOfAscendingNode Degrees="55" />
        <ArgumentOfPeriapsis Degrees="0" />
        <MeanAnomalyAtEpoch Degrees="0" />
        <MeanRadius Km="6" />
        <Mass Kg="12166330000000000" />
        <Color R="0.941176" G="0.901961" B="0.54902" />
        <Diffuse Path="Textures/Pluto_Diffuse.dds" Category="Terrain"/>
        <Normal Path="Textures/Pluto_Normal.dds" Category="Terrain"/>
        <Height Path="Textures/Pluto_Height.htx2" Category="Terrain">
            <Maximum Km="3.8734"/>
            <Minimum Km="-0.3869"/>
        </Height>
    </TerrestrialBody>
```
