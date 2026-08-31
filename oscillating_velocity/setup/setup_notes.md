# Setup — Stokes' Second Problem (Oscillating Plate)

Ansys Fluent 2024 R1 — 2D, double precision, pressure-based, laminar, transient
Solver launch: `2d, dp, pbns, lam, transient, 4-processes` (parallel, 4 processes)

## 1. Physics Models

| Model | Setting |
|-------|---------|
| Multiphase | Off |
| Energy | Off |
| Viscous | **Laminar** |
| Radiation | Off |
| Species | Off |
| Discrete Phase | Off |

Stokes' second problem is an unsteady, incompressible, isothermal laminar flow
driven purely by the oscillating motion of a wall. No turbulence, energy, or
species models are required.

## 2. Named Expressions

The wall/plate motion is defined through named expressions. The driving angular
frequency is defined once and reused so the frequency stays consistent across
the translational and rotational velocity definitions.

| Name | Expression | Notes |
|------|-----------|-------|
| `omega` | `2*PI*0.25[s^-1]` | Angular frequency, f = 0.25 Hz → ω ≈ 1.5708 rad/s |
| `translational-velocity` | `0.1[m s^-1]*cos(omega*t)` | Plate translational velocity, amplitude 0.1 m/s |
| `rotational-velocity` | `-2[rad s^-1]*cos(omega*t)` | Rotational velocity, amplitude 2 rad/s |

The plate velocity follows the classic Stokes form

$$u_{\text{plate}}(t) = U_0 \cos(\omega t), \qquad U_0 = 0.1\ \text{m/s}, \quad \omega = 2\pi(0.25)\ \text{rad/s}$$

## 3. Report Definitions

Three expression report definitions track the imposed motion each time step,
each writing to its own report file and plot:

- `rotational-velocity` → `rotational-velocity-rfile`, `rotational-velocity-rplot`
- `translational-velocity` → `translational-velocity-rfile`, `translational-velocity-rplot`
- `omega` → `omega-rfile`, `omega-rplot`

Average Over: 1 (instantaneous value each step).

## 4. Solution — Run Calculation

| Parameter | Value |
|-----------|-------|
| Time Advancement Type | Fixed |
| Method | User-Specified |
| Number of Time Steps | 80 |
| Time Step Size | 0.05 s |
| Max Iterations / Time Step | 40 |
| Reporting Interval | 1 |
| Profile Update Interval | 1 |

Total simulated time: 80 × 0.05 = **4.0 s** (one full period T = 1/0.25 = 4 s).
Initialization: Standard.

## 5. Post-Processing

- Velocity vectors colored by velocity magnitude (`vector-2`), range ≈ 2.35e-07 to 8.22e-02 m/s.
- Velocity-magnitude vectors show the diffusion of momentum from the oscillating
  wall into the fluid — the characteristic Stokes boundary layer that grows and
  decays each cycle.
- Y-coordinate vs. X-velocity profiles extracted at successive times for comparison
  against the analytical Stokes' second problem solution (see `validation/`).

## 6. Files

The heavy case/data files (`*.cas.h5`, `*.dat.h5`) are excluded via `.gitignore`.
This document plus the images in `geometry_mesh/` and `results/` reproduce the setup.
