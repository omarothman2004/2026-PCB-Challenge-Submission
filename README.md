# 2026-pcb-design-challenge

# UCL x KCL x Oxford x Imperial x Cambridge x Edinburgh: 2026 PCB Challenge Submission

[![KiCad Version](https://img.shields.io/badge/KiCad-v10.0-blue.svg)](https://kicad.org/)
[![License](https://img.shields.io/badge/License-CERN--OHL--S--v2-green.svg)](LICENSE)
[![DRC Status](https://img.shields.io/badge/DRC-Passing-brightgreen.svg)]()

This repository contains my official entry for the **2026 PCB Challenge**, hosted jointly by UCL, KCL, Oxford, Cambridge, Imperial, and Edinburgh, and sponsored by **PCBWay** and **Jane Street**.

--- 

## 1. Project Summary & Metrics

* **Primary Objective:** Implement the provided un-annotated schematic into a fully routed, DRC-clean, single contiguous 4-layer PCB optimized for minimum bounding box area, while deducing the board's digital logic function.
* **Bounding Box Dimensions:** `[X.XX] mm` × `[Y.YY] mm`
* **Total PCB Footprint Area:** `[ZZZ.ZZ] mm²`
* **Layer Stackup:** 4-Layer FR4 (1.6 mm thickness)
  * **Top Layer (F.Cu):** High-speed digital signals, components & ESP32 antenna edge projection
  * **Inner Layer 1 (In1.Cu):** Continuous Ground Reference Plane (GND)
  * **Inner Layer 2 (In2.Cu):** Split Power Rails (3.3V / 2.5V LDO output)
  * **Bottom Layer (B.Cu):** Signal routing & secondary ground copper pour
* **KiCad Version:** KiCad v10.0

---

## 2. Digital Logic Deduction (Executive Summary)

### High-Level PCB Function
> **Summary Statement:** [Insert concise 1–2 sentence description of what the circuit does once deduced, e.g., "The board acts as an 8-bit arithmetic pipeline processing parallel inputs via multiplexed logic, storing state in a 24-bit shift register, and driving buffered status outputs controlled by an ESP32-S3."]

### Functional Block Breakdown
1. **Input Stage (Bottom Left):** Signal conditioning using single/gate logic (`74AHC1G86` XOR, `74LV1T08GV` AND, `74LVC1GU04` inverters) combined with `NMUX1308BQ` multiplexer arrays.
2. **State & Processing (Top Left):** 24-bit shift/state memory constructed from 24× `74AUP1G80` single D flip-flops, feeding an 8-bit arithmetic operation block built with dual `CD74HC283M96` 4-bit adders, coordinated by an **ESP32-S3-MINI-1**.
3. **Driver Output Stage (Top Right):** 15 dual NPN/PNP transistor pairs (`Q15`–`Q29` in SOT-1123) driving output lines based on internal logic states.

*For the complete logic derivation, truth tables, and signal flow diagrams, see [`LOGIC_DEDUCTION.md`](./LOGIC_DEDUCTION.md).*

---

## 3. Design Rule Constraints & Compliance

The layout complies with all global DRC rules specified in the competition guidelines:

| Metric | Rule Limit | Implemented Design Spec | DRC Status |
| :--- | :--- | :--- | :--- |
| **Min Trace Width** | 0.15 mm (6 mil) | 0.15 mm *(0.17 mm on Flip-Flop pads)* | Pass |
| **Min Clearance** | 0.12 mm (6 mil) | 0.12 mm | Pass |
| **Copper to Edge** | 0.30 mm | $\ge 0.30\text{ mm}$ | Pass |
| **Via Hole / Annular Ring** | 0.30 mm drill / 0.15 mm ring | 0.30 mm drill / 0.60 mm pad | Pass |
| **Via in Pad** | Prohibited | None used | Pass |
| **IC Courtyard Routing** | Prohibited within IC Courtyards | Zero traces/vias inside courtyards | Pass |

---

## 4. Repository Structure

```text
.
├── README.md                     # This file
├── LOGIC_DEDUCTION.md            # In-depth schematic logic breakdown & proof
├── LICENSE                       # License file
├── hardware/                     # KiCad v10 project files
│   ├── pcb_challenge.kicad_pro
│   ├── pcb_challenge.kicad_sch
│   ├── pcb_challenge.kicad_pcb
│   └── pcb_challenge.kicad_dru
├── production/                   # Manufacturing Gerber & drill packages
│   ├── gerbers/
│   ├── drill/
│   └── pcb_challenge_fab.zip
└── docs/                         # Verification images and exported PDF schematic
    ├── schematic.pdf
    ├── drc_report.txt
    ├── board_dimensions.png
    └── images/
