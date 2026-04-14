# Executive Summary — Adversary Emulation & Detection Validation Lab

**Author:** <!-- your name -->  
**Date:** <!-- fill in -->  
**Lab Environment:** Windows 10 VM + Caldera C2 + Splunk Free + Sysmon  

---

## Objective

Simulate APT29 (Cozy Bear) TTPs against an isolated Windows 10 victim using MITRE Caldera, then measure and improve detection coverage in Splunk.

---

## Scope

**Threat Group Emulated:** APT29  
**Techniques Tested:** 8 ATT&CK techniques across 5 tactics  
**Detection Platform:** Splunk Free + Sysmon + Windows Event Forwarding  

| Tactic | Techniques Tested |
|--------|------------------|
| Execution | T1059.001 (PowerShell), T1047 (WMI) |
| Credential Access | T1003.001 (LSASS Dump) |
| Persistence | T1053.005 (Scheduled Task), T1547.001 (Registry Run Key) |
| Lateral Movement | T1021.002 (SMB/PsExec) |
| Discovery | T1046 (Network Scan), T1087 (Account Discovery) |

---

## Results Summary

| Phase | Techniques Detected | Coverage |
|-------|-------------------|----------|
| Before Tuning | __ / 8 | ___ % |
| After Tuning | __ / 8 | ___ % |
| **Delta** | **+__** | **+___ %** |

---

## Key Findings

### What Was Detected
<!-- List techniques that fired alerts before any tuning -->

### Detection Gaps Found
<!-- List what was missed and why -->

### Improvements Made
<!-- Describe the SPL tuning or Sysmon config changes you made -->

---

## ATT&CK Coverage Heatmap

See `navigator/coverage-layer.json` — import into [ATT&CK Navigator](https://mitre-attack.github.io/attack-navigator/) to view the heatmap.

- **Red** — Technique executed, not detected
- **Yellow** — Partial detection
- **Green** — Fully detected

---

## Detection Rules Developed

| Rule | ATT&CK ID | File | Fidelity |
|------|-----------|------|----------|
| LSASS Access Detection | T1003.001 | detections/credential-dumping.spl | High |
| Scheduled Task via CLI | T1053.005 | detections/persistence.spl | High |
| PowerShell Encoded Command | T1059.001 | detections/powershell-execution.spl | High |
| PsExec Lateral Movement | T1021.002 | detections/lateral-movement.spl | Medium |
| Registry Run Key | T1547.001 | detections/persistence.spl | High |

---

## Lessons Learned

1. <!-- What surprised you? -->
2. <!-- What would you do differently? -->
3. <!-- What's next to test? -->

---

## Next Steps

- [ ] Expand to additional APT29 techniques (T1078 Valid Accounts, T1027 Obfuscated Files)
- [ ] Add alert fatigue analysis (false positive rate)
- [ ] Test against Windows Defender enabled
- [ ] Build Splunk dashboard for real-time coverage view
