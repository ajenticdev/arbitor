---
truth_dimension: permanent
authority_level: normative
document_role: governance_contract
mutation_policy: reviewed_change_only
---

# Arbitor Governance

## Purpose

This document defines the governance model for Arbitor.

Governance exists to prevent architectural drift, evidence corruption, unsafe automation, ambiguous compliance claims, hidden network behavior, weak parser boundaries, uncontrolled dependency expansion, and unreviewable analysis.

Arbitor is security software intended to process sensitive operational data. Its governance model must assume restricted environments, hostile input, strict audit expectations, and deployment contexts where government, defense, or high-sensitivity organizational requirements may apply.

This document is authoritative for repository-level governance unless superseded by a more specific governance document with equal or higher authority.

## Governance Scope

This document governs:

- authority boundaries
- truth dimensions
- evidence handling
- state mutation
- parser safety
- model and AI assistance boundaries
- reporting and export behavior
- network behavior
- secret handling
- dependency review
- build and release posture
- compliance claim language
- restricted environment requirements
- audit expectations
- repository placement rules
- governance failure conditions

This document does not record completed work.

Completed work belongs in `CHANGELOG.md`.

Planned work belongs in `docs/roadmap/` or another approved planning surface.

## Secure Defensive Engineering Doctrine

Arbitor must be built with a defensive programming mindset suitable for restricted and high-sensitivity environments.

The system must assume:

- input may be malformed, malicious, oversized, truncated, encoded unexpectedly, or intentionally crafted to exploit parsers
- logs may contain sensitive operational data, credentials, tokens, internal hostnames, usernames, IP addresses, vulnerability details, mission context, or Controlled Unclassified Information (CUI)
- deployment environments may be disconnected, firewalled, monitored, locked down, or prohibited from reaching the public internet
- dependencies may introduce supply-chain risk
- network behavior may require authorization, documentation, and audit
- exports may become official incident records or evidence packages
- automated analysis may be wrong unless tied to evidence and reviewed

The default security posture is:

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

## Truth Dimensions

Arbitor separates repository documents by truth dimension.

Truth dimensions must not be mixed.

### Permanent Truth

Permanent truth defines stable rules, invariants, authority boundaries, architecture contracts, and model contracts.

Examples:

- `docs/GOVERNANCE.md`
- `docs/ARCHITECTURE.md`
- `docs/INVARIANTS.md`
- `docs/MODEL_DESIGN.md`
- `docs/SECURE_ENGINEERING.md`

Permanent truth must not contain implementation status, phase history, release notes, validation output, or future plans.

### Historical Truth

Historical truth records completed changes.

Examples:

- `CHANGELOG.md`
- signed release records
- release notes where approved

Historical truth must not contain future plans, desired behavior, speculative features, unimplemented promises, or normative rules that belong in permanent truth.

### Planning Truth

Planning truth records intended future work.

Examples:

- `docs/roadmap/phase-map.md`
- phase plans
- milestone plans
- implementation sequencing documents
- issue plans

Planning truth must not claim completed behavior.

Planning truth must not override permanent governance or architecture rules.

### Procedural Truth

Procedural truth records execution steps for a bounded task or operating process.

Examples:

- checklists
- runbooks
- release procedures
- test execution notes
- operational review procedures

Procedural truth must not redefine architecture, governance, or invariants.

### Executable Truth

Executable truth is enforced by code, tests, schemas, and build gates.

Examples:

- Rust code
- TypeScript code
- JSON schemas
- tests
- CI workflows
- validation scripts

Executable truth is the final arbiter of runtime behavior.

If documentation and executable behavior disagree, the discrepancy must be treated as governance debt or implementation defect.

## Authority Model

Rust is authoritative.

TypeScript is non-authoritative.

Tauri is a controlled boundary.

Documentation is not runtime authority.

Schemas define data contracts.

Tests verify behavior.

CI gates enforce minimum repository integrity.

No component may gain authority by convenience, refactor, UI need, integration pressure, dependency choice, or model output.

## Rust Authority Rule

Rust is the only authoritative runtime layer.

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

All authoritative state mutation must pass through approved Rust core paths.

Unsafe Rust is prohibited unless separately justified, isolated, reviewed, and tested.

## TypeScript Non-Authority Rule

TypeScript is the presentation and operator workflow layer.

TypeScript may:

- render application state
- collect operator input
- display investigation records
- display parser warnings
- display candidate findings
- display reports
- display export status
- submit typed requests to Rust
- maintain non-authoritative UI state

TypeScript must not:

- parse raw logs
- create authoritative findings
- approve findings directly
- mutate evidence directly
- perform trusted correlation
- execute arbitrary commands
- hold long-lived secrets
- perform hidden network access
- bypass Rust validation
- become the source of record for investigation state
- make compliance claims

