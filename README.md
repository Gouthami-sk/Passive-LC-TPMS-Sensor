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

The project implemented a parametric simulation framework in MATLAB App Designer to evaluate how internal dielectric structuring impacts passive sensor performance. By systematically comparing an air baseline against a vertical series stack and a horizontal parallel layout (integrating Teflon, $\epsilon_r = 2.1$).

**The Outcome:** 
The study successfully demonstrated that the **Horizontal (Parallel)** configuration—utilizing an effective permittivity of $\epsilon_{eff} = 1.55$—drastically outperforms traditional models. It yields a **55.5% sensitivity improvement** in standard ranges, which scales up to **59.6% in higher frequency bands (>5 MHz)** due to reduced capacitive reactance. Furthermore, when mapped against a strict **225 kPa safety reference**, the optimized horizontal design provides a preemptive warning at a wider, safer gap of $3.8\text{ }\mu\text{m}$ compared to $2.47\text{ }\mu\text{m}$ for the standard air baseline, proving its superior viability for high-resolution automotive telemetry. The entire workflow is managed through an interactive GUI built with custom MATLAB App Designer callbacks (`startupFcn` and `SliderValueChanged`) for real-time telemetry and live threshold tracking.

---

## Applications
1. **Advanced Automotive TPMS:** Early detection of tire under-inflation to prevent blowout hazards and optimize fuel economy.
2. **Fleet & Autonomous Vehicle Telemetry:** Battery-less monitoring for high-stress environments where active sensors are impractical.

---

DME.md                # Project documentation
