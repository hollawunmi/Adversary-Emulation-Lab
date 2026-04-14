# Lab Setup Guide

Step-by-step instructions to build the Adversary Emulation & Detection Validation Lab from scratch.

---

## Prerequisites

- Host machine with at least 16GB RAM and 100GB free disk
- VirtualBox or VMware Workstation
- Internet access for downloads

---

## Step 1 — Network Design

Create an **isolated host-only network** in VirtualBox/VMware so the victim VM cannot reach the internet but can communicate with Caldera and Splunk.

```
Network: 192.168.56.0/24
Caldera Server: 192.168.56.10
Windows Victim:  192.168.56.20
Splunk:          192.168.56.30  (or run on host)
```

---

## Step 2 — Deploy Caldera Server

**Option A: Docker (recommended)**
```bash
git clone https://github.com/mitre/caldera.git --recursive
cd caldera
docker-compose up -d
```

**Option B: Python**
```bash
git clone https://github.com/mitre/caldera.git --recursive
cd caldera
pip3 install -r requirements.txt
python3 server.py --insecure
```

Access the UI at: `http://192.168.56.10:8888`
Default creds: `admin / admin`

Enable plugins: `response`, `stockpile`, `compass`

---

## Step 3 — Set Up Windows 10 Victim VM

1. Install Windows 10 22H2 (evaluation ISO from Microsoft)
2. Assign static IP: `192.168.56.20`
3. Disable Windows Defender (for lab purposes)
4. Install Sysmon (see Step 4)
5. Configure WEF (see Step 5)
6. Deploy Caldera Sandcat agent (see Step 6)

---

## Step 4 — Install Sysmon

Download Sysmon from Sysinternals. Use the config in this repo:

```powershell
# Run as Administrator on the victim VM
Invoke-WebRequest -Uri "https://download.sysinternals.com/files/Sysmon.zip" -OutFile Sysmon.zip
Expand-Archive Sysmon.zip -DestinationPath C:\Sysmon
C:\Sysmon\Sysmon64.exe -accepteula -i C:\sysmon-config.xml
```

Copy `sysmon-config.xml` from this repo to the victim VM before running the above.

Verify Sysmon is running:
```powershell
Get-Service Sysmon64
```

---

## Step 5 — Configure Windows Event Forwarding (WEF)

On the victim VM, configure WEF to ship logs to Splunk's Universal Forwarder.

**Install Splunk Universal Forwarder on victim VM:**
```powershell
# Download from splunk.com, then:
msiexec.exe /i splunkforwarder.msi RECEIVING_INDEXER="192.168.56.30:9997" /quiet
```

Copy `splunk-inputs.conf` from this repo to:
`C:\Program Files\SplunkUniversalForwarder\etc\system\local\inputs.conf`

Restart the forwarder:
```powershell
Restart-Service SplunkForwarder
```

---

## Step 6 — Deploy Caldera Agent (Sandcat)

In Caldera UI: **Agents > Deploy Agent > choose Windows**

Copy the generated PowerShell one-liner and run it on the victim VM as Administrator.

Verify the agent appears in Caldera UI under **Agents**.

---

## Step 7 — Configure Splunk

1. Install Splunk Free on a Linux VM or the host machine
2. Enable receiving on port 9997: **Settings > Forwarding and Receiving**
3. Create an index called `windows`
4. Install the **Splunk Add-on for Microsoft Sysmon** from Splunkbase
5. Verify events are flowing: search `index=windows` in Splunk

---

## Step 8 — Set Up ATT&CK Navigator

Open ATT&CK Navigator in your browser:
`https://mitre-attack.github.io/attack-navigator/`

Import the layer file from `navigator/coverage-layer.json` to see your technique coverage.

---

## Validation Checklist

- [ ] Caldera UI accessible
- [ ] Sandcat agent checked in to Caldera
- [ ] Sysmon service running on victim
- [ ] Splunk receiving events (`index=windows` returns results)
- [ ] ATT&CK Navigator layer loaded
