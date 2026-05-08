---
truth_dimension: permanent
authority_level: normative
document_role: error_logging_contract
mutation_policy: reviewed_change_only
---

# Arbitor Error Logging

## Purpose

This document defines the error logging model for Arbitor.

Error logging exists to support operational monitoring, troubleshooting, security review, deployment audit, and integration with external logging infrastructure.

Error logging is not a substitute for audit records.

Audit records preserve authoritative security-relevant actions.

Error logs preserve runtime failure, warning, diagnostic, and health information.

Both surfaces must remain distinct.

## Logging Scope

Arbitor must support structured local error logging.

Arbitor should support optional emission to external logging systems, including syslog servers and other approved monitoring tools.

Logging must support restricted and government-adjacent environments where centralized monitoring, Security Information and Event Management (SIEM) forwarding, audit review, and operational alerting are required.

## Logging Authority

Rust owns authoritative logging behavior.

TypeScript may display logging status and submit typed logging configuration requests.

TypeScript must not directly emit authoritative application logs.

Tauri commands that configure or test log emission must be explicit, typed, allowlisted, validated, and auditable.

Scripts must not bypass Arbitor logging policy.

## Audit And Error Log Separation

Audit records and error logs are separate surfaces.

Audit records answer:

- what security-relevant action occurred
- who or what performed the action
- when the action occurred
- which authoritative record was affected
- whether the action succeeded or failed

Error logs answer:

- what runtime condition occurred
- which component emitted it
- what severity it had
- whether operator or administrator action is needed
- whether forwarding succeeded or failed
- whether the application remains healthy

An error log may reference an audit event identifier.

An audit record may reference a logging failure if the failure affects a security-relevant action.

Error logs must not replace audit records.

Audit records must not be downgraded into ordinary logs.

## Logging Levels

Arbitor logging levels are:

