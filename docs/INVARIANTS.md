---
truth_dimension: permanent
authority_level: normative
document_role: invariant_catalog
mutation_policy: reviewed_change_only
---

# Arbitor Invariants

## Purpose

This document defines invariants for Arbitor.

An invariant is a rule that must remain true across implementation phases, refactors, UI changes, parser additions, detection additions, reporting changes, export changes, connector additions, and release hardening.

If an invariant is violated, the system must be treated as incorrect until the violation is resolved.

Invariants are permanent truth. They do not record completed work, implementation status, future plans, validation output, or release history.

Completed work belongs in `CHANGELOG.md`.

Planning truth belongs in `docs/roadmap/`.

## Invariant Scope

These invariants govern:

- authority boundaries
- evidence handling
- defensive input handling
- parser behavior
- schema behavior
- normalization
- filtering
- analysis
- correlation
- indicator matching
- finding lifecycle
- reporting
- exports
- audit records
- network behavior
- restricted environment operation
- secret handling
- cryptography
- dependency posture
- model and AI assistance boundaries
- compliance claim boundaries
- release readiness posture

No component may bypass these invariants by implementation convenience, UI need, integration pressure, dependency behavior, model output, or later phase expansion.

## I1. Rust Authority Invariant

Rust is the only authoritative runtime layer.

All authoritative state mutation must pass through approved Rust core paths.

TypeScript, plugins, scripts, imported templates, imported rules, imported indicators, report generators, and model outputs must not mutate authoritative state directly.

## I2. TypeScript Non-Authority Invariant

TypeScript is non-authoritative.

TypeScript may render state, collect operator input, display analysis results, preview reports, and submit typed requests to Rust.

TypeScript must not parse raw logs, create authoritative findings, approve findings, perform trusted correlation, mutate evidence, access secrets, execute shell commands, perform hidden network access, or become the source of record for investigation state.

## I3. Tauri Boundary Invariant

Tauri commands must be explicit, typed, allowlisted, and validated by Rust.

No generic command execution path may be exposed.

No command may accept unbounded raw instructions from the UI and execute them as trusted operations.

No command may expose secrets, broad filesystem access, uncontrolled network behavior, or unaudited state mutation.

## I4. State Mutation Invariant

Only approved Rust core paths may mutate authoritative state.

Authoritative state includes:

- raw evidence records
- normalized event records
- entity records
- indicator records
- correlation records
- findings
- finding lifecycle status
- analyst approvals
- analyst notes
- report records
- export records
- audit records
- security configuration
- connector configuration
- storage configuration
- secret metadata

State mutation must be typed, validated, auditable, and bounded.

## I5. Evidence Immutability Invariant

Raw evidence is append-only after ingestion.

Raw evidence must not be edited, rewritten, normalized in place, hidden, or deleted to support analysis.

The system must not overwrite raw evidence to make parsing, querying, reporting, or visualization easier.

Deletion, if supported later, must use a governed retention or purge workflow with audit records.

## I6. Source Preservation Invariant

The system must preserve source identity.

Every ingested record must retain source metadata where available, including:

- source identifier
- source type
- vendor
- product
- collection method
- ingest time
- parser attempted
- parser version
- parser result
- parser warnings or errors
- raw evidence reference
- raw evidence hash where practical

Source-specific records must not bypass the defensive input boundary.

## I7. Derived Record Traceability Invariant

Every normalized event, entity, indicator match, correlation, finding, report statement, and export must be traceable to source evidence or analyst-approved notes.

If traceability is incomplete, the record must disclose that limitation.

No derived record may sever source evidence references.

## I8. Defensive Input Invariant

All external input must be treated as hostile.

External input includes:

- log files
- exported Security Information and Event Management (SIEM) data
- firewall logs
- Windows Event Logs
- Linux syslog
- API responses
- Open Source Intelligence (OSINT) feeds
- configuration files
- imported rules
- imported indicators
- report templates
- analyst-provided text where it crosses an authority boundary

Input must be bounded, parsed through explicit schemas or parser contracts, validated before use, and rejected or quarantined on ambiguity where authority would be affected.

Malformed input must not crash Arbitor.

Malformed input must not silently produce authoritative findings.

## I9. Parser Fail-Closed Invariant

Parsers must be bounded, testable, and fail-closed.

