Run original failing test; must pass. Run related tests. For intermittent bugs, run 5+ times.
| Crash/Panic | Stack trace backward; trace the bad value to its source |
| Wrong Output | Binary search; log midpoint, halve search space each iteration |
| Intermittent | Compare passing vs failing run logs; find ordering divergence |
| Regression | `git bisect`; find the offending commit |
| Performance | Timing at stage boundaries; find the bottleneck |