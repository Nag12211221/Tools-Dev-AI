# Key-Off Load / NM Wake Diagnostic Tool

## Executive Summary
This tool automates detection of ECUs that keep the CAN network awake after key-off, enabling faster root-cause analysis for parasitic battery drain issues and reducing warranty investigation effort.

## Problem Overview
- Some ECUs continue NM activity after key-off and delay or prevent network sleep.
- Persistent wake states can increase current draw beyond acceptable limits (>50 mA).
- Manual review of large log files is slow and inconsistent across teams.

## Solution Overview
- Automated log analysis for ASC/CSV/TXT input.
- ECU classification: **NORMAL**, **SLOW**, **VIOLATION**, **STUCK AWAKE**.
- Timeline evidence and ECU-level detail for technical reviews.
- Exportable CSV/JSON outputs for issue tracking and reporting.

## Analysis Workflow
1. Load a log file.
2. Set key-off timestamp and timing thresholds.
3. Run analysis.
4. Review violations and export results.

## Core Capabilities
| Capability | Detail |
|---|---|
| Log parsing | Supports Vector ASC, CSV, TXT formats |
| ECU classification | Normal, Slow, Violation, Stuck Awake |
| Timeline view | Per-ECU activity around key-off |
| Configurability | Key-off time, NM timeout, max wake time |
| Export | CSV, JSON, and printable report output |
| Scale validation | Tested with up to 50 ECUs and 2,700+ messages |

## ECU Status Definitions
| Status | Meaning | Recommended Action |
|---|---|---|
| NORMAL | ECU entered sleep within NM timeout | No action |
| SLOW | ECU slept within max window, but late | Monitor and tune thresholds |
| VIOLATION | ECU exceeded max wake duration before sleeping | Investigate NM configuration |
| STUCK AWAKE | ECU never entered sleep in the log window | Critical root-cause analysis |

## Included Validation Datasets
| File | ECUs | Scenario | Expected Outcome |
|---|---:|---|---|
| `01_normal_shutdown_PASS.csv` | 8 | Baseline normal shutdown | 0 violations |
| `02_multiple_violators_FAIL.csv` | 15 | Mixed healthy and failing ECUs | 7 violations |
| `03_vector_asc_format.asc` | 8 | Real Vector trace format | 2 stuck ECUs |
| `04_edge_cases.csv` | 7 | Boundary and re-wake patterns | Mixed outcomes |
| `05_realworld_complex_25ECUs.csv` | 25 | Full vehicle simulation | 8 violations |
| `06_stress_test_50ECUs.csv` | 50 | High-volume stress run | 15 violations |

## Package Contents
- `KeyOffLoad_Diagnostic_Tool.html` — Main diagnostic tool
- `Expo_OnePager.md` — One-page summary document
- `Expo_Showcase_Presentation.pptx` — Presentation deck
- `OUTPUT_ANALYSIS_GUIDE.md` — Output interpretation guide
- `test_data/` — Demo and validation datasets

## Demo Steps
1. Open the diagnostic tool file.
2. Load demo data or paste your own log.
3. Set key-off timestamp.
4. Run analysis and review ECU status/output exports.
