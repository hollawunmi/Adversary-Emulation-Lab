# Adversary Emulation Lab — Executive Summary

**Author:** Segun Olawunmi
**Date:** April 2026
**GitHub:** https://github.com/hollawunmi/Adversary-Emulation-Lab

---

## Project Overview

Built a home adversary emulation lab to simulate real-world APT attacks and
validate detection coverage using a SIEM. The lab emulates APT29 (Cozy Bear)
— a Russian nation-state threat group known for targeting government and
enterprise environments.

The goal was to answer a core question every SOC team faces:
**"If this attack happened right now, would we detect it?"**

---

## Lab Architecture

```
Windows 11 Host
├── Kali Linux VM — MITRE Caldera 5.3.0 C2 server (attacker)
│   └── IP: 192.168.56.101
└── Windows 10 VM — Victim + Splunk SIEM (defender)
    ├── Sysmon v15.20 (endpoint telemetry)
    ├── Splunk Enterprise Free (log analysis)
    └── IP: 192.168.56.102
```

Network: Host-Only adapter (fully isolated, no internet exposure)

---

## Methodology

1. **Built detection stack** — installed Sysmon with SwiftOnSecurity config
   and configured Splunk to ingest Windows and Sysmon event logs
2. **Deployed C2 framework** — installed MITRE Caldera 5.3.0 on Kali Linux
3. **Deployed agent** — deployed Sandcat agent on Windows victim VM
   with elevated privileges
4. **Emulated APT29** — built adversary profile with 8 TTPs mapped to
   MITRE ATT&CK framework
5. **Ran operation** — executed full APT29 operation against Windows victim
6. **Detected and analyzed** — wrote Splunk SPL detection rules and measured
   coverage against executed techniques

---

## APT29 Techniques Emulated

| Technique | ATT&CK ID | Description |
|-----------|-----------|-------------|
| PowerShell Execution Policy Bypass | T1059.001 | Disabled PS restrictions to run scripts |
| Credential Dumping via NPPSpy | T1003 | Installed credential harvester DLL |
| Credential Listing via cmdkey | T1003 | Enumerated saved Windows credentials |
| Scheduled Task Persistence | T1053.005 | Created hidden scheduled task via XML |
| System Network Config Discovery | T1016 | Enumerated network configuration |
| System Information Discovery | T1082 | Collected host system information |
| Process Discovery via Get-Process | T1057 | Listed running processes via PowerShell |
| Process Discovery via tasklist | T1057 | Listed running processes via CMD |

---

## Detection Rules Written

### Rule 1 — PowerShell Execution Policy Bypass (T1059.001)
**Logic:** Alert when powershell.exe is launched with `-ExecutionPolicy Bypass`
**Severity:** High
**Result:** 5 events detected ✅

### Rule 2 — Credential Dumping (T1003)
**Logic:** Alert when NPPSpy or cmdkey appears in command line arguments
**Severity:** Critical
**Result:** 2 events detected ✅

### Rule 3 — Scheduled Task Persistence (T1053.005)
**Logic:** Alert when PowerShell uses RegisterByXml or TaskScheduler namespace
**Severity:** High
**Result:** 1 event detected ✅

### Rule 4 — Process Discovery (T1057)
**Logic:** Alert when tasklist or Get-Process is executed
**Severity:** Medium
**Result:** 2 events detected ✅

---

## Detection Coverage Results

| Metric | Value |
|--------|-------|
| TTPs emulated | 8 |
| TTPs detected | 4 |
| Detection coverage | 57% |
| Total Sysmon events captured | 32,000+ |
| Detection rules written | 4 |

---

## Key Findings

**What worked well:**
- Sysmon captured every command Caldera executed with full command line logging
- PowerShell-based attacks were highly visible in logs
- Credential harvesting left clear forensic artifacts in Sysmon EventID 1

**Detection gaps identified:**
- Network discovery techniques (T1016, T1082) were not detected — need
  additional rules targeting specific system commands like `ipconfig` and `systeminfo`
- No lateral movement detection rules written yet — Phase 2 of the project

**Realistic attacker behavior observed:**
- Windows Defender detected and quarantined the Sandcat agent on first deployment
  (flagged as Trojan:Win64/SandCat.RTSIMTB) — real APT29 uses signed binaries
  and living-off-the-land techniques to avoid this

---

## Skills Demonstrated

- MITRE ATT&CK framework — mapping real attacks to technique IDs
- Adversary emulation — building and running realistic threat actor profiles
- SIEM detection engineering — writing Splunk SPL queries from scratch
- Endpoint telemetry — Sysmon configuration and log analysis
- Incident investigation — hunting through 32,000+ events to find attack activity
- Lab documentation — structured reporting of findings

---

## Next Steps (Phase 2)

- [ ] Add detection rules for T1016 and T1082 (system discovery)
- [ ] Run Phase 2 attacks using real Kali tools (nmap, hydra, netcat)
- [ ] Detect brute force via Windows Security Event ID 4625
- [ ] Detect reverse shell via Sysmon network connection events
- [ ] Build ATT&CK Navigator heatmap showing full coverage
- [ ] Improve coverage from 57% to 80%+

