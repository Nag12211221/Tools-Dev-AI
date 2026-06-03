# 📊 Output Analysis Guide — Key-Off Load Diagnostic Tool

## How to Use the Test Files

1. Open `KeyOffLoad_Diagnostic_Tool.html` in your browser
2. Click the upload zone or paste data from any test file
3. Set the **Key-Off Timestamp** (see table below per file)
4. Click **"🔍 Analyze Log"**

### Test File Configuration

| Test File | Key-Off Time | Max Wake Time | Expected Result |
|-----------|:------------:|:-------------:|-----------------|
| `01_normal_shutdown_PASS.csv` | 5.0s | 10.0s | ✅ ALL PASS — no violations |
| `02_multiple_violators_FAIL.csv` | 3.0s | 10.0s | ❌ 7 ECUs violating (3 slow + 4 stuck) |
| `03_vector_asc_format.asc` | 10.0s | 10.0s | ❌ 2 ECUs stuck awake (EPS, ABS) |
| `04_edge_cases.csv` | 2.0s | 10.0s | ❌ Mixed results — tests boundary conditions |
| `05_realworld_complex_25ECUs.csv` | 8.0s | 10.0s | ❌ 8 ECUs violating (4 slow + 4 stuck) |
| `06_stress_test_50ECUs.csv` | 5.0s | 10.0s | ❌ 15 ECUs violating (8 slow + 7 stuck) |

---

## Understanding the Output

### 1. Summary Cards (Top Section)

```
┌──────────────┬──────────────┬──────────────┬──────────────┐
│ Total ECUs   │ Violating    │ Normal       │ Log Duration │
│     25       │     8 ❌     │    17 ✅     │   120.0s     │
└──────────────┴──────────────┴──────────────┴──────────────┘
```

- **Total ECUs Detected** — Number of unique ECUs found in the log
- **Violating ECUs** (RED) — ECUs that exceeded the max wake time or never slept
- **Normal Shutdown** (GREEN) — ECUs that entered sleep within the allowed timeframe
- **Log Duration** — Total time span of the log data

### 2. NM State Timeline (Visual Bar Chart)

```
ECU Name    [====BLUE (pre key-off)====|==RED (awake)==|===GREEN (asleep)===]
                                       ↑
                                  Key-Off Line (yellow)
```

**Color Coding:**
| Color | Meaning |
|-------|---------|
| 🔵 Blue | Pre key-off — normal active state |
| 🔴 Red | Post key-off, still AWAKE — potential drain |
| 🟢 Green | ASLEEP — no current draw |
| 🟡 Yellow dashed line | Key-off moment |
| ⚠️ "STUCK" label | ECU never entered sleep |

**How to read it:**
- Short red sections = ECU shut down quickly (GOOD)
- Long red sections = ECU took too long to sleep (BAD)
- Red extending to end of bar with "STUCK" = ECU never slept (CRITICAL)

### 3. ECU Detail Table

