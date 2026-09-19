# High-Efficiency Continuous Conduction Mode (CCM) DC-DC Boost Converter

[![Tool: LTspice](https://img.shields.io/badge/Simulator-LTspice-darkred.svg)](https://www.analog.com/en/resources/design-tools-and-calculators/ltspice-simulator.html)
[![Domain: Power Electronics](https://img.shields.io/badge/Domain-Power%20Electronics-blue.svg)](https://en.wikipedia.org/wiki/Power_electronics)
[![Institution: University of Bologna](https://img.shields.io/badge/B.Sc.-University%20of%20Bologna-red.svg)](https://www.unibo.it/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Analytical design, component sizing, and SPICE transient validation of a high-efficiency **DC-DC Step-Up (Boost) Converter** operating in Continuous Conduction Mode (CCM). 

Designed for the *Information Engineering Laboratory* within the B.Sc. in Electronic Engineering for Energy and Information at **Alma Mater Studiorum – Università di Bologna** (Campus of Cesena).

---

## Design Specifications & Component Sizing

The converter steps up a fixed 5 V DC supply to an output of 10 V DC across a variable load current range (200 mA to 600 mA) with an output voltage ripple strictly below 1%.

### 1. Operating Requirements
- **Input Voltage (Vin):** 5.0 V
- **Target Output Voltage (Vo):** 10.0 V
- **Output Current Range (Io):** 200 mA to 600 mA (Load: Rmax = 50.0 Ω, Rmin = 16.67 Ω)
- **Switching Frequency (fsw):** 10 kHz (Period Ts = 100 µs)
- **Output Voltage Ripple Target:** ΔVo / Vo <= 1%

### 2. Analytical Calculations & Safety Margin Overdesign
- **Theoretical Duty Cycle:**
  $$D = 1 - \frac{V_{\text{in}}}{V_o} = 1 - \frac{5}{10} = 0.50 \quad (50\%)$$
- **CCM Boundary Inductance:**
  Calculated at minimum load (Io,min = 200 mA) using:
  $$L_{\text{crit}} = \frac{V_{\text{in}} \cdot T_s}{2 I_{o,\min}} \cdot D (1 - D) = 312.5\text{ }\mu\text{H}$$
  Selected inductance with a +20% safety margin: **L = 375 µH** (Peak inductor current IL,peak = 1.6 A).
- **Output Filter Capacitance:**
  Calculated at maximum load (Rmin = 16.67 Ω) for ΔVo / Vo = 1%:
  $$C_{\min} = \frac{D \cdot T_s}{R_{\min} \cdot (\Delta V_o / V_o)} = 300\text{ }\mu\text{F}$$
  Selected capacitance with a +20% safety margin: **C = 360 µF**.

---

## Power Semiconductor Selection

- **Power MOSFET (M1 - IRFML8244):** Logic-level N-channel TrenchFET (Vds = 30 V, Id = 5.8 A, ultralow Rds(on) = 24 mΩ) to minimize conduction losses.
- **Schottky Diode (D1 - MBRS340):** Surface-mount Schottky barrier rectifier (Vrrm = 40 V, If,avg = 3.0 A) chosen for minimal forward voltage drop (Vf ≈ 0.4 V) and negligible reverse recovery time.

---

## SPICE Simulation & Performance Metrics

Automated measurement directives (`.meas`) execute in steady-state from 40 ms to 45 ms to bypass cold-start transients. Duty cycles are fine-tuned to compensate for non-ideal Schottky diode forward voltage drops.

| Operational Scenario | Duty Cycle (D) | Measured Vo | Measured Io | Output Power (Pout) | Input Power (Pin) | Conversion Efficiency (η) | Output Voltage Ripple |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Max Load (Rmin = 16.67 Ω)** | 52.0% | **10.03 V** | 601.8 mA | 6.04 W | 6.27 W | **96.27%** | **0.90%** (ΔVo ≈ 90 mV) |
| **Min Load (Rmax = 50.0 Ω)** | 51.5% | **10.01 V** | 200.3 mA | 2.01 W | 2.07 W | **96.93%** | **0.30%** (ΔVo ≈ 30 mV) |

- **CCM Confirmation:** Inductor current iL(t) stays strictly above 180 mA under minimum load (no discontinuous conduction).
- **Ripple Compliance:** Output ripple remains under the 1% threshold across the entire load spectrum (0.3% to 0.9%).

---

## Repository Structure

```text
├── docs/
│   └── Boost_converter.pdf             # Complete technical report
│
├── sim/
│   └── Boost_converter.asc             # LTspice schematic & .meas automated testbench
│
├── .gitignore
├── LICENSE
└── README.md
