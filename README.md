# CrowdStrike LQL Hunting Pack

**Author:** Ala Dabat  
**Platform:** CrowdStrike Falcon (LogScale / LQL)  
**Audience:** SOC Analysts, Threat Hunters, Detection Engineers  
**Focus:** Behavioural detection of real-world attacker tradecraft using LOLBins, scripting engines, ingress tools, and persistence mechanisms.

---

## Overview

This repository contains a **curated set of production-tested CrowdStrike LQL threat hunts** focused on **living-off-the-land abuse**, **fileless execution**, **payload ingress**, **persistence**, and **data exfiltration**.

These rules are not theoretical. They are written to reflect:
- How attackers *actually* operate in enterprise environments
- The realities of Falcon telemetry
- The balance between **signal fidelity** and **operational noise**

The pack is designed to be:
- **Readable in GitHub** (no proprietary file formats)
- **Directly usable in Falcon LogScale**
- **Explainable in interviews and SOC reviews**

---

## Design Philosophy

**Behaviour > Signatures**

- No hash or IOC dependency
- Focus on *execution patterns*, *command-line intent*, and *process relationships*
- Resistant to payload mutation, encryption, and staging changes

**Noise-aware by default**

- Explicit parent process exclusions
- Host-count and event-count aggregation for triage
- Avoids alert spam from vulnerability scanners and management agents

**SOC-first output**

- Grouped results (not single-event spam)
- Context-rich fields: parent process, command line, host count
- Easy pivoting for investigations

---

## Repository Structure

Each file is **numbered intentionally** so the repo reads like a hunting playbook, from initial execution → persistence → exfiltration.

> Files are stored as `.txt` for maximum GitHub readability (CrowdStrike does not require a `.lql` extension).

---

## Hunt Catalogue

### 01 — PowerShell Advanced Hunt (King Rule)
**`01_PowerShell_Advanced_Hunt.txt`**

Detects:
- Encoded commands (`-enc`)
- `IEX` / `Invoke-Expression`
- In-memory loaders
- Web-based payload staging
- AMSI tampering
- Office → PowerShell execution chains

This is the core rule that surfaces the majority of modern fileless intrusions.

---

### 02 — CertUtil Ingress & Decode
**`02_CertUtil_Ingress_Decode.txt`**

Detects:
- `certutil -urlcache` downloads
- Encode/decode abuse
- Binary and script reconstruction on disk

Commonly abused by APT and commodity malware alike.

---

### 03 — MSHTA Proxy Execution
**`03_Mshta_Proxy_Execution.txt`**

Detects:
- `javascript:` / `vbscript:` protocol handlers
- Remote HTA execution
- Temp-based staging

Classic LOLBin execution proxy.

---

### 04 — Regsvr32 Squiblydoo
**`04_Regsvr32_Squiblydoo.txt`**

Detects:
- `/i:` remote scriptlet loading
- `scrobj.dll` abuse
- SCT / HTA execution via regsvr32

High-signal and still effective.

---

### 05 — Rundll32 Ordinal & WebDAV Abuse
**`05_Rundll32_Ordinal_WebDav.txt`**

Detects:
- Ordinal execution (`#,`)
- WebDAV cookie abuse
- Script protocol execution

Used heavily in bypass chains.

---

### 06 — Persistence Pack (Tasks & Services)
**`06_Persistence_Pack.txt`**

Detects:
- Scheduled task creation using interpreters
- Service creation pointing to LOLBins
- Script-based persistence

Covers multiple ATT&CK persistence techniques in one view.

---

### 07 — Script Hosts (WScript / CScript)
**`07_Scripting_Hosts.txt`**

Detects:
- Script execution from the internet
- `ADODB.Stream`, `ActiveXObject`
- Temp and web-based staging

Common in phishing and initial access.

---

### 08 — Bitsadmin Transfer Abuse
**`08_Bitsadmin_Transfer.txt`**

Detects:
- `/transfer` and `/addfile`
- Binary and script ingress
- Silent background downloads

Still widely abused despite deprecation.

---

### 09 — MSIExec Remote Install
**`09_Msiexec_Remote.txt`**

Detects:
- MSI installs from URLs
- Browser-launched MSI abuse
- Temp-based installer execution

Seen in both malware and hands-on-keyboard attacks.

---

### 10 — FTP Automated Exfiltration
**`10_Ftp_Exfil.txt`**

Detects:
- Scripted FTP sessions
- Automated `put` / `mput`
- Credential and data exfiltration patterns

Low noise, high intent.

---

### 11 — Registry Run-Key Persistence (Advanced)
**`11_Registry_Persistence_RunKeys.txt`**

Detects:
- Obfuscated Run / RunOnce entries
- Encoded commands
- Script-based autoruns
- Network-based persistence

This is a **complex, high-fidelity registry hunt** with carefully tuned exclusions.

---

## MITRE ATT&CK Coverage (High-Level)

- **TA0001** Initial Access  
- **TA0002** Execution  
- **TA0003** Persistence  
- **TA0005** Defense Evasion  
- **TA0006** Credential Access  
- **TA0010** Exfiltration  

Mapped implicitly through behaviour rather than explicit tagging.

---

## How to Use This Repo

1. Copy a hunt into **CrowdStrike Falcon LogScale**
2. Run as:
   - Ad-hoc threat hunt
   - Scheduled search
3. Tune:
   - Parent process exclusions
   - Host count thresholds
4. Promote high-signal hunts to detections if desired

---

## Why This Repo Exists

This repository demonstrates:
- Real-world detection engineering skill
- Deep understanding of Windows tradecraft
- Practical SOC experience with CrowdStrike telemetry
- The ability to explain *why* something is malicious, not just that it is

These are the kinds of hunts that catch attackers **before** they escalate.

---

## Disclaimer

All queries are provided for **defensive and educational purposes only**.  
Tune appropriately for your environment before production use.

---
