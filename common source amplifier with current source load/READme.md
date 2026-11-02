# Single-Stage Common-Source Amplifier with Current-Source Load

A high-gain, high-bandwidth single-stage Common-Source (CS) analog amplifier designed and simulated in **Cadence Virtuoso** using the **GPDK90 (90nm)** Process Design Kit (PDK). 

This design implements a wide NMOS driver coupled with a PMOS active current-source load to maximize output impedance (ro) and maximize small-signal voltage gain without relying on passive resistive loads.

---

## 🛠️ Circuit Schematic & Architecture

The amplifier consists of a standard NMOS driver (`NM0`) operating under a fixed power rail, loaded by a PMOS current source (`PM0`) biased in the saturation region.

*   **Technology Node:** GPDK90 (90nm CMOS)
*   **Supply Voltage (VDD):** 1.2 V
*   **Architecture:** Common-Source with PMOS Current-Source Load

### Transistor Sizing & Parameters

| Transistor | Instance Name | Model | Total Width (W) | Length (L) | Multiplicity (m) | Layout Strategy |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **NMOS Driver** | `NM0` | `nmos1v` | 40 um | 360 nm | 1 | Multi-finger (Wf = 5 um, nf = 8) |
| **PMOS Load** | `PM0` | `pmos1v` | 80 um | 360 nm | 1 | Multi-finger (Wf = 5 um, nf = 16) |

---

## 📈 Bias & Simulation Specifications

To guarantee symmetric maximum output voltage swing and keep both transistors locked firmly in **Saturation (Region 2)**, the following bias conditions were established via DC analysis sweeps:

### DC Operating Points

*   **VDD Power Rail:** 1.2 V
*   **PMOS Bias Voltage (Vbias):** 750 mV
*   **NMOS Input DC Bias (Vin,DC):** 444.26 mV *(From mid-supply transfer curve alignment)*
*   **Quiescent Output Voltage (Vout,DC):** 596.32 mV (~ VDD/2)

### Testbench Signal Configuration (`vsin`)
*   **DC Voltage:** 444.26 mV
*   **AC Magnitude:** 1 V
*   **Transient Amplitude:** 5 mV (10 mV Peak-to-Peak)
*   **Input Frequency:** 1 kHz *(Transient Test)* / Full Spectrum Sweep *(AC Test)*

---

## 🚀 Performance Summary & Optimization Results

The circuit underwent geometric optimization to combat deep sub-micron short-channel non-idealities (specifically Channel Length Modulation). Increasing the channel length and scaling widths proportionally yielded a substantial increase in output resistance (ro) and voltage gain.

### Comparison: Baseline vs. Optimized Design

| Design Phase | Transistor Dimensions (W/L) | DC Input Bias (Vin) | Transient Output (Vout, p-p) | Voltage Gain (Av) |
| :--- | :--- | :--- | :--- | :--- |
| **Initial Baseline** | NMOS: 20 um / 180 nm<br>PMOS: 40 um / 180 nm | 412.00 mV | 76.52 mV | **7.652** (17.67 dB) |
| **Optimized Stage** | NMOS: 40 um / 360 nm<br>PMOS: 80 um / 360 nm | 444.26 mV | 198.75 mV | **19.938** (26.02 dB) |

### Final Verified Performance Metrics
*   **3dB Cutoff Frequency (f3dB):** **720 MHz**
*   **True Gain-Bandwidth Product (GBW):** **14.399 GHz** *(Gain_linear x f3dB)*
*   **Signal Integrity:** Perfectly smooth, undistorted transient output wave with zero voltage rail clipping or compression.
