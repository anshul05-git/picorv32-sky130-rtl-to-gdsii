# PicoRV32 Synthesis Summary

## Technology and Tool

- Synthesis tool: Yosys 0.62
- PDK: SKY130A
- Standard-cell library: sky130_fd_sc_hd
- Evidence: Baseline synthesis reports, 12 November 2026

## Synthesis Metrics

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

## Major Cell Groups

| Standard cell | Count |
|---|---:|
| `sky130_fd_sc_hd__dfxtp_2` | 1,597 |
| `sky130_fd_sc_hd__mux2_1` | 1,496 |
| `sky130_fd_sc_hd__mux4_2` | 528 |
| `sky130_fd_sc_hd__or2_2` | 302 |
| `sky130_fd_sc_hd__a21o_2` | 265 |

## Diagnostic Note

The synthesis log contained:

`ABC: Error: The network is combinational.`

Synthesis completed and the recorded synthesis checks reported zero errors. However, the root cause of this ABC diagnostic was not conclusively established.

## Conclusion

The baseline synthesis produced 6,709 mapped standard-cell instances with no inferred latches, unmapped instances, or reported synthesis-check errors. The 489 lint warnings and unresolved ABC diagnostic remain disclosed.

