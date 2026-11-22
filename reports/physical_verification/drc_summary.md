# PicoRV32 Physical Design Rule Check (DRC) Summary

## 1. Magic DRC

| Rule | Coordinate entries |
|---|---:|
| nwell.2a | 4,086 |
| LU.3 | 1,086 |
| nwell.4 | 614 |
| nwell.1 | 251 |
| Total | 6,037 |

These are raw coordinate entries, not necessarily unique physical violations.

## 2. KLayout DRC

Rule deck: SKY130A `sky130A_mr.drc`

| Rule | Markers |
|---|---:|
| nwell.2a | 3,886 |
| nwell.1 | 502 |
| hvtp.1 | 502 |
| hvtp.2 | 502 |
| Total | 5,392 |

All other rules listed in the KLayout JSON summary reported zero markers.

## 3. Interpretation

Magic and KLayout produced different marker counts and rule-category breakdowns. Their results are reported independently and must not be added together.

**DRC status: Not clean.**

The reported DRC findings remain unresolved. The layout should not be described as DRC-clean or sign-off-ready.

