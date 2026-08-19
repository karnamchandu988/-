# 📸 Output Gallery — OpenPiton TILE · Synthesis to GDSII

Screenshots captured from a cloud-based EDA environment (DC + ICC2). Hosted via GitHub comment-image upload — drag each image into any issue comment box, copy the URL it generates, and paste it into the placeholders below. Close the issue (do not delete it) to keep images live.

---

## 1 · Synthesis — Netlist Schematic

**Stage:** Synthesis · **Tool:** Design Vision (TopLevel.2 — tile)

The gate-level netlist of the TILE block after `compile_ultra` in Design Compiler. The dense blue interconnect pattern confirms a fully synthesised, linked netlist — the starting point handed off to ICC2 for physical implementation.

![Synthesis Netlist Schematic](PASTE_URL_HERE)

---

## 2 · Floorplan — Macro Placement

**Stage:** Floorplan · **Tool:** IC Compiler II

All 18 SRAM macros placed and fixed at the block boundaries — L1 icache, L1 dcache, L2 data/tag/state arrays, and the directory array. The large open central region is the standard-cell placement area that will be filled in subsequent stages.

![Floorplan Macro Placement](PASTE_URL_HERE)

---

## 3 · Floorplan — Core Utilization Report (70%)

**Stage:** Floorplan / Post-Route · **Tool:** ICC2 `report_utilization -config core_util`

Custom utilization configuration (`core_util`) reporting against the full core area — includes macros in both numerator and denominator — giving the design-intent utilization of **70%** (0.7000), matching the original floorplan specification.

![Core Utilization 70% Report](PASTE_URL_HERE)

---

## 4 · Placement — Cell Density Heatmap

**Stage:** Placement · **Tool:** ICC2 Map Mode: Cell Density

Cell density map after `place_opt`. The predominantly green distribution (60–70%) with isolated yellow spots confirms the 60% partial blockages around macro channels successfully spread placement evenly. No red (>100%) hotspots were present.

![Cell Density Heatmap](PASTE_URL_HERE)

---

## 5 · Placement — Congestion Map

**Stage:** Placement · **Tool:** ICC2 Map Mode: Global Route Congestion (Hard)

Routing demand vs. supply across the design after placement. Result: **0.11% horizontal, 1.18% vertical congestion** — within acceptable limits to proceed to CTS without additional intervention. Sparse purple areas indicate isolated overflow, not systemic congestion.

![Congestion Map](PASTE_URL_HERE)

---

## 6 · Placement — Spare Cell Distribution

**Stage:** Placement / Post-Route · **Tool:** IC Compiler II

1,360 spare cells distributed across the design at a 70×70 µm repetitive window. These were re-inserted with `dont_touch` applied before `place_opt` to prevent the tool from removing them as unloaded logic (Issue #2 in the implementation document).

![Spare Cell Distribution](PASTE_URL_HERE)

---

## 7 · Chip Finish — Standard Cell and Filler Layout

**Stage:** Chip Finish · **Tool:** IC Compiler II (tile.ndm:chipfinish.design)

The final chip-finish view showing the packed standard-cell core — flip-flops (`DFND2BWP40P140HVT`), decap cells (`DCAP8B`), and filler cells filling all gaps. IO buffer padding was removed before filler insertion to allow fillers to close the gaps between IO cells.

![Chip Finish Filler Layout](PASTE_URL_HERE)

---

## 8 · Post-Route — Layout View

**Stage:** Post-Route · **Tool:** IC Compiler II

Full post-route layout showing the routed standard-cell region with macro columns at the boundaries. All 38 DRC violations and 6 shorts from the initial routing were cleared to zero through `route_opt` reruns and targeted manual fixes.

![Post-Route Layout](PASTE_URL_HERE)

---

## 9 · Chip Finish — Utilization Report (100% Site-Row)

**Stage:** Chip Finish · **Tool:** ICC2 `report_utilization`

`report_utilization` at chip finish excludes hard macros and keepouts from both numerator and denominator — measuring only standard-cell site rows. The **1.0000 ratio confirms all available rows are fully packed** after filler insertion.

![Chip Finish Utilization Report](PASTE_URL_HERE)

---

## 10 · Signoff — Global Timing Report (Clean)

**Stage:** Post-Route Final Signoff · **Tool:** ICC2 `report_global_timing` · Date: Fri Jul 10 2026

The cleanest output in any PD flow:

```
No setup violations found.
No hold violations found.
```

Achieved after enabling CPPR (hold violations 104 → 9 → 0), custom VT-swap TCL script for setup closure, and I/O delay constraint relaxation.

![Final Timing Signoff — No Violations](PASTE_URL_HERE)
