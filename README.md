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

---

## Working Principle

* **Mechanical Input:** Tire deflation or external load compresses the capacitive gap ($d$).
* **Electrical Shift:** The reduction in $d$ increases total capacitance ($C$).
* **Frequency Regulation:** Governed by the classic LC resonance equation:
  $$f_{res} = \frac{1}{2\pi\sqrt{L \cdot C}}$$
* **The Wireless Handshake (Impedance Dip):** The external reader sweeps frequencies. When its frequency matches the sensor's natural $f_{res}$, inductive coupling draws energy from the reader, creating a sharp, measurable **Impedance Dip** on the receiver side.

---

## Project Technicalities
* **Effective Permittivity Scaling:** The project evaluates the arithmetic mean for **Horizontal (Parallel)** configurations ($\epsilon_{eff} = 1.55$) versus the harmonic mean for **Vertical (Series)** configurations ($\epsilon_{eff} \approx 1.35$). The parallel layout maximizes electric flux through high-permittivity zones, yielding a massive **55.5% sensitivity gain**.
* **High-Frequency Supercharge:** In higher frequency bands (>5 MHz), reduced capacitive reactance stretches the frequency tuning range, pushing the sensitivity improvement up to **59.6%**.
* **Safety Threshold Integration:** A strict operational reference is set at **225 kPa**. Because of the amplified sensitivity, the horizontal configuration triggers an early warning alert at a wide, safe gap of $3.8\text{ }\mu\text{m}$, compared to $2.47\text{ }\mu\text{m}$ for the standard air baseline.
* **MATLAB App Designer Environment:** The core application utilizes customized `startupFcn` and `SliderValueChanged` callbacks to render real-time telemetry plots, dynamic dielectric switching, and interactive safety limit markers.

---

## Applications
1. **Advanced Automotive TPMS:** Early detection of tire under-inflation to prevent blowout hazards and optimize fuel economy.
2. **Fleet & Autonomous Vehicle Telemetry:** Battery-less monitoring for high-stress environments where active sensors are impractical.

---

DME.md                # Project documentation
