---
truth_dimension: planning
authority_level: normative_for_phase_alignment
document_role: phase_map
mutation_policy: reviewed_change_only
---

# ARBITOR PHASE MAP

## Purpose

This document defines the phase map for Arbitor.

The phase map governs planning alignment, phase sequencing, and scope control. It does not record completed work. Completed work belongs in `CHANGELOG.md`.

This document is normative for phase alignment. It is not historical truth and must not claim that planned work has been implemented.

## Truth boundary

This document may contain:

- planned phases
- phase ordering
- phase goals
- phase gates
- phase non-goals
- dependency relationships
- scope constraints

This document must not contain:

- completed implementation claims
- release notes
- validation results
- test outputs
- historical status
- production readiness claims
- compliance certification claims

Historical truth belongs in `CHANGELOG.md`.

Permanent governance truth belongs in:

- `docs/GOVERNANCE.md`
- `docs/ARCHITECTURE.md`
- `docs/INVARIANTS.md`
- `docs/MODEL_DESIGN.md`
- `docs/SECURE_ENGINEERING.md`

## Phase control rules

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

## Phase status terms

Use the following status terms only:

```text
planned
active
completed
deferred
blocked
superseded
````

Status terms in this document are planning aids only.

The authoritative record of completed work is `CHANGELOG.md`.

## Version posture

Arbitor uses pre-production versioning until formal release readiness is established.

Recommended convention:

```text
v0.0.x  bootstrap, governance, documentation, skeletons, validation gates
v0.1.x  Rust core foundation and authority boundary
v0.2.x  evidence, ingestion, and parser foundation
v0.3.x  normalization, schemas, and entity extraction
v0.4.x  query, filtering, and local case state
v0.5.x  correlation, detection, and IOC matching
v0.6.x  finding lifecycle and analyst review
v0.7.x  reporting, exports, and audit package generation
v0.8.x  TypeScript/Tauri operator interface hardening
v0.9.x  security hardening, restricted environment packaging, release evidence
v1.0.0  production candidate only after explicit readiness review
```

Version numbers are planning markers. They are not marketing labels.

## Phase map

### Phase 0: repository bootstrap and governance surfaces

Status: planned

Goal:

Create the initial repository structure and permanent truth documents.

Allowed surfaces:

* `README.md`
* `LICENSE` or `LICENSE.md`
* `CHANGELOG.md`
* `docs/`
* `docs/roadmap/`
* `core/.gitkeep`
* `ui/.gitkeep`
* `schemas/.gitkeep`
* `tests/`
* `scripts/check.sh`
* `.gitignore`

Required outputs:

* repository skeleton
* permanent truth documents
* secure engineering document
* roadmap directory
* validation script
* changelog entry

Gate:

* `scripts/check.sh` validates required files and directories.

Non-goals:

* no Rust runtime implementation
* no TypeScript/Tauri implementation
* no parser implementation
* no analysis implementation
* no storage implementation
* no network connector
* no OSINT integration
* no reporting implementation
* no compliance claim

---

### Phase 1: Rust core authority skeleton

Status: planned

Goal:

Create the Rust core skeleton and establish Rust as the only authoritative runtime layer.

Allowed surfaces:

* `core/`
* `core/src/`
* `core/Cargo.toml`
* `docs/ARCHITECTURE.md`
* `docs/INVARIANTS.md`
* `CHANGELOG.md`
* `scripts/check.sh`

Required outputs:

* Rust crate skeleton
* module boundaries for core authority
* no-op or type-only module stubs where needed
* baseline Rust validation command
* changelog entry

Gate:

* Rust project checks pass.
* No authoritative state mutation exists outside Rust.

Non-goals:

* no parser logic
* no Tauri command exposure
* no UI implementation
* no storage engine
* no network connector
* no detection logic

---

### Phase 2: schema and model contract foundation

Status: planned

Goal:

Define initial versioned schemas and Rust model types for Arbitor’s core records.

Allowed surfaces:

* `schemas/`
* `core/src/model/`
* `docs/MODEL_DESIGN.md`
* `docs/INVARIANTS.md`
* `tests/`
* `CHANGELOG.md`

Required outputs:

* source model
* evidence model
* normalized event model
* entity model
* indicator model
* detection rule model
* finding model
* audit event model
* schema validation tests

Gate:

* models are typed and versioned.
* schema validation rejects malformed records.

Non-goals:

* no real ingestion
* no parser implementation
* no database storage
* no UI
* no report generation

---

### Phase 3: immutable evidence store foundation

Status: planned

Goal:

Implement local evidence record creation with immutable, append-oriented semantics.

Allowed surfaces:

* `core/src/evidence/`
* `core/src/audit/`
* `tests/fixtures/`
* `tests/golden/`
* `docs/INVARIANTS.md`
* `CHANGELOG.md`

Required outputs:

* evidence record creation
* evidence hash calculation
* source metadata preservation
* append-only audit event for ingestion
* rejection behavior for mutation attempts
* tests for immutability

Gate:

* raw evidence cannot be modified through approved APIs.
* evidence records preserve source references and hashes.

Non-goals:

* no parser-specific support
* no normalized analysis
* no detection logic
* no UI
* no network ingestion

---

### Phase 4: parser contract and defensive input handling

Status: planned

Goal:

Create parser interfaces and defensive input handling requirements before implementing source-specific parsers.

Allowed surfaces:

* `core/src/parser/`
* `core/src/errors/`
* `tests/fixtures/malformed/`
* `docs/SECURE_ENGINEERING.md`
* `docs/INVARIANTS.md`
* `CHANGELOG.md`

Required outputs:

* parser trait or equivalent interface
* structured parser error model
* bounded input handling
* malformed input tests
* oversized input tests where practical
* no silent record dropping

Gate:

* malformed input produces structured errors.
* parser failures do not crash the application.

Non-goals:

* no full vendor parser
* no UI display
* no detection logic
* no external enrichment

---

### Phase 5: first local parser implementations

Status: planned

Goal:

Implement initial local parsers for bounded offline test data.

Allowed sources:

* Linux syslog sample files
* Windows Event Log exported sample data
* Cisco text log sample data
* Palo Alto sample CSV or text data

Allowed surfaces:

* `core/src/parser/`
* `tests/fixtures/`
* `tests/golden/`
* `schemas/`
* `CHANGELOG.md`

Required outputs:

* at least one source parser
* golden normalized output
* malformed input coverage
* parser version recorded in output
* parser warnings where needed

Gate:

* parser output conforms to normalized event schema.
* parser failure paths are tested.

Non-goals:

* no live connector
* no Splunk API
* no network access
* no automated findings

---

### Phase 6: normalization and entity extraction

Status: planned

Goal:

Normalize parsed records and extract durable investigation entities.

Allowed surfaces:

* `core/src/normalization/`
* `core/src/entity/`
* `schemas/`
* `tests/golden/`
* `docs/MODEL_DESIGN.md`
* `CHANGELOG.md`

Required outputs:

* normalized event projection
* entity extraction
* source reference preservation
* timestamp handling
* canonical entity values
* tests for partial and unknown fields

Gate:

* every entity preserves event references.
* normalized records preserve evidence references.

Non-goals:

* no correlation graph
* no detection engine
* no reporting
* no UI authority

---

### Phase 7: query and non-destructive filtering

Status: planned

Goal:

Implement typed local query and layered filtering over normalized records.

Allowed surfaces:

* `core/src/query/`
* `core/src/filter/`
* `schemas/`
* `tests/`
* `docs/INVARIANTS.md`
* `CHANGELOG.md`

Required outputs:

* typed query model
* layered filter model
* reversible view semantics
* saved view records if introduced
* audit record for query execution where applicable

Gate:

* filters do not mutate evidence, normalized events, entities, or findings.

Non-goals:

* no UI query builder
* no detection engine
* no report generation
* no network query execution

---

### Phase 8: local case state and audit trail

Status: planned

Goal:

Implement local case records and append-only audit records.

Allowed surfaces:

* `core/src/case/`
* `core/src/audit/`
* `core/src/storage/`
* `schemas/`
* `tests/`
* `CHANGELOG.md`

Required outputs:

* case creation
* case metadata
* audit event append path
* audit event validation
* tests for audit immutability
* no hidden state mutation

Gate:

* security-relevant actions produce audit records.
* audit records are append-only.

Non-goals:

* no report export
* no UI case workspace
* no live connectors

---

### Phase 9: IOC model and offline indicator matching

Status: planned

Goal:

Implement offline indicator import and deterministic indicator matching.

Allowed surfaces:

* `core/src/ioc/`
* `schemas/`
* `tests/fixtures/iocs/`
* `docs/MODEL_DESIGN.md`
* `CHANGELOG.md`

Required outputs:

* indicator model
* offline import path
* indicator source metadata
* exact match logic
* match result records
* tests for stale, duplicate, and malformed indicators

Gate:

* indicator matches record source, retrieval/import metadata, and matched event references.

Non-goals:

* no automatic OSINT retrieval
* no TAXII client
* no network access
* no automatic conclusion promotion

---

### Phase 10: detection rule foundation

Status: planned

Goal:

Implement deterministic detection rule contracts and local rule execution.

Allowed surfaces:

* `core/src/detection/`
* `schemas/`
* `tests/fixtures/rules/`
* `docs/MODEL_DESIGN.md`
* `CHANGELOG.md`

Required outputs:

* detection rule model
* rule versioning
* rule validation
* exact match rules
* threshold rules
* basic sequence rules
* rule execution results

Gate:

* every rule result preserves rule version and supporting event references.

Non-goals:

* no remote rule feed
* no model-based detection
* no automatic approved findings

---

### Phase 11: correlation engine foundation

Status: planned

Goal:

Implement deterministic event and entity correlation.

Allowed surfaces:

* `core/src/correlation/`
* `core/src/entity/`
* `schemas/`
* `tests/golden/`
* `CHANGELOG.md`

Required outputs:

* time-window correlation
* entity-linked correlation
* sequence correlation
* correlation records
* confidence basis
* limitations field

Gate:

* correlation records do not assert confirmed compromise by themselves.

Non-goals:

* no UI graph
* no report generation
* no response automation

---

### Phase 12: candidate finding lifecycle

Status: planned

Goal:

Create candidate findings from deterministic matches and correlations.

Allowed surfaces:

* `core/src/finding/`
* `core/src/detection/`
* `core/src/correlation/`
* `schemas/`
* `tests/`
* `CHANGELOG.md`

Required outputs:

* candidate finding creation
* finding status model
* severity model
* confidence model
* supporting evidence references
* rule and indicator references
* analyst action placeholder model

Gate:

* automated analysis creates only candidate findings.
* findings cannot be approved without governed analyst action.

Non-goals:

* no report generation
* no UI approval workflow
* no model-authored conclusions

---

### Phase 13: analyst action and finding review model

Status: planned

Goal:

Implement governed analyst actions for finding review.

Allowed surfaces:

* `core/src/finding/`
* `core/src/analyst_action/`
* `core/src/audit/`
* `schemas/`
* `tests/`
* `CHANGELOG.md`

Required outputs:

* promote finding
* suppress finding
* annotate finding
* merge finding if introduced
* audit record for lifecycle changes
* before/after state references

Gate:

* every finding lifecycle change is auditable.

Non-goals:

* no UI workflow yet
* no report export
* no response action automation

---

### Phase 14: report model and deterministic report assembly

Status: planned

Goal:

Implement structured report records assembled from findings, timelines, and analyst notes.

Allowed surfaces:

* `core/src/report/`
* `schemas/`
* `tests/golden/`
* `docs/MODEL_DESIGN.md`
* `CHANGELOG.md`

Required outputs:

* executive summary report model
* technical report model
* incident ticket model
* evidence appendix model
* source reference preservation
* deterministic report assembly tests

Gate:

* report statements trace to findings, evidence, timeline records, or analyst notes.

Non-goals:

* no PDF or DOCX export yet
* no AI-generated prose
* no UI editor

---

### Phase 15: export package foundation

Status: planned

Goal:

Implement export package metadata and local export integrity records.

Allowed surfaces:

* `core/src/export/`
* `core/src/report/`
* `core/src/audit/`
* `schemas/`
* `tests/golden/`
* `CHANGELOG.md`

Required outputs:

* export package model
* export hash
* export metadata
* included records manifest
* audit event for export creation
* redaction profile placeholder if needed

Gate:

* export records preserve what was exported, when, by whom, and from which report.

Non-goals:

* no final PDF/DOCX rendering requirement
* no email sending
* no external ticket creation

---

### Phase 16: TypeScript/Tauri shell foundation

Status: planned

Goal:

Create the desktop application shell without authoritative UI behavior.

Allowed surfaces:

* `ui/`
* Tauri configuration files
* TypeScript app skeleton
* `core/src/tauri_api/`
* `docs/ARCHITECTURE.md`
* `CHANGELOG.md`

Required outputs:

* minimal Tauri app
* no remote content
* strict CSP where applicable
* explicit command boundary placeholder
* no generic shell command
* no secret exposure

Gate:

* UI cannot mutate authoritative state except through typed Rust commands.

Non-goals:

* no full dashboard
* no live case workflow
* no parser UI
* no reporting UI

---

### Phase 17: Tauri command boundary

Status: planned

Goal:

Expose typed, allowlisted Rust commands for non-destructive UI operations.

Allowed surfaces:

* `core/src/tauri_api/`
* `ui/src/`
* `schemas/`
* `tests/`
* `CHANGELOG.md`

Required outputs:

* typed command request models
* typed command response models
* validation for every command
* error model
* tests for rejected malformed requests

Gate:

* no generic command execution exists.
* every command has explicit validation.

Non-goals:

* no direct file mutation from UI
* no raw secret access
* no hidden network access

---

### Phase 18: investigation workspace UI

Status: planned

Goal:

Implement non-authoritative UI views for cases, events, filters, findings, and reports.

Allowed surfaces:

* `ui/src/`
* `core/src/tauri_api/`
* generated types if introduced
* `CHANGELOG.md`

Required outputs:

* case list view
* event table view
* filter view
* candidate finding view
* report preview view
* no direct state mutation

Gate:

* UI renders authoritative Rust state and submits typed requests only.

Non-goals:

* no UI-only analysis
* no UI-only finding creation
* no browser-side parser logic

---

### Phase 19: visualization and correlation graph UI

Status: planned

Goal:

Render trend charts and correlation graph views from Rust-provided datasets.

Allowed surfaces:

* `ui/src/`
* `core/src/tauri_api/`
* `core/src/visualization/` if needed
* `CHANGELOG.md`

Required outputs:

* trend data view
* entity relationship view
* event timeline view
* finding-linked graph view

Gate:

* visualization does not become authoritative analysis.

Non-goals:

* no graph-derived findings in TypeScript
* no UI-only correlation logic

---

### Phase 20: restricted environment hardening

Status: planned

Goal:

Harden Arbitor for restricted, offline, and no-hidden-egress environments.

Allowed surfaces:

* `docs/SECURE_ENGINEERING.md`
* `scripts/`
* CI configuration if present
* build configuration
* `core/`
* `ui/`
* `CHANGELOG.md`

Required outputs:

* network behavior review
* offline operation validation
* dependency review record
* no hidden telemetry validation
* secret handling review
* Tauri command boundary review

Gate:

* core local functionality works without public internet access.
* all network-capable features are explicitly enabled and auditable.

Non-goals:

* no formal authorization claim
* no FIPS compliance claim
* no DoD approval claim

---

### Phase 21: compliance mapping package

Status: planned

Goal:

Create a control mapping package for standards alignment support.

Allowed surfaces:

* `docs/compliance/`
* `docs/SECURE_ENGINEERING.md`
* `docs/GOVERNANCE.md`
* `CHANGELOG.md`

Required outputs:

* NIST CSF support mapping
* NIST SP 800-53 control support mapping
* NIST SP 800-171 applicability notes if CUI is in scope
* NIST SP 800-218 SSDF evidence mapping
* CISA Secure by Design mapping
* DoD RMF support notes
* claim limitations

Gate:

* no certification or authorization claim is made.

Non-goals:

* no formal ATO package
* no STIG certification
* no CMMC certification
* no FIPS validation claim

---

### Phase 22: release evidence and packaging foundation

Status: planned

Goal:

Create release evidence practices for pre-production builds.

Allowed surfaces:

* build scripts
* release scripts
* docs/release/
* CI configuration if present
* `CHANGELOG.md`

Required outputs:

* checksums
* build metadata
* dependency lockfile policy
* SBOM plan or initial SBOM generation
* signing plan or initial signing support
* release notes boundary

Gate:

* release artifacts identify source revision and build metadata.

Non-goals:

* no production release claim
* no compliance certification claim

---

### Phase 23: adversarial and negative testing expansion

Status: planned

Goal:

Expand test coverage against malformed input, parser abuse, boundary violations, and unsafe state transitions.

Allowed surfaces:

* `tests/`
* `core/`
* `ui/`
* `scripts/`
* CI configuration if present
* `CHANGELOG.md`

Required outputs:

* malformed parser tests
* oversized input tests where practical
* Tauri command rejection tests
* finding lifecycle negative tests
* evidence mutation rejection tests
* report traceability failure tests

Gate:

* known invalid states are rejected deterministically.

Non-goals:

* no new product feature required
* no broadened runtime authority

---

### Phase 24: early production readiness audit

Status: planned

Goal:

Audit whether Arbitor is ready to enter production-candidate hardening.

Allowed surfaces:

* `docs/audits/`
* `docs/compliance/`
* `CHANGELOG.md`
* existing test reports if present

Required outputs:

* architecture boundary review
* evidence integrity review
* parser safety review
* analysis explainability review
* reporting traceability review
* network behavior review
* dependency review
* known limitations
* readiness decision

Gate:

* readiness decision must be evidence-based.

Non-goals:

* no new runtime behavior
* no production declaration unless supported by audit evidence

---

## Phase Dependency Chain

```text
Phase 0  -> repository skeleton and governance surfaces
Phase 1  -> Rust authority foundation
Phase 2  -> model and schema contracts
Phase 3  -> immutable evidence
Phase 4  -> parser safety contract
Phase 5  -> first parsers
Phase 6  -> normalization and entities
Phase 7  -> query and filtering
Phase 8  -> case and audit state
Phase 9  -> offline IOC matching
Phase 10 -> detection rules
Phase 11 -> correlation
Phase 12 -> candidate findings
Phase 13 -> analyst review
Phase 14 -> reports
Phase 15 -> exports
Phase 16 -> Tauri shell
Phase 17 -> command boundary
Phase 18 -> investigation UI
Phase 19 -> visualization UI
Phase 20 -> restricted environment hardening
Phase 21 -> compliance mapping
Phase 22 -> release evidence
Phase 23 -> adversarial tests
Phase 24 -> readiness audit
```

## Standing Gates

The following gates apply to every phase.

### Documentation gate

Every phase that changes architecture, governance, invariants, secure engineering, or model design must update the relevant permanent truth document.

Every phase with completed work must update `CHANGELOG.md`.

### Security Gate

A phase must not introduce:

* hidden network access
* unaudited state mutation
* plaintext secret storage
* parser panic paths on malformed input
* UI authority expansion
* unreviewed dependency expansion
* unsupported compliance claims

### Evidence Gate

A phase must not weaken evidence immutability, source preservation, or traceability.

### Analysis Gate

A phase must not allow automated analysis to create approved conclusions without governed analyst review.

### Reporting Gate

A phase must not allow report claims to detach from evidence, findings, timelines, or analyst-approved notes.

### Restricted Environment Gate

A phase must preserve offline operation unless the phase is explicitly about optional network integration.

Optional network features must remain disabled by default.

## CHANGELOG Requirement

Every completed phase must add a `CHANGELOG.md` entry using this format:

```markdown
## vX.Y.Z - YYYY-MM-DD

