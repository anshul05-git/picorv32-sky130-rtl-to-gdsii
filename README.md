# PicoRV32 RTL-to-GDSII Implementation on SKY130

A physical-design implementation of the PicoRV32 RISC-V processor core using the SKY130A process design kit and SkyWater High Density standard-cell library.

> **Implementation status:** GDSII generation completed. The current implementation is not sign-off clean. Setup timing violations, DRC markers, and an LVS mismatch remain.

## 1. Project Overview

| Parameter | Details |
|---|---|
| Design | PicoRV32 processor core |
| Design scope | Standalone processor block, not a complete SoC |
| Architecture | RV32I |
| Technology | SKY130A |
| Standard-cell library | sky130_fd_sc_hd |
| Clock constraint | 10 ns |
| Physical-design flow | LibreLane with OpenROAD |
| RTL origin | Upstream PicoRV32 project by YosysHQ |

The processor RTL originates from the upstream PicoRV32 project. This repository documents the configuration, implementation, analysis, and physical-design work performed on that design. It does not claim authorship of the processor architecture or upstream RTL.

## 2. Tools and Environment

| Tool | Version / Details |
|---|---|
| LibreLane | 3.0.16 |
| Yosys | 0.62 |
| OpenROAD | Build dcf36133a369abc8f3c5e5738cd4d82e4903c0e0 |
| Verilator | 5.044 |
| KLayout | 0.30.7 |
| Magic | 8.3.623 |
| Netgen | 1.5.316 |
| PDK | SKY130A |

## 3. Physical-Design Flow

1. RTL preparation and constraint configuration
2. Yosys synthesis and technology mapping
3. Floorplanning
4. Power Distribution Network (PDN) generation
5. Global placement and placement optimization
6. Clock Tree Synthesis (CTS)
7. Global routing and detailed routing
8. Parasitic extraction using OpenRCX
9. Post-route static timing analysis (STA)
10. GDSII stream-out
11. Physical verification using Magic, KLayout, and Netgen

OpenROAD Tcl commands and its GUI were also used to inspect and analyze implementation stages.

## 4. Synthesis Results

Baseline synthesis using Yosys and the SKY130 HD library:

| Metric | Result |
|---|---:|
| Mapped standard-cell instances | 6,709 |
| Sequential cells | 1,597 |
| Combinational cells | 5,112 |
| Standard-cell area | 88,613.7376 µm² |
| Sequential-cell area | 33,968.8288 µm² |
| Sequential area percentage | 38.33% |
| Unmapped instances | 0 |
| Inferred latches | 0 |
| Synthesis check errors | 0 |
| Lint errors | 0 |
| Lint warnings | 489 |

An ABC diagnostic, `ABC: Error: The network is combinational.`, appeared during synthesis. Synthesis completed, but the diagnostic's root cause was not conclusively established.

## 5. Post-Route Timing Analysis

Static timing analysis was performed across nine RC/PVT corners.

| Timing metric | Result |
|---|---:|
| Worst setup slack | -3.4267 ns |
| Aggregate setup violations across nine corners | 1,925 |
| Worst hold slack | +0.0773 ns |
| Hold violations | 0 |
| Maximum-capacitance violations | 17 |
| Maximum-slew violations | 2,313 |

All three SS corners reported setup violations. All three TT and all three FF corners had positive setup slack and zero setup violations.

### Worst Setup Critical Path

| Parameter | Result |
|---|---|
| Corner | `max_ss_100C_1v60` |
| Startpoint | `_09946_` |
| Endpoint | `_11225_` |
| Data arrival time | 14.974246 ns |
| Data required time | 11.547532 ns |
| Setup slack | -3.426714 ns |

The examined path contains a long chain of `sky130_fd_sc_hd__and4_4` gates, followed by `sky130_fd_sc_hd__a21oi_2` and `sky130_fd_sc_hd__o21a_2` cells.

The negative setup slack indicates that the path does not meet the configured timing constraint at this corner.

## 6. Physical Verification Results

### Antenna Check

The post-route antenna summary reported no antenna violations.

### Magic DRC

Magic reported 6,037 coordinate entries:

| Rule | Coordinate entries |
|---|---:|
| nwell.2a | 4,086 |
| LU.3 | 1,086 |
| nwell.4 | 614 |
| nwell.1 | 251 |
| Total | 6,037 |

These are raw coordinate entries, not necessarily unique physical violations.

### KLayout DRC

The configured SKY130A rule deck reported 5,392 markers:

| Rule | Markers |
|---|---:|
| nwell.2a | 3,886 |
| nwell.1 | 502 |
| hvtp.1 | 502 |
| hvtp.2 | 502 |
| Total | 5,392 |

Magic and KLayout results are reported independently. Their counts must not be combined.

### LVS

Netgen reported the following comparison:

| Metric | Extracted layout | Reference netlist |
|---|---:|---:|
| Devices | 10,103 | 10,001 |
| Nets | 10,461 | 10,098 |

Additional findings:
- 67 disconnected top-level nodes: 32 IRQ nodes, 32 PCPI read-data nodes, and 3 PCPI control nodes.
- 98 unmatched decap instance records.
- 4 unmatched antenna-diode instance records.
- Power and body-bias connectivity differences were reported.

**Final LVS result: Netlists do not match.**

The reported mismatches have not been conclusively resolved.

## 7. Functional Simulation

A Verilator smoke test recorded:

- Instruction fetches: 182
- Memory reads: 45
- Memory writes: 46
- Simulation reached `$finish` at 11 us.

This was a smoke test, not exhaustive RV32I instruction-set or processor compliance verification.

## 8. GDSII Generation

The physical-design flow completed detailed routing, post-route parasitic extraction, timing analysis, and GDSII stream-out.

The generated GDSII was inspected locally using KLayout 0.30.7.

GDSII generation does not imply successful physical sign-off. The outstanding setup timing, DRC, and LVS findings remain unresolved.

## 9. Known Limitations

This is an educational and portfolio-oriented physical-design implementation, not a tapeout-ready result.

Outstanding findings include:

- Setup timing violations at all three SS corners.
- DRC markers reported by both Magic and KLayout.
- LVS mismatch reported by Netgen.
- Maximum-capacitance and maximum-slew violations.
- An unresolved ABC diagnostic during synthesis.
- Unresolved disconnected-node and physical-only instance mismatches.
- Functional verification limited to a Verilator smoke test.

These results are disclosed transparently and are not presented as successful sign-off checks.

## 10. Repository Contents

The repository is organized into the following sections:

- `images/` — physical-design screenshots.
- `reports/synthesis/` — synthesis summary.
- `reports/timing_sta/` — post-route timing and critical-path summaries.
- `reports/physical_verification/` — DRC, LVS and antenna summaries.
- `results/` — project metrics.
- `COPYING` — original ISC license notice.
- `.gitignore` — excludes private and generated files.

Original RTL, testbenches, complete LibreLane run directories, PDK libraries, extracted parasitics, and large internal archives are excluded from this initial public release.

## 11. Attribution and License

PicoRV32 originates from the upstream YosysHQ project:

https://github.com/YosysHQ/picorv32

The original RTL copyright notice attributes the work to Claire Xenia Wolf (2015–2021).

The original ISC license and its copyright and permission notices are retained in the `COPYING` file.

This repository documents the physical-design implementation and analysis performed on the upstream processor design. It does not claim authorship of the original processor RTL.

---

**Project focus:** RTL synthesis, standard-cell mapping, floorplanning, placement, clock-tree synthesis, routing, parasitic extraction, static timing analysis, and physical verification using SKY130A.
