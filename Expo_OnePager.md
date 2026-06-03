# 🛡️ CAN Bus Spoofing & Injection Diagnostic Tool — Expo One-Pager

---

## 🔴 The Problem

Modern vehicles rely on **Controller Area Network (CAN)** buses to transmit thousands of messages per second between Electronic Control Units (ECUs). This protocol, designed decades ago, has **no built-in authentication or encryption**, making it vulnerable to:

| Threat | Impact | Real-World Risk |
|--------|--------|-----------------|
| Message Injection | Attacker sends fake messages at high frequency to override legitimate ECU signals | Unauthorized steering, braking, or acceleration commands |
| Replay Attack | Previously captured legitimate messages are re-transmitted | Bypassing immobilizer, replaying unlock sequences |
| Denial of Service (DoS) | Bus flooded with highest-priority messages, starving legitimate ECUs | Complete loss of vehicle communication |
| Fuzzing | Random or semi-random messages injected to discover exploitable behaviors | Crashes in ECU firmware, unexpected vehicle behavior |

**Current Challenges:**
- Manual analysis of CAN logs is **extremely time-consuming** (hours per log file)
- Timing anomalies at microsecond precision are **invisible to human reviewers**
- No standardized tooling for **cybersecurity-focused CAN analysis** at the engineering team level
- Traditional tools focus on protocol conformance, **not intrusion detection**

---

## ✅ The Solution

A **browser-based, offline-capable diagnostic tool** that automatically detects spoofed or injected CAN messages by analyzing timing patterns, frequency anomalies, and payload irregularities.

### Key Differentiators:
- **Zero Installation**: Runs in any modern browser — no Python, no dependencies, no server
- **100% Offline**: All data stays local — critical for security-sensitive automotive data
- **Sub-second Analysis**: Processes thousands of messages in milliseconds
- **Professional Reporting**: Export CSV, JSON, or full text reports for documentation
- **Configurable Detection**: Adjustable thresholds for different vehicle architectures

---

## 🔧 What the Tool Does

The CAN Bus Spoofing Diagnostic Tool performs **multi-dimensional analysis** on CAN traffic logs:

### Detection Capabilities:

| Capability | Description | Method |
|---|---|---|
| **Timing Analysis** | Detects messages arriving faster than expected cyclic rate | Inter-message delta comparison against configurable threshold |
| **Frequency Analysis** | Identifies IDs transmitting at abnormal rates | Statistical analysis of message rate per CAN ID |
| **Burst Detection** | Finds clusters of rapid messages (injection windows) | Sliding window anomaly detection |
| **Per-ID Classification** | Classifies each CAN ID as Normal, Suspicious, or Spoofed | Combined scoring based on anomaly count vs. total messages |
| **Statistical Profiling** | Computes avg, min, max, std dev of intervals per ID | Full statistical breakdown with deviation alerting |
| **Network Health Scoring** | Overall bus health percentage | Weighted anomaly-to-total-message ratio |

### Classification Logic:
```
NORMAL      → No timing violations detected
SUSPICIOUS  → Some violations found (< 30% of messages for that ID)
SPOOFED     → Significant violations (> 30% of messages for that ID)
```

---

## 🖥️ How to Use the Tool

### Step 1: Open the Tool
- Download `CAN_Spoofing_Diagnostic_Tool.html`
- Double-click to open in Chrome, Edge, or Firefox
- No installation or internet connection required

### Step 2: Load Data
Choose one of three methods:
- **Drag & Drop**: Drag a `.asc`, `.csv`, `.txt`, or `.log` file onto the drop zone
- **File Browser**: Click the drop zone to open a file picker
- **Paste**: Copy-paste log data directly into the text area
- **Demo Data**: Click one of three built-in demo buttons (Normal, Attack, Complex)

### Step 3: Configure (Optional)
- **Spoofing Threshold**: Messages arriving faster than this are classified as SPOOFED (default: 5ms)
- **Suspicious Threshold**: Messages faster than this but slower than spoofing threshold are SUSPICIOUS (default: 8ms)
- **Analysis Mode**: Choose Timing, Frequency, Payload, or Comprehensive analysis
- **Severity Filter**: Filter results to show only the severity levels you care about

### Step 4: Run Analysis
- Click **"▶ Analyze Log"**
- Results appear instantly across multiple views:
  - **Summary Statistics**: Total messages, unique IDs, anomaly count, health score
  - **Messages Tab**: Full message-by-message breakdown with severity badges
  - **Timeline Tab**: Visual timeline showing message distribution per CAN ID
  - **Per-ID Tab**: Statistical breakdown per CAN ID with classification

### Step 5: Export Results
- **CSV**: Spreadsheet-compatible export of all messages with severity
- **JSON**: Machine-readable structured output for integration
- **Full Report**: Human-readable diagnostic report with recommendations
- **Print**: Browser print dialog for PDF generation

---

## 📊 How to Analyze the Output

### Reading the Summary Dashboard:
| Metric | What It Tells You |
|--------|-------------------|
| Total Messages | Volume of traffic captured |
| Unique CAN IDs | Number of distinct ECUs communicating |
| Normal IDs | ECUs with healthy, expected timing |
| Suspicious IDs | ECUs with minor timing irregularities (investigate further) |
| Spoofed IDs | ECUs with clear injection patterns (immediate action needed) |
| Network Health % | Overall bus integrity (>80% = Good, 50-80% = Degraded, <50% = Under Attack) |

### Reading the Per-ID Table:
- **Avg Interval**: Expected cyclic time — compare against ECU specification
- **Min Interval**: Shortest gap observed — if this is much smaller than average, injection is likely
- **Std Dev**: High standard deviation indicates inconsistent timing (possible intermittent attack)
- **Anomalies**: Raw count of timing violations for this ID

### Interpreting the Timeline:
- **Green markers**: Normal message timing
- **Yellow markers**: Suspicious intervals
- **Red markers**: Confirmed timing violations — dense red clusters indicate attack windows

### Acting on Recommendations:
The tool provides automated diagnostic recommendations based on findings:
- **Under Attack**: Lists affected IDs, suggests physical inspection, gateway hardening, and SecOC deployment
- **Suspicious Activity**: Recommends extended monitoring and threshold tuning
- **Healthy**: Confirms normal operation, suggests continued periodic monitoring

---

## 📁 Supported Input Formats

| Format | Extension | Example |
|--------|-----------|---------|
| Space-separated | `.asc`, `.txt`, `.log` | `0.001000 100 8 FF FF FF FF FF FF FF FF` |
| CSV | `.csv` | `0.001000,0x100,8,FF,FF,FF,FF,FF,FF,FF,FF` |
| Vector ASC | `.asc` | `   0.001000 1  100  Rx d 8 FF FF FF FF FF FF FF FF` |

---

## 🚀 Future Enhancements (Roadmap)
- Machine learning–based anomaly detection
- Real-time CAN bus monitoring via USB-CAN adapter integration
- AUTOSAR SecOC validation
- Integration with vehicle DTC (Diagnostic Trouble Code) databases
- Multi-bus analysis (CAN-FD, LIN, FlexRay correlation)

---

## 📞 Contact & Resources
- **Repository**: Contains tool, presentation, test data, and documentation
- **Test Data**: 10 pre-built scenarios covering normal traffic, injection, DoS, replay, and fuzzing attacks
- **Offline**: Tool works without any network connection — ideal for secure environments
