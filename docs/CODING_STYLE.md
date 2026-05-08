---
truth_dimension: permanent
authority_level: normative
document_role: coding_style_contract
mutation_policy: reviewed_change_only
---

# Arbitor Coding Style

## Purpose

This document defines the coding style for Arbitor.

Arbitor code must be simple, plain, reviewable, and defensively written.

The goal is not clever code. The goal is code that outside engineers can read, review, test, audit, and maintain without relying on hidden assumptions or specialized knowledge.

This document applies to all source code, scripts, schemas, generated types, tests, examples, and build logic unless a more specific language standard applies.

## Style Doctrine

Arbitor code must favor:

- clarity over cleverness
- explicit behavior over implicit behavior
- small functions over large functions
- shallow control flow over deep nesting
- typed contracts over informal structure
- simple data flow over hidden mutation
- readable names over abbreviations
- boring patterns over tricky methods
- defensive validation over optimistic assumptions
- testable units over tightly coupled logic

A reviewer should be able to understand what code does, what data it accepts, what it returns, what can fail, and what authority it has.

## General Rules

Code must be written for review.

Code must be easy to read before it is easy to optimize.

Code must not rely on clever language tricks, obscure idioms, surprising side effects, or hidden global behavior.

Code must not hide important behavior behind overly abstract helpers.

Code must not compress logic so aggressively that review becomes difficult.

Code must not use complex patterns where plain functions, explicit types, and clear control flow are sufficient.

## Simplicity Rule

Prefer the simplest correct implementation.

Do not introduce abstraction until there is a clear need.

Do not introduce generic frameworks for single-use behavior.

Do not introduce macros, decorators, dynamic dispatch, reflection, metaprogramming, or code generation unless the benefit is explicit and reviewable.

Do not use clever one-line expressions when a few plain lines are easier to understand.

Simple code is a security control.

## Function Size Rule

Functions should be small and focused.

A function should do one clear thing.

A function should have a name that states what it does.

A function should avoid mixing validation, transformation, persistence, logging, and reporting unless the function is explicitly an orchestration boundary.

Large functions must be split when they combine unrelated decisions.

## Nesting Rule

Control flow must remain shallow.

Avoid deeply nested conditionals, loops, match statements, callbacks, or promise chains.

Prefer early return for invalid input, denied policy, failed validation, and unsupported cases.

Do not create nested logic that requires a reviewer to hold many states in memory at once.

If logic requires deep nesting, split it into smaller named functions.

## Naming Rule

Names must be plain and descriptive.

Use names that explain domain meaning.

Avoid unclear abbreviations.

Avoid single-letter names except for narrow, conventional loop variables in local contexts.

Avoid names that describe implementation details when the domain concept is more useful.

Examples of acceptable names:

