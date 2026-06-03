# 📖 Output Analysis Guide — CAN Bus Spoofing Diagnostic Tool

## Table of Contents
1. [Overview](#overview)
2. [Understanding the Dashboard](#understanding-the-dashboard)
3. [Summary Statistics Explained](#summary-statistics-explained)
4. [Message Table Interpretation](#message-table-interpretation)
5. [Per-ID Analysis Deep Dive](#per-id-analysis-deep-dive)
6. [Timeline Visualization](#timeline-visualization)
7. [Network Health Score](#network-health-score)
8. [Classification Logic](#classification-logic)
9. [Diagnostic Recommendations](#diagnostic-recommendations)
10. [Export Formats](#export-formats)
11. [Common Scenarios & Patterns](#common-scenarios--patterns)
12. [Threshold Tuning Guide](#threshold-tuning-guide)
13. [Troubleshooting](#troubleshooting)

---

## Overview

This guide explains how to interpret every piece of output produced by the CAN Bus Spoofing Diagnostic Tool. The tool analyzes CAN traffic logs and produces multi-dimensional results including statistical summaries, per-message severity classifications, per-ID behavioral profiling, visual timelines, and actionable recommendations.

---

## Understanding the Dashboard

After running analysis, the tool presents results across four main sections:

```
┌─────────────────────────────────────────────────────┐
│  SUMMARY STATISTICS (8 key metrics + health bar)    │
├─────────────────────────────────────────────────────┤
│  DETAILED RESULTS (4 tabs)                          │
│  ├── Messages: Per-message breakdown                │
│  ├── Timeline: Visual activity map                  │
│  ├── Per-ID: Statistical profile per CAN ID         │
│  └── Export: Download results                       │
├─────────────────────────────────────────────────────┤
│  DIAGNOSTIC RECOMMENDATION (AI-generated guidance)  │
└─────────────────────────────────────────────────────┘
```

---

## Summary Statistics Explained

### Total Messages
- **Definition**: Number of valid CAN messages successfully parsed from the input
- **Expected Range**: Typically 100–10,000+ for meaningful analysis
- **Insight**: Low counts may indicate truncated logs; very high counts indicate long capture sessions

### Unique CAN IDs
- **Definition**: Number of distinct message identifiers found in the log
- **Expected Range**: 5–100+ depending on vehicle complexity
- **Insight**: A suddenly appearing new ID not in the vehicle's DBC could indicate injection

### Normal IDs
- **Definition**: CAN IDs where ALL inter-message intervals exceed the spoofing threshold
- **Color**: Green
- **Action**: No action needed — these ECUs are transmitting normally

### Suspicious IDs
- **Definition**: CAN IDs with SOME timing violations (< 30% of their messages are anomalous)
- **Color**: Yellow/Orange
- **Action**: Investigate further — could be:
  - Bus load causing legitimate delays/compression
  - ECU firmware bug causing occasional burst transmissions
  - Low-level intermittent attack

### Spoofed IDs
- **Definition**: CAN IDs with SIGNIFICANT timing violations (> 30% of messages are anomalous)
- **Color**: Red
- **Action**: IMMEDIATE investigation required — strong indicator of:
  - Active message injection attack
  - Replay attack
  - DoS flood using this message ID

### Anomaly Events
- **Definition**: Total count of individual messages that violated timing thresholds
- **Calculation**: Sum of all per-ID anomaly counts
- **Insight**: High count relative to total messages indicates widespread attack

### Log Duration (s)
- **Definition**: Time span from first to last message in the log
- **Use**: Context for understanding attack duration and traffic density

### Avg Msg/s
- **Definition**: Total messages divided by log duration
- **Expected Range**: 500–8000 msg/s for typical CAN buses (500kbps)
- **Insight**: Abnormally high rates may indicate flooding

---

## Message Table Interpretation

The Messages tab shows every parsed message with these columns:

| Column | Description |
|--------|-------------|
| **#** | Sequential message number (after sorting by timestamp) |
| **Timestamp** | Absolute time in seconds from log start |
| **CAN ID** | Hexadecimal message identifier |
| **DLC** | Data Length Code (number of data bytes, typically 8) |
| **Data** | Payload bytes in hexadecimal |
| **Delta (ms)** | Time since the PREVIOUS message with the SAME CAN ID |
| **Severity** | Classification badge for this specific message |

### Severity Badges:
- **`first`** — First message for this ID (no delta available) — always shown as normal
- **`normal`** — Delta exceeds both thresholds — healthy interval
- **`suspicious`** — Delta is between spoofing and suspicious thresholds
- **`spoofed`** — Delta is below spoofing threshold — timing violation

### Reading Patterns in the Table:
- **Clusters of red badges** → Active attack window
- **Alternating normal/spoofed** → Intermittent injection
- **All data bytes = 00 or FF during red** → Classic injection payload signature
- **Normal data with red timing** → Sophisticated replay attack (using real payloads)

---

## Per-ID Analysis Deep Dive

The Per-ID tab provides statistical profiling for each CAN ID:

| Metric | What to Look For |
|--------|------------------|
| **Count** | Total messages for this ID. Very high counts relative to duration suggest flooding |
| **Avg Interval (ms)** | Should match the ECU's configured cycle time (from DBC file) |
| **Min Interval (ms)** | If significantly lower than average, injection occurred |
| **Max Interval (ms)** | Very high max may indicate bus-off recovery or ECU restart |
| **Std Dev** | Low = consistent timing (healthy). High = variable timing (suspicious) |
| **Anomalies** | Number of messages that violated the threshold |
| **Classification** | Overall verdict for this CAN ID |

### Interpreting Standard Deviation:
```
Std Dev < 10% of Avg → Very consistent (healthy ECU)
Std Dev 10-30% of Avg → Some jitter (normal for loaded buses)
Std Dev > 50% of Avg → Significant variation (investigate)
Std Dev > 100% of Avg → Bimodal distribution (likely attack mixed with normal)
```

### Cross-Referencing with DBC:
If you have the vehicle's DBC (Database CAN) file:
- Compare **Avg Interval** against the specified cycle time
- A message specified at 10ms showing 2ms average is being injected
- A message specified at 100ms showing 10ms average is being flooded

---

## Timeline Visualization

The timeline shows a horizontal bar for each CAN ID with colored markers:

- **Green markers**: Messages with normal timing
- **Yellow markers**: Messages with suspicious timing
- **Red markers**: Messages with spoofed/violated timing

### What to Look For:
- **Dense red clusters**: Concentrated attack window — note the time range
- **Scattered red dots**: Intermittent spoofing — harder to detect, may be stealthy
- **Entire bar red**: Continuous injection throughout the capture
- **Red appearing on multiple IDs simultaneously**: Coordinated multi-ID attack

### Correlating Attack Windows:
Look at the X-axis (time) position of red markers across different IDs:
- Simultaneous red on multiple IDs → Multi-vector attack from single source
- Sequential red on different IDs → Attacker scanning/probing different ECUs
- Single ID with periodic red bursts → Automated attack script with intervals

---

## Network Health Score

The health bar provides a single-number summary:

```
Health % = 100 - (Anomalies / Total Messages × 500)
Capped at 0-100%
```

| Score | Status | Meaning |
|-------|--------|---------|
| 80-100% | 🟢 Good | Network is healthy, minimal anomalies |
| 50-79% | 🟡 Degraded | Notable anomalies present — investigate |
| 0-49% | 🔴 Under Attack | Significant injection/spoofing detected |

**Note**: Health score is weighted aggressively — even 5% anomalous messages result in significant score reduction, because in safety-critical automotive systems, even small amounts of injected messages can be dangerous.

---

## Classification Logic

### Per-Message Classification:
```
IF delta < spoofing_threshold (default 5ms):
    severity = "SPOOFED"
ELSE IF delta < suspicious_threshold (default 8ms):
    severity = "SUSPICIOUS"
ELSE:
    severity = "NORMAL"
```

### Per-ID Classification:
```
IF anomaly_count > (message_count × 0.30):
    classification = "SPOOFED"
ELSE IF anomaly_count > 0:
    classification = "SUSPICIOUS"
ELSE:
    classification = "NORMAL"
```

### Why 30% Threshold?
- Below 30%: Could be legitimate bus contention or ECU timing jitter
- Above 30%: Statistically impossible to attribute to normal operation
- This threshold is conservative — real attacks often show 50-90% anomaly rates

---

## Diagnostic Recommendations

The tool generates contextual recommendations based on findings:

### When Spoofing Detected:
1. **Isolate affected ECU(s)** — Disconnect suspected malicious node from bus
2. **Physical inspection** — Check for unauthorized CAN tap points, OBD-II devices
3. **Gateway hardening** — Add message filtering rules for affected IDs
4. **SecOC deployment** — Implement Secure Onboard Communication (AUTOSAR)
5. **IDS deployment** — Install real-time intrusion detection on gateway ECU

### When Suspicious Activity Found:
1. **Extended capture** — Record longer sessions to confirm pattern
2. **Threshold tuning** — Adjust thresholds based on vehicle-specific timing specs
3. **Bus load analysis** — High bus load can cause legitimate timing compression
4. **ECU firmware check** — Some ECUs have known timing bugs

### When Network is Healthy:
1. **Baseline documentation** — Save this analysis as the known-good reference
2. **Periodic re-assessment** — Schedule regular monitoring captures
3. **Threshold validation** — Confirm thresholds match vehicle specifications

---

## Export Formats

### CSV Export
- One row per message
- Columns: Timestamp, CAN_ID, DLC, Data, Delta_ms, Severity
- Compatible with Excel, Google Sheets, Python pandas, MATLAB

### JSON Export
- Structured object with `summary`, `perIdAnalysis`, and `messages` arrays
- Compatible with any programming language for further automated processing
- Includes all statistical metrics computed during analysis

### Full Report (TXT)
- Human-readable narrative report
- Includes complete per-ID breakdown
- Suitable for pasting into investigation tickets or email reports

### Print / PDF
- Uses browser print function
- Creates formatted PDF of the current screen
- Good for attaching to engineering reviews or safety reports

---

## Common Scenarios & Patterns

### Scenario 1: Single-ID Injection
**Pattern**: One CAN ID shows red/spoofed while all others are green/normal
**Cause**: Attacker targeting specific ECU functionality (e.g., steering angle, brake pressure)
**Signature**: Sudden burst of rapid messages with uniform payload (often all-zeros or all-FF)

### Scenario 2: Multi-ID Coordinated Attack
**Pattern**: Multiple IDs turn red simultaneously
**Cause**: Sophisticated attacker controlling multiple vehicle functions
**Signature**: Attack starts at same timestamp across multiple IDs

### Scenario 3: DoS Flood
**Pattern**: New IDs (often 0x000-0x00F, highest priority) appear with massive message counts
**Cause**: Attacker flooding bus with highest-priority messages to starve legitimate ECUs
**Signature**: Very low min interval (< 0.5ms), payload often all-FF

### Scenario 4: Replay Attack
**Pattern**: Normal-looking data but at elevated frequency
**Cause**: Attacker recorded legitimate traffic and replays at faster rate
**Signature**: Data values are realistic but timing is abnormal

### Scenario 5: Intermittent Stealth Attack
**Pattern**: Periodic short bursts of red scattered through the timeline
**Cause**: Attacker uses short injection windows to avoid detection
**Signature**: Per-ID classification may show "suspicious" rather than "spoofed" — use lower thresholds

---

## Threshold Tuning Guide

### Default Thresholds:
| Parameter | Default | Range | When to Adjust |
|-----------|---------|-------|----------------|
| Spoofing Threshold | 5ms | 1-50ms | Lower for high-speed buses; higher for slow ECUs |
| Suspicious Threshold | 8ms | 2-100ms | Should always be > spoofing threshold |
| Min Messages | 3 | 2-50 | Higher for noisy environments to reduce false positives |
| Baseline Window | 1.0s | 0.1-10s | Longer for intermittent attacks |

### Tuning for Specific Vehicle Architectures:
- **High-speed powertrain CAN (500kbps)**: Spoofing threshold = 3-5ms
- **Body/comfort CAN (125-250kbps)**: Spoofing threshold = 8-15ms
- **ADAS CAN (500kbps-1Mbps)**: Spoofing threshold = 2-3ms
- **Diagnostic CAN**: Spoofing threshold = 20-50ms (request/response pattern)

### Reducing False Positives:
1. Start with default thresholds
2. Run on known-good (baseline) data
3. If false positives appear, increase spoofing threshold by 1-2ms
4. Repeat until baseline shows 100% normal
5. Use this calibrated threshold for production analysis

---

## Troubleshooting

### "Not enough valid messages to analyze"
- Ensure log format matches expected pattern (Timestamp ID DLC Data)
- Check for header lines that aren't CAN messages
- Verify timestamps are numeric (decimal seconds)

### All messages show as "normal" but attack is suspected
- Lower the spoofing threshold (try 3ms or 2ms)
- Check if attacker is injecting just above your threshold
- Try Comprehensive analysis mode

### Too many false positives on legitimate traffic
- Increase spoofing threshold
- Check if vehicle has ECUs with naturally fast cycle times
- Use the Per-ID table to identify which specific ID is triggering
- Cross-reference with DBC to validate expected cycle times

### Large file takes too long
- The tool handles 5000+ messages comfortably
- For files > 50,000 messages, consider splitting by time window
- Use severity filter to reduce displayed results

---

*This guide is part of the CAN Bus Spoofing Diagnostic Tool project. For questions or enhancements, refer to the project repository.*
