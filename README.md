# Adversary Emulation & Detection Validation Lab

A home lab that simulates real-world cyberattacks using MITRE Caldera and measures detection coverage using Splunk as the SIEM.

---

## Architecture

![Lab Architecture](lab-architecture.png)

---

## Tools

| Tool | Role | Version |
|------|------|---------|
| MITRE Caldera | Adversary emulation / C2 | 5.3.0 |
| Splunk Enterprise | SIEM / log analysis | Free |
| Sysmon | Endpoint telemetry | v15.20 |
| Windows 10 | Victim VM | 22H2 |
| ATT&CK Navigator | Coverage heatmap | v4.x |

---

## Lab Setup

See [`lab-setup/`](lab-setup/) for full configuration files and setup instructions.

**Quick Overview:**
1. Deploy Caldera server on Kali Linux VM
2. Deploy Windows 10 victim VM on isolated Host-Only network
3. Install Sysmon v15.20 with SwiftOnSecurity config on victim
4. Configure Splunk inputs.conf to ingest Windows and Sysmon logs
5. Deploy Sandcat agent on victim VM
6. Import ATT&CK Navigator layer for coverage tracking

---

## Operations

Each folder under [`operations/`](operations/) documents a full emulation run:

| Operation | Threat Group | Techniques | Status |
|-----------|-------------|------------|--------|
| [APT29 - Op1](operations/apt29-op1/) | APT29 (Cozy Bear) | T1003, T1053, T1057, T1059 | ✅ Completed |

---

## Detections

SPL queries for each ATT&CK technique are in [`detections/`](detections/).

| Technique | ID | Detection File | Coverage |
|-----------|-----|---------------|---------|
| Credential Dumping (NPPSpy + cmdkey) | T1003 | [credential-dumping.spl](detections/credential-dumping.spl) | ✅ Detected |
| Scheduled Task Persistence | T1053.005 | [persistence.spl](detections/persistence.spl) | ✅ Detected |
| Process Discovery | T1057 | [lateral-movement.spl](detections/lateral-movement.spl) | ✅ Detected |
| PowerShell Execution Bypass | T1059.001 | [powershell-execution.spl](detections/powershell-execution.spl) | ✅ Detected |

---

## ATT&CK Coverage

The [`navigator/`](navigator/) folder contains the layer JSON file for ATT&CK Navigator.

**Color key:**
- 🟢 Green — Fully detected and alerted
- 🟡 Yellow — Partial detection
- 🔴 Red — Technique executed, not detected

---

## Reports

- [Executive Summary](reports/executive-summary.md)

---

## Skills Demonstrated

- MITRE ATT&CK framework — TTP mapping and ATT&CK Navigator
- SIEM detection engineering — Splunk SPL query writing
- Endpoint telemetry — Sysmon configuration and log analysis
- Adversary emulation — MITRE Caldera C2 operations
- Blue team and incident response workflows
- Lab documentation and structured threat reporting
