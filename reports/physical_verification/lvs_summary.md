# PicoRV32 Layout Versus Schematic (LVS) Summary

## 1. Verification Result

Tool: Netgen 1.5.316

**Final result: Netlists do not match.**

## 2. Device and Net Comparison

| Metric | Extracted layout | Reference netlist |
|---|---:|---:|
| Devices | 10,103 | 10,001 |
| Nets | 10,461 | 10,098 |

## 3. Disconnected Top-Level Nodes

Netgen reported 67 disconnected nodes:

| Interface | Disconnected nodes |
|---|---:|
| IRQ | 32 (`irq[31:0]`) |
| PCPI read data | 32 (`pcpi_rd[31:0]`) |
| PCPI control | 3 (`pcpi_ready`, `pcpi_wait`, `pcpi_wr`) |
| Total | 67 |

## 4. Unmatched Physical-Instance Records

The report contains 102 one-sided unmatched-instance records:

| Instance type | Records |
|---|---:|
| `sky130_fd_sc_hd__decap_3` | 98 |
| `sky130_fd_sc_hd__diode_2` | 4 |
| Total | 102 |

These records identify physical-instance comparison differences. They are not, by themselves, a complete explanation of the overall LVS mismatch.

## 5. Connectivity Findings

The Netgen report also identifies net-connectivity differences involving power and body-bias pins, including:

- VPB
- VNB
- VPWR
- VGND

The extracted layout and reference netlist show differences in how certain pins and nets are associated.

The underlying cause of these connectivity differences has not been conclusively established.

## 6. Interpretation and Status

The extracted layout and reference netlist do not match.

The disconnected nodes, unmatched physical-instance records, and power/body-bias connectivity differences remain unresolved. No findings have been waived as harmless.

**LVS status: FAILED — NOT MATCHED.**

The implementation must not be described as LVS-clean or physically sign-off-ready.

