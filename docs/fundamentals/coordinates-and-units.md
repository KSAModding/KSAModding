# Coordinates and Units

Because KSA is a realistic spaceflight simulator, **units and coordinate systems matter a lot**.

This page documents:

- The base unit system  
- Coordinate conventions  
- Reference frames  
- Special considerations for orbital mechanics  

---

## Base Units

KSA uses a standard physics unit system:

- **Length:** meters  
- **Time:** seconds  
- **Mass:** kilograms  

All position, velocity, acceleration, and force values throughout the simulation are expressed using these base units. :contentReference[oaicite:1]{index=1}

---

## Coordinate Conventions

### Handedness  
**All coordinate frames in KSA are right-handed.**  
This applies universally—global, local, orbital, and body-fixed frames. :contentReference[oaicite:2]{index=2}

![Handedness](../assets/images/handedness.png)

### Vector Naming  
Vectors are suffixed with the frame in which they are expressed: :contentReference[oaicite:3]{index=3}

- `positionCce` — position in Celestial-Centered Ecliptic  
- `velocityCcf` — velocity in Celestial-Centered, Celestial-Fixed  

### Rotation Naming  
Frame rotations use the notation `X2Y` to denote a **passive rotation** taking vectors from frame X into frame Y. Examples: :contentReference[oaicite:4]{index=4}

- `cci2Ccf` — inertial → celestial-fixed  
- `orb2Cce` — orbital → ecliptic  

Active vector rotations are also suffixed:  

- `jointAngleBody` — rotation expressed in the vehicle’s body frame  

---

## Inertial vs Non-Inertial Frames

- **Inertial frames** are non-accelerating and non-rotating; ideal for describing motion.  
- **Non-inertial frames** rotate or accelerate, producing fictitious forces (centrifugal, Coriolis, etc.). These include frames fixed to rotating planets or thrusting vehicles. :contentReference[oaicite:5]{index=5}

---

# Reference Frames

Below is a summary of all major coordinate frames used in the simulation.

---

## ECL — Ecliptic Frame

The global “world” frame used for rendering and for comparing orientations throughout the system.  
Identical to CCE and CCI when centered on the sun. :contentReference[oaicite:6]{index=6}

![EclipticFrame](../assets/images/eclipticframe.png)

- **Type:** Inertial  
- **Origin:** Center of the solar system  
- **X:** Direction of periapsis for a zero-inclination/zero-argument solar orbit  
- **Y:** Completes the frame  
- **Z:** Up direction; perpendicular to ecliptic  

---

## CCF — Celestial-Centered, Celestial-Fixed Frame

A surface-fixed rotating frame used for surface-relative calculations. :contentReference[oaicite:7]{index=7}

![CCF](../assets/images/ccf.png)

- **Type:** Non-inertial  
- **Origin:** Celestial center  
- **X:** Equator–prime meridian intersection  
- **Y:** Completes the frame  
- **Z:** Celestial north pole  

---

## CCI — Celestial-Centered Inertial Frame

Used for orbital simulation around a celestial.  
Axes remain inertially fixed with respect to the ecliptic. :contentReference[oaicite:8]{index=8}

![CCI](../assets/images/cci.png)

- **Type:** Inertial  
- **Origin:** Celestial center  
- **X:** Periapsis direction for zero-inclination orbits  
- **Y:** Completes the frame  
- **Z:** Celestial rotation axis  

---

## CCE — Celestial-Centered Ecliptic Frame

Shares the celestial’s origin but uses global ecliptic axes.  
Useful for cross-celestial orientation comparisons. :contentReference[oaicite:9]{index=9}

![CCE](../assets/images/cce.png)

- **Type:** Inertial  
- **Origin:** Celestial center  
- **X/Y/Z:** Identical to ECL axes  

---

## ORB — Orbit Frame (Perifocal)

Defines the orbital plane of a spacecraft or celestial. Commonly used in Keplerian element conversions. :contentReference[oaicite:10]{index=10}

![ORB](../assets/images/orb.png)

- **Type:** Inertial  
- **Origin:** Orbital focus (celestial center)  
- **X:** Toward periapsis  
- **Y:** Completes the frame  
- **Z:** Orbital angular momentum direction  

---

## LVLH — Local Vertical / Local Horizontal

Useful for spacecraft attitude control.  
Provides intuitive axes (down, forward, etc.) for a vehicle in orbit. :contentReference[oaicite:11]{index=11}

![LVLH](../assets/images/lvlh.png)

- **Type:** Non-inertial  
- **Origin:** Celestial center  
- **Z:** Toward celestial center  
- **Y:** Opposite orbital angular momentum  
- **X:** Completes the frame  

---

## VLF — Velocity Frame

Designed for maneuver planning using prograde/normal/radial axes. :contentReference[oaicite:12]{index=12}

![VLF](../assets/images/vlf.png)

- **Type:** Non-inertial  
- **Origin:** Celestial center  
- **X:** Prograde  
- **Y:** Normal  
- **Z:** Radial (completes the frame)  

---

## BODY — Vehicle Body Frame

Fixed to the spacecraft geometry. Defines roll, pitch, and yaw axes. :contentReference[oaicite:13]{index=13}

![BODY](../assets/images/body.png)

- **Type:** Non-inertial  
- **Origin:** Vehicle structural origin  
- **X:** Forward / nose direction (roll axis)  
- **Y:** Pitch axis  
- **Z:** Downward / yaw axis  

---

## AERO — Aerodynamic Frame

Used for computing aerodynamic forces.  
Aligns X with the relative wind direction. :contentReference[oaicite:14]{index=14}

![AERO](../assets/images/aero.png)

- **Type:** Non-inertial  
- **Origin:** Object center of mass  
- **X:** Into the relative wind (drag axis)  
- **Y:** Completes the frame  
- **Z:** Toward parent celestial  