**Status:** <phase name and capability surface>.

### Added
- <completed change>
- <completed change>

### Notes
- This phase <states explicit non-authoritative or non-production boundary>.
- This phase makes no compliance, certification, authorization, FIPS, STIG, RMF, CMMC, NIST, CISA, or DoD approval claim unless separately validated.
```

## Stop Conditions

A phase must stop before completion if any of the following occurs:

* raw evidence can be mutated without audit
* TypeScript gains direct authority over core state
* parser failures are silent
* malformed input crashes the application
* model output becomes authoritative without review
* hidden network activity is introduced
* compliance language exceeds evidence
* findings lack source evidence references
* reports contain unsupported factual claims
* secrets are exposed outside approved paths
* validation cannot be run
* required changelog entry is missing

## Production-candidate Entry Criteria

Arbitor must not be treated as production candidate until the following are true:

* Rust authority boundary is implemented and tested
* raw evidence immutability is implemented and tested
* parser fail-closed behavior is implemented and tested
* normalized events preserve evidence references
* findings preserve supporting event and evidence references
* analyst review workflow exists for finding promotion
* reports preserve traceability
* exports include integrity metadata
* audit records exist for security-relevant actions
* hidden network access is absent
* restricted offline operation is validated
* dependency review exists
* release evidence package exists
* compliance claims are reviewed and bounded
* readiness audit is completed

## Non-authoritative Note

This phase map guides planned work and phase alignment.

It does not prove implementation, readiness, compliance, certification, authorization, or security posture by itself.