```text
evidence_record
normalized_event
parser_error
finding_status
audit_event
source_metadata
validated_request
````

Examples of weak names:

```text
x
data
thing
obj
tmp2
handleStuff
processIt
```

## Comment Rule

Comments must be minimal and useful.

Comments should explain why code exists, why a defensive decision was made, or why a constraint matters.

Comments should not restate obvious code.

Comments must not compensate for unclear naming or complicated structure.

If code needs many comments to be understandable, simplify the code first.

Acceptable comment purposes:

* explain a security constraint
* explain a governance boundary
* explain a compatibility reason
* explain a non-obvious parser edge case
* explain why a failure path is intentionally strict
* cite a source format assumption where needed

Unacceptable comment purposes:

* narrate every line
* restate function names
* hide unclear logic
* justify unnecessary complexity
* preserve obsolete behavior
* describe future plans better suited for roadmap documents

## Error Handling Rule

Errors must be explicit.

Code must not silently ignore failures.

Code must not collapse distinct failure cases into vague errors when the distinction matters for security, audit, parsing, or analyst review.

Errors should preserve enough context to explain what failed without leaking secrets or sensitive raw data.

Malformed input must produce structured failure where practical.

Unexpected input must not cause uncontrolled panics.

## Defensive Input Rule

All external input must be treated as hostile.

External input includes files, logs, API responses, configuration, imported rules, imported indicators, report templates, UI requests, and model output.

Code handling external input must:

* bound input size where practical
* validate format before use
* reject ambiguous input where authority would be affected
* preserve structured errors
* avoid uncontrolled memory growth
* avoid path traversal
* avoid command injection
* avoid unsafe deserialization
* avoid silent record dropping

Parser output must not become authoritative until validation succeeds.

## Authority Boundary Rule

Code must preserve Arbitor authority boundaries.

Rust is authoritative.

TypeScript is non-authoritative.

Tauri is a controlled command boundary.

UI code must not duplicate or bypass Rust authority.

Scripts must not mutate authoritative state directly.

Model output must not become authoritative without governed review.

Any code that changes authority must be rejected unless the change is explicit, reviewed, and reflected in the appropriate governance documents.

## Mutation Rule

Mutation must be explicit and limited.

Prefer immutable data where practical.

Avoid hidden mutation through shared state.

Avoid broad mutable references.

Avoid mutation across unrelated layers.

State changes must pass through approved paths.

Authoritative state mutation must be typed, validated, auditable, and owned by Rust core logic.

## Data Flow Rule

Data flow must be visible.

A reviewer should be able to trace:

* where input enters
* where validation occurs
* where transformation occurs
* where state changes occur
* where audit records are created
* where output leaves the system

Do not hide data flow behind unclear helpers, global state, implicit context, or overly generic abstractions.

## Dependency Rule

Dependencies must be minimized.

A dependency must solve a clear problem.

Do not add a dependency for convenience when standard library functionality is sufficient.

Authority-layer dependencies require stricter review than UI-only dependencies.

Before adding a dependency, evaluate:

* purpose
* maintenance status
* license compatibility
* known vulnerability history
* transitive dependency size
* platform support
* runtime authority
* network behavior
* filesystem behavior
* cryptographic behavior
* parsing or deserialization behavior
* code execution behavior

## Rust Style

Rust code must be plain, typed, and explicit.

Rust code should use:

* clear module boundaries
* explicit structs and enums
* typed errors
* narrow function responsibilities
* pattern matching when it improves clarity
* early returns for validation failures
* standard library functionality where sufficient
* `Result` for recoverable failure
* tests close to the behavior being verified where appropriate

Rust code must avoid:

* unnecessary macros
* unnecessary generics
* hidden global state
* broad mutable references
* panic paths on untrusted input
* unchecked unwraps on untrusted input
* unsafe code unless separately justified, isolated, reviewed, and tested

`unwrap`, `expect`, and panic behavior may be acceptable in tests or build-time code when the failure is intentional and reviewable. They must not be used on hostile or external runtime input.

## TypeScript Style

TypeScript code must be plain, typed, and non-authoritative.

TypeScript should use:

* explicit interfaces or generated types
* simple components
* readable state transitions
* typed Rust command wrappers
* clear error display
* minimal local state
* presentation-focused logic

TypeScript must avoid:

* browser-side parser authority
* duplicated detection logic
* UI-only finding creation
* direct authoritative mutation
* hidden network calls
* broad access to local files
* secret access
* complex component state where a smaller component is clearer

The UI may present and request. Rust decides.

## Tauri Command Style

Tauri commands must be narrow and explicit.

Each command must have:

* a clear name
* typed request model
* typed response model
* validation path
* error model
* authority boundary check where applicable
* audit behavior where applicable
* malformed request tests

Tauri commands must not expose:

* generic shell execution
* arbitrary filesystem access
* secrets
* hidden network behavior
* unvalidated state mutation
* broad command routers
* unbounded raw instructions

## Script Style

Scripts must be simple and auditable.

Scripts should use strict modes where available.

Shell scripts should use:

```sh
set -euo pipefail
```

Scripts must avoid:

* hidden network calls
* broad destructive commands
* silent failures
* mutation outside their stated scope
* complex shell logic when a clearer language or smaller script is needed
* writing secrets to logs

Scripts must print clear failure messages.

## Test Style

Tests must be clear and behavior-focused.

A test should state the behavior being verified.

Tests should avoid excessive abstraction.

Tests should not rely on hidden ordering unless ordering is the behavior under test.

Security-relevant tests must include negative cases.

Parser tests must include malformed input.

Authority-boundary tests must verify rejection paths.

Finding tests must verify traceability.

Report tests must verify provenance.

Export tests must verify integrity metadata where applicable.

## Schema Style

Schemas must be readable and versioned.

Schemas should use clear field names.

Schemas must not encode ambiguous authority.

Schema changes must be explicit.

Schema validation must reject malformed records where authoritative behavior would otherwise be affected.

Generated types must not be hand-edited.

If TypeScript types are generated from schemas, the generated output must remain generated.

## Logging Style

Logs must be useful and safe.

Logs must not expose:

* secrets
* tokens
* passwords
* private keys
* raw credentials
* sensitive raw log content unless explicitly governed
* unnecessary personal or operational data

Logs should identify:

* operation
* result
* relevant record identifier
* safe error category
* timestamp where applicable

Logs must not replace audit records.

## Audit Style

Audit records are security-relevant records.

Audit behavior must be explicit.

Audit records must preserve enough information to reconstruct security-relevant actions without leaking secrets.

Audit records must be append-only.

Audit records must not be edited to simplify history.

Correction records may be appended where needed.

## Report Generation Style

Report generation code must be deterministic where authoritative.

Reports must be assembled from structured findings, timelines, evidence references, and analyst notes.

Report generation must not invent facts.

Generated prose must not hide uncertainty.

Report text must preserve traceability to source records, findings, timelines, or analyst-approved notes.

AI-generated or template-generated language must remain distinguishable from analyst-authored conclusions.

## Reviewability Rule

Every code change should be easy to review.

A reviewer should be able to answer:

* What changed?
* Why did it change?
* What authority does it affect?
* What input does it accept?
* What output does it produce?
* What can fail?
* What tests cover the behavior?
* What security boundary is involved?
* What records are mutated?
* What audit trail is produced?

If a change cannot be reviewed clearly, it must be simplified before merge.

## Complexity Triggers

The following are signs that code should be simplified:

* deeply nested conditionals
* large functions
* unclear names
* broad mutable state
* unclear ownership
* generic helpers used by unrelated domains
* hidden side effects
* implicit network behavior
* unclear error handling
* comments explaining what unclear code does
* duplicated validation logic
* UI logic duplicating Rust authority
* tests that require understanding internal tricks
* code that is hard to explain in one paragraph

## Prohibited Patterns

The following patterns are prohibited unless separately justified and reviewed:

* hidden outbound network behavior
* generic shell execution exposed to UI
* direct UI mutation of authoritative state
* parser panic paths on malformed input
* silent input dropping
* silent error swallowing
* plaintext secret storage
* logging secrets
* custom cryptography
* clever code that reduces reviewability
* broad filesystem scanning without governed configuration
* dynamic execution of imported rules or templates
* model output becoming authoritative without governed review
* dependency additions without purpose and review

## Preferred Patterns

The following patterns are preferred:

* explicit types
* small functions
* early validation
* early return on failure
* narrow interfaces
* clear error enums
* immutable records
* append-only audit paths
* deterministic transformations
* schema-validated records
* golden tests for parser output
* negative tests for authority boundaries
* boring code that is easy to audit

## Code Review Standard

A code review should reject code when it is:

* hard to understand
* hard to test
* hard to audit
* unnecessarily clever
* deeply nested without need
* weakly typed where types are available
* unclear about failure behavior
* unclear about authority boundaries
* unsafe with external input
* missing negative tests for security-relevant behavior
* inconsistent with Arbitor governance, architecture, invariants, or secure engineering requirements

## Final Coding Rule

Code must be simple enough for an outside engineer to review, secure enough for hostile input, and explicit enough to preserve Arbitor authority boundaries.

If code cannot be understood, tested, and audited, it must not be added.
