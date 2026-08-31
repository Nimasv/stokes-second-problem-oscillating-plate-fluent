# Stokes' Second Problem — Oscillating Plate (Ansys Fluent 2024 R1)

## Physical Problem
Stokes' Second Problem describes the flow induced by an infinite flat plate
oscillating harmonically in its own plane within a viscous fluid. The plate
moves with velocity:

    u(0, t) = U * cos(omega * t)

where U is the peak velocity amplitude and omega is the angular frequency.
The analytical solution gives a velocity profile that decays exponentially
with distance from the plate:

    u(y, t) = U * exp(-y * sqrt(omega / (2*nu))) * cos(omega*t - y*sqrt(omega/(2*nu)))

where nu is the kinematic viscosity. The penetration depth (Stokes layer
thickness) is:

    delta = sqrt(2 * nu / omega)

---

## Geometry & Mesh

Software : Ansys 2024 R1 — Meshing module
Mesh type: Structured quadrilateral (2-D)
Cells     : 19,000
Faces     : 38,380
Nodes     : 19,380

## Geometry & Mesh

![Mesh overview](geometry_mesh/StaticFigure14.png)
![Mesh detail](geometry_mesh/StaticFigure16.png)


---

## Solver Setup

Solver   : Ansys Fluent 2024 R1
Model    : Pressure-Based | Transient | Laminar | 2-D
Fluid    : Water-liquid (nu = 1.004e-6 m^2/s, rho = 998.2 kg/m^3)

### Named Expressions (Fluent Expression Language)

  omega
      1 [rad/s]

  translational-velocity
      1 [m/s] * cos(omega * flow-time)

  rotational-velocity
      0 [rad/s]

### Boundary Conditions

  Oscillating wall (bottom plate):
      Type                : Moving Wall
      Motion              : Translational
      Translational speed : translational-velocity   [expression]
      Rotational speed    : rotational-velocity       [expression]

  Top boundary:
      Type : Symmetry

  Left / Right boundaries:
      Type : Periodic (translational)

### Time Discretization

  Scheme         : Second-Order Implicit
  Time step size : 0.01 s
  Number of steps: 1000   (10 oscillation periods at omega = 1 rad/s)
  Max iterations / time step : 20

---

## Results

![Velocity animation](results/IMG_9784.gif)


The animation shows the x-velocity profile (Y-Coordinate vs. X Velocity)
evolving over time and converging toward the analytical Stokes solution.

---

## Validation

The simulated velocity profile was compared against the analytical solution
at multiple time instances. Good agreement was observed across the Stokes
layer depth, confirming correct implementation of the oscillating boundary
condition and adequate mesh resolution near the wall.

---

## Project Structure

stokes-second-problem-oscillating-plate-fluent/
├── geometry_mesh/
│   ├── StaticFigure14.png       (mesh overview)
│   └── StaticFigure16.png       (near-wall mesh detail)
├── results/
│   └── IMG_9784.gif             (velocity profile animation)
├── setup/
│   └── [Fluent case/data files]
└── README.txt

---

## Software & Version

Ansys Fluent 2024 R1
Operating System: Windows 11