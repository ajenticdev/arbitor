---
truth_dimension: permanent
authority_level: normative
document_role: architecture_contract
mutation_policy: reviewed_change_only
---

# Arbitor Architecture

## Purpose

Arbitor is a cross-platform security analysis suite for Security Operations Center (SOC) analysts, incident responders, and IT security professionals.

Arbitor ingests heterogeneous security logs, preserves raw evidence, normalizes records into a common event model, correlates activity across sources, applies indicator and behavior-based detections, supports non-destructive investigation workflows, and generates evidence-backed reports for SOC leadership and technical responders.

Arbitor is designed as a local-first, defensive, evidence-preserving analysis system suitable for restricted and high-sensitivity environments.

Arbitor is not a general-purpose Security Information and Event Management (SIEM) replacement, endpoint detection and response platform, vulnerability scanner, malware sandbox, or autonomous response system.

## Architectural Goal

The primary architectural goal is to preserve source evidence while enabling structured, explainable, and auditable analysis.

The system must allow analysts to answer:

- What happened?
- Which source records support that conclusion?
- Which users, assets, indicators, and techniques are involved?
- Which analysis logic produced the finding?
- Which findings were generated automatically?
- Which findings were reviewed, modified, promoted, or suppressed by an analyst?
- Which reports or exports used the finding?
- Which assumptions, limitations, and evidence gaps remain?

Arbitor must treat every security conclusion as evidence-bound. If a conclusion cannot be traced to source evidence or analyst-approved notes, it must not become an authoritative finding or report statement.

## Security posture

Arbitor must be built with a defensive programming mindset suitable for restricted and high-sensitivity environments.

The system must assume:

- input may be malformed, malicious, oversized, truncated, encoded unexpectedly, or intentionally crafted to exploit parsers
- logs may contain sensitive operational data, credentials, tokens, internal hostnames, usernames, IP addresses, vulnerability details, mission context, or Controlled Unclassified Information (CUI)
- deployment environments may be disconnected, firewalled, monitored, locked down, or prohibited from reaching the public internet
- dependencies may introduce supply-chain risk
- network behavior may require authorization, documentation, and audit
- exports may become official incident records or evidence packages
- automated analysis may be wrong unless tied to evidence and reviewed

The default posture is:

- deny by default
- local first
- least privilege
- no hidden egress
- explicit enablement
- auditable behavior
- fail-closed validation
- immutable evidence preservation
- explainable findings
- restricted-network deployability

Arbitor must not rely on operator caution as its primary security control.

## Technology Decision

Arbitor uses a Rust authoritative core with a TypeScript user interface delivered through Tauri.

Rust owns authority.

TypeScript owns presentation.

Tauri owns desktop packaging and the controlled inter-process communication boundary between the TypeScript interface and the Rust core.

This architecture is selected to reduce runtime attack surface, preserve strong authority separation, support local-first operation, and allow cross-platform deployment on Windows, Linux, and macOS.

## Language authority model

### Rust

Rust is the authoritative implementation layer.

Rust owns:

- ingestion
- parser contracts
- parsing
- defensive input validation
- normalization
- schema validation
- entity extraction
- query execution
- non-destructive filtering
- correlation
- detection logic
- indicator matching
- technique mapping
- finding generation
- finding lifecycle enforcement
- evidence storage
- audit logging
- report assembly
- export generation
- cryptographic operations
- secret access paths
- local security policy enforcement
- Tauri command validation
- restricted-network behavior enforcement

Rust is the only layer that may mutate authoritative investigation state.

Rust code that handles untrusted input must be bounded, validated, structured, and fail-closed.

Unsafe Rust is prohibited unless separately justified, isolated, reviewed, and tested.

### TypeScript

TypeScript is the visibility and operator workflow layer.

TypeScript owns:

- dashboard layout
- investigation workspace views
- filter builder views
- event table rendering
- correlation graph visualization
- finding review workflows
- report preview
- analyst annotations submitted through approved commands
- user preference display
- non-authoritative UI state

TypeScript must not:

- parse raw logs
- create authoritative findings
- perform trusted correlation
- mutate evidence directly
- approve findings directly
- execute arbitrary commands
- hold long-lived secrets
- perform hidden network access
- make compliance claims
- bypass Rust validation
- become the source of record for investigation state

The UI may request actions through typed Rust commands. The Rust core decides whether the action is valid.

### Tauri

Tauri is the desktop shell and controlled bridge.

Tauri owns:

- desktop windowing
- platform packaging
- controlled command exposure
- platform integration
- file picker mediation
- application lifecycle integration

Tauri commands must be explicit, typed, allowlisted, and validated by Rust before execution.

No generic shell execution command may be exposed to the UI.

No Tauri command may accept arbitrary instructions from the UI and execute them as trusted operations.

No Tauri command may expose secrets, broad filesystem access, or uncontrolled network behavior.


Also patch the existing permanent truth documents with these sections.


## Error Logging Architecture

Arbitor must support structured local error logging.

Arbitor should support optional emission to external logging systems, including syslog servers and approved monitoring tools.

Error logging is distinct from audit records.

Audit records preserve authoritative security-relevant action history.

Error logs preserve runtime failure, warning, diagnostic, forwarding, and health information.

The Rust core owns authoritative logging behavior.

TypeScript may display logging status and submit typed logging configuration requests, but it must not directly emit authoritative application logs.

Remote log emission is outbound network behavior and must follow Arbitor network rules.

Remote log emission must be:

- disabled by default
- explicitly enabled
- configured through approved paths
- visible to the operator or administrator
- auditable
- bounded
- redacted before transmission
- compatible with offline operation

Logs must not expose secrets, raw credentials, unnecessary sensitive data, raw evidence, full report contents, or Controlled Unclassified Information (CUI) unless explicitly governed.

Logging failures must be surfaced locally where practical.

Failure to reach a remote logging destination must not disable local ingestion, local analysis, local filtering, local reporting, local evidence review, or local export.

## System Layers

```text
User
  |
  v
TypeScript UI
  |
  v
Tauri command boundary
  |
  v
Rust authoritative core
  |
  +--> defensive input validation
  +--> ingestion
  +--> parsing
  +--> normalization
  +--> entity extraction
  +--> query and filtering
  +--> correlation
  +--> detection
  +--> findings
  +--> analyst actions
  +--> reporting
  +--> export
  +--> audit
  +--> storage
  +--> secrets
  +--> security policy
```
