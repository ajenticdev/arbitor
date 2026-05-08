---
truth_dimension: permanent
authority_level: normative
document_role: secure_engineering_contract
mutation_policy: reviewed_change_only
---

# Arbitor secure engineering

## Purpose

This document defines secure engineering requirements for Arbitor.

Arbitor is security software that processes sensitive operational data. The system must be engineered for defensive use, restricted environments, and auditable behavior.

## Security baseline

Arbitor development must align with:

- NIST SP 800-218 Secure Software Development Framework (SSDF)
- NIST SP 800-53 security and privacy control families
- CISA Secure by Design principles
- DISA STIG-aligned deployment expectations where applicable
- DoD Risk Management Framework (RMF) support expectations where applicable

Alignment does not imply certification, authorization, or approval.

## Default posture

Arbitor defaults to:

- local-first operation
- no hidden telemetry
- no default public internet dependency
- no automatic external enrichment
- no automatic upload of logs, reports, findings, or diagnostics
- deny-by-default command exposure
- explicit operator action for sensitive operations
- fail-closed validation
- append-only audit records

## Defensive programming requirements

All code that handles external input must:

- bound input size
- validate input format
- handle malformed input
- reject ambiguous input where authority would be affected
- avoid panics on untrusted data
- avoid unsafe deserialization
- avoid command injection paths
- avoid path traversal
- avoid arbitrary file reads
- avoid uncontrolled memory growth
- avoid silent data loss
- preserve parser errors as structured records

## Parser requirements

Parsers must be treated as attack surfaces.

Each parser must have:

- valid input tests
- malformed input tests
- oversized input tests where practical
- encoding edge-case tests where practical
- golden output tests
- parser versioning
- structured error output
- no silent record dropping

Parser output must not become authoritative until schema validation succeeds.

## Dependency requirements

Dependencies must be minimized.

Each dependency must have a clear purpose.

Before adding a dependency, evaluate:

- whether standard library functionality is sufficient
- maintenance status
- license compatibility
- known vulnerability history
- transitive dependency size
- platform support
- whether it runs in the authoritative Rust core or non-authoritative UI
- whether it introduces network, file-system, cryptographic, or code execution behavior

Authority-layer dependencies require stricter review than UI-only dependencies.

## Build requirements

Builds should support:

- lockfiles
- reproducible build practices where practical
- software bill of materials generation where practical
- checksum generation
- signed release artifacts where practical
- offline build documentation for restricted environments
- dependency audit tooling
- static analysis where practical

## Runtime requirements

Runtime behavior must be observable and bounded.

The application must:

- log security-relevant actions to append-only audit records
- avoid logging secrets
- avoid logging raw sensitive data unless explicitly required and governed
- expose network access status
- expose connector status
- expose evidence import status
- expose parser failures
- expose analysis limitations
- expose report generation sources

## Cryptography requirements

Cryptographic claims must be precise.

Arbitor must not claim FIPS compliance unless the cryptographic modules used are FIPS-validated and operating in validated configurations.

Encryption, hashing, signing, and key storage must be implemented through reviewed libraries or operating system facilities.

Custom cryptography is prohibited.

## Secret handling requirements

Secrets must not be stored in plaintext.

Secrets must not be exposed to the UI.

Secrets must not be written to logs, reports, exports, crash dumps, or debug output.

Secrets must be accessed only through approved Rust core paths.

## Network requirements

Network behavior must be explicit.

Each network-capable feature must define:

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

## Reporting requirements

Generated reports may contain sensitive information.

Report generation must support:

- source traceability
- redaction workflows
- classification or handling banners where configured
- export audit records
- export hashes where practical
- clear distinction between evidence-derived facts and analyst-authored conclusions

## AI assistance requirements

AI assistance is optional and non-authoritative.

AI assistance must not receive sensitive data unless explicitly enabled by governed configuration.

AI assistance must not use external model APIs by default.

AI-generated text must be marked as candidate text until accepted by an analyst.

## Release readiness requirements

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
