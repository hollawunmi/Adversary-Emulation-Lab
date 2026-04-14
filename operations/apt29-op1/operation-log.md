# Operation Log — APT29 Emulation (Op 1)

**Threat Group:** APT29 (Cozy Bear)  
**Date:** <!-- fill in when you run -->  
**Caldera Operation Name:** apt29-op1  
**Operator:** <!-- your name -->  
**Victim Host:** Windows 10 VM (192.168.56.20)  
**Caldera Server:** 192.168.56.10:8888  
**Objective:** Simulate APT29 TTPs and measure Splunk detection coverage before and after tuning.

---

## Pre-Operation Checklist

- [ ] Caldera server running and accessible
- [ ] Sandcat agent checked in on victim
- [ ] Sysmon running on victim (`Get-Service Sysmon64`)
- [ ] Splunk receiving events (`index=windows` returns results)
- [ ] Snapshot taken of victim VM (clean state)

---

## Techniques Executed

| # | Technique | ATT&CK ID | Caldera Ability | Result |
|---|-----------|-----------|----------------|--------|
| 1 | PowerShell execution | T1059.001 | | |
| 2 | LSASS memory dump | T1003.001 | | |
| 3 | Scheduled task persistence | T1053.005 | | |
| 4 | Registry run key | T1547.001 | | |
| 5 | WMI remote execution | T1047 | | |
| 6 | SMB lateral movement | T1021.002 | | |
| 7 | Network discovery | T1046 | | |
| 8 | Account discovery | T1087 | | |

---

## Pre-Tuning Detection Results

*Run the operation, then check Splunk. Fill in what fired.*

| Technique | ATT&CK ID | Alert Fired? | Alert Name | Notes |
|-----------|-----------|-------------|------------|-------|
| PowerShell execution | T1059.001 | | | |
| LSASS memory dump | T1003.001 | | | |
| Scheduled task | T1053.005 | | | |
| Registry run key | T1547.001 | | | |
| WMI execution | T1047 | | | |
| SMB lateral movement | T1021.002 | | | |
| Network discovery | T1046 | | | |
| Account discovery | T1087 | | | |

**Coverage Before:** __ / 8 techniques detected (__ %)

---

## Detection Gaps & Tuning Actions

*Document what missed and what you changed.*

| Gap | Root Cause | Fix Applied |
|-----|-----------|-------------|
| | | |
| | | |

---

## Post-Tuning Detection Results

*Re-run operation after tuning SPL queries / Sysmon config.*

| Technique | ATT&CK ID | Alert Fired? | Alert Name |
|-----------|-----------|-------------|------------|
| PowerShell execution | T1059.001 | | |
| LSASS memory dump | T1003.001 | | |
| Scheduled task | T1053.005 | | |
| Registry run key | T1547.001 | | |
| WMI execution | T1047 | | |
| SMB lateral movement | T1021.002 | | |
| Network discovery | T1046 | | |
| Account discovery | T1087 | | |

**Coverage After:** __ / 8 techniques detected (__ %)  
**Coverage Delta:** +__ techniques detected after tuning

---

## Screenshots

- `before-coverage.png` — ATT&CK Navigator heatmap before tuning
- `after-coverage.png` — ATT&CK Navigator heatmap after tuning

---

## Key Findings

<!-- What did you learn? What surprised you? -->

1. 
2. 
3. 

---

## SPL Queries Used

See [`../../detections/`](../../detections/) for all SPL queries referenced in this operation.
