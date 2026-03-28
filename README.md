# ⌨️ HackPad v1 | Engineering Journal & Documentation

![Status: Active Development](https://img.shields.io/badge/Status-Active_Development-orange)
![Hardware: RP2040](https://img.shields.io/badge/Hardware-Seeed_XIAO_RP2040-green)
![EDA: KiCad 8.0](https://img.shields.io/badge/EDA-KiCad_8.0-blue)
![MCAD: Fusion 360](https://img.shields.io/badge/MCAD-Fusion_360-red)

> **A custom, parametric mechanical macropad built from scratch.**  
> Designed, routed, and modeled for the **Hack Club Blueprint** hardware initiative.

**Lead Designer:** Hiten (Class 12 Student, Kolkata)  
**Current Iteration:** v1.0 (Lower Tray Architecture)

---

## 📑 Table of Contents
1. [Project Overview](#-project-overview)

2. [Development Environment](#-development-environment)

3. [Mechanical Engineering Log](#-mechanical-engineering-log-tray-design)

4. [Parametric Workflow & Logic](#-parametric-workflow--logic)

5. [Current Roadmap & Pending Tasks](#-current-roadmap--pending-tasks)

6. [Repository Structure](#-repository-structure)

---

## 🎯 Project Overview

HackPad v1 is a custom mechanical macropad powered by the **Seeed Studio XIAO RP2040**. The goal of this project is not just to build a functional macropad, but to document the entire engineering lifecycle—from schematic capture and PCB routing in KiCad to parametric enclosure design in Autodesk Fusion 360. 

This repository serves as the central documentation hub and engineering journal for Blueprint reviewers.

---

## 🛠️ Development Environment

To ensure perfect reproducibility and a stable version control pipeline, this project is maintained on the following stack:

### Hardware Stack
*   **Microcontroller:** Seeed Studio XIAO RP2040 (chosen for its small footprint and native USB-C) x 1
*   **Mechanical Switches:** MX-Style Switches - Gateron, Cherry or Outemu x 8
*   **Keycaps:** 3D Printed x 8
*   **1N4148 Diodes:** Small signal switching diodes x 9
*   **EC11 Rotary Encoder:** 20 pulses per rotation with push-switch x 1
*   **3D-Printed Knob:** For the encoder x 1
*   **PCB:** Custom made x 1
*   **Bottom Case (Tray):** 3D Printed x 1
*   **Top Case:** 3D Printed x 1
*   **Mounting Screws:** M2 screws x 4

### Software & DevOps Stack
*   **EDA (Electronics):** KiCad 9.0 (Schematic & PCB layout).
*   **MCAD (Mechanics):** Autodesk Fusion 360 (Parametric 3D modeling).
*   **Version Control:** Git/GitHub via Windows PowerShell.
*   **Upstream Link:** Pushed via `git push origin main`.

---

## 🏗️ Mechanical Engineering Log: "Tray" Design

The enclosure is currently engineered as a **Lower Tray**, intended to be paired with a future Top Lid. The design logic is heavily parametric, ensuring that any future changes to the PCB automatically cascade into the 3D printed enclosure.

### 1. The Reference PCB (Global Zero)
*   **Import Method:** The PCB layout was exported from KiCad as a `.step` file and imported into Fusion 360.
*   **Orientation Logic:** The PCB serves as the absolute **"Global Zero"** for all sketch planes. 
*   **Z-Axis Offset:** All enclosure components are built on the underside of the board (Negative Z-axis relative to the top board face). This ensures the MCU and switch pins have adequate clearance.

### 2. The Chassis Floor
*   **Geometry:** Traced directly from the PCB outer boundary using a **2.0 mm positive offset**.
*   **Thickness:** 2.0 mm solid plastic, optimized for standard FDM 3D printing.
*   **State:** Created as a "New Body" to prevent Fusion 360 from accidentally joining the plastic with the imported electronic `.step` components.
*   **📝 Journal Entry: The "Swiss Cheese" Fix**  
    *Initially, the floor generated as a mess of holes because Fusion auto-projected the switch pin cutouts from the PCB. Instead of patching them manually, I utilized Timeline Editing to re-select only the inner boundary profiles, generating a clean, solid, and dust-proof floor.*

### 3. The Standoffs (Pillars)
*   **Clearance Logic:** The standoffs elevate the PCB exactly **4.0 mm** off the chassis floor. This is a critical dimension that provides clearance for bottom-side solder joints, the XIAO MCU, and future under-glow LEDs.
*   **Dimensions:** 5.0 mm Outer Diameter, precisely centered on the PCB's designated mounting holes.
*   **Integration:** Extruded 4.0 mm UP and set to the "Join" operation, merging them seamlessly into the chassis floor body for structural rigidity.

### 4. The Perimeter Walls
*   **Wall Thickness:** 2.0 mm (Matching the floor to ensure uniform cooling/shrinkage during 3D printing).
*   **📝 Journal Entry: Stack-up Analysis**  
    *The original wall height was arbitrarily set to 16.0 mm. After performing a physical Stack-up Analysis in the CAD environment (Standoff [4mm] + PCB [1.6mm] + Switch Housing), I determined 16.0 mm caused severe interference. Using the parametric timeline, I reduced the height to **8.0 mm**.* 
*   **Aesthetic Impact:** 8.0 mm perfectly covers the internal gap and the rough PCB edge, while leaving the upper switch housings exposed. This results in a clean **"Floating Key"** aesthetic and leaves exact clearance for a future top lid.
*   **Plan Changes:** I dropped the idea to create the top lid, that would close the whole enclosure. It is now an open design with all the internals visible. Also, it would be translucent back for proper LED diffusion effect.

---

## ⚙️ Parametric Workflow & Logic

This enclosure is not a static 3D mesh; it is a dynamic, parametric assembly. 

*   **Design History:** `Capture Design History` is active. Every dimension is fully editable via the Fusion 360 timeline.
*   **Dependency Chain:** 
    `KiCad PCB Boundary` ➡️ `Floor Sketch` ➡️ `Perimeter Walls`
    Because the floor sketch is projected directly from the KiCad PCB boundary, any future alterations to the board layout (if re-imported) will propagate through the model automatically.
*   **Extrusion Logic:** Extrusions rely heavily on Fusion's visual feedback ("Blue Arrow") to confirm vector directions. Given the workspace orientation, positive Z extrusions push the floor downward, matching the sketch plane's inverted orientation.

---

## 🚀 Current Roadmap & Pending Tasks

The v1.0 Tray is functionally modeled, but the following constraints are actively being worked on for the final Blueprint submission:

- [ ] **USB-C Port Interference:** 
  *   *Issue:* The rear 8.0 mm wall currently blocks access to the XIAO RP2040's USB-C port.
  *   *Solution:* Model a rectangular "Cut" extrusion on the rear wall face. Need to measure the exact USB-C cable boot clearance.
- [ ] **Mounting Hardware Tolerances:** 
  *   *Issue:* Standoff holes currently have a 1:1 diameter ratio with the PCB holes.
  *   *Solution:* Determine the final assembly method. If using **M2/M3 Brass Heat-Set Inserts**, the holes need to be enlarged for clearance. If using self-tapping screws, they need to be slightly undersized.
- [ ] **Top Lid Attachment Geometry:** 
  *   *Issue:* No current mechanism exists to attach the future top lid.
  *   *Solution:* Design a recessed "Lip" or a friction "Snap-fit" geometry along the top perimeter of the 8.0 mm walls.

---

## 📂 Repository Structure

```text
📦 HackPad
 ┣ 📜 README.md                # Project documentation and engineering journal
 ┣ 📂 Hardware                 # All the hardware related files
 ┃ ┗ 📂 CAD-Mechanics          # Master Fusion 360 Parametric Archive
 ┣ 📂 Images                   # Images of the PCB
 ┗ 📂 Production               # Gerber and DRILL files