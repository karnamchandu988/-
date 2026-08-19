# 📸 Output Gallery — OpenPiton TILE · Synthesis to GDSII

Screenshots captured from a cloud-based EDA environment using **Synopsys Design Compiler (DC)** and **Synopsys IC Compiler II (ICC2)**.

---

## 1 · Synthesis — Netlist Schematic

**Stage:** Synthesis · **Tool:** Design Vision (`TopLevel.2 — tile`)

The gate-level netlist of the TILE block after `compile_ultra` in Design Compiler. The dense blue interconnect pattern confirms a fully synthesised and linked netlist, providing the starting point for physical implementation in ICC2.

<p align="center">
  <img src="https://github.com/user-attachments/assets/02c10b29-e126-46f0-a5ad-93b002788e47" alt="Synthesis Netlist Schematic" width="900">
</p>

---

## 2 · Floorplan — Macro Placement

**Stage:** Floorplan · **Tool:** IC Compiler II

All **18 SRAM macros** placed and fixed at the block boundaries, including L1 I-cache, L1 D-cache, L2 data/tag/state arrays, and the directory array. The large open central region provides the standard-cell placement area for subsequent implementation stages.

<p align="center">
  <img src="https://github.com/user-attachments/assets/ec1100e5-35af-4f84-bf42-0ca81d3695c5" alt="Floorplan Macro Placement" width="900">
</p>

---

## 3 · Floorplan — Core Utilization Report (70%)

**Stage:** Floorplan / Post-Route · **Tool:** ICC2 `report_utilization -config core_util`

A custom utilization configuration (`core_util`) was used to report utilization against the full core area, including macros in both the numerator and denominator. The resulting **70% utilization (0.7000)** matches the original floorplan specification.

<p align="center">
  <img src="https://github.com/user-attachments/assets/63babede-cd07-4b1d-b56d-2f0ad33e51bd" alt="Core Utilization 70 Percent Report" width="900">
</p>

---

## 4 · Placement — Cell Density Heatmap

**Stage:** Placement · **Tool:** ICC2 · **Map Mode:** Cell Density

Cell density map after `place_opt`. The predominantly green distribution, representing approximately **60–70% density**, with isolated yellow regions indicates that the **60% partial blockages** around macro channels successfully helped distribute standard cells evenly. No red regions exceeding 100% density were observed.

<p align="center">
  <img src="https://github.com/user-attachments/assets/eb56fed0-7179-458b-8b01-86e6b7a81d89" alt="Cell Density Heatmap" width="900">
</p>

---

## 5 · Placement — Congestion Map

**Stage:** Placement · **Tool:** ICC2 · **Map Mode:** Global Route Congestion (Hard)

Routing demand versus available routing supply across the design after placement. The reported congestion was **0.11% horizontal** and **1.18% vertical**, remaining within acceptable limits for proceeding to CTS without additional congestion intervention. The sparse purple regions represent isolated overflow rather than systemic congestion.

<p align="center">
  <img src="https://github.com/user-attachments/assets/c2a50a4d-8270-4cef-93c3-baa1ba8e305f" alt="Global Route Congestion Map" width="900">
</p>

---

## 6 · Placement — Spare Cell Distribution

**Stage:** Placement / Post-Route · **Tool:** IC Compiler II

A total of **1,360 spare cells** were distributed across the design using a **70 × 70 µm repetitive window**. The spare cells were re-inserted with `dont_touch` applied before `place_opt` to prevent the optimizer from removing them as unloaded logic.

<p align="center">
  <img src="https://github.com/user-attachments/assets/c08a3ade-6347-4679-b3ff-23817143904d" alt="Spare Cell Distribution" width="900">
</p>

---

## 7 · Chip Finish — Standard Cell and Filler Layout

**Stage:** Chip Finish · **Tool:** IC Compiler II (`tile.ndm:chipfinish.design`)

The final chip-finish view showing the packed standard-cell core, including flip-flops (`DFND2BWP40P140HVT`), decap cells (`DCAP8B`), and filler cells closing the remaining gaps.

IO buffer padding was removed before filler insertion so that filler cells could be inserted between the IO cells where required.

<p align="center">
  <img src="https://github.com/user-attachments/assets/41a873bc-16a1-4154-87b0-7875af85edd7" alt="Chip Finish Standard Cell and Filler Layout" width="900">
</p>

---

## 8 · Chip Finish — Utilization Report (100% Site-Row)

**Stage:** Chip Finish · **Tool:** ICC2 `report_utilization`

At chip finish, `report_utilization` excludes hard macros and keepouts from both the numerator and denominator, measuring utilization across the available standard-cell site rows.

The reported **1.0000 ratio (100%)** confirms that the available site rows are fully occupied after filler insertion.

<p align="center">
  <img src="https://github.com/user-attachments/assets/45bd8a8d-45f0-43fc-8ee6-cb0f4fe4bf3c" alt="Chip Finish 100 Percent Site Row Utilization Report" width="900">
</p>

---

## 9 · Signoff — Global Timing Report (Clean)

**Stage:** Post-Route Final Signoff · **Tool:** ICC2 `report_global_timing` · **Date:** Fri Jul 10 2026

The final global timing report shows a clean timing signoff:

```text
No setup violations found.
No hold violations found.
```

Timing closure was achieved after enabling **CPPR**, reducing hold violations from **104 → 9 → 0**, along with setup optimization using a custom VT-swap TCL script and I/O delay constraint adjustments.

<p align="center">
  <img src="https://github.com/user-attachments/assets/943084be-e046-455f-85be-b01ca800997f" alt="Final Timing Signoff - No Violations" width="900">
</p>

---

## ✅ Final Signoff Status

| Check         |                     Result |
| ------------- | -------------------------: |
| Synthesis     |                 ✅ Complete |
| Floorplan     |                 ✅ Complete |
| Placement     |                 ✅ Complete |
| Congestion    | ✅ Within acceptable limits |
| CTS           |                 ✅ Complete |
| Routing       |                 ✅ Complete |
| Chip Finish   |                 ✅ Complete |
| Setup Timing  |            ✅ No violations |
| Hold Timing   |            ✅ No violations |
| Final Signoff |                    ✅ Clean |

---

### 🏁 OpenPiton TILE — RTL to GDSII

**Technology:** 28 nm
**Implementation:** Synopsys Design Compiler + IC Compiler II
**Final Timing:** Setup clean · Hold clean
**Final Utilization:** 70% core design intent / 100% site-row occupancy after chip finish
