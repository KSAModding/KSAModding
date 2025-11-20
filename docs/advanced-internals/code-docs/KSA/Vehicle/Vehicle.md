<sub><sup>Tested with: **KSA Version 2025.11.4.2791**</sup></sub>
# class KSA.Vehicle
> **Namespace:** `KSA`  
> **Assembly:** `KSA.dll`

The `Vehicle` class represents any player-controllable vessel or spacecraft within the KSA simulation.
It integrates orbital mechanics, physics simulation, rendering, flight computer automation, navigation, input handling, audio, and UI presentation into a single high-level object.

A `Vehicle` maintains its own **FlightPlan**, **FlightComputer**, **PartTree**, **kinematic state**, **reference frames**, and **orbit**, and is responsible for updating and rendering itself each frame.
It also manages manual controls (engine, RCS thrusters), stabilization modes, targeting, burn planning, region detection (surface / atmospheric / orbital), and interactions with the universe simulation.

Most gameplay-visible behaviors—thrust, rotation, orbit updates, burn nodes, navball data, engine sound, patch transitions, and debug overlays—are ultimately computed or routed through this class.


!!! warning "Documentation Incomplete"
    This documentation page is not finished yet. Some sections or method pages may still be missing.



## Code Example

```csharp
// Get the current controlled rocket
var rocket = Program.ControlledVehicle;

// Access core systems
var fc = rocket.FlightComputer;
var plan = rocket.FlightPlan;

// Control the vessel
rocket.SetTarget(someSatellite);
fc.RateHold(rocket.NavBallData.Frame);

// Update per-frame data (typically done by the game loop)
rocket.UpdatePerFrameData();
```

---

## Constructors

### `Vehicle(CelestialSystem system, VehicleTemplate template, Astronomical parent, string id)`

Creates a new vehicle using the given template and orbital parent.
Initializes:

* Flight plan from the template's orbital data
* Full kinematic state
* Navball reference frame
* Part tree (if the template contains a root part)
* Vehicle configuration (center of mass, bounding volumes, engine data)
* Flight computer
* Audio registration

### *Internal & copy/serialization-related constructors are handled through:*

* `DeserializeSave(VehicleData data)`
* `CloneOrbit(Vehicle source)`

(These have their own method pages.)

---

## Public Members

> **Note:** This section lists only the major public members. Many additional fields and properties exist; method-level documentation will link to them individually.

### Orbit & Flight Planning

* `FlightPlan FlightPlan`
* `BurnPlan BurnPlan => FlightComputer.BurnPlan`
* `PatchedConic Patch`
* `Orbit Orbit`
* `Astronomical Parent`
* `FlightComputer FlightComputer`
* `SimTime TimeAtPeriapsis`, `NextPeriapsisTime`, `NextApoapsisTime`, `NextPatchEventTime`

### Kinematics

* `ref readonly KinematicStates LastKinematicStates`
* `KinematicMeasurements KinematicMeasurements`
* `double3 BodyRates`
* `double3 AccelerationBody`
* `double3 AngularAccelerationBody`

### Reference Frames & Attitude

* `VehicleRegion VehicleRegion`
* `doubleQuat Body2Cce`
* `doubleQuat Orb2Cci`
* `ref readonly NavBallData NavBallData`

### Mass & Physics

* `double InertMass`
* `double PropellantMass`
* `double TotalMass`
* `double3 MassSpecificPmi`

### Parts & Rendering

* `PartTree Parts`
* `(double3 Min, double3 Max) BoundingBox`
* `float BoundingSphereRadius`
* `float ObjectRadius`
* `AstronomicalTemplate BodyTemplate`

### Targeting & UI

* `IOrbiting? Target`
* `bool ShowOrbit`
* `bool ShowGroundTrack`
* `bool ShowDebugVelocityArrow`

### Input & Control Flags

* `ManualControlInputs` (via internal access)
* `EngineFlags`
* `ControlsLockout` (struct)
* `ThrusterTemplate? EditingThruster`
* `bool EditingThrusterTranslateFlag`
* `bool EditingThrusterRotationFlag`

### Audio

* `float AudibleDistance`
* `bool DrawnUiBox`

### Internal

* `List<Astronomical> ChildList`

## Method Pages (Overview)

All methods for `Vehicle` will be documented in separate pages.
The following list groups them by purpose.

### Saving & Loading

* `DeserializeSave(...)`
* `SerializeSave()`

### Orbital & Kinematic Data

* `CloneOrbit(...)`
* `UpdatePerFrameData()`
* `GetStateVectorsAt(...)`
* `GetPosition*`, `GetVelocity*`, `GetCce2Cci()`, `GetCci2Orb()` *(multiple coordinate transforms)*

### Targeting

* `SetTarget(...)`
* `SetTargetOf(...)`
* `RemoveTargetOf(...)`
* `ContainsPatch(...)`

### Input Handling

* `OnKey(...)`
* `OnMouseButton(...)`
* `StopThrust()`
* `ToggleStabilization()`
* `SetStabilization(...)`
* `ToggleManualThrustMode()`

### Rendering & UI

* `OnDrawUi(...)`
* `DrawOrbitView()`
* `DrawPatchWindow(...)`
* `UpdateRenderData(...)`
* `PrepareTechniques(...)`
* `OnPreRender(...)`

### Flight Plan & Burn Control

* `OpenPatchWindow()`
* `TogglePatchWindow()`
* `UpdateBurnNodes(...)`
* `ResetFlightBurnPlanExpiry()`

### Vehicle Configuration

* `UpdateVehicleConfiguration(...)`
* `GetTemplate()`

### Teleportation & Debugging

* `Teleport(...)`
* `DrawPhysicsDebugWindow(...)`
* `DrawSetOrbitDebugWindow(...)`
* `DrawFlightComputerDebugWindow(...)`

### Utility

* `GetInertialSpeed()`
* `GetSurfaceSpeed()`
* `GetRadarAltitude()`
* `GetBarometricAltitude()`
* `RefillConsumables()`
* `ToConsole()`
* `ToRow()`

---

## Operators

This class does **not** define any custom operators.

