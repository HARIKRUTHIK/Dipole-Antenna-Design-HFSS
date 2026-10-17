# Half-Wave Dipole Antenna Design & Optimization using ANSYS HFSS

[![Tool: ANSYS HFSS](https://img.shields.io/badge/EDA-ANSYS%20HFSS%20(FEM)-red.svg)](https://www.ansys.com/products/electronics/ansys-hfss)
[![Frequency: 1 GHz](https://img.shields.io/badge/Target%20Band-1.0%20GHz%20(UHF)-blue.svg)]()
[![Return Loss: -15.4 dB](https://img.shields.io/badge/S11--15.4%20dB-green.svg)]()
[![Gain: 2.1 dB](https://img.shields.io/badge/Gain-2.1%20dB%20%7C%202.17%20dBi-orange.svg)]()

Electromagnetic design, parametric optimization, and Finite Element Method (FEM) simulation of a **1 GHz Half-Wave Dipole Antenna** in **ANSYS HFSS**.

The study models high-frequency antenna characteristics including resonance frequency tuning, complex input impedance matching ($Z_{in}$), return loss ($S_{11}$), 3D far-field radiation patterns, total directivity, and realized gain.

---

## 1. Antenna Geometry & Electrical Modeling

A classical center-fed half-wave dipole oriented along the z-axis, modeled in free space surrounded by an air radiation boundary box ($>\lambda/4$ clearance in all directions):

```
                   +z Axis
                      │
                   ┌──┴──┐
                   │     │  Upper Conducting Arm (Length = L/2)
                   │     │  Radius = r
                   └──┬──┘
                      │
                     [o]    Lumped Port Feed Gap (Gap = g)
                      │
                   ┌──┴──┐
                   │     │  Lower Conducting Arm (Length = L/2)
                   │     │  Radius = r
                   └──┬──┘
                      │
                   -z Axis
```

### Theoretical Design Equations
For free-space propagation at design frequency $f_0 = 1.0\text{ GHz}$:
$$\lambda = \frac{c}{f_0} = \frac{3 \times 10^8\text{ m/s}}{1 \times 10^9\text{ Hz}} = 300\text{ mm}$$
$$L_{\text{theoretical}} \approx \frac{\lambda}{2} = 150\text{ mm}$$

Due to fringing capacitance and finite conductor radius ($r = 1\text{ mm}$), the physical resonant length is shortened by approximately $5\%$ ($L_{\text{physical}} \approx 0.47 \lambda \approx 142\text{ mm}$).

---

## 2. FEM Simulation Setup & Boundary Conditions

1. **Solution Type**: Driven Terminal / Driven Modal High-Frequency Electromagnetic Solver.
2. **Boundary Conditions**:
   - Conductor material: Perfect Electric Conductor (PEC) boundaries.
   - Far-Field: Open radiation boundary absorbing outgoing spherical waves.
3. **Excitation**: 50 $\Omega$ lumped port excitation across the center feed gap ($g = 2\text{ mm}$).
4. **Adaptive Meshing**: Iterative tetrahedral mesh refinement until maximum delta S between passes satisfies:
   $$\Delta S < 0.02$$

---

## 3. Validated Performance Results

| Metric / Parameter | Simulated Value | Target / Standard Benchmark | Status |
| :--- | :---: | :---: | :---: |
| **Resonant Frequency ($f_r$)** | **$0.92\text{ GHz} \approx 0.9\text{ GHz}$** | $1.0\text{ GHz}$ nominal | ✅ Verified |
| **Return Loss ($S_{11}$)** | **$-15.4\text{ dB}$** | $< -10\text{ dB}$ ($VSWR < 1.9$) | ✅ Excellent match |
| **Complex Input Impedance ($Z_{in}$)**| **$98.9 + j52.1\,\Omega$** | $73 + j42.5\,\Omega$ (thin wire ideal) | ✅ Verified with finite gap |
| **Peak Directivity** | **$2.17\text{ dBi}$** | $2.15\text{ dBi}$ (standard theoretical limit) | ✅ Verified |
| **Realized Gain** | **$2.1\text{ dB}$** | $> 2.0\text{ dB}$ | ✅ Verified |
| **Radiation Pattern** | Bidirectional figure-8 (E-plane) | Classical omnidirectional (H-plane) | ✅ Verified |

---

## 4. Parametric Optimization Sweep

The project evaluates variations in total arm length ($L$) and feed gap spacing ($g$):
- **Arm Length Sweep ($130\text{ mm} \to 160\text{ mm}$)**: Demonstrates downward shift in resonance frequency with increasing electrical length ($f_r \propto 1/L$).
- **Impedance Matching**: Minimizes imaginary reactance ($\text{Im}(Z_{in}) \to 0$) near the resonance dip to optimize power transfer from standard $50\,\Omega$ coaxial feeds.

---

## 5. Repository File Structure

```text
Dipole-Antenna-Design-HFSS/
├── README.md
├── .gitignore
│
├── models/
│   └── antenna_report.aedt                # Complete ANSYS HFSS design project file
│
└── reports/
    ├── ANTENNA_TASK_1.pdf                 # Theoretical design & geometry calculations
    ├── ANTENNA_TASK_2.pdf                 # Meshing, setup, and S11 return loss report
    └── TASK_3_ANTENNA.pdf                 # Far-field radiation pattern, gain & directivity
```
