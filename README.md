# Piezoelectric-Fiber-Reinforced-Composite-PRFC-
<div align="center">

# Effective Electromechanical Properties of Piezoelectric Fiber Reinforced Composites (PFRC) with Elliptical Cross-Section Fibers

### A Computational Micromechanics Study Using COMSOL Multiphysics

![COMSOL](https://img.shields.io/badge/COMSOL-Multiphysics-blue?style=for-the-badge)
![Excel](https://img.shields.io/badge/Post--Processing-Excel-green?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=for-the-badge)

---

*Determination of effective stiffness, piezoelectric coupling, and dielectric permittivity matrices of PFRC composites with elliptical fiber cross-sections using Representative Volume Element (RVE) based finite element homogenization in COMSOL Multiphysics.*

</div>

---

## 📑 Table of Contents

- [Background & Motivation](#-background--motivation)
- [Literature & Prior Work](#-literature--prior-work)
- [Objective of This Work](#-objective-of-this-work)
- [Theoretical Framework](#-theoretical-framework)
- [Model Description](#-model-description)
- [COMSOL Simulation Setup](#-comsol-simulation-setup)
- [Methodology — Step-by-Step](#-methodology--step-by-step)
- [Results & Post-Processing](#-results--post-processing)
- [Repository Structure](#-repository-structure)
- [Simulation Screenshots](#-simulation-screenshots)
- [How to Use This Repository](#-how-to-use-this-repository)
- [Key Observations](#-key-observations)
- [Future Scope](#-future-scope)
- [References](#-references)
- [Author](#-author)

---

## 🔬 Background & Motivation

**Piezoelectric materials** are a class of smart materials that exhibit electromechanical coupling — they generate an electric charge in response to applied mechanical stress (*direct piezoelectric effect*) and undergo mechanical deformation when subjected to an electric field (*converse piezoelectric effect*). This dual functionality makes them indispensable in a wide range of engineering applications:

| Application | Function |
|---|---|
| **Sensors** | Pressure sensors, accelerometers, vibration monitors |
| **Actuators** | Precision positioning, micro-pumps, adaptive structures |
| **Energy Harvesting** | Converting ambient vibrations to electrical energy |
| **Structural Health Monitoring** | Embedded sensing in aerospace and civil structures |

However, monolithic piezoelectric ceramics (such as PZT — Lead Zirconate Titanate) suffer from a critical limitation: **inherent brittleness**. They are prone to cracking under mechanical loads, thermal cycling, or impact, severely limiting their structural integration and long-term reliability.

### The Composite Solution

To overcome this limitation, **Piezoelectric Fiber Reinforced Composites (PFRCs)** were developed. In a PFRC:

- The **piezoelectric ceramic** is fabricated in the form of **fibers** (reinforcement phase)
- These fibers are embedded in a **polymeric resin matrix** (e.g., epoxy)

This architecture provides:
- ✅ **Enhanced ductility and flexibility** from the matrix
- ✅ **Retained piezoelectric functionality** from the fibers
- ✅ **Directional tailoring** of electromechanical properties
- ✅ **Improved damage tolerance** and fatigue resistance
- ✅ **Conformability** to curved surfaces

The effective properties of the composite depend on constituent material properties, fiber volume fraction, fiber geometry, and fiber orientation — making computational micromechanics essential for predicting composite behavior.

---

## 📚 Literature & Prior Work

Extensive research has been conducted on determining the effective electromechanical properties of PFRCs using analytical models (Mori-Tanaka, Self-Consistent Scheme, Asymptotic Homogenization) and numerical methods (FEM-based RVE analysis).

> **Critical gap identified:** The vast majority of existing studies assume a **circular cross-section** for the piezoelectric fiber within the RVE. While computationally convenient, this assumption does not capture the effects of fiber geometry anisotropy introduced by non-circular cross-sections.

**Elliptical cross-sections** are practically relevant because:
1. Manufacturing processes (extrusion, drawing) can produce fibers with non-circular profiles
2. Fibers may deform into elliptical shapes during composite processing
3. Elliptical geometry provides an additional **design parameter** (aspect ratio and orientation angle) to tailor electromechanical response
4. The orientation of the elliptical cross-section relative to the loading direction significantly influences stress transfer and electric field distribution

---

## 🎯 Objective of This Work

This project extends the existing body of PFRC micromechanics research by:

1. **Replacing the conventional circular fiber cross-section with an elliptical cross-section** in the RVE
2. Analyzing **two distinct composite architectures**:
   - **Case 1:** Piezoelectric fiber (elliptical) embedded directly in resin matrix
   - **Case 2:** Piezoelectric fiber (elliptical) with an intermediate **carbon fiber layer** between the piezo fiber and the resin matrix
      <img width="400" height="300" alt="Image" src="https://github.com/user-attachments/assets/cf2d7222-d7dc-4693-a4c5-bd3c050c4d9d" /> <img width="400" height="300" alt="Image" src="https://github.com/user-attachments/assets/5b8ce6e3-f241-49dd-9c6f-2aa2841cc74d" />
4. **Parametric investigation** of the effect of:
   - **Ellipse rotation angle** (orientation of the major axis in the cross-sectional plane)
   - **Temperature** (studying property degradation, especially beyond the **Curie temperature** where piezoelectric properties vanish)
5. **Computing all three effective property matrices** of the composite:
   - Effective Stiffness Matrix **[C]** (6×6)
   - Effective Piezoelectric Coupling Matrix **[e]** (3×6)
   - Effective Dielectric Permittivity Matrix **[κ]** (3×3)





---

## 📐 Theoretical Framework

### Piezoelectric Constitutive Equations

The behavior of a linear piezoelectric material is governed by **two coupled constitutive equations** that relate mechanical and electrical field variables:

**Stress-Charge Form (e-form):**

```
{σ} = [C^E]{ε}- [e]^T {E}
{D} = [e] {ε} + [κ^ε] {E}
```

Where:

| Symbol | Quantity | Units |
|---|---|---|
| `σ` | Stress tensor | N/m² (Pa) |
| `ε` | Strain tensor | dimensionless |
| `E` | Electric field vector | V/m |
| `D` | Electric displacement vector | C/m² |
| `[C^E]` | Elastic stiffness matrix (at constant **E**) — 6×6 | N/m² (Pa) |
| `[e]` | Piezoelectric coupling matrix — 3×6 | C/m² |
| `[κ^ε]` | Dielectric permittivity matrix (at constant **ε**) — 3×3 | F/m |

In **Voigt notation** (contracted indices), these become matrix equations:

```
| σ1 |   | C11 C12 C13 C14 C15 C16 | | ε1 |   | e11 e21 e31 |
| σ2 |   | C21 C22 C23 C24 C25 C26 | | ε2 |   | e12 e22 e32 |
| σ3 | = | C31 C32 C33 C34 C35 C36 | | ε3 | - | e13 e23 e33 | | E1 |
| σ4 |   | C41 C42 C43 C44 C45 C46 | | ε4 |   | e14 e24 e34 | | E2 |
| σ5 |   | C51 C52 C53 C54 C55 C56 | | ε5 |   | e15 e25 e35 | | E3 |
| σ6 |   | C61 C62 C63 C64 C65 C66 | | ε6 |   | e16 e26 e36 |


| D1 |   | e11 e12 e13 e14 e15 e16 | | ε1 |   | κ11 κ12 κ13 | | E1 |
| D2 | = | e21 e22 e23 e24 e25 e26 | | ε2 | + | κ21 κ22 κ23 | | E2 |
| D3 |   | e31 e32 e33 e34 e35 e36 | | ε3 |   | κ31 κ32 κ33 | | E3 |
                                     | ε4 |
                                     | ε5 |
                                     | ε6 |
```

### RVE Homogenization Approach

The **Representative Volume Element (RVE)** is the smallest volume element of a heterogeneous composite that is statistically representative of the overall composite behavior. By applying appropriate boundary conditions on the RVE and computing volume-averaged field quantities, one can determine the **effective (homogenized) properties** of the composite.

**Key principle:** If we apply boundary conditions such that only one component of the strain tensor is non-zero and the electric field is zero, the resulting volume-averaged stresses and electric displacements give one column of the stiffness matrix **[C]** and coupling matrix **[e]** respectively.

Similarly, if we constrain all displacements (zero strain) and apply an electric potential in one direction, the resulting volume-averaged electric displacement gives one column of the permittivity matrix **[κ]**.

---

## 🏗 Model Description

### Case 1 — Standard Elliptical PFRC

   <img width="350" height="250" alt="Screenshot 2026-09-05 155721" src="https://github.com/user-attachments/assets/dff02226-0eeb-4a2c-89e2-5a2040997560" />


- **RVE Geometry:** Cubic block of **10 mm × 10 mm × 10 mm**
- **Fiber:** Elliptical cross-section piezoelectric material (e.g., PZT-5A) running along the fiber direction (z-axis)
- **Matrix:** Polymeric resin (e.g., epoxy) surrounding the fiber
- The elliptical fiber cross-section can be **rotated** in the x-y plane by a parametric angle

### Case 2 — Carbon-Fiber-Layered Elliptical PFRC

<img width="350" height="250" alt="Screenshot 2026-09-05 154206" src="https://github.com/user-attachments/assets/1c390e13-2fab-4eb4-8009-34d71a7d54ea" />

- Same RVE dimensions as Case 1
- **Additional feature:** A **carbon fiber layer** (annular/conformal shell) wraps around the piezoelectric fiber
- This carbon fiber interphase is sandwiched between the piezo fiber and the resin matrix
- The carbon fiber layer enhances mechanical load transfer and structural reinforcement

---

## ⚙ COMSOL Simulation Setup

### Geometry & Dimensions

| Parameter | Value |
|---|---|
| RVE dimensions | 10 mm × 10 mm × 10 mm |
| Fiber cross-section | Elliptical |
| Ellipse semi-major axis (a) | Defined parametrically |
| Ellipse semi-minor axis (b) | Defined parametrically |
| Fiber direction | Along z-axis |
| Applied deformation | 0.00001 m (10 μm) |

### Physics Modules

The simulation employs a **multiphysics** coupling in COMSOL:

1. **Solid Mechanics** — for computing stress, strain, and deformation in all domains
2. **Electrostatics** — for computing electric potential, electric field, and electric displacement
3. **Piezoelectric Effect (Multiphysics Coupling)** — couples Solid Mechanics and Electrostatics in the piezoelectric fiber domain

The model tree in COMSOL includes:
- `Piezoelectric Material 1` — assigned to the fiber domain
- `Prescribed Displacement 1–5` — boundary conditions for controlled deformation
- `Electrostatics (es)` — Free Space, Zero Charge, Charge Conservation (Piezoelectric), Ground, Electric Potential
- `Multiphysics` — coupling node
- `Mesh 1` — tetrahedral mesh

### Material Assignment

| Domain | Material | Type |
|---|---|---|
| Fiber (inner) | PZT-5A (Lead Zirconate Titanate) | Piezoelectric |
| Carbon layer (Case 2 only) | Carbon Fiber | Structural |
| Matrix | Epoxy Resin | Structural |

### Parametric Study Setup

COMSOL's **Parameters** feature was used to define:

1. **Rotation Angle (θ):** Controls the orientation of the elliptical cross-section's major axis in the x-y plane. Swept through multiple angles (e.g., 0° to 90° in discrete steps).

2. **Temperature (T):** Controls the operating temperature. The piezoelectric material properties are temperature-dependent:
   - Below the **Curie temperature (T_c):** Full piezoelectric properties are active
   - Above the **Curie temperature:** The piezoelectric material undergoes a phase transition, **loses its polarization**, and the piezoelectric coupling coefficients drop to zero — the material no longer deforms in response to an applied electric potential

---

## 🔧 Methodology — Step-by-Step

### Step 1 — Determination of Stiffness Matrix [C] and Piezoelectric Coupling Matrix [e]

The stiffness and coupling matrices are determined simultaneously by applying **six independent strain states** to the RVE. For each strain state, only one component of the strain tensor is non-zero while all others are zero. The electric field is set to zero (short-circuit condition).

#### How strain is applied:

A prescribed displacement of **δ = 0.00001 m** is applied to the RVE boundaries. Given the RVE edge length **L = 0.01 m**, the resulting engineering strain is:

```
ε = δ / L = 0.00001 / 0.01 = 0.001
```

#### The Six Load Cases:

| Load Case | Applied Condition | Non-zero Strain Component | Obtained Column |
|---|---|---|---|
| **LC 1** | Tensile displacement along **X** | ε₁₁ ≠ 0 | 1st column of [C] and [e] |
| **LC 2** | Tensile displacement along **Y** | ε₂₂ ≠ 0 | 2nd column of [C] and [e] |
| **LC 3** | Tensile displacement along **Z** | ε₃₃ ≠ 0 | 3rd column of [C] and [e] |
| **LC 4** | Shear displacement in **YZ** plane | ε₂₃ ≠ 0 | 4th column of [C] and [e] |
| **LC 5** | Shear displacement in **XZ** plane | ε₁₃ ≠ 0 | 5th column of [C] and [e] |
| **LC 6** | Shear displacement in **XY** plane | ε₁₂ ≠ 0 | 6th column of [C] and [e] |

#### Extracting the matrix elements:

For each load case, after solving, the **volume-averaged stress components** (σ₁ through σ₆) and **volume-averaged electric displacement components** (D₁, D₂, D₃) are evaluated over the entire RVE using COMSOL's **Derived Values → Volume Average** feature.

From the constitutive equation (with E = 0):

```
σi = Cij · εj    ⇒    Cij = ⟨σi⟩ / εj
Di = eij · εj    ⇒    eij = ⟨Di⟩ / εj
```

where ⟨·⟩ denotes volume averaging and j corresponds to the load case number.

**Example — Load Case 1 (Tensile along X):**

Apply displacement δ along x on the +x face, fix the -x face, and leave other faces free (or appropriately constrained for periodicity). Only ε₁₁ is non-zero.

From volume-averaged results:
- σ₁ / ε₁₁ → C₁₁
- σ₂ / ε₁₁ → C₂₁
- σ₃ / ε₁₁ → C₃₁
- σ₄ / ε₁₁ → C₄₁
- σ₅ / ε₁₁ → C₅₁
- σ₆ / ε₁₁ → C₆₁
- D₁ / ε₁₁ → e₁₁
- D₂ / ε₁₁ → e₂₁
- D₃ / ε₁₁ → e₃₁

Repeating for all 6 load cases gives the **complete 6×6 stiffness matrix [C]** and the **complete 3×6 coupling matrix [e]**.

### Step 2 — Determination of Dielectric Permittivity Matrix [κ]

For the permittivity matrix, the conditions are:
- **All faces of the RVE are fixed** (prescribed displacement = 0 on all boundaries) → **strain tensor = 0 everywhere**
- An **electric potential (voltage)** is applied across two opposite faces in one direction at a time

With ε = 0, the constitutive equation reduces to:

```
Di = κij · Ej
```

#### The Three Electrical Load Cases:

| Electrical LC | Applied Potential Direction | Non-zero E Component | Obtained Column |
|---|---|---|---|
| **ELC 1** | Voltage along **X** | E₁ ≠ 0 | 1st column of [κ] |
| **ELC 2** | Voltage along **Y** | E₂ ≠ 0 | 2nd column of [κ] |
| **ELC 3** | Voltage along **Z** | E₃ ≠ 0 | 3rd column of [κ] |

For each case:
- One pair of opposite faces: one is set to **Electric Potential (V)**, the other to **Ground (0 V)**
- All remaining faces: **Zero Charge / Symmetry** condition
- All faces: **Fixed constraint** (zero displacement)

The electric field is:

```
E = V / L
```

From volume-averaged electric displacement:

```
κij = ⟨Di⟩ / Ej
```

This yields the **complete 3×3 permittivity matrix [κ]**.

### Step 3 — Parametric Sweeps (Rotation Angle & Temperature)

The entire procedure (all 6 mechanical load cases + 3 electrical load cases = 9 simulations) is repeated for each combination of:

1. **Ellipse rotation angle θ:**
   - The major axis of the elliptical cross-section is rotated in the x-y plane
   - Angles swept: 0°, 15°, 30°, 45°, 60°, 75°, 90° (or finer steps)
   - This captures the **anisotropy introduced by elliptical geometry**

2. **Temperature T:**
   - Multiple temperature values below and above the Curie temperature
   - Below T_c: Full piezoelectric coupling → material deforms under electric field
   - At/Above T_c: Piezoelectric coupling coefficients → 0, material becomes a regular dielectric ceramic → **no deformation under applied potential**
   - This captures the **thermal degradation of piezoelectric properties**

COMSOL's **Parametric Sweep** study step automates this process, solving for each (θ, T) combination and storing results in tables.

---

## 📊 Results & Post-Processing

### Data Extraction

After solving all parametric cases, the following data is extracted from COMSOL:

1. **Volume-averaged stress tensor components** (σ₁₁, σ₂₂, σ₃₃, σ₂₃, σ₁₃, σ₁₂) — from `Derived Values → Volume Average`
2. **Volume-averaged electric displacement components** (D₁, D₂, D₃) — from `Derived Values → Volume Average`
3. Results are exported to **Tables (Table 1, Table 2)** within COMSOL and then to external files

### Post-Processing in Excel

The extracted data is processed in Microsoft Excel to:

1. Compute all elements of **[C]**, **[e]**, and **[κ]** matrices for each (θ, T) combination
2. Generate plots of:
   - Individual matrix elements vs. rotation angle θ
   - Individual matrix elements vs. temperature T
   - Comparison between Case 1 (standard) and Case 2 (carbon-fiber-layered)

### Visualization in COMSOL

The following field quantities are visualized as 3D plots:

- **Von Mises Stress** distribution (N/m²) — shown via volume plots
- **Electric Potential** distribution (V) — shown via volume/surface plots
- **Electric Field** distribution (V/m) — shown via arrow/volume plots
- **Deformed shape** of the RVE under various load cases

---

## 📁 Repository Structure

```
Elliptical-PFRC-Analysis/
│
├── README.md                          # This file
│
├── COMSOL_Models/
│   ├── Elliptical_PFRC_Case1.mph      # Case 1: Standard Elliptical PFRC
│   └── Elliptical_PFRC_Case2.mph      # Case 2: Carbon-Fiber-Layered PFRC
│
├── Results/
│   ├── Case1/
│   │   ├── Stiffness_Matrix_Data.xlsx
│   │   ├── Coupling_Matrix_Data.xlsx
│   │   └── Permittivity_Matrix_Data.xlsx
│   └── Case2/
│       ├── Stiffness_Matrix_Data.xlsx
│       ├── Coupling_Matrix_Data.xlsx
│       └── Permittivity_Matrix_Data.xlsx
│
├── Plots/
│   ├── Stiffness_vs_Angle/
│   ├── Coupling_vs_Angle/
│   ├── Permittivity_vs_Angle/
│   ├── Stiffness_vs_Temperature/
│   ├── Coupling_vs_Temperature/
│   └── Permittivity_vs_Temperature/
│
├── Screenshots/
│   ├── RVE_Geometry_Case1.png
│   ├── RVE_Geometry_Case2.png
│   ├── VonMises_Tensile_X.png
│   ├── VonMises_Tensile_Z.png
│   ├── VonMises_Shear.png
│   ├── Electric_Potential.png
│   └── Deformed_Shape.png
│
└── Documentation/
    └── Methodology_Detailed.pdf
```

---

## 🖼 Simulation Screenshots

### Case 1 — Standard Elliptical PFRC

| Load Case | Von Mises Stress Distribution |
|---|---|
| Tensile along X (ε₁₁) | ![Tensile X](Screenshots/case1_tensile_x.png) |
| Tensile along Z (ε₃₃) | ![Tensile Z](Screenshots/case1_tensile_z.png) |
| Shear YZ (ε₂₃) | ![Shear YZ](Screenshots/case1_shear_yz.png) |

*Stress concentrations are clearly visible around the elliptical fiber cross-section, with maximum von Mises stress reaching ~4.5 × 10⁶ N/m² for tensile cases and ~5 × 10⁴ N/m² for electrical loading cases.*

### Case 2 — Carbon-Fiber-Layered Elliptical PFRC

| Load Case | Von Mises Stress Distribution |
|---|---|
| Tensile (high stress) | ![Case2 Tensile](Screenshots/case2_tensile.png) |
| Electrical loading | ![Case2 Electric](Screenshots/case2_electric.png) |

*The presence of the carbon fiber layer creates a distinct stress distribution pattern, with stress transfer occurring through the carbon layer. Maximum von Mises stress reaches ~1.9 × 10⁷ N/m² due to the high stiffness of the carbon fiber interphase.*

> **Note:** The elliptical fiber cross-section and carbon fiber layer boundary are clearly visible in the top-view cross-sectional plots.

---

## 🚀 How to Use This Repository

### Prerequisites

- **COMSOL Multiphysics** (version 5.x or 6.x) with:
  - Structural Mechanics Module
  - Piezo Electricity
- **Microsoft Excel** or equivalent for data post-processing
- Basic understanding of piezoelectric materials and composite mechanics

### Steps to Reproduce

1. **Open the COMSOL model** (`.mph` file) in COMSOL Multiphysics

2. **Set parameters:**
   - Adjust the ellipse rotation angle `theta` in the Parameters node
   - Adjust the temperature `T` in the Parameters node
   - Modify ellipse semi-major axis `a` and semi-minor axis `b` if desired

3. **Run the study:**
   - For individual cases: Run `Study 1 → Stationary`
   - For parametric sweeps: Enable the Parametric Sweep node and specify the parameter range

3. **Extract results:**
   - Navigate to `Results → Derived Values → Volume Average`
   - Evaluate stress components and electric displacement components
   - Export data from `Tables` to Excel

5. **Compute effective properties:**
   - Use the formulas described in the Methodology section to compute [C], [e], [κ] from the volume-averaged quantities

---

## 🔑 Key Observations

1. **Effect of Ellipse Rotation Angle:**
   - The stiffness and coupling coefficients exhibit a **sinusoidal variation** with rotation angle due to the geometric anisotropy of the elliptical cross-section
   - At 0° and 90°, the cross-section is aligned with the principal axes, yielding extreme values
   - At 45°, off-diagonal coupling terms become significant

2. **Effect of Temperature:**
   - Below the Curie temperature: All three matrices show stable values
   - As temperature approaches T_c: Gradual degradation of piezoelectric coupling coefficients
   - Above T_c: **Coupling matrix [e] → 0** — the composite loses its piezoelectric functionality entirely
   - Stiffness matrix shows modest variation with temperature (thermal softening)
   - Permittivity matrix shows anomalous behavior near T_c (dielectric anomaly)

3. **Effect of Carbon Fiber Layer (Case 2 vs Case 1):**
   - The carbon fiber layer **increases the effective stiffness** of the composite significantly
   - **Higher stress concentrations** are observed at the carbon-piezo interface
   - The coupling coefficients are affected by the presence of the non-piezoelectric carbon layer
   - The carbon layer provides improved mechanical load transfer from matrix to piezo fiber

---

## 🔮 Future Scope

- [ ] Extend to **other cross-sectional shapes** (rectangular, diamond, star-shaped fibers)
- [ ] Implement **periodic boundary conditions** for more accurate RVE homogenization
- [ ] Include **nonlinear piezoelectric behavior** at high electric fields
- [ ] Study the effect of **fiber volume fraction** variation
- [ ] Validate computational results with **analytical models** (Mori-Tanaka, Eshelby)
- [ ] Investigate **dynamic/frequency-dependent** effective properties
- [ ] Couple with **thermal expansion** effects for thermo-electromechanical analysis
- [ ] Extend to **multi-fiber model** and random fiber distribution

---

## 📖 References

1. Berger, H., Kari, S., Gabbert, U., et al. "An analytical and numerical approach for calculating effective material coefficients of piezoelectric fiber composites." *International Journal of Solids and Structures*, 42(21-22), 5692-5714, 2005.

2. Kar-Gupta, R., & Venkatesh, T.A. "Electromechanical response of piezoelectric composites: Effects of geometric connectivity and grain size." *Acta Materialia*, 56(15), 3810-3823, 2008.

3. Pettermann, H.E., & Suresh, S. "A comprehensive unit cell model: a study of coupled effects in piezoelectric 1–3 composites." *International Journal of Solids and Structures*, 37(39), 5447-5464, 2000.

4. IEEE Standard on Piezoelectricity, ANSI/IEEE Std 176-1987.

5. COMSOL Multiphysics Reference Manual & Piezoelectric Devices Module User's Guide.

---

## 👤

**[D Lakshman Nithish ]**

- 🎓 [Mechanical Engineering / IIT BHUBANESWAR]
- 📧 [24me01030@iitbbs.ac.in
      lakshman4933@gmail.com]
- 🔗 []

---

## 📄 License

This project is licensed under the MIT License —

---

<div align="center">

**⭐ If you find this project useful, please consider giving it a star! ⭐**

*This project was developed as part of [Composites / research project] at [University name] under guidance of [DR SOUMY RANJAN SAHOO].*

</div>