## Tauri Boundary Rule

Tauri is a controlled bridge between the TypeScript interface and the Rust core.

Every Tauri command must have:

- explicit name
- typed request model
- typed response model
- validation path
- error model
- authorization or policy check where applicable
- audit behavior where applicable
- malformed request tests

No Tauri command may:

- execute arbitrary shell commands
- accept unbounded raw instructions
- expose secrets
- read arbitrary files without explicit operator selection or governed configuration
- write authoritative records without Rust validation
- perform network access without explicit configuration and audit
- bypass evidence, finding, reporting, or export rules

## State Mutation Rule

Only approved Rust core paths may mutate authoritative state.

Authoritative state includes:

- raw evidence records
- normalized event records
- entity records
- indicator records
- correlation records
- candidate findings
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

No UI component may directly mutate authoritative state.

No plugin, script, model, report template, or assistive tool may directly mutate authoritative state.

State mutation must be typed, validated, auditable, and bounded.

## Evidence Rule

Raw evidence is append-only.

Raw evidence must not be modified to fit a parser, query, finding, visualization, report, or export.

Derived records must reference source evidence.

If source evidence is unavailable, incomplete, ambiguous, or partially parsed, derived records must disclose that limitation.

Evidence records must preserve source identity where available.

Evidence must not be silently dropped.

Deletion, if supported, must use a governed retention or purge workflow with audit records.

## Source Preservation Rule

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

## Defensive Input Rule

All external input must be treated as hostile.

External input includes:

- log files
- exported SIEM data
- firewall logs
- Windows Event Logs
- Linux syslog
- API responses
- OSINT feeds
- configuration files
- imported rules
- imported indicators
- report templates
- analyst-provided text where it crosses an authority boundary

Input must be:

- bounded
- parsed through explicit parser contracts
- validated before use
- rejected or quarantined on ambiguity where authority would be affected
- recorded with structured errors on failure
- prevented from causing panics or uncontrolled memory growth

Malformed input must not crash Arbitor.

Malformed input must not silently produce authoritative findings.

## Parser Rule

Parsers are attack surfaces.

Parsers must be bounded, testable, and fail-closed.

A malformed input must not crash Arbitor.

A parser failure must produce a structured parser error.

A parser must not silently drop records unless the drop is recorded with a reason.

Parser output must not become authoritative until schema validation succeeds.

Each parser must have:

- valid input tests
- malformed input tests
- oversized input tests where practical
- encoding edge-case tests where practical
- golden output tests
- parser versioning
- structured error output
- no silent record dropping

A parser failure must not create an authoritative finding.

## Schema Rule

Normalized records must conform to versioned schemas.

Schema changes must be explicit.

Schema changes must preserve migration, compatibility, or rejection behavior.

Schema validation must reject malformed records where authoritative behavior would otherwise be affected.

Schema contracts must not be bypassed by UI code, parser convenience paths, imported data, model output, or reporting templates.

## Normalization Rule

Normalization converts source-specific records into a common event model.

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

Normalization must not destroy, overwrite, or hide the original source record.

Normalization must not convert uncertainty into certainty.

## Non-Destructive Filtering Rule

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

## Analysis Rule

Automated analysis creates candidate findings.

A candidate finding is not an analyst-approved conclusion.

A candidate finding must include:

- detection source
- supporting event references
- evidence references
- matched rule, indicator, behavior, or correlation logic
- confidence value
- severity value where applicable
- affected entities
- limitations where known
- creation timestamp
- analysis engine version

A finding may not become approved without an auditable analyst action or an explicitly governed approval workflow.

Automated analysis must not suppress contradictory evidence.

Automated analysis must not hide uncertainty where uncertainty is known.

## Correlation Rule

Correlation records link events, entities, indicators, rules, or time windows.

Correlation does not equal confirmed malicious activity.

A correlation record must include:

- correlation type
- time window where applicable
- supporting event references
- affected entity references
- analysis engine version
- confidence value
- limitations where known

Correlation records may support candidate findings.

Correlation records must not become approved findings without governed review.

## Indicator Rule

Indicator matching must preserve source context.

Indicator records must include:

- indicator type
- indicator value
- source name
- source version or retrieval time where available
- confidence where available
- license or handling metadata where available
- expiration or staleness metadata where available

Indicator matches must include:

- matched event references
- matched field
- match type
- indicator source
- match timestamp
- limitations where known

Indicator matches must not automatically become approved findings.

## Technique Mapping Rule

Technique mappings classify behavior against an adversary technique framework or internal technique catalog.

Technique mappings are candidate classifications unless reviewed.

A technique mapping must include:

