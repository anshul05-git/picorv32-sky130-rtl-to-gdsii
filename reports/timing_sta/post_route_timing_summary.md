# PicoRV32 Post-Route Timing Summary

## Analysis Configuration

- Analysis stage: Post-route STA
- Technology: SKY130A
- Standard-cell library: sky130_fd_sc_hd
- Clock: core_clk
- Clock period: 10 ns
- Analysis coverage: Nine RC/PVT corners
- Parasitics: Post-route extracted using OpenRCX

## Nine-Corner Setup Results

| Corner | Worst Setup Slack (ns) | Setup Violations |
|---|---:|---:|
| nom_tt_025C_1v80 | +2.7873 | 0 |
| nom_ss_100C_1v60 | -3.2910 | 636 |
| nom_ff_n40C_1v95 | +3.7872 | 0 |
| min_tt_025C_1v80 | +2.8592 | 0 |
| min_ss_100C_1v60 | -3.1763 | 494 |
| min_ff_n40C_1v95 | +3.8329 | 0 |
| max_tt_025C_1v80 | +2.7114 | 0 |
| max_ss_100C_1v60 | -3.4267 | 795 |
| max_ff_n40C_1v95 | +3.7411 | 0 |

Total setup violations across the nine corners: **1,925**.

## Nine-Corner Hold Results

| Corner | Worst Hold Slack (ns) | Hold Violations |
|---|---:|---:|
| nom_tt_025C_1v80 | +0.2640 | 0 |
| nom_ss_100C_1v60 | +0.6617 | 0 |
| nom_ff_n40C_1v95 | +0.0790 | 0 |
| min_tt_025C_1v80 | +0.2614 | 0 |
| min_ss_100C_1v60 | +0.7372 | 0 |
| min_ff_n40C_1v95 | +0.0773 | 0 |
| max_tt_025C_1v80 | +0.2669 | 0 |
| max_ss_100C_1v60 | +0.5622 | 0 |
| max_ff_n40C_1v95 | +0.0811 | 0 |

Total hold violations across the nine corners: **0**.

## Overall Electrical Checks

| Check | Reported violations |
|---|---:|
| Maximum capacitance | 17 |
| Maximum transition / slew | 2,313 |

## Key Findings

- All three SS corners have negative setup slack.
- All three TT and all three FF corners have positive setup slack.
- The worst setup slack is -3.4267 ns at `max_ss_100C_1v60`.
- All nine corners pass the reported hold checks.
- Maximum-capacitance and maximum-slew violations remain.

## Conclusion

The post-route implementation passes the reported hold checks across all nine RC/PVT corners but fails setup timing at all three SS corners.

The implementation is not timing-clean and should not be considered sign-off ready.

