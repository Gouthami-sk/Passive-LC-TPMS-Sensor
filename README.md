# Passive Wireless LC-TPMS Sensor: Dielectric Optimization Study

A MATLAB simulation framework that models and optimizes passive wireless Tire Pressure Monitoring Systems (TPMS) by introducing a dielectric layer.

## About
Traditional passive TPMS sensors often suffer from weak signal responses and limited sensitivity due to standard air-filled capacitor constraints.
This project investigates how structuring internal dielectric configurations—comparing Air, Vertical/Series, and Horizontal/Parallel layouts—can drastically amplify sensor sensitivity without altering physical dimensions or requiring active battery power for real-time telemetry and safety threshold tracking.

---

## Core Architecture 
The system is modeled around a passive wireless telemetry loop consisting of:
1. **Reader Coil:** An external coil that sweeps across frequency bands to interrogate the sensor.
2. **Planar Micro-Inductor ($L$):** A fixed inductive element that stores magnetic energy and acts as the receiver antenna.
3. **Variable Dielectric Capacitor ($C$):** The pressure-sensing core where mechanical gap displacement ($d$) alters total capacitance.
4. **Composite Dielectric Layers:** Strategic integration of Air ($\epsilon_r = 1.0$) and Polytetrafluoroethylene (Teflon) ($\epsilon_r = 2.1$).

Capacitor and Inductor are connected in series to form a LC circuit 
---

## Working Principle

- **Mechanical input:** Tire deflation or external load reduces the distance between parallel plates of the capacitor ($d$).
- **Change in Capacitance:** As $d$ decreases, total capacitance ($C$) increases.

$$C = \frac{\varepsilon_0 \. \varepsilon_{eff} \. A}{d}$$

  where $\varepsilon_0$ is the permittivity of free space, $\varepsilon_{eff}$ is the effective relative permittivity of the dielectric, $A$ is the plate area and $d$ is the gap.
- **Change in resonant Frequency:** Increase in $C$ results in drop of the resonant frequency, governed by the LC resonance equation:

$$f_{res} = \frac{1}{2\pi\sqrt{L\.C}}$$

- **The wireless handshake (impedance dip):** The external reader sweeps frequencies. When its frequency matches the sensor's natural $f_{res}$, inductive coupling draws energy from the reader, creating a measurable impedance dip.

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

All three layouts use $C = \dfrac{\varepsilon_0 \. \varepsilon_{eff} \. A}{d}$ with a different $\varepsilon_{eff}$:

| Configuration | How it combines | $\varepsilon_{eff}$ |
|---|---|---|
| Air only | Plain air gap | $1.00$ |
| Horizontal / parallel (half air, half Teflon) | $\dfrac{1.0 + 2.1}{2}$ | $1.55$ |
| Vertical / series (air layer + Teflon layer, $d/2$ each) | $\dfrac{2 \times 1.0 \times 2.1}{1.0 + 2.1}$ | $\approx 1.355$ |

**Pressure Mapping:**
   
   $$P = P_{min} + (P_{max} - P_{min}) \cdot \frac{d_{max} - d}{d_{max} - d_{min}}$$

**Gauge zones:** red below 169 kPa, yellow 169–225 kPa, green above 225 kPa.

---

## Dashboard Preview (Constant $2.46\text{ }\mu\text{m}$ Gap Comparison)
*Comparing response characteristics across configurations at an identical physical gap position:*

<p align="center">
  <img src="assets/2.46um_air.png" width="32%" alt="Air Baseline">
  <img src="assets/2.46um_vertical.png" width="32%" alt="Vertical Series">
  <img src="assets/2.46um_harizontal.png" width="32%" alt="Horizontal Parallel">
</p>

---

## Outcome

With the dashboard normalized to track true pressure, the pressure gauge remains consistent across layouts for any given physical gap. Instead of shifting calibration offsets, the advantage of adding Teflon is directly quantified through **transduction sensitivity ($S_C = dC/dP$)** and **minimum detectable resolution ($\Delta P_{min}$)** assuming a fixed external reader resolution ($\Delta C_{res} = 2\text{ pF}$).

| Metric | Air only | Vertical (series) | Horizontal (parallel) |
|---|---|---|---|
| Effective permittivity factor ($\varepsilon_{eff}$) | 1.00 | about 1.355 | 1.55 |
| **Capacitive sensitivity ($S_C$ at $2.46\text{ }\mu\text{m}$)** | **0.429 pF/kPa** | **0.581 pF/kPa** | **0.665 pF/kPa** |
| **Minimum detectable pressure change ($\Delta P_{min}$)** | **4.66 kPa** | **3.44 kPa** | **3.01 kPa (Highest Resolution)** |
| Capacitance at $2.46\text{ }\mu\text{m}$ | 89.8 pF | 121.7 pF | 139.2 pF |
| Resonant Frequency | 5.31 MHz | 4.56 MHz | 4.27 MHz |

Key findings:

- **Maximized Sensitivity:** The **horizontal (parallel)** layout achieves the highest capacitive sensitivity ($0.665\text{ pF/kPa}$ at $2.46\text{ }\mu\text{m}$), requiring a smaller minimum detectable pressure change ($\Delta P_{min} = 3.01\text{ kPa}$) compared to the air baseline ($4.66\text{ kPa}$).
- **Signal Resolution Advantage:** Because Teflon multiplies total capacitance without distorting true pressure normalization, the enhanced $dC/dP$ slope allows external reader circuits to resolve finer pressure fluctuations.
- **Resonance Shift:** Incorporating high-permittivity dielectrics lowers absolute resonant frequencies (down to $4.27\text{ MHz}$ for horizontal vs. $5.31\text{ MHz}$ for air at the same gap), providing distinct frequency bands for multi-material sensor design.

> **Note:** This simulation considers an ideal sensor model and assumes a linear gap-to-pressure relationship.

---

## Future Scope
- Temperature sensing and compensation.
- Four-wheel dashboard with individual alerts.
- Finite-element diaphragm model in place of the linear gap mapping.

## Applications

1. **Advanced Automotive TPMS:** Early detection of tire under-inflation to prevent blowout hazards and optimize fuel economy.
2. **Fleet & Autonomous Vehicle Telemetry:** Battery-less monitoring for high-stress environments where active sensors are impractical.
