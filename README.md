<div align="center">

# 🔲 OpenPiton TILE — Synthesis to GDSII

**28nm · 588 MHz · 18 Macros · 324K Cells · Clean Signoff**

[![Status](https://img.shields.io/badge/Status-Signoff%20Complete-2EC4B6?style=for-the-badge)](.)
[![DRC](https://img.shields.io/badge/DRC-0%20Violations-2EC4B6?style=for-the-badge)](.)
[![Timing](https://img.shields.io/badge/WNS%20%2F%20TNS-0%20ns-2EC4B6?style=for-the-badge)](.)
[![Tech](https://img.shields.io/badge/Technology-28nm%20TSMC-FF9F1C?style=for-the-badge)](.)

</div>

---

<!-- Replace with your GitHub-hosted screenshot — see OUTPUTS.md for hosting instructions -->
<p align="center">
  <img src="https://github.com/user-attachments/assets/0585ce7c-da98-4aee-abce-b18c71a47f49" alt="TILE — Final Layout" width="50%"/>
  <br/>
  <sub>Final chip-finish layout · Full stage-by-stage gallery → <a href="OUTPUTS.md">OUTPUTS.md</a></sub>
</p>

---

## 🧩 About

Full **Synthesis-to-GDSII** backend implementation of the **OpenPiton TILE** block — a SPARC processor tile containing an L1 cache, L2 cache with tag/data/state/directory arrays, and a prefetch unit, backed by **18 hard SRAM macros** in **28nm TSMC** technology.

Custom SDC constraints and TCL automation scripts written from scratch. All issues encountered during the flow were root-cause analysed and documented — see [`TILE_PD_Implementation.pdf`](TILE_PD_Implementation.pdf).

---

## 📐 Specifications

| Parameter | Value |
|:---|:---|
| **Design** | OpenPiton TILE |
| **Technology** | 28nm (TSMC N28 9T) |
| **Tools** | DC V-2023.12-SP4 · ICC2 X-2025.06-SP2 |
| **Clock / Frequency** | 1.70 ns · **588 MHz** |
| **Clock Sinks** | 101,158 |
| **Hard Macros** | 18 (L1 i/d cache · L2 data/tag/state · directory) |
| **Total Cells** | 324,021 |
| **Die Area** | 1,354,337 µm² (1163.82 × 1163.70 µm) |
| **Core Area** | 1,350,290 µm² |
| **Core Utilization** | **70%** |
| **CTS Cells** | 1,247 (402 buffers · 212 inverters) |
| **Global Skew (worst)** | 650 ps |
| **Spare Cells** | 1,360 |
| **Final Power** | **90.10 mW** (from 286.43 mW at synthesis) |

---

## 🛣️ Implementation Flow

```
RTL  →  Synthesis (DC)  →  Floorplan  →  Placement  →  CTS  →  Routing  →  ECO  →  Chip Finish  →  GDSII
```

| Stage | Highlight |
|:---|:---|
| **Synthesis** | Custom SDC · dont_use LVT+CK · clock gating · WNS –0.21 ns |
| **Floorplan** | 18 macros fixed · PG mesh built · PG DRC cleared (21,817 → 0) |
| **Placement** | 70% util · 0.11% H / 1.18% V congestion · 1,360 spare cells |
| **CTS** | 1,247 clock cells · skew 650 ps · balanced with custom app options |
| **Routing** | Full route · DRCs 38 → 0 · Shorts 6 → 0 · Opens 1 → 0 |
| **Post-Route ECO** | VT-swap TCL script · CPPR enabled (hold 104 → 9 → 0) · I/O delay fix |
| **Chip Finish** | Fillers inserted · GDSII + DEF + netlist generated |

---

## ✅ Final Signoff Results

| Check | Result |
|:---|:---|
| Setup WNS / TNS | **0.00 ns / 0.00 ns** |
| Hold WNS / TNS | **0.00 ns / 0.00 ns** |
| Max Transition Violations | **0** |
| Max Capacitance Violations | **0** |
| DRC Violations | **0** |
| Shorts / Opens / Antenna | **0 / 0 / 0** |
| Placement Legality | ✅ Clean |
| GDSII / DEF / Netlist | ✅ Generated |

---

## ⚙️ Issues & Fixes

8 real engineering issues were encountered and resolved during this flow. Full root-cause analysis and fix documentation is in the attached [`TILE_PD_Implementation.pdf`](TILE_PD_Implementation.pdf). Key highlights:

- **PG DRC (M1 + M4)** — blockage overlap fixed + fat-metal app option applied
- **Spare cells removed by place_opt** — re-inserted with `dont_touch` attribute
- **CTS skew vs timing trade-off** — 0.1 ns target chosen over 0.05 ns
- **AND/OR gates in clock network** — replaced with CK/LVT variants via `size_cell` loop
- **Over-constrained I/O delays** — relaxed from 70% → 60% of clock period
- **Custom VT-swap TCL script** — recalculates slack after every swap; stops on first positive slack
- **CPPR disabled** — enabling it collapsed hold violations from 104 → 9 instantly
- **Post-route DRCs** — cleared via `route_opt` reruns + targeted manual fixes

---

## 📂 Repository

```
tile-openpiton-physical-design/
├── README.md
├── outputs.md                     ← Screenshot gallery (stage-by-stage)
├── TILE_PD_Implementation.pdf     ← Full implementation document
```

---

## 👤 Credits

**Karnam Chandra Shekar**
Final project · Physical Design Course · Sumeda IT
📧 karnamchandu988@gmail.com · [LinkedIn](#) · [GitHub](#)

---

<div align="center">
  <sub>⭐ Star this repo if the documentation was useful to you</sub>
</div>
