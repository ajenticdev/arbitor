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
```
