# 🛡️ CAN Bus Spoofing & Injection Diagnostic Tool v2.0

**Professional-grade automated detection of malicious message injection, replay attacks, and DoS flooding on vehicle CAN networks.**

---

## 🚀 Quick Start

### Open the Tool
1. Download `CAN_Spoofing_Diagnostic_Tool.html`
2. Double-click to open in **any modern browser** (Chrome, Edge, Firefox)
3. Click **"⚠️ Attack Data"** → then **"▶ Analyze Log"** to see it in action
4. Or drag & drop any of the test data files from the `test_data/` folder

> ✅ **Runs fully offline** — All processing happens locally in your browser. No data is sent anywhere. No installation needed.

---

## 📂 Repository Contents

| File / Folder | Description |
|---|---|
| `CAN_Spoofing_Diagnostic_Tool.html` | Main diagnostic tool — dark-themed, professional UI with full analysis capabilities |
| `Expo_OnePager.md` | Detailed one-pager covering problem, solution, tool features, usage, and output interpretation |
| `OUTPUT_ANALYSIS_GUIDE.md` | Comprehensive guide to interpreting all tool outputs, patterns, and recommendations |
| `Expo_Showcase_Presentation.pptx` | Professional presentation for expo/showcase events |
| `CAN_Spoofing_Presentation.pptx` | Technical presentation variant |
| `README.md` | This file |
| `test_data/` | 10 pre-built test scenarios (500–5000 messages each) |
| `expo_assets/` | Supporting assets for presentations |

---

## 🔬 Tool Features

### Analysis Capabilities
- ⚡ **Timing Analysis** — Detects messages arriving faster than configured thresholds
- 📊 **Statistical Profiling** — Avg, min, max, std dev of intervals per CAN ID
- 🎯 **Per-ID Classification** — NORMAL / SUSPICIOUS / SPOOFED verdict per ECU
- 📈 **Network Health Score** — Single-number bus integrity metric
- 🗺️ **Visual Timeline** — Color-coded activity map per CAN ID
- 🤖 **Smart Recommendations** — Automated diagnostic guidance based on findings

### User Interface
- 🌑 Professional dark theme designed for engineering environments
- 📁 Drag & drop file loading
- ⚙️ Configurable thresholds (spoofing, suspicious, analysis mode)
- 📋 Tabbed results (Messages, Timeline, Per-ID, Export)
- 📊 Real-time summary statistics dashboard
- 🔍 Severity filtering

### Export Options
- 📊 **CSV** — Full message-level export for spreadsheet analysis
- 📄 **JSON** — Structured data for programmatic processing
- 📝 **TXT Report** — Human-readable diagnostic report
- 🖨️ **Print/PDF** — Browser print for documentation

---

## 📁 Test Data

10 comprehensive test scenarios covering different attack patterns:

| File | Messages | Description |
|------|----------|-------------|
| `01_normal_traffic_500msg_PASS.asc` | 500 | Clean traffic, 8 ECUs — baseline reference |
| `02_single_id_injection_800msg_FAIL.asc` | 800 | Single ID injection attack on 0x100 |
| `03_multi_id_coordinated_attack_1200msg_FAIL.asc` | 1,200 | Coordinated attack on multiple IDs simultaneously |
| `04_replay_attack_pattern_1000msg_FAIL.asc` | 1,000 | Replay attack with captured message patterns |
| `05_stress_test_25ECUs_3000msg.asc` | 3,000 | 25 ECUs with sporadic anomalies |
| `06_DoS_flood_attack_2000msg_FAIL.asc` | 2,000 | Denial of Service flood on high-priority IDs |
| `07_csv_format_mixed_800msg.csv` | 800 | CSV format with timing anomalies |
| `08_vector_asc_format_1500msg.asc` | 1,500 | Vector ASC format with fuzzing attack |
| `09_intermittent_spoofing_2500msg.asc` | 2,500 | Intermittent stealth attack bursts |
| `10_realistic_40ECUs_5000msg.asc` | 5,000 | Realistic 40-ECU capture with two attack windows |

---

## 📊 Presentations

### Expo Showcase (`Expo_Showcase_Presentation.pptx`)
Professional presentation suitable for technology expos and innovation showcases. Covers:
- Problem statement and automotive cybersecurity landscape
- Solution architecture and detection methodology
- Live demo walkthrough
- Results interpretation
- Future roadmap

---

## 📖 Documentation

### One-Pager (`Expo_OnePager.md`)
Complete single-document reference covering:
- Problem definition with threat matrix
- Solution overview and key differentiators
- Full feature breakdown with detection methods
- Step-by-step usage instructions
- Output interpretation guide
- Supported formats and roadmap

### Output Analysis Guide (`OUTPUT_ANALYSIS_GUIDE.md`)
Deep-dive reference for interpreting results:
- Dashboard layout explanation
- Every metric defined with expected ranges
- Per-ID statistical interpretation with DBC cross-referencing
- Timeline pattern recognition
- Classification logic and thresholds
- 5 common attack scenario patterns
- Threshold tuning guide for different vehicle architectures
- Troubleshooting guide

---

## 🛠️ Supported Input Formats

| Format | Extensions | Example Line |
|--------|-----------|--------------|
| Space-separated | `.asc`, `.txt`, `.log` | `0.001000 100 8 FF FF FF FF FF FF FF FF` |
| CSV | `.csv` | `0.001000,0x100,8,FF,FF,FF,FF,FF,FF,FF,FF` |
| Vector ASC | `.asc` | `   0.001000 1  100  Rx d 8 FF FF FF FF FF FF FF FF` |

---

## 🔒 Security & Privacy

- **No network calls** — Tool operates 100% offline
- **No data storage** — Nothing is saved to disk (unless you export)
- **No telemetry** — Zero tracking or analytics
- **Local processing** — All computation happens in your browser's JavaScript engine

---

## 📞 Getting Started

1. Clone or download this repository
2. Open `CAN_Spoofing_Diagnostic_Tool.html` in your browser
3. Load any file from `test_data/` to explore different attack scenarios
4. Read `OUTPUT_ANALYSIS_GUIDE.md` for detailed interpretation help
5. Use `Expo_OnePager.md` for quick reference or presentation preparation
