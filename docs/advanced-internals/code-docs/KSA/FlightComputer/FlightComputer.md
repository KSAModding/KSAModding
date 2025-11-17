<sub><sup>Tested with: **KSA Version 2025.11.5.2819**</sup></sub>
# struct KSA.FlightComputer
> **Namespace:** `KSA`  
> **Assembly:** `KSA.dll`

The FlightComputer class manages all automated and semi-automated control logic for a vessel.
It is responsible for attitude control, burn planning and execution, manual & pulse-mode RCS control, and integrating navigation measurements to generate actuator outputs.

A FlightComputer also tracks a vessel’s configuration (such as thruster authority, torque/force capability, and engine performance), allowing it to compute rotation rates, deadbands, flip times, and burn durations accurately.

This class is typically owned by a Vehicle object and is updated once per simulation step.

!!! warning "Documentation Incomplete"
    This documentation page is not finished yet. Some sections or method pages may still be missing.

    Here is a complete **Overview page** you can paste directly after your existing header.


## Code Example (Optional)

```csharp
// Accessing a vessel’s flight computer
var fc = activeVessel.FlightComputer;

// Switch to auto attitude mode and track prograde
fc.TrackTarget(FlightComputerAttitudeTrackTarget.Prograde);

// Set burn mode to auto
fc.ToggleBurnMode();

// Update the control output
fc.ComputeControl(ref nav, ref inputs, ref outputs);
```

---

## Constructors

### `FlightComputer()`

Creates a new instance with default values and an empty `BurnPlan`.

### `FlightComputer(FlightComputer existing)`

Creates a deep copy of an existing flight computer, cloning:

* Burn and attitude modes
* Target and deadband values
* Rates, masses, configuration
* Burn plan
* Vehicle configuration info

---

## Public Members

### Modes & State

* `FlightComputerBurnMode BurnMode`
* `FlightComputerAttitudeMode AttitudeMode`
* `VehicleReferenceFrame AttitudeFrame`
* `FlightComputerAttitudeTrackTarget AttitudeTrackTarget`
* `double3 CustomAttitudeTarget`

### Control Parameters

* `float AngleDeadband`
* `float RateLimit`
* `FlightComputerRollMode RollMode`
* `AttitudeTarget AttitudeTarget`
* `DeadbandState[] DeadbandStates`

### Burn

* `BurnTarget? Burn`
* `BurnPlan BurnPlan { get; }`

### Vehicle State & Configuration

* `float3 Pmi`
* `float InertMass`
* `float PropellantMass`
* `float TotalMass` *(calculated)*
* `float TotalAcceleration`
* `VehicleConfigInfo VehicleConfig { get; private set; }`

### RCS Control State

* `SimTime LastThrusterPulseTime`
* `float3 RcsTorqueAuthority`
* `float3 AngleTurnaround`
* `float3 RateDeadband`
* `float3 ErrorAngles`
* `float3 ErrorRates`
* `float ConservativeFlipTime`
* `float DetumbleRateLimit`

## Method Pages (Overview)

The following methods have dedicated pages:

### Saving & Loading

* `DeserializeSave(...)`
* `SerializeSave()`

### Vehicle Configuration

* `ReadUpdatedVehicleConfiguration(...)`

### Attitude / Rotation Control

* `SetNullRot(...)`
* `SetInertialNullRot()`
* `UpdateAttitudeError(...)`
* `UpdateAttitudeRateError(...)`
* `UpdateAttitudeTrackError(...)`

### Burn Control

* `ComputeBurnControl(...)`
* `UpdateBurnTarget(...)`
* `LoadBurn(...)`
* `UnloadBurn()`
* `UpdateBurn(...)`

### Control Computation

* `ComputeControl(...)`
* `ComputeRcsPulseControl(...)`
* `ComputeRcsRateControl(...)`
* `ComputeRcsTrackControl(...)`
* `SelectJetsToFire(...)`

### Switching Line & Guidance Math

* `EvaluateParabolicPhaseLine(...)`
* `EvaluateParabolicPhaseLineInverse(...)`
* `EvaluateTargetLine(...)`
* `EvaluateLowerSwitchingLine(...)`
* `EvaluateUpperSwitchingLine(...)`
* `EvaluateSwitchingLine(...)`
* `EvaluateSwitchingLineViolationAngle(...)`

### User Interaction

* `TrackTarget(...)`
* `RateHold(...)`
* `IsTrackingTarget(...)`
* `IsRateHolding(...)`
* `ToggleThrustMode()`
* `ToggleAttitudeMode()`
* `ToggleBurnMode()`
* `Handle(...)`

### Burn Plan Management

* `CalculateNewFlightPlans(...)`
* `AddBurn(...)`
* `RemoveBurn(...)`
* `BurnUpdated(...)`



## Operators

This class does not define any custom operators.

