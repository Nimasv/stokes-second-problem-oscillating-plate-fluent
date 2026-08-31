# Stokes' Second Problem — Oscillating Plate
## Ansys Fluent 2024 R1 | Transient Laminar CFD

---

## Overview

This project simulates **Stokes' Second Problem**: a flat plate oscillating harmonically
in its own plane, inducing a transient laminar boundary layer in a viscous fluid.
The simulation is performed in Ansys Fluent 2024 R1 using a 2D pressure-based
transient solver.

---

## Physical Problem

An infinite plate at y = 0 oscillates with velocity:

    U(t) = U₀ · cos(ωt)

where:
- U₀ = 0.1 m/s       (amplitude)
- ω  = 2π × 0.25 = π/2 rad/s   (angular frequency)
- T  = 1/f = 4 s      (period)

The analytical solution for velocity in the fluid:

    u(y, t) = U₀ · exp(−y√(ω/2ν)) · cos(ωt − y√(ω/2ν))

where ν is the kinematic viscosity of the fluid.

---

## Geometry & Mesh

| Parameter     | Value       |
|---------------|-------------|
| Mesh type     | Structured  |
| Cells         | 19,000      |
| Faces         | 38,380      |
| Nodes         | 19,380      |
| Solver        | 2D          |

- The mesh is refined near the oscillating wall (y = 0) to resolve the
  Stokes boundary layer thickness: δ = √(2ν/ω).
- Static mesh figures are available in `results/StaticFigure14.png` (overview)
  and `results/StaticFigure16.png` (near-wall detail).

---

## Solver Setup

### Models
| Setting            | Value              |
|--------------------|--------------------|
| Solver type        | Pressure-Based     |
| Time formulation   | Transient          |
| Viscous model      | Laminar            |
| Energy equation    | Off                |

### Named Expressions
Defined in Fluent via **Parameters & Expressions > Expressions**:

| Name                    | Definition                          |
|-------------------------|-------------------------------------|
| `omega`                 | `2 * PI * 0.25 [s^-1]`              |
| `translational-velocity`| `0.1 [m s^-1] * cos(omega * t)`     |
| `rotational-velocity`   | `-2 [rad s^-1] * cos(omega * t)`    |

### Boundary Conditions
- **Moving wall (plate):** Wall with translational motion driven by
  `translational-velocity` expression.
- **Top boundary:** Velocity inlet or symmetry (quiescent fluid far field).
- **Lateral boundaries:** Periodic or symmetry.

### Run Controls
| Parameter         | Value   |
|-------------------|---------|
| Time step size    | 0.05 s  |
| Number of steps   | 80      |
| Total time        | 4.0 s   |
| Max iterations/step | 20    |

---

## Results

- **Velocity profiles** (Y-Coordinate vs X-Velocity) extracted at multiple
  time instants are stored in `results/`.
- **Animation** of the oscillating velocity boundary layer:
  `results/animation-velocity-profile.mp4`
- **Vector plots** of velocity magnitude are available as static figures.

---

## How to Reproduce

1. Open `2024/` in Ansys Meshing and verify the mesh statistics.
2. Launch Fluent 2024 R1 in 2D double-precision mode.
3. Read the case file from `setup/`.
4. Confirm Named Expressions (`omega`, `translational-velocity`,
   `rotational-velocity`) are defined under Parameters & Expressions.
5. Set Time Step = 0.05 s, Number of Steps = 80.
6. Initialize and run the calculation.
7. Export velocity profile XY-data and animation from CFD-Post or
   Fluent's built-in post-processor.

---

## Project Structure

```
stokes-second-problem-oscillating-plate-fluent/
├── geometry_mesh/          # Ansys Meshing project files
├── results/
│   ├── StaticFigure14.png  # Full mesh overview
│   ├── StaticFigure16.png  # Near-wall mesh detail
│   ├── animation-velocity-profile.mp4
│   └── *.png               # Velocity profile plots
├── setup/                  # Fluent case & data files (.cas.h5, .dat.h5)
├── validation/             # Analytical vs. CFD comparison scripts/plots
├── README.txt              # This file
└── .gitignore
```

---

## Dependencies

- Ansys Fluent 2024 R1 (or compatible version)
- Python 3.x with `numpy`, `matplotlib` (for validation plots)

---

## Notes

- All simulations assume incompressible Newtonian fluid with constant properties.
- The rotational-velocity expression (`-2 [rad s^-1] * cos(omega * t)`) is
  defined for completeness; verify it is applied only if rotational wall motion
  is intended in your specific case setup.
- Do not commit `.cas.h5` or `.dat.h5` files to version control — see `.gitignore`.

---

