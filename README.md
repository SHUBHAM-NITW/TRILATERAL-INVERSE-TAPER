# Trilateral Inverse Taper — Photonic Edge Coupler

> A high-efficiency fiber-to-chip edge coupler based on a novel trilateral inverse taper geometry, designed for silicon photonic integrated circuits. Simulated using **Lumerical FDTD** and **Lumerical MODE**.

---

## Table of Contents

1. [Introduction](#introduction)
2. [What is an Edge Coupler?](#what-is-an-edge-coupler)
3. [What is a Taper?](#what-is-a-taper)
4. [Straight Taper vs. Inverse Taper](#straight-taper-vs-inverse-taper)
   - [Straight (Adiabatic) Taper](#straight-adiabatic-taper)
   - [Inverse Taper](#inverse-taper)
   - [Why Use One Over the Other?](#why-use-one-over-the-other)
5. [Trilateral Inverse Taper — The Proposed Design](#trilateral-inverse-taper--the-proposed-design)
   - [Geometry and Cross-Section](#geometry-and-cross-section)
   - [Design Parameters](#design-parameters)
6. [Simulation Setup](#simulation-setup)
   - [Lumerical FDTD — Coupling Efficiency](#lumerical-fdtd--coupling-efficiency)
   - [Lumerical MODE — Mode Analysis](#lumerical-mode--mode-analysis)
7. [Results](#results)
8. [File Structure](#file-structure)
9. [References](#references)

---

## Introduction

Coupling light between an **optical fiber** and a **photonic chip** is one of the most fundamental and challenging problems in silicon photonics. The large mismatch between the mode field diameter (MFD) of a standard single-mode fiber (~10 µm) and a silicon nanowire waveguide (~450 nm × 220 nm) leads to significant insertion loss at the chip facet.

This project proposes and simulates a **Trilateral Inverse Taper** — a novel edge coupler structure with a triangular cross-sectional geometry — to maximize fiber-to-chip coupling efficiency while minimizing fabrication complexity.

---

## What is an Edge Coupler?

An **edge coupler** (also called a **facet coupler**) is a waveguide structure placed at the cleaved or polished edge of a photonic chip that allows light to be coupled directly from an optical fiber into an on-chip waveguide.

```
    Optical Fiber                     On-chip Waveguide
    (MFD ~10 µm)                      (width ~450 nm)
         |                                    |
         |  ------[  Edge Coupler  ]------    |
         |                                    |
    Large mode                          Small mode
    (Gaussian)         →→→→        (Confined silicon mode)
```

**Key challenges in edge coupling:**

- **Mode size mismatch** — fiber mode is ~20× larger than the chip waveguide mode
- **Numerical aperture (NA) mismatch** — fiber and waveguide have different divergence angles
- **Polarization sensitivity** — efficient coupling must work for both TE and TM polarization
- **Fabrication tolerance** — the structure must be robust to nanoscale fabrication errors

Edge couplers are preferred over grating couplers in many applications because they offer **broader bandwidth**, **polarization independence**, and **lower insertion loss** when optimized correctly.

---

## What is a Taper?

A **taper** in photonics is a waveguide structure whose cross-sectional dimensions change gradually along the propagation direction (z-axis). The purpose of a taper is to transform the optical mode from one size/shape to another with minimal loss.

```
    Cross-section changes along propagation direction (z)

         z = 0                             z = L
    ┌─────────────┐                   ┌───┐
    │   Wide end  │  →→→→→→→→→→→→→→  │ N │  Narrow end
    └─────────────┘                   └───┘
         w_tip ≠ w_tip                  (or vice versa)
```

A taper works by **adiabatically** (slowly and continuously) transforming the guided mode so that light remains in the fundamental mode throughout the transition, with negligible coupling to higher-order modes or radiation modes.

The taper length `L` must satisfy the **adiabaticity condition**:

```
   dw/dz  <<  (β₁ - β₂) / (2π)
```

where `β₁` and `β₂` are the propagation constants of the fundamental and first higher-order mode respectively, and `w` is the local waveguide width.

---

## Straight Taper vs. Inverse Taper

### Straight (Adiabatic) Taper

A **straight taper** (also called a forward taper or adiabatic taper) starts **narrow** at the input (chip side) and widens toward the output to transition the mode from a small waveguide into a larger waveguide or multimode region.

```
    Propagation direction  →

         z = 0                     z = L
         ┌──┐
         │  │──────────────────────────────────────────┐
         │  │                                          │  Wide output
         └──┘──────────────────────────────────────────┘
         Narrow tip                              Wide end
         (small mode)                           (large mode)
```

**Characteristics:**
- Mode field expands as the waveguide widens
- Used to transition from single-mode to multimode regions
- Common in power splitters and MMI devices
- **Not ideal for fiber-to-chip coupling** because the narrow waveguide tip still has a much smaller mode than the fiber

---

### Inverse Taper

An **inverse taper** starts **wide** at the chip interior and narrows toward the chip facet (tip). The narrowing of the waveguide causes the optical mode to **delocalize** — as the waveguide becomes subwavelength in width, the mode is no longer well-confined inside the silicon and spreads out into the surrounding cladding (air or polymer).

```
    Propagation direction  →  (fiber to chip)

    Fiber side (input)                    Chip interior (output)
         z = 0                                  z = L
    ┌───────────────────────────────────────────────────┐
    │   Narrow tip (subwavelength)                Wide  │
    │   Mode spreads into cladding →→→ Confined mode    │
    └───────────────────────────────────────────────────┘
         w_tip ~ 100–180 nm                w_out ~ 450 nm
```

**Why the mode delocalizes at the tip:**

In a subwavelength waveguide, the effective refractive index `n_eff` approaches the cladding index. The mode field diameter (MFD) grows dramatically, approaching the mode size of a standard single-mode fiber.

```
    As width w → 0:   n_eff → n_cladding   and   MFD → ∞ (in cladding)
    As width w → ∞:   n_eff → n_core       and   MFD → small (confined)
```

This property is exploited in inverse tapers to **match the large fiber mode** at the chip facet.

---

### Why Use One Over the Other?

| Property | Straight Taper | Inverse Taper |
|---|---|---|
| Tip width | Narrow (starts small) | Narrow at facet (mode expands) |
| Mode at facet | Small, confined | Large, delocalized into cladding |
| Fiber mode overlap | Poor (~1–5 dB loss) | Good (~0.5–2 dB loss) |
| Bandwidth | Moderate | Broad |
| Polarization sensitivity | Higher | Lower (geometry-dependent) |
| Best application | On-chip transitions | Fiber-to-chip edge coupling |
| Cladding requirement | Optional | Required (SU-8, polymer, or lensed fiber) |

**Summary:** Straight tapers are used for on-chip mode conversion. Inverse tapers are the preferred solution for **fiber-to-chip edge coupling** because they naturally expand the mode to match the fiber's Gaussian profile at the chip facet.

---

## Trilateral Inverse Taper — The Proposed Design

### Geometry and Cross-Section

A conventional inverse taper has a **rectangular** cross-section. The **Trilateral Inverse Taper** introduces a **triangular (trilateral) cross-sectional profile** — the waveguide narrows not just in width (x-direction) but also tapers in height (y-direction) simultaneously toward the tip.

```
    Top View (x-z plane):

    Facet                              Chip interior
    |←——————————  Length L  ——————————→|
    
         w_tip                              w_out
          ┌──┐                         ┌──────────┐
          │  │ ←——————————————————————→│          │
          └──┘                         └──────────┘

    Cross-section at facet tip         Cross-section at output
    (Triangular / trilateral)          (Rectangular)

          /\                                ████
         /  \                               ████
        /    \    →→→→→→→  →→→→→→→→→→→→→   ████
       /      \                             ████
      /________\

    Width narrows AND height tapers    Full silicon height (220 nm)
    toward tip → trilateral shape      preserved at output
```

**Key distinction from rectangular inverse taper:**

In a standard rectangular inverse taper, only the **width** is tapered. In the trilateral design:
- Both **width** and **height** are tapered simultaneously
- The cross-section transitions from a **triangular tip** → **rectangular body**
- This produces a **more symmetric** near-field at the facet, improving overlap with the circular Gaussian mode of the optical fiber

---

### Design Parameters

| Parameter | Symbol | Value |
|---|---|---|
| Taper tip width | w_tip | ~100–150 nm |
| Taper output width | w_out | 450 nm |
| Silicon waveguide height | h | 220 nm |
| Taper length | L | 10–30 µm |
| Operating wavelength | λ | 1550 nm (C-band) |
| Waveguide platform | — | Silicon-on-Insulator (SOI) |
| Cladding material | — | SiO₂ / SU-8 polymer |
| Fiber MFD (target) | — | ~10 µm |
| Buried oxide (BOX) thickness | h_BOX | 2 µm |

> **Note:** Replace these values with your actual simulated parameters.

---
## Structure Of the Taper
<img width="859" height="619" alt="SIDE_VIEW" src="https://github.com/user-attachments/assets/694ff9f4-6323-4414-9449-563fee7a9526" />
<img width="1315" height="745" alt="TOP_VIEW" src="https://github.com/user-attachments/assets/2ef84bc2-2078-448a-bc54-c14719e8a9fe" />
<img width="1205" height="754" alt="3D_VIEW" src="https://github.com/user-attachments/assets/91e8803e-e814-4fff-b3b8-ca2b9562e109" />

## Simulation Setup

### Lumerical FDTD — Coupling Efficiency

Lumerical FDTD Solutions was used to simulate the full 3D electromagnetic field propagation and calculate fiber-to-chip coupling efficiency.

**Simulation region:**
- Source: Gaussian beam (fiber mode) injected at the facet
- Monitor: Power monitor placed at the output of the taper (inside the silicon waveguide)
- Boundary conditions: PML (Perfectly Matched Layer) on all sides
- Mesh override: Applied in the taper region (mesh size ~10 nm in x and y)

**Coupling efficiency calculation:**

```
    η_coupling = P_transmitted / P_incident  ×  100%
```

where `P_transmitted` is the power recorded at the waveguide output monitor and `P_incident` is the total injected power from the fiber source.

**Insertion loss:**

```
    IL (dB) = -10 × log₁₀(η_coupling)
```

---

### Lumerical MODE — Mode Analysis

Lumerical MODE Solutions was used to:

1. Calculate the **mode field profile** at each cross-section of the taper
2. Compute the **effective refractive index** (n_eff) as a function of taper width
3. Verify the **adiabaticity** of the taper transition
4. Calculate the **mode overlap integral** between the fiber Gaussian mode and the taper tip mode

**Mode overlap integral:**

```
    Γ = |∫∫ E_fiber* · E_taper dA|²
        ────────────────────────────────────────────────
        (∫∫ |E_fiber|² dA) × (∫∫ |E_taper|² dA)
```

where `E_fiber` is the Gaussian fiber mode field and `E_taper` is the simulated mode field at the taper tip.

---

## Results

> **Note:** Replace the placeholder values below with your actual simulation results and figures.

### Coupling Efficiency vs. Tip Width

| Tip Width (nm) | Coupling Efficiency (%) | Insertion Loss (dB) |
|---|---|---|
| 80 | — | — |
| 100 | — | — |
| 120 | — | — |
| 150 | — | — |

*Table: Coupling efficiency as a function of inverse taper tip width at λ = 1550 nm.*

### Mode Field at Taper Tip

> *(Insert MODE Solutions mode profile figure here — e.g., `figures/mode_profile_tip.png`)*

### Coupling Efficiency vs. Wavelength

> *(Insert FDTD broadband coupling efficiency sweep plot here — e.g., `figures/coupling_vs_wavelength.png`)*

### n_eff vs. Taper Width

> *(Insert MODE Solutions effective index dispersion curve here — e.g., `figures/neff_vs_width.png`)*

---

## File Structure

```
trilateral-inverse-taper/
│
├── README.md                   ← This file
│
├── lumerical/
│   ├── fdtd/
│   │   ├── trilateral_taper_3D.fsp     ← FDTD project file
│   │   └── run_sweep.lsf               ← FDTD sweep script
│   │
│   └── mode/
│       ├── mode_analysis.lms           ← MODE project file
│       └── neff_sweep.lsf              ← n_eff vs width sweep script
│
├── scripts/
│   └── plot_results.m                  ← MATLAB/Python post-processing
│
├── figures/
│   ├── taper_geometry_3D.png
│   ├── mode_profile_tip.png
│   ├── coupling_vs_wavelength.png
│   └── neff_vs_width.png
│
└── data/
    ├── coupling_efficiency.csv
    └── neff_data.csv
```

---

## References

1. Taillaert, D. et al. (2004). "Grating couplers for coupling between optical fibers and nanophotonic waveguides." *Japanese Journal of Applied Physics*, 45(8A), 6071–6077.

2. Almeida, V. R. et al. (2003). "Nanotaper for compact mode conversion." *Optics Letters*, 28(15), 1302–1304.

3. Marchetti, R. et al. (2019). "Coupling strategies for silicon photonics integrated chips." *Photonics Research*, 7(2), 201–239.

4. Lumerical Inc. FDTD Solutions Reference Manual. [https://www.lumerical.com](https://www.lumerical.com)

5. Lumerical Inc. MODE Solutions Reference Manual. [https://www.lumerical.com](https://www.lumerical.com)

---

> **Author:** Shubham Ghosh
> **Institution:** NIT Warangal
> **Domain:** Silicon Photonics — Fiber-to-Chip Edge Coupling
> **Status:** Simulation complete — results to be updated