```text
trace
debug
info
warn
error
critical
````

`trace` is for temporary, high-volume diagnostic detail.

`debug` is for development and controlled troubleshooting.

`info` is for normal operational milestones.

`warn` is for unexpected but recoverable conditions.

`error` is for failed operations requiring attention.

`critical` is for severe conditions that may affect evidence integrity, security posture, restricted-environment operation, or safe continued use.

Production and restricted-environment builds must not enable `trace` or `debug` by default.

## Required Log Fields

Structured error logs must include:

```text
timestamp
level
component
event_code
message
result
correlation_id
case_id
operation_id
audit_event_id
source_type
record_id
error_kind
safe_context
```

Fields may be omitted only when unavailable or not applicable.

Logs must remain structured where practical.

Freeform text-only logs are allowed only for early bootstrap or non-authoritative developer tooling.

## Event Codes

Every stable log event that may be used for monitoring or alerting must have an event code.

Event codes must be stable, documented, and reviewable.

Event codes should follow this shape:

```text
ARB-LOG-0000
ARB-PARSER-0000
ARB-STORAGE-0000
ARB-AUDIT-0000
ARB-NETWORK-0000
ARB-EXPORT-0000
ARB-SECURITY-0000
ARB-TAURI-0000
```

Event codes must not encode secrets, user identifiers, hostnames, IP addresses, or case-specific data.

## Sensitive Data Rule

Logs must be safe by default.

Logs must not expose:

* passwords
* API tokens
* private keys
* refresh tokens
* session tokens
* raw credentials
* full raw log records
* sensitive report text
* unnecessary usernames
* unnecessary internal hostnames
* unnecessary internal IP addresses
* Controlled Unclassified Information (CUI), where applicable
* mission or operational context unless explicitly governed

Logs may include stable internal identifiers where needed for correlation.

Sensitive values must be redacted, hashed, tokenized, or omitted.

Redaction must happen before local write or external emission.

## Local Logging

Arbitor must support local logging.

Local logs must be written to an approved application log location.

Local logs must not be stored inside raw evidence records.

Local logs must not mutate evidence, findings, reports, exports, or audit records.

Local logs should support rotation, retention limits, and bounded file size.

Logging failure must not silently hide critical runtime failures.

If local logging fails, Arbitor must surface the failure to the operator where practical.

## Syslog Emission

Arbitor should support optional syslog emission for monitoring and audit support.

Syslog emission must be disabled by default.

Syslog emission must require explicit configuration.

Syslog emission must be visible to the operator or administrator.

Syslog emission must be auditable when enabled, disabled, tested, or modified.

Syslog emission must support restricted environments where approved logging infrastructure is internal and controlled.

Syslog emission must not send raw evidence, secrets, full report contents, or sensitive case details unless explicitly governed.

## External Logging Integrations

External logging integrations may include:

* syslog
* local operating system event logging
* SIEM forwarders
* file-based collectors
* endpoint management logging tools
* approved enterprise monitoring agents

Every external logging integration must define:

```text
purpose
destination type
transport
format
data fields emitted
sensitive data handling
authentication method if applicable
failure behavior
offline behavior
operator visibility
audit event behavior
configuration storage
```

No external logging integration may be enabled silently.

## Network Behavior

Remote log emission is outbound network behavior.

Remote log emission must follow Arbitor network rules.

Remote log emission must be:

* explicitly enabled
* configured by an approved path
* visible to the operator or administrator
* auditable
* bounded
* redacted before transmission
* disabled by default
* compatible with offline operation

Failure to reach a remote log destination must not disable local ingestion, local analysis, local filtering, local reporting, local evidence review, or local export.

Remote logging failure must produce a local warning or error event where practical.

## Logging Configuration

Logging configuration must be typed and validated.

Logging configuration may include:

```text
local_logging_enabled
local_log_level
local_log_path
local_retention_policy
syslog_enabled
syslog_destination
syslog_port
syslog_protocol
syslog_facility
syslog_minimum_level
syslog_tls_required
external_logging_enabled
redaction_profile
```

Logging configuration must not store secrets in plaintext.

Logging configuration changes must produce audit records.

## Logging Failure Behavior

Logging failure must be explicit.

Failures may include:

* local log path unavailable
* local log write failure
* log rotation failure
* syslog destination unreachable
* syslog authentication failure
* TLS validation failure
* redaction failure
* serialization failure
* queue overflow
* dropped external log event

Logging failures must not crash Arbitor unless continued operation would create unsafe, unauditable, or misleading behavior.

Critical logging failures must be surfaced to the operator where practical.

## Log Queueing And Backpressure

External log emission must be bounded.

Arbitor must not allow logging queues to grow without limit.

If remote logging is unavailable, Arbitor may buffer logs only within configured limits.

When limits are reached, Arbitor must record that log events were dropped or suppressed.

Dropped log counts must be visible through local logs or operator status where practical.

## Log Integrity

Logs should preserve enough information to support monitoring and troubleshooting.

Logs intended for restricted or government-adjacent deployments should support integrity controls where practical.

Integrity controls may include:

* append-only local log mode
* rotation metadata
* checksums
* sequence numbers
* signed log batches where later approved
* forwarding status records
* audit references

Log integrity controls must not replace evidence integrity controls or audit record integrity controls.

## Operator Visibility

The UI may display:

* logging enabled or disabled state
* local logging status
* syslog forwarding status
* last successful emission time
* last emission failure
* configured minimum log level
* redaction profile status
* dropped log count where available

The UI must not display secrets.

The UI must not display sensitive raw log content unless explicitly governed.

## Testing Requirements

Logging behavior must be testable.

Tests should cover:

* structured log creation
* severity assignment
* sensitive data redaction
* local logging failure
* syslog disabled by default
* syslog configuration validation
* syslog emission failure handling
* bounded queue behavior where implemented
* audit record creation for logging configuration changes
* rejection of malformed logging configuration

## Release Readiness Requirements

A release intended for restricted or government-adjacent environments must not proceed unless logging behavior has been reviewed.

The review must cover:

* local error logging
* sensitive data redaction
* syslog or external emission behavior where implemented
* disabled-by-default remote emission
* logging configuration auditability
* failure behavior
* dropped log behavior
* operator visibility
* documentation accuracy

## Final Logging Rule

If Arbitor cannot log errors safely, surface logging failures clearly, and prevent sensitive data exposure through logs or remote emission, the logging capability must not be enabled for production or restricted-environment use.
