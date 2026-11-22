# PicoRV32 Worst Setup Critical Path

## Analysis Context

- Analysis stage: Post-route STA
- Path type: Maximum delay (setup)
- Path group: core_clk
- Worst corner: max_ss_100C_1v60
- Clock period: 10 ns
- Technology: SKY130A
- Standard-cell library: sky130_fd_sc_hd

## Critical Path Details

| Parameter | Result |
|---|---|
| Startpoint | `_09946_` |
| Endpoint | `_11225_` |
| Data arrival time | 14.974246 ns |
| Data required time | 11.547532 ns |
| Setup slack | -3.426714 ns |
| Timing status | VIOLATED |

## Path Characteristics

The reported path starts at flip-flop `_09946_` and terminates at flip-flop `_11225_`, both clocked by `core_clk`.

The examined combinational path contains a long sequence of `sky130_fd_sc_hd__and4_4` gates, followed by:
- `sky130_fd_sc_hd__a21oi_2`
- `sky130_fd_sc_hd__o21a_2`

The report shows the signal `decoder_trigger` immediately after the launching flip-flop.

## Timing Interpretation

The data arrival time is 14.974246 ns, while the required arrival time is 11.547532 ns.

The resulting setup slack is:

`11.547532 - 14.974246 = -3.426714 ns`

The negative slack indicates that the path misses the configured setup timing requirement by approximately 3.427 ns at this corner.

The long sequence of combinational gates is an observed characteristic of this path. Further logic-level investigation would be required to establish the complete root cause and select an optimization.

## Conclusion

The worst reported setup path is between `_09946_` and `_11225_` at the max SS corner. It is a setup timing violation and contributes to the overall timing failure of the implementation.

