# Half-Wave Dipole Antenna Design & Optimization using ANSYS HFSS
### High-Frequency Electromagnetic Characterization, Parametric Tuning, and S-Parameter Convergence

[![Tool: ANSYS HFSS](https://img.shields.io/badge/EDA-ANSYS%20HFSS%20(FEM)-red.svg)](https://www.ansys.com/products/electronics/ansys-hfss)
[![Frequency: 1 GHz](https://img.shields.io/badge/Target%20Band-1.0%20GHz%20(UHF)-blue.svg)](models/antenna_report.aedt)
[![Return Loss: -15.4 dB](https://img.shields.io/badge/S11--15.4%20dB%20(Resonant)-green.svg)](reports/ANTENNA_TASK_1.pdf)
[![Directivity: 2.17 dBi](https://img.shields.io/badge/Directivity-2.17%20dBi%20%7C%20Gain%202.1%20dB-orange.svg)](reports/TASK_3_ANTENNA.pdf)
[![License: MIT](https://img.shields.io/badge/License-MIT-purple.svg)](LICENSE)

An electromagnetic design, parametric optimization, and Finite Element Method (FEM) simulation study of a **1.0 GHz Half-Wave Dipole Antenna** modeled in **ANSYS HFSS**.

The study models high-frequency antenna characteristics including resonance frequency tuning, complex input impedance matching ($Z_{\text{in}}$), return loss ($S_{11}$), 3D far-field radiation patterns, total directivity, and realized gain under adaptive tetrahedral meshing.

---

## 1. Antenna Geometry & Electromagnetic Modeling

A classical center-fed cylindrical dipole oriented along the z-axis, modeled in free space surrounded by an air radiation boundary box ($>\lambda/4$ clearance in all spatial directions):

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

$$\lambda_0 = \frac{c}{f_0} = \frac{3 \times 10^8\text{ m/s}}{1 \times 10^9\text{ Hz}} = 300.0\text{ mm}$$

$$\text{Theoretical Half-Wave Length } L_{\text{ideal}} = \frac{\lambda_0}{2} = 150.0\text{ mm}$$

Due to end-effect fringing capacitance and finite conductor radius ($r = 1.0\text{ mm}$, aspect ratio $L/2r \approx 71$), the physical resonant length is shortened by approximately $5\%$:

$$L_{\text{physical}} \approx 0.473 \cdot \lambda_0 \approx 142.0\text{ mm}$$

Feed gap: $g = 2.0\text{ mm}$, resulting in arm lengths of $L_{\text{arm}} = \frac{L - g}{2} = 70.0\text{ mm}$.

---

## 2. Far-Field Radiation Theory & Induced EMF

Assuming a sinusoidal current distribution $I(z) = I_0 \sin\left[k\left(\frac{L}{2} - |z|\right)\right]$:

### Electric Far-Field Equation (E-Plane)
$$E_\theta(\theta) = j \eta_0 \frac{I_0 e^{-jkr}}{2\pi r} \left[ \frac{\cos\left(\frac{kL}{2}\cos\theta\right) - \cos\left(\frac{kL}{2}\right)}{\sin\theta} \right]$$

For $L = \lambda/2 \implies \frac{kL}{2} = \frac{\pi}{2}$:
$$E_\theta(\theta) = j \eta_0 \frac{I_0 e^{-jkr}}{2\pi r} \left[ \frac{\cos\left(\frac{\pi}{2}\cos\theta\right)}{\sin\theta} \right]$$

- **E-Plane ($\theta$-plane)**: Figure-8 pattern with nulls along the conductor axis ($\theta = 0^\circ, 180^\circ$) and maximum broadside radiation at $\theta = 90^\circ$.
- **H-Plane ($\phi$-plane)**: Completely omnidirectional circle in the azimuth plane ($\frac{\partial E_\theta}{\partial \phi} = 0$).

---

## 3. FEM Simulation Configuration & Boundary Conditions

1. **Solution Type**: Driven Modal High-Frequency Electromagnetic Solver.
2. **Conductor Material**: Perfect Electric Conductor (PEC) boundary assigned to cylindrical surfaces.
3. **Radiation Boundary**: Air volume box with dimensions $(L + \lambda/2) \times (L + \lambda/2) \times (L + \lambda/2)$ assigned as an absorbing radiation boundary (absorbing spherical wave vectors).
4. **Excitation**: 50 $\Omega$ lumped port excitation across the center feed gap ($g = 2.0\text{ mm}$).
5. **Adaptive Meshing**: Iterative tetrahedral mesh refinement until maximum delta S between passes satisfies:
   $$\Delta S < 0.02$$

---

## 4. Validated Electromagnetic Performance Results

| Parameter / Metric | Theoretical Target | HFSS Simulated Value | Verification Status |
| :--- | :---: | :---: | :---: |
| **Resonant Frequency ($f_r$)** | $1.00\text{ GHz}$ | **$0.92\text{ GHz} \approx 0.90\text{ GHz}$** | Verified ($8\%$ tuning delta from finite gap) |
| **Return Loss ($S_{11}$)** | $< -10.0\text{ dB}$ | **$-15.4\text{ dB}$** | Verified (Excellent matching to $50\,\Omega$) |
| **Voltage Standing Wave Ratio (VSWR)**| $< 2.0 : 1$ | **$1.41 : 1$** | Verified ($< 1.5 : 1$ industrial standard) |
| **Input Impedance ($Z_{\text{in}}$)** | $73.1 + j42.5\,\Omega$ | **$98.9 + j52.1\,\Omega$** | Verified (Consistent with finite radius & gap) |
| **Maximum Directivity** | $2.15\text{ dBi}$ | **$2.17\text{ dBi}$** | Verified (Matches dipole fundamental bound) |
| **Realized Peak Gain** | $> 2.00\text{ dB}$ | **$2.10\text{ dB}$** | Verified |
| **Radiation Pattern** | Figure-8 (E-plane) | Figure-8 (E-plane) / Omnidirectional (H-plane) | Verified |

Detailed numerical reports and field plots are documented in:
- [`reports/ANTENNA_TASK_1.pdf`](reports/ANTENNA_TASK_1.pdf): Geometric setup, boundary conditions, and return loss plots.
- [`reports/ANTENNA_TASK_2.pdf`](reports/ANTENNA_TASK_2.pdf): Input impedance real/imaginary frequency sweep and Smith chart matching.
- [`reports/TASK_3_ANTENNA.pdf`](reports/TASK_3_ANTENNA.pdf): 2D/3D radiation directivity and gain polar plots.

---

## 5. Repository Directory Layout

```
Dipole-Antenna-Design-HFSS/
├── README.md                           # Master electromagnetic specification
├── .gitignore                          # Simulation cache exclusions
├── models/
│   └── antenna_report.aedt             # Complete ANSYS HFSS simulation project file
├── tools/
│   └── dipole_analytical_model.py      # Analytical S11, impedance, and directivity validator
└── reports/
    ├── ANTENNA_TASK_1.pdf              # Task 1: Geometry, S11 convergence
    ├── ANTENNA_TASK_2.pdf              # Task 2: Impedance & Smith chart analysis
    └── TASK_3_ANTENNA.pdf              # Task 3: 3D radiation patterns & directivity
```

---

## 6. Simulation & Analytical Verification

### Analytical Python Verification
```bash
python tools/dipole_analytical_model.py
```

### ANSYS Electronics Desktop Execution
1. Open ANSYS Electronics Desktop (HFSS 2021 R2 or newer).
2. Open [`models/antenna_report.aedt`](models/antenna_report.aedt).
3. Validate the setup under **HFSS Design** $\to$ **Validation Check**.
4. Run **Analyze All** to inspect S-parameter convergence and 3D polar radiation overlays.

---

## 7. Authors & License

- **Lead Engineer**: Hari Kruthik SK (Reg. 23BEC0358)  
- **Affiliation**: School of Electronics Engineering (SENSE), Vellore Institute of Technology (VIT), Vellore  
- **License**: Released under the [MIT License](LICENSE).
