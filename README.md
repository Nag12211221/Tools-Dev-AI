# 🔋 Key-Off Load/NM Wake Diagnostic Tool

**Automated detection of ECUs causing parasitic battery drain after key-off.**

---

## 🚀 Quick Start

### GUI Tool (No Installation Required!)
1. Download `KeyOffLoad_Diagnostic_Tool.html`
2. Double-click to open in **any modern browser** (Chrome, Edge, Firefox)
3. Click **"Load Demo Data"** → then **"Analyze Log"** to see it in action
4. Or paste/upload your own NM log files (.asc, .csv, .txt)

> ✅ **100% browser-based** — No Python, no installation, no licenses needed.  
> ✅ **100% offline** — All processing happens locally; no data is sent anywhere.

---

## 📊 Presentation
- Download `KeyOffLoad_Presentation.pptx` for a ready-to-use 5-slide presentation covering:
  - Problem statement & business impact
  - Solution overview & time savings
  - How it works (Input → Analyze → Report)
  - Demo instructions & future enhancements

---

## 📋 Features

| Feature | Description |
|---------|-------------|
| 🔍 Log Parsing | Supports ASC (Vector), CSV, and TXT NM log formats |
| ⏱️ Timeline View | Visual per-ECU state timeline showing sleep/wake transitions |
| ⚠️ Violation Detection | Flags ECUs that exceed max allowed wake time or never sleep |
| 📊 Summary Dashboard | Total ECUs, violations, normal shutdowns at a glance |
| 📥 Export | CSV, JSON export + Print-friendly report |
| ⚙️ Configurable | Key-off time, max wake time, NM timeout — all adjustable |

---

## 📂 Repository Contents

```
├── KeyOffLoad_Diagnostic_Tool.html   ← GUI Tool (open in browser)
├── KeyOffLoad_Presentation.pptx      ← PowerPoint presentation (5 slides)
├── OUTPUT_ANALYSIS_GUIDE.md          ← How to read & analyze tool output
├── test_data/
│   ├── 01_normal_shutdown_PASS.csv       ← All ECUs sleep properly (baseline)
│   ├── 02_multiple_violators_FAIL.csv    ← 15 ECUs, 7 violations
│   ├── 03_vector_asc_format.asc          ← Vector CANalyzer ASC format test
│   ├── 04_edge_cases.csv                 ← Boundary conditions & tricky scenarios
│   ├── 05_realworld_complex_25ECUs.csv   ← Full vehicle (25 ECUs, 1000+ msgs)
│   └── 06_stress_test_50ECUs.csv         ← Stress test (50 ECUs, 2700+ msgs)
└── README.md                         ← This file
```

---

## 🎯 Problem Solved

**Sleep mode failures** where ECUs hold the CAN bus awake after key-off cause:
- Battery drain (parasitic current > 50mA)
- Dead battery complaints & warranty claims ($200-500/vehicle)

---

## 🧪 Test Data & Demo

Six test files are provided in `test_data/` to validate the tool:

| File | ECUs | Scenario | Expected |
|------|:----:|----------|----------|
| `01_normal_shutdown_PASS.csv` | 8 | All ECUs sleep properly | ✅ Zero violations |
| `02_multiple_violators_FAIL.csv` | 15 | Mixed good + bad ECUs | ❌ 7 violations |
| `03_vector_asc_format.asc` | 8 | Real Vector ASC format | ❌ 2 stuck ECUs |
| `04_edge_cases.csv` | 7 | Re-wake, flapping, border | ❌ Boundary tests |
| `05_realworld_complex_25ECUs.csv` | 25 | Full vehicle simulation | ❌ 8 violations |
| `06_stress_test_50ECUs.csv` | 50 | Stress test (2700+ lines) | ❌ 15 violations |

📖 See **[OUTPUT_ANALYSIS_GUIDE.md](OUTPUT_ANALYSIS_GUIDE.md)** for detailed instructions on reading and interpreting results.
- Time-consuming manual log analysis (8-16 hours → reduced to < 5 minutes)

---

## 📧 Sharing

Just send the `KeyOffLoad_Diagnostic_Tool.html` file to anyone — they can open it directly in their browser without any setup.

---

## 🛠️ For Developers

If you want to extend the tool:
- The HTML file is self-contained (HTML + CSS + JavaScript in one file)
- No build tools or dependencies required
- Edit with any text editor

---

## 🎪 Open Expo Assets

For event/demo sharing:

- `Expo_OnePager.html` — colorful one-pager with visuals, printable handout format
- `Expo_Showcase_Presentation.pptx` — colorful showcase deck for live presentation
- `expo_assets/` — visual images used in the one-pager and PPT

**Direct paths:**
- `./Expo_OnePager.html`
- `./Expo_Showcase_Presentation.pptx`