| Column | Meaning |
|--------|---------|
| **ECU** | ECU node name from the log |
| **First Seen** | Timestamp of first message from this ECU |
| **Last Activity** | Timestamp of last message (awake state) |
| **Sleep Time** | When the ECU entered sleep mode ("NEVER" if it didn't) |
| **Time to Sleep** | Duration from key-off to sleep entry |
| **Msgs After Key-Off** | Number of NM messages sent after key-off |
| **Status** | Classification (see below) |

**Status Classifications:**

| Status | Meaning | Action |
|--------|---------|--------|
| `NORMAL` | Slept within NM timeout (default 5s) | ✅ No action needed |
| `SLOW` | Slept within max wake time but slower than NM timeout | ⚠️ Monitor — may need NM tuning |
| `VIOLATION` | Exceeded max wake time before sleeping | ❌ Investigate NM config |
| `STUCK AWAKE` | Never entered sleep mode | 🚨 CRITICAL — root cause required |

### 4. Violations Section (Red Box)

This section only appears if violations are detected:

```
⚠️ Violations Detected
┌─────────────┬────────────────────────┬─────────────────────────────────┬──────────┐
│ ECU         │ Type                   │ Detail                          │ Severity │
├─────────────┼────────────────────────┼─────────────────────────────────┼──────────┤
│ TCU         │ Never entered sleep    │ ECU still active at 25.000s     │ CRITICAL │
│ HVAC        │ Exceeded max wake time │ Took 14.2s to sleep (max: 10s) │ WARNING  │
└─────────────┴────────────────────────┴─────────────────────────────────┴──────────┘
```

**Severity Levels:**
- **CRITICAL** — ECU never slept → guaranteed battery drain source
- **WARNING** — ECU slept late → intermittent drain, may pass/fail depending on conditions

### 5. Root Cause Summary (Bottom Red Box)

Provides:
- List of all offending ECUs with their specific issue
- Recommended next steps for investigation

---

## How to Analyze Results — Step by Step

### Step 1: Check the Summary Cards
- If **Violating ECUs = 0** → All clear, no drain issue from NM
- If **Violating ECUs > 0** → Continue to Step 2

### Step 2: Look at the Timeline
- Identify ECUs with long red bars (post key-off awake time)
- Look for "STUCK" labels — these are your primary suspects

### Step 3: Review the Detail Table
- Sort mentally by **Status** column
- Focus on `STUCK AWAKE` first, then `VIOLATION`
- Note the **Messages After Key-Off** count — high numbers indicate active communication

### Step 4: Cross-Reference Violations
- Check which ECUs are CRITICAL
- These are your **root cause candidates** for battery drain

### Step 5: Determine Root Cause Category

| Pattern | Likely Root Cause |
|---------|-------------------|
| Single ECU stuck | Pending diagnostic session, incomplete OTA, or stuck NM state machine |
| Multiple related ECUs stuck | Partial network awake (one ECU keeping others awake via NM ring) |
| ECU sleeps then re-wakes | External wakeup source (LIN sub-bus, hardwired wakeup line, sensor) |
| All ECUs slow but eventually sleep | NM timing parameters too conservative, or gateway holding ring |
| Random ECU stuck each test | Software bug in NM stack — race condition |

### Step 6: Export & Report
- Click **"Export CSV"** for data to attach to JIRA/issue tracker
- Click **"Print Report"** for a clean printout to share in meetings

---

## Real-World Investigation Tips

1. **Correlate with current measurement** — If you have parasitic current data, overlay timestamps
2. **Check NM ring dependencies** — One stuck ECU can keep its NM cluster awake
3. **Look at diagnostic sessions** — UDS 0x10 02 (extended session) prevents sleep
4. **Check for OTA/FOTA activity** — Download managers hold bus awake
5. **Verify wakeup line routing** — Hardware wakeup pins can override NM sleep
6. **Test multiple key-off cycles** — Intermittent issues may not appear every time

---

## Expected Output for Each Test File

### File 01 — Normal Shutdown (PASS)
- All 8 ECUs show `NORMAL` status
- Timeline shows short red sections followed by green
- Zero violations
- **This is your baseline "healthy" reference**

### File 02 — Multiple Violators (FAIL)
- 15 ECUs total, 7 violations expected
- HVAC, TPMS, PDC → `VIOLATION` (slow sleep, 12-18s)
- SRS, PEPS, HUD, AMP → `STUCK AWAKE` (never sleep)
- Good test for team demo

### File 03 — Vector ASC Format
- Tests the tool's ability to parse real CAN trace format
- ECU_570 (EPS) and ECU_580 (ABS) should show as stuck
- Verifies .asc file compatibility

### File 04 — Edge Cases
- REWAKE_ECU: Sleeps then wakes again (tricky!)
- BORDERLINE_ECU: Sleeps at 11.999s (just over 10s limit)
- FLAPPING_ECU: Rapid state changes then stuck
- SINGLE_MSG_ECU: Only one post-key-off message
- Tests tool robustness

### File 05 — Real-World 25 ECUs
- Simulates a full vehicle network
- Mix of fast, normal, slow, and stuck ECUs
- Best for realistic demo to management

### File 06 — Stress Test 50 ECUs
- 2700+ log lines, 50 ECUs
- Tests performance with large datasets
- 15 violators across warning and critical
- Use to verify tool handles scale

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| "No valid log entries found" | Check format matches CSV or ASC. First line should be header or data. |
| All ECUs show as "Awake" | Verify your log contains "Sleep" state entries |
| Incorrect violation count | Check Key-Off timestamp matches your log's actual key-off time |
| ASC file not parsing | Ensure it's a standard Vector CANalyzer .asc export with hex timestamps |
