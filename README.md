# CAN Bus Spoofing Diagnostic Tool

## Executive Summary
This tool automates the detection of spoofed or injected CAN messages by analyzing timing patterns and frequency anomalies, enabling faster root-cause analysis for cybersecurity intrusion detection on vehicle networks.

## Problem Overview
- Malicious actors inject high-frequency CAN messages to override legitimate ECU signals.
- Manual review of large log files to find microsecond timing violations is slow and error-prone.

## Solution Overview
- Automated log analysis for ASC/CSV/TXT input.
- Message classification: **NORMAL**, **SUSPICIOUS**, **SPOOFED**.
- Timeline evidence of injection attacks.

## Analysis Workflow
1. Load a log file.
2. Run analysis.
3. Review violations and export results.