- framework or catalog name
- technique identifier where available
- technique name
- mapping reason
- confidence value
- supporting event or finding references

Technique mappings must not be used to inflate severity without evidence.

## Finding Lifecycle Rule

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

Finding lifecycle changes must preserve:

- actor
- timestamp
- prior state
- new state
- reason where supplied
- affected finding identifier
- related audit event

A finding must not be deleted to hide prior analysis.

Suppression must be auditable.

## Analyst Action Rule

Analyst actions are governed state transitions.

Analyst actions include:

- promote finding
- suppress finding
- merge findings
- annotate finding
- edit report text
- approve report
- export report
- change configuration

Analyst actions must preserve:

- operator identity where available
- action type
- timestamp
- target record
- before state reference where practical
- after state reference where practical
- reason where supplied

Analyst notes must remain distinguishable from automated analysis.

## Reporting Rule

Reports must be generated from structured findings, evidence references, timelines, and analyst notes.

Report text must not sever the link between conclusion and supporting evidence.

Any generated report language must remain traceable to source records, findings, timeline records, or analyst-approved notes.

Report statements that assert incident facts must not be unsupported.

Generated reports may contain sensitive information.

Report generation must support:

- source traceability
- redaction workflows
- classification or handling banners where configured
- export audit records
- export hashes where practical
- clear distinction between evidence-derived facts and analyst-authored conclusions
- clear distinction between generated draft text and analyst-approved text

Generated report language must not invent facts.

Generated report language must not hide uncertainty where uncertainty is known.

## Export Rule

Exports may become official incident records or evidence packages.

Export packages must preserve:

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

Exports must be auditable.

Exports must not change evidence, findings, reports, or audit records.

Export behavior must not bypass redaction, classification, or handling controls where configured.

## Model Rule

AI or language model assistance may help draft summaries, explain findings, suggest next steps, or produce candidate report language.

AI assistance must not:

- create authoritative evidence
- modify raw evidence
- approve findings
- suppress findings
- execute response actions
- change security configuration
- bypass deterministic validation
- hide uncertainty
- remove contradictory evidence
- access secrets
- receive sensitive data unless explicitly enabled by governed configuration
- use external model APIs by default

Model output is candidate text unless explicitly reviewed and accepted by an analyst through a governed workflow.

Model-generated content must remain distinguishable from analyst-authored conclusions.

## Sensitive Data Handling Rule

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

## Network Access Rule

Arbitor is local-first by default.

The application must not perform outbound network activity unless:

- the capability is explicitly enabled
- the target is visible to the operator
- the purpose is documented
- credentials are handled through approved secret paths
- the action is auditable
- offline behavior is defined

Examples of controlled network access include:

- Splunk API ingestion
- firewall management API ingestion
- TAXII feed retrieval
- optional update checks
- optional model integration if later approved
- optional ticketing or external reporting integration if later approved

No hidden telemetry is allowed.

## No Hidden Egress Rule

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

The absence of network access must not disable local ingestion, local analysis, local filtering, local reporting, local evidence review, or local export.

## Restricted Environment Rule

Arbitor must be able to operate in restricted, offline, and no-internet environments.

Core capability must not require public internet access.

Network-dependent features must be optional, explicitly enabled, visible to the operator, and auditable.

The core application must support:

- offline installation
- offline operation
- local case storage
- local ingestion
- local parsing
- local normalization
- local filtering
- local analysis
- local reporting
- local export
- disabled-by-default network access
- explicit configuration for any external connector
- auditable connector use

The application must not require public package registries, remote model APIs, online OSINT feeds, or cloud services for core analysis.

## Secret Handling Rule

Secrets must not be stored in plaintext configuration files.

Secrets include:

- API tokens
- passwords
- private keys
- client secrets
- refresh tokens
- signing keys
- encryption keys

Secrets must use operating system native secret storage where available or an approved encrypted local secret store.

Secrets must not be exposed to TypeScript except through narrowly scoped, non-sensitive status responses.

Secrets must not be written to logs, reports, exports, crash dumps, debug output, screenshots, or model prompts.

The UI may display secret presence, connector status, or validation result, but not secret value.

## Cryptography Rule

Cryptographic claims must be precise.

Arbitor must not claim FIPS compliance unless the cryptographic modules used are FIPS-validated and operating in validated configurations.

Encryption, hashing, signing, and key storage must be implemented through reviewed libraries or operating system facilities.

Custom cryptography is prohibited.

Cryptographic behavior must be documented where it affects:

- evidence hashing
- export hashing
- local encryption
- signing
- secure update
- secret storage
- audit integrity

## Dependency Rule

Dependencies are supply-chain risk.

Each new dependency must have a reason.

Dependencies must be reviewed for:

