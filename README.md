<div align="center">

# **Arbitor** ### **The SOC Analyst Suite**

<br />
  <p>
    <img src="https://img.shields.io/badge/Rust-black?style=for-the-badge&logo=rust" />
    <img src="https://img.shields.io/badge/TypeScript-blue?style=for-the-badge&logo=typescript" />
    <img src="https://img.shields.io/badge/Tauri-white?style=for-the-badge&logo=tauri&logoColor=24C8DB" />
    <img src="https://img.shields.io/badge/Cross--Platform-gray?style=for-the-badge" />
  </p>
</div>

---

## Overview

Arbitor is a **local‑first security analysis suite** built for SOC analysts, incident responders, and security leadership. It ingests telemetry from enterprise sources—including industry standard tools like: Splunk, Palo Alto Networks, Cisco, Fortinet, and system logs—normalizing disparate data into a unified, evidence‑preserving investigation workspace.

Designed for high-compliance environments (NIST, CISA, and DoD-aligned frameworks), Arbitor provides a controlled, offline-capable sandbox. This ensures sensitive investigative data remains on-premises, eliminating the risks inherent in third-party cloud processing or ungoverned automation.

Arbitor is a **precision investigation tool**, not a SIEM replacement. It is purpose-built to enhance an analyst's ability to reconstruct events, establish causality, and isolate the specific evidence required to support conclusive findings.

---

## Why IT Leaders Choose Arbitor

### 1. Built for NIST‑Aligned Security Operations

Arbitor supports workflows aligned with:

- NIST Cybersecurity Framework (CSF)  
- NIST SP 800‑53  
- NIST SP 800‑171 (CUI environments)  
- NIST SP 800‑218 (Secure Software Development Framework)  

Arbitor does not claim certification — instead, it provides **evidence‑backed investigation records** that help organizations meet audit, reporting, and incident‑response obligations.

---

### 2. CISA Secure‑by‑Design Principles, Implemented

Arbitor is engineered around CISA’s Secure‑by‑Design expectations:

- **Local‑first**: No hidden telemetry, no silent network access  
- **Evidence‑preserving**: Raw logs remain immutable  
- **Deterministic analysis**: No unreviewable automation  
- **Governed workflows**: Analysts approve findings, not models  
- **Strict authority boundaries**: Rust owns truth; UI cannot mutate state  

This makes Arbitor suitable for regulated, air‑gapped, or high‑assurance environments.

---

### 3. Evidence‑Preserving Investigation Model

Arbitor maintains a strict chain of custody:

- Raw evidence is **append‑only**  
- Normalized events retain references to source records  
- Correlations, detections, and findings are fully traceable  
- Reports link every conclusion to supporting evidence  

This ensures investigations remain defensible, auditable, and reviewable.

---

### 4. Multi‑Source Security Telemetry, Unified

Arbitor ingests:

- Splunk exports or API results  
- Palo Alto firewall logs  
- Cisco IOS / ASA / SD‑WAN logs  
- Symantec endpoint logs  
- Windows Event Logs  
- Linux syslog  
- Offline evidence files  

All sources are normalized into a **common event model**, enabling cross‑vendor correlation and unified analysis.

---

### 5. Local‑First, Zero‑Trust‑in‑Cloud

Arbitor runs entirely on the analyst’s workstation:

- No cloud dependencies  
- No external API calls unless explicitly enabled  
- No background telemetry  
- No automatic updates  
- No ungoverned model execution  

This reduces supply‑chain risk and supports environments with strict data‑sovereignty requirements.

---

### 6. Designed for Analyst Productivity

Arbitor provides:

- High‑performance Rust‑based correlation  
- Fast, local search and filtering  
- Entity graph visualization  
- Timeline reconstruction  
- Candidate finding generation  
- Analyst review workflows  
- Evidence‑backed reporting  

The UI is intentionally simple, predictable, and optimized for investigation flow.

---

## Technology Stack

Arbitor uses a **Rust authoritative core** with a **TypeScript UI** delivered through **Tauri**.

- **Rust** — ingestion, parsing, normalization, correlation, detection, findings, storage, audit  
- **TypeScript** — dashboards, views, workflows, visualization  
- **Tauri** — desktop shell, IPC boundary, packaging  
- **SQLite / content‑addressed storage** — local evidence and derived records  
- **Specta** — typed Rust→TypeScript IPC contracts  

For architectural details, see:  
- Architecture  
- Governance  
- Invariants  
- Model Design

---

## **Project Status**

Arbitor is under active development.  
All permanent‑truth documents define stable architecture and authority boundaries.  
Implementation progress is tracked in:

- `CHANGELOG.md` (historical truth)  
- `ROADMAP.md` (planning truth)