A parser failure must produce a structured parser error.

A parser failure must not crash the application.

A parser failure must not create an authoritative finding.

A parser must not silently drop records unless the drop is recorded with a reason.

Parser output must not become authoritative until schema validation succeeds.

## I10. Parser Test Invariant

Every parser must have defensive test coverage.

Parser test coverage must include:

- valid input tests
- malformed input tests
- oversized input tests where practical
- encoding edge-case tests where practical
- golden output tests
- parser version behavior
- structured error output
- no silent record dropping

No parser is complete without malformed input coverage.

## I11. Schema Contract Invariant

Normalized records must conform to versioned schemas.

Schema changes must be explicit.

Schema changes must preserve migration, compatibility, or rejection behavior.

Schema validation must reject malformed records where authoritative behavior would otherwise be affected.

Schema contracts must not be bypassed by UI code, parser convenience paths, imported data, model output, or reporting templates.

## I12. Normalization Invariant

Normalization converts source-specific records into a common event model without destroying source context.

Normalization must preserve:

- original source type
- original vendor and product
- original timestamp where available
- normalized timestamp
- source record reference
- parser version
- parser warnings
- parse confidence
- raw evidence reference

Normalization must not convert uncertainty into certainty.

## I13. Entity Traceability Invariant

Every extracted entity must preserve references to the normalized events and evidence records from which it was derived.

Entity extraction must support unknown, partial, ambiguous, and conflicting values without forcing false certainty.

Entity canonicalization must not erase observed source values.

## I14. Non-Destructive Filtering Invariant

Filters create views over data.

Filters must not mutate:

- raw evidence
- normalized events
- entities
- indicators
- correlations
- findings
- reports
- exports
- audit records

Layered filters must be reversible unless explicitly saved as a named view.

Saved views must preserve query structure and scope.

## I15. Query Auditability Invariant

Queries that affect investigation workflow, saved views, finding generation, reporting, export, or analyst review must be auditable.

A query record must preserve enough information to reconstruct scope, filter logic, time window, and relevant source records where practical.

Query execution must not mutate underlying evidence.

## I16. Candidate Finding Invariant

Automated detections create candidate findings.

A candidate finding is not an approved conclusion.

Promotion to an approved finding requires a governed analyst action or a separately documented approval workflow.

Automated analysis must not bypass analyst review where review is required.

## I17. Finding Explainability Invariant

A finding must state why it exists.

At minimum, a finding must preserve:

- matched rule, indicator, behavior, or correlation logic
- supporting event references
- evidence references
- affected entities
- confidence value
- severity value where applicable
- detection timestamp
- analysis engine version
- limitations where known

A finding without supporting evidence references is invalid.

## I18. Finding Lifecycle Invariant

A finding begins as a candidate.

Allowed finding states are:

- candidate
- under review
- approved
- suppressed
- merged
- exported
- reopened

Only approved workflows may change finding state.

Every finding state transition must be auditable.

A finding must not be deleted to hide prior analysis.

Suppression must be auditable.

## I19. Analyst Action Invariant

Analyst actions are governed state transitions.

Analyst actions must preserve:

- operator identity where available
- action type
- timestamp
- target record
- before state reference where practical
- after state reference where practical
- reason where supplied

Analyst notes must remain distinguishable from automated analysis.

## I20. Correlation Non-Conclusion Invariant

Correlation does not equal confirmed malicious activity.

A correlation record may support a candidate finding, but it must not become an approved conclusion by itself.

Correlation records must preserve:

- correlation type
- time window where applicable
- supporting event references
- affected entity references
- analysis engine version
- confidence value
- limitations where known

## I21. Indicator Context Invariant

Indicator matching must preserve source context.

Indicator records must include:

- indicator type
- indicator value
- source name
- source version or retrieval time where available
- confidence where available
- license or handling metadata where available
- expiration or staleness metadata where available

Indicator matches must not automatically become approved findings.

## I22. Technique Mapping Invariant

Technique mappings are candidate classifications unless reviewed.

A technique mapping must include:

- framework or catalog name
- technique identifier where available
- technique name
- mapping reason
- confidence value
- supporting event or finding references

Technique mappings must not be used to inflate severity without evidence.

## I23. Confidence Invariant

Confidence describes evidentiary strength.

Confidence must remain distinct from severity.

Allowed confidence values are:

- low
- medium
- high

Confidence must not be increased solely because a report or interface needs stronger wording.

## I24. Severity Invariant

Severity describes potential operational impact.

Severity must remain distinct from confidence.

Allowed severity values are:

- informational
- low
- medium
- high
- critical

Severity must be based on impact, affected assets, privilege level, exploitability, exposure, and analyst context.

Severity must not be inflated only because an event matched an indicator.

## I25. Report Traceability Invariant

Reports must preserve traceability.

A report statement that asserts a fact about an incident must originate from evidence, a finding, a timeline record, or an analyst-approved note.

Generated prose must not invent unsupported facts.

Generated prose must not hide uncertainty where uncertainty is known.

## I26. Report Authorship Invariant

Report content must distinguish evidence-derived facts, automated draft text, and analyst-authored conclusions.

AI-generated or template-generated text is candidate text until accepted through a governed workflow where review is required.

Analyst edits must be preserved as analyst actions or analyst-authored content.

## I27. Export Integrity Invariant

Exports may become official incident records or evidence packages.

Export packages must include enough metadata to verify origin and integrity.

Export packages should include:

- export identifier
- report identifier
- export format
- creation time
- creator identity where available
- included finding references
- included evidence references
- included report sections
- redaction profile where applicable
- classification or handling marking where configured
- export hash where practical
- tool version
- source revision or build metadata where practical

Exports must not change evidence, findings, reports, or audit records.

## I28. Auditability Invariant

Security-relevant actions must produce append-only audit records.

Audit records must be sufficient to reconstruct what happened, when it happened, what record was affected, and what component or operator performed the action.

Audit records must not expose secrets.

Audit records must avoid raw sensitive data unless explicitly required and governed.

## I29. Audit Immutability Invariant

Audit records are append-only.

Audit records must not be edited to conceal, simplify, or retroactively reinterpret prior behavior.

Correction records may be appended where needed, but prior audit records must remain preserved unless governed retention or purge controls apply.

## I30. Local-First Invariant

Arbitor must operate locally by default.

Core capability must not require public internet access.

Local ingestion, local parsing, local normalization, local filtering, local analysis, local reporting, local evidence review, and local export must remain available without network access.

## I31. Restricted Network Invariant

Arbitor must be able to operate in restricted, offline, and no-internet environments.

Network-dependent features must be optional, explicitly enabled, visible to the operator, and auditable.

The absence of network access must not disable core analysis.

The application must not require public package registries, remote model APIs, online OSINT feeds, telemetry services, cloud services, or automatic update services for core analysis.

## I32. No Hidden Egress Invariant

Arbitor must not perform hidden outbound communication.

Outbound communication includes:

- telemetry
- analytics
- crash reporting
- update checks
- license checks
- OSINT retrieval
- model API calls
- dependency fetching
- remote enrichment
- cloud synchronization

Any outbound communication must be explicitly enabled, documented, visible to the operator, and recorded in audit logs.

## I33. Network Explicitness Invariant

Every network-capable feature must define:

- purpose
- destination type
- data sent
- data received
- credential use
- audit event
- failure behavior
- offline behavior
- configuration control

No network feature may be enabled silently.

## I34. Least Privilege Invariant

Arbitor must request the minimum local, network, and filesystem access required for the selected operation.

The application must not scan broad filesystem locations, enumerate unrelated directories, read arbitrary files, access credentials, or contact external systems without explicit operator action or governed configuration.

## I35. Sensitive Data Handling Invariant

Arbitor must assume ingested logs and generated reports may contain sensitive data.

Sensitive data includes:

- usernames
- internal IP addresses
- hostnames
- authentication records
- session identifiers
- tokens
- credentials
- vulnerability data
- incident details
- mission or operational context
- Controlled Unclassified Information (CUI), where applicable

Sensitive data must not be exposed through logs, telemetry, crash reports, debug output, screenshots, exports, or model prompts unless explicitly allowed by governed configuration.

Sensitive data handling must be conservative by default.

## I36. Secret Isolation Invariant

Secrets must not be stored in plaintext or exposed to non-authoritative layers.

Secrets include:

- API tokens
- passwords
- private keys
- client secrets
- refresh tokens
- signing keys
- encryption keys

Secrets must use approved storage and retrieval paths.

The UI may display secret presence, connector status, or validation result, but not secret value.

Secrets must not be written to logs, reports, exports, crash dumps, debug output, screenshots, or model prompts.

## I37. Cryptography Precision Invariant

Cryptographic claims must be precise.

Arbitor must not claim FIPS compliance unless the cryptographic modules used are FIPS-validated and operating in validated configurations.

Encryption, hashing, signing, and key storage must be implemented through reviewed libraries or operating system facilities.

Custom cryptography is prohibited.

## I38. Dependency Review Invariant

Dependencies are supply-chain risk.

Every dependency must have a clear purpose.

Before adding a dependency, evaluate:

- whether standard library functionality is sufficient
- license compatibility
- maintenance status
- known vulnerability history
- transitive dependency risk
- platform support
- whether it runs in the authoritative Rust core or non-authoritative UI
- whether it introduces network, filesystem, cryptographic, parsing, deserialization, or code execution behavior

Rust authority-layer dependencies require stricter review than UI-only dependencies.

No dependency may be introduced only for convenience in an authority-sensitive path without review.

## I39. Build Evidence Invariant

Release artifacts intended for restricted or government-adjacent environments must preserve build evidence.

Build evidence should include:

- version
- source revision
- build environment record
- dependency lockfiles
- software bill of materials where available
- signatures where supported
- checksums
- known limitations

Unsigned or unaudited builds must not be presented as production-ready.

## I40. Release Readiness Invariant

A release intended for restricted or government-adjacent environments must not proceed unless the repository can produce evidence for:

- dependency review
- vulnerability scan results
- parser test coverage
- command boundary review
- network behavior review
- secret handling review
- audit behavior review
- export behavior review
- compliance claim review
- known limitations

Release readiness does not imply certification, authorization, accreditation, or government approval.

## I41. Model Non-Authority Invariant

AI or language model output is candidate material.

Model output must not become authoritative unless accepted through a governed workflow.

Model output must not modify evidence, suppress findings, approve findings, alter configuration, access secrets, perform network actions, or create audit records on its own behalf.

## I42. AI Sensitive Data Invariant

AI assistance must not receive sensitive data unless explicitly enabled by governed configuration.

AI assistance must not use external model APIs by default.

AI-generated content must remain distinguishable from analyst-authored conclusions.

AI assistance must not hide uncertainty or remove contradictory evidence.

## I43. Compliance Precision Invariant

Compliance language must be precise.

Arbitor may claim support, mapping, or alignment only when the claim is accurate.

Arbitor must not claim certification, authorization, accreditation, approval, or compliance without formal evidence and approval.

Allowed language includes:

- designed to support
- mapped to
- aligned with
- provides evidence for
- supports workflows associated with

Restricted language unless formally validated includes:

- certified
- authorized
- DoD approved
- FIPS compliant
- NIST compliant
- CMMC compliant
- STIG compliant
- RMF approved

## I44. Compliance Boundary Invariant

Compliance support does not equal certification.

Arbitor may be designed to support security operations aligned with NIST, CISA, DoD Risk Management Framework (RMF), or DISA Security Technical Implementation Guide (STIG) expectations, but formal compliance status must be separately assessed and documented.

No documentation, UI text, report, export, or release note may imply government approval without formal authorization evidence.

## I45. Documentation Truth Invariant

Truth dimensions must not be mixed.

Permanent truth belongs in:

- `docs/GOVERNANCE.md`
- `docs/ARCHITECTURE.md`
- `docs/INVARIANTS.md`
- `docs/MODEL_DESIGN.md`
- `docs/SECURE_ENGINEERING.md`

Planning truth belongs in:

- `docs/roadmap/`
- phase plans
- milestone plans

Historical truth belongs in:

- `CHANGELOG.md`
- release records

Procedural truth belongs in:

- checklists
- runbooks
- validation procedures

Executable truth belongs in:

- source code
- schemas
- tests
- validation scripts
- CI workflows

## I46. Documentation Accuracy Invariant

Documentation must not claim implemented behavior unless executable truth supports it.

Documentation must not claim production readiness unless a readiness review supports it.

Documentation must not claim compliance, certification, authorization, accreditation, or government approval unless formal evidence supports it.

## I47. Phase Boundary Invariant

Each phase must preserve scope control.

A phase must not:

- mix unrelated architecture layers
- implement future phase behavior early
- claim compliance or production readiness without evidence
- introduce hidden network access
- introduce unaudited state mutation
- weaken evidence traceability
- bypass Rust authority
- let TypeScript mutate authoritative state
- let model output become authoritative without review

## I48. Standing Security Gate Invariant

No phase may introduce:

- hidden network access
- unaudited state mutation
- plaintext secret storage
- parser panic paths on malformed input
- UI authority expansion
- unreviewed dependency expansion
- unsupported compliance claims
- unsupported report claims
- unsupported finding promotion
- broad filesystem access without governed configuration
- secret exposure outside approved paths

## I49. Standing Evidence Gate Invariant

No phase may weaken:

- evidence immutability
- source preservation
- source traceability
- parser error visibility
- finding support references
- report provenance
- export integrity

## I50. Standing Analysis Gate Invariant

No phase may allow automated analysis to create approved conclusions without governed analyst review.

No phase may allow candidate findings to hide uncertainty, omit supporting evidence, or suppress contradictory records.

## I51. Standing Reporting Gate Invariant

No phase may allow report claims to detach from evidence, findings, timelines, or analyst-approved notes.

No phase may allow generated text to become authoritative without governed review where review is required.

## I52. Standing Restricted Environment Gate Invariant

No phase may break offline core operation.

Optional network features must remain disabled by default.

Core analysis must not require public internet access.

## I53. No Silent Authority Expansion Invariant

No component may gain authority by convenience, refactor, UI need, integration pressure, dependency behavior, model output, or later phase expansion.

Authority changes require explicit governance review.

Silent authority expansion is prohibited.

## I54. Governance Failure Invariant

A governance failure exists if any of the following occur:

- raw evidence can be mutated without audit
- TypeScript can mutate authoritative state directly
- model output can become authoritative without review
- compliance claims exceed evidence
- source records cannot be traced from findings
- report statements cannot be traced to evidence, findings, timelines, or analyst-approved notes
- parser failures are silent
- malformed input crashes the application
- network access occurs without explicit configuration
- hidden egress is introduced
- secrets are exposed outside approved paths
- historical, planning, and permanent truth are mixed
- dependencies are added without authority-sensitive review
- findings lack supporting evidence references
- exports lack integrity or provenance metadata where required
- audit records expose secrets
- restricted offline operation is broken by default

Governance failures must be corrected before production release.

## I55. Production-Candidate Bar Invariant

Arbitor must not be treated as a production candidate until the following are true:

- Rust authority boundary is implemented and tested
- raw evidence immutability is implemented and tested
- parser fail-closed behavior is implemented and tested
- normalized events preserve evidence references
- findings preserve supporting event and evidence references
- analyst review workflow exists for finding promotion
- reports preserve traceability
- exports include integrity metadata
- audit records exist for security-relevant actions
- hidden network access is absent
- restricted offline operation is validated
- dependency review exists
- release evidence package exists
- compliance claims are reviewed and bounded
- readiness audit is completed

Production-candidate status does not imply certification, authorization, accreditation, or government approval.

## I56. Error Logging Separation Invariant

Error logs and audit records are separate surfaces.

Audit records preserve authoritative security-relevant action history.

Error logs preserve runtime failures, warnings, diagnostics, forwarding status, and health information.

Error logs must not replace audit records.

Audit records must not be downgraded into ordinary logs.

## I57. Safe Logging Invariant

Logs must not expose secrets, raw credentials, unnecessary sensitive data, raw evidence, full report contents, or Controlled Unclassified Information (CUI) unless explicitly governed.

Sensitive values must be redacted, hashed, tokenized, or omitted before local write or external emission.

## I58. External Log Emission Invariant

External log emission, including syslog emission, is outbound network behavior.

External log emission must be disabled by default, explicitly enabled, visible, auditable, bounded, redacted, and compatible with offline operation.

Failure to reach a remote logging destination must not disable local ingestion, local analysis, local filtering, local reporting, local evidence review, or local export.

## Final Invariant

If a component cannot preserve authority boundaries, evidence traceability, restricted-environment operation, defensive input handling, and auditable behavior, it must not be added to Arbitor.
