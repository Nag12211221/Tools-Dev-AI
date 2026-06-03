# 🔋 Key-Off Load / NM Wake Diagnostic Tool

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
└── README.md                         ← This file
```

---

## 🎯 Problem Solved

**Sleep mode failures** where ECUs hold the CAN bus awake after key-off cause:
- Battery drain (parasitic current > 50mA)
- Dead battery complaints & warranty claims ($200-500/vehicle)
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