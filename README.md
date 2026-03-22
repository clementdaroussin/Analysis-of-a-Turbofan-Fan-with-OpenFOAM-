# 🚀 CFD-Based Aeroacoustic Analysis of a Turbofan (OpenFOAM)

**Clément DAROUSSIN - February 2025** *Self-Initiated Project | Computational Fluid Dynamics | Aeroacoustics*

## 📌 Overview
This project focuses on the **transient aeroacoustic and aerodynamic behavior** of a high-bypass turbofan (inspired by GE90/Trent 1000) during its startup phase. 
The simulation was performed using **OpenFOAM 2406** on a dynamic mesh (AMI) to capture the complex rotor-stator interactions.

> **Key Challenge:** Successfully running a complex transient rotating case with limited hardware (Google Colab / 2 CPUs), requiring extreme optimization of the meshing strategy and solver stability.

---

## 🛠️ Numerical Methodology

### 1. Meshing Strategy (snappyHexMesh)
* **Base Mesh:** Structured `blockMesh` with non-uniform grading to focus resolution on the rotating zone.
* **Refinement:** ~588k cells, predominantly hexahedral for numerical accuracy.
* **Gap Management:** Implementation of `gapLevel` to resolve flow between the 30 blades without exploding the global cell count.
* **AMI Interface:** Setup of an **Arbitrary Mesh Interface (AMI)** to handle the rigid body rotation between the stationary and rotating domains.

### 2. Physical Model & Solver
* **Solver:** `pimpleFoam` (Transient, Incompressible).
* **Turbulence Modeling:** `k-ω SST` (RANS) to accurately capture flow separation and near-wall effects on the blades.
* **Dynamic Motion:** Table-driven `omega` profile simulating a smooth transient acceleration from **0 to 50 rad/s** over 0.5s.
* **Numerical Schemes:** Second-order spatial discretization with implicit Euler time-stepping for maximum stability.

---

## 📊 Key Aerodynamic Results

### Velocity & Pressure Fields
* **Radial Diffusion:** Observation of the flow expanding outward due to the absence of a duct, a characteristic behavior in open-fan configurations.
* **Startup Phase:** Identification of the velocity peak (~17 m/s) at the blade tips, combining rotational speed and axial induction.
* **Pressure Profiles:** Low-pressure regions identified at the trailing edges, consistent with aerodynamic lift and thrust generation.

### Turbulence & Vorticity
* **Tip Vortices:** Visualization of circular vortical structures at the blade tips using the **Q-Criterion**.
* **Turbulent Kinetic Energy (k):** Mapping of energy dissipation zones, highlighting the wake region as the primary source of aerodynamic losses.
* **Dissipation (ω):** High dissipation rates located in the shear layers, providing a proxy for potential noise generation zones.

---

## 📂 Repository Structure
* `0/`: Initial & Boundary conditions (`p`, `U`, `k`, `omega`).
* `constant/`: Physical properties and `dynamicMeshDict`.
* `system/`: Solver control (`fvSolution`) and discretization schemes (`fvSchemes`).
* `postProcessing/`: Extracted data for forces and probes.

---

## 🚀 How to Run
1. **Mesh Generation:**
   ```bash
   surfaceFeatureExtract
   blockMesh
   snappyHexMesh -overwrite
