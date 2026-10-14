# Passive Wireless LC-TPMS Sensor: Dielectric Optimization Study

A MATLAB simulation framework that models and optimizes passive wireless Tire Pressure Monitoring Systems (TPMS) through innovative dielectric layer engineering.

## About
Traditional passive TPMS sensors often suffer from weak signal responses and limited sensitivity due to standard air-filled capacitor constraints.
This project investigates how structuring internal dielectric configurations—comparing Air, Vertical/Series, and Horizontal/Parallel layouts—can drastically amplify sensor sensitivity without altering physical dimensions or requiring active battery power for real-time telemetry and safety threshold tracking.

---

## Core Architecture 
The system is modeled around a passive wireless telemetry loop consisting of:
1. **Reader Coil:** An external antenna that sweeps across frequency bands to interrogate the sensor.
2. **Planar Micro-Inductor ($L$):** A fixed inductive element that stores magnetic energy and acts as the receiver antenna.
3. **Variable Dielectric Capacitor ($C$):** The pressure-sensing core where mechanical gap displacement ($d$) alters total capacitance.
4. **Composite Dielectric Layers:** Strategic integration of Air ($\epsilon_r = 1.0$) and Polytetrafluoroethylene (Teflon) ($\epsilon_r = 2.1$).

Capacitor and Inductor are connected in series to form a LC circuit 
---

## Working Principle

- **Mechanical input:** Tire deflation or external load reduces the distance between parallel plates of the capacitor (d).
- **Change in Capacitance:** As d decreases, total capacitance (C) increases.

$$C = \frac{\varepsilon_0 \, \varepsilon_{eff} \, A}{d}$$

  where ε₀ is the permittivity of free space, ε_eff is the effective relative permittivity of the dielectric, A is the plate area and d is the gap.
- **Change in resonant Frequency:** Increase in C results in drop of the resonant frequency, governed by the LC resonance equation:

 $$f_{res} = \frac{1}{2\pi\sqrt{L\,C}}$$

- **The wireless handshake (impedance dip):** The external reader sweeps frequencies. When its frequency matches the sensor's natural f_res, inductive coupling draws energy from the reader, creating a measurable impedance dip on the reader side.

## Constants and Parameters Used

| Parameter | Symbol | Value |
|---|---|---|
| Inductance | L | 10 µH |
| Permittivity of free space | ε₀ | 8.854 × 10⁻¹² F/m |
| Relative permittivity of air | εr (air) | 1.0 |
| Relative permittivity of Teflon | εr (Teflon) | 2.1 |
| Plate area | A | 5 mm × 5 mm = 25 mm² |
| Maximum gap (minimum pressure) | d_max | 5 µm |
| Minimum gap (maximum pressure) | d_min | 1 µm |
| Minimum pressure | P_min | 10 kPa |
| Maximum pressure | P_max | 350 kPa |
| Reference (recommended) pressure | P_ref | 225 kPa (about 32.6 psi) |
| Red-zone threshold (25% below reference) | P_warn | 169 kPa |

**Effective permittivity factors**

| Configuration | Capacitance | Factor k |
|---|---|---|
| Air only | C = ε₀A/d | 1.00 |
| Horizontal / parallel (half air, half Teflon) | C = ε₀(A/2)/d + ε₀εr(A/2)/d | 1.55 |
| Vertical / series (air layer + Teflon layer, d/2 each) | C = C₁C₂ / (C₁ + C₂) | 1.355 |

**Pressure–gap mapping (linear, assumed):** P = P_min + (P_max − P_min) · (d_max − d) / (d_max − d_min)

**Gauge zones:** red below 169 kPa, yellow 169–225 kPa, green above 225 kPa.


## Outcome

It was observed that adding a Teflon dielectric raises the capacitance by a fixed factor at every gap, which increases the **capacitive sensitivity** (dC/dP) of the sensor.

| Metric | Air only | Vertical (series) | Horizontal (parallel) |
|---|---|---|---|
| Effective permittivity factor | 1.00 | about 1.355 | 1.55 |
| **Capacitive sensitivity improvement** | baseline | **+35.5%** | **+55%** |
| Capacitance range, 5 → 1 µm | 44 → 221 pF | 60 → 300 pF | 69 → 343 pF |
| Resonant frequency, 5 → 1 µm | 7.6 → 3.4 MHz | 6.5 → 2.9 MHz | 6.1 → 2.7 MHz |
| Absolute frequency shift per kPa | baseline | about 14% lower | about 20% lower |
| Apparent pressure at true 10 kPa | 10 kPa | 121 kPa | 161 kPa |

**Safety threshold gaps** (the gap at which the gauge crosses each zone boundary):

| Boundary | Air only | Vertical | Horizontal |
|---|---|---|---|
| Red → yellow (169 kPa) | 3.13 µm | 4.24 µm | 4.85 µm |
| Yellow → green (225 kPa) | 2.47 µm | 3.35 µm | 3.83 µm |

Key findings:

- The **horizontal (parallel)** layout gives the largest capacitive sensitivity improvement (+55%), followed by the vertical (series) layout (+35.5%). The improvement is constant over the whole 1–5 µm range.
- The gain is in **capacitance**. The relative (fractional) frequency shift per kPa is the same for all three layouts, and the absolute frequency shift per kPa is smaller for the Teflon layouts.
- The Teflon layouts reach the 225 kPa boundary at larger gaps (3.83 µm and 3.35 µm, against 2.47 µm for air). This is an offset caused by using the air-only calibration, where the sensor reads a higher apparent pressure than the true pressure. It is not an early-warning advantage: the red zone is reached only at about 23 kPa (horizontal) and 75 kPa (vertical) true pressure, so the low-pressure warning is delayed unless each layout is calibrated separately (divide C by 1.55 or about 1.355 before converting to pressure).
  
- > **Note:** This simulation considers an idea sensor model and assumes gap to pressure relationship is linear.

---
## Future Scope
- Temperature sensing and compensation.
- Four-wheel dashboard with individual alerts.
- Finite-element diaphragm model in place of the linear gap mapping.

## Applications

1. **Advanced Automotive TPMS:** Early detection of tire under-inflation to prevent blowout hazards and optimize fuel economy.
2. **Fleet & Autonomous Vehicle Telemetry:** Battery-less monitoring for high-stress environments where active sensors are impractical.


