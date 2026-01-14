# CrowdStrike LQL Hunting Pack

**Author:** Ala Dabat  
**Platform:** CrowdStrike Falcon (LogScale / LQL)  
**Audience:** SOC Analysts, Threat Hunters, Detection Engineers  
**Focus:** Practical, production-aligned hunting for LOLBin abuse and attacker tradecraft within CrowdStrike telemetry constraints
> **Note on testing / scope**
>
> This repo is a **portable LQL hunting pack** assembled from my prior operational hunting work and retained rule templates.  
> I currently **do not have access to a Falcon tenant for end-to-end validation**, so queries here should be treated as **Falcon-ready starting points** that require:
> - tenant-specific field verification (e.g., `ProcessRollup2` coverage and CommandLine population)
> - environment baselining (7–14 days)
> - tuning/allow-listing for common admin tooling
>
> The value of this pack is the **tradecraft logic** (what to look for and why), plus a clear validation/tuning workflow to operationalise it quickly once access is available.
---

## Overview

This repository contains a **curated set of CrowdStrike LQL threat hunts** focused on **living-off-the-land abuse**, **PowerShell and scripting misuse**, **payload ingress**, and **context reconstruction for incomplete EDR alerts**.

These hunts are intentionally scoped to reflect:
- How **real attackers** abuse native Windows tooling
- The **telemetry and schema realities** of CrowdStrike Falcon
- The role of LQL as a **triage and investigative language**, not a full detection engineering platform

This pack is designed to be:
- **Readable and reviewable in GitHub**
- **Directly usable in Falcon LogScale**
- **Operationally realistic for SOC environments**
- **Explainable in interviews, playbooks, and case reviews**

---

## Detection Architecture Context

CrowdStrike LQL is used here as a **scoping and pivoting layer**.

These hunts intentionally focus on:
- High-signal LOLBins (`powershell.exe`, `cmd.exe`, `rundll32.exe`, `mshta.exe`, etc.)
- Command-line intent and abuse patterns
- Parent/child relationships where Falcon telemetry is reliable
- Analyst-driven reconstruction of execution context when alerts lack full lineage

**Complex attack-chain correlation, scoring, and lifecycle-level detection logic are intentionally implemented outside the EDR layer** (e.g. SIEM / data-lake platforms), where telemetry depth and cross-domain reasoning are available.

This separation reflects how **mature SOC and detection architectures operate in production**.

---

## Design Philosophy

### Behaviour > Signatures

- No static hash or IOC dependency
- Focus on **execution patterns**, **command-line semantics**, and **process context**
- Resilient to payload mutation, encoding, and polymorphism

### Noise-aware by design

- Explicit parent process exclusions
- User-writable path bias
- Aggregation and rarity logic where applicable
- Rules intended to surface **actionable signal**, not alert volume

### Platform-conscious implementation

- Rules are written to **fit Falcon’s event model**, not fight it
- No attempt to force long attack chains or scoring logic into LQL
- Hunts assume analyst reasoning and follow-on pivoting

---

## Contents

This pack currently includes hunts for:

- **LOLBins abuse**
  - PowerShell execution patterns
  - Cmd.exe misuse
  - Script host and binary proxy execution

- **PowerShell tradecraft**
  - Encoded and obfuscated command lines
  - Execution context analysis
  - Inline Base64 decoding for analyst visibility

- **Alert context reconstruction**
  - Pivoting from web, script, or partial alerts
  - Identifying originating files (e.g. LNK, script, dropped payloads)
  - Timeline expansion for investigation write-ups

---

## Intended Use

These hunts are designed for:
- **Threat hunting**
- **Alert enrichment**
- **Incident investigation**
- **SOC training and analyst enablement**

They are **not** intended to replace:
- SIEM-based correlation
- Detection engineering pipelines
- Prevention or policy logic

Instead, they complement those layers by providing **fast, high-signal endpoint visibility** where Falcon excels.

---

## Related Work

For full **behavioral detection engineering**, **attack-chain modeling**, and **scoring-based logic**, see my primary research and KQL-based work in other repositories on this profile.

This repository exists to demonstrate **effective CrowdStrike operation**, not to reimplement SIEM functionality inside an EDR.

---

## Final Note

CrowdStrike is a powerful EDR for **endpoint visibility and response**.  
These hunts are written with that reality in mind.

They prioritise **clarity, operational usefulness, and analyst effectiveness** over theoretical completeness — exactly as required in real SOC environments.