- whether standard library functionality is sufficient
- license compatibility
- maintenance status
- known vulnerability history
- transitive dependency risk
- platform support
- whether it runs in the authoritative Rust core or non-authoritative UI
- whether it introduces network, file-system, cryptographic, parsing, deserialization, or code execution behavior

Rust authority-layer dependencies require stricter review than UI-only dependencies.

No dependency may be introduced only for convenience in an authority-sensitive path without review.

## Build And Release Rule

Release artifacts must be reproducible to the extent practical.

Release artifacts should include:

- version
- source revision
- build environment record
- dependency lockfiles
- software bill of materials where available
- signatures where supported
- checksums

Builds should support:

- lockfiles
- reproducible build practices where practical
- software bill of materials generation where practical
- checksum generation
- signed release artifacts where practical
- offline build documentation for restricted environments
- dependency audit tooling
- static analysis where practical

Unsigned or unaudited builds must not be presented as production-ready.

## Release Readiness Rule

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

## Audit Rule

Security-relevant actions must produce append-only audit records.

Audit records include:

- ingestion actions
- parser failures
- normalization results
- source connection attempts
- query execution
- finding creation
- finding lifecycle changes
- analyst actions
- report generation
- report approval
- export generation
- configuration changes
- secret creation or update metadata
- network feed retrieval
- connector use
- authentication or credential events where applicable

Audit records must be sufficient to reconstruct what happened, when it happened, what record was affected, and what component or operator performed the action.

Audit records must not expose secrets.

Audit records must avoid raw sensitive data unless explicitly required and governed.

## Compliance Claim Rule

Arbitor may describe alignment with standards only when the claim is specific.

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

Formal compliance claims require separate evidence, validation, and approval.

Compliance support does not equal certification.

Certification, authorization, accreditation, or formal compliance status must be separately assessed and documented.

## Supported Compliance Alignment Language

Arbitor may be described as designed to support security operations aligned with:

- NIST Cybersecurity Framework (CSF)
- NIST SP 800-53
- NIST SP 800-171 where Controlled Unclassified Information (CUI) is in scope
- NIST SP 800-218 Secure Software Development Framework (SSDF)
- CISA Secure by Design principles
- DoD Risk Management Framework (RMF) support workflows
- DISA STIG-aligned deployment expectations where applicable

This language does not imply approval, certification, authorization, accreditation, or compliance.

## Review Rule

Changes that affect authority boundaries require review.

Authority-boundary changes include:

- state mutation paths
- evidence handling
- parser behavior
- schema behavior
- normalization behavior
- detection logic
- correlation logic
- finding lifecycle logic
- report generation
- export generation
- cryptography
- secrets handling
- Tauri command exposure
- network access behavior
- storage behavior
- audit behavior
- model or AI assistance integration

A change that expands authority must be explicit.

Silent authority expansion is prohibited.

## Repository Placement Rule

Documents must live in the correct truth surface.

Code must not rely on prose documentation for runtime enforcement.

Runtime enforcement belongs in code, schema, validation, tests, and CI gates.

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

## Documentation Rule

Every phase that changes architecture, governance, invariants, secure engineering, or model design must update the relevant permanent truth document.

Every phase with completed work must update `CHANGELOG.md`.

Documentation must not claim implemented behavior unless executable truth supports it.

Documentation must not claim production readiness unless a readiness review supports it.

Documentation must not claim compliance, certification, authorization, or government approval unless formal evidence supports it.

## Phase Governance Rule

Each phase must have:

- one primary goal
- bounded allowed surfaces
- explicit non-goals
- validation expectations
- changelog entry requirement
- no unstated authority expansion

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

## Standing Security Gate

A phase must not introduce:

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

## Standing Evidence Gate

A phase must not weaken:

- evidence immutability
- source preservation
- source traceability
- parser error visibility
- finding support references
- report provenance
- export integrity

## Standing Analysis Gate

A phase must not allow automated analysis to create approved conclusions without governed analyst review.

A phase must not allow candidate findings to hide uncertainty, omit supporting evidence, or suppress contradictory records.

## Standing Reporting Gate

A phase must not allow report claims to detach from evidence, findings, timelines, or analyst-approved notes.

A phase must not allow generated text to become authoritative without governed review where review is required.

## Standing Restricted Environment Gate

A phase must preserve offline operation unless the phase is explicitly about optional network integration.

Optional network features must remain disabled by default.

Core analysis must not require public internet access.

## Governance Failure

A governance failure occurs when:

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

## Production-Candidate Governance Bar

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

Production-candidate status does not imply formal certification, authorization, accreditation, or government approval.

## Final Governance Rule

If a component cannot preserve authority boundaries, evidence traceability, restricted-environment operation, and auditable behavior, it must not be added to Arbitor.
