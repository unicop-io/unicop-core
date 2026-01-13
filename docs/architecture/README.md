# UNICOP Core — Architecture Documentation

## Navigation
- [Purpose](#purpose)
- [Directory Layout](#directory-layout)
- [Authoritative Boundaries](#authoritative-boundaries)
- [Architecture Domains](#architecture-domains)
- [Governance Rules](#governance-rules)
- [Change Policy](#change-policy)

---

## Purpose

This directory contains the **canonical, authoritative architecture specifications** for the UNICOP platform.

Documents in this directory define:
- System semantics
- Invariants and constraints
- Formal contracts between components

These documents are **normative**.

If an implementation, design, or runtime behavior contradicts anything defined here, the **implementation is wrong**.

---

## Directory Layout

~~~text
unicop-core/docs/architecture/
├── README.md
├── federation/
│   ├── README.md
│   ├── federation-model.md
│   ├── scope-model.md
│   ├── global-directory.md
│   ├── routing-contract.md
│   ├── migration-state-machine.md
│   ├── api-spec.md
│   └── schemas/
│       ├── README.md
│       └── v1/
│           ├── scope.proto
│           ├── shard.proto
│           ├── federation.proto
│           ├── directory.proto
│           └── router.proto
├── desired-state/
├── execution/
├── events/
├── identity/
├── observability/
└── security/
~~~

---

## Authoritative Boundaries

This directory defines **what the system is** and **how it must behave**.

It explicitly does **not** define:
- Concrete technologies (Envoy, Redis, Kafka, etc.)
- Programming languages
- Deployment models
- Performance tuning
- Operational runbooks

Those belong in implementation repositories (e.g. `unicop-federation`).

---

## Architecture Domains

### Federation

Defines:
- Shared-nothing sharding model
- Scope semantics
- Global Directory authority
- Stateless routing contracts
- Migration state machine
- Canonical federation APIs and schemas

Location:
~~~text
docs/architecture/federation/
~~~

---

### Desired State

Defines:
- Desired state representation
- Snapshot semantics
- Versioning rules
- Deterministic projection requirements

Location:
~~~text
docs/architecture/desired-state/
~~~

---

### Execution

Defines:
- Orchestration boundaries
- Execution ownership
- Intra-shard execution rules
- Prohibited cross-boundary behaviors

Location:
~~~text
docs/architecture/execution/
~~~

---

### Events

Defines:
- Event taxonomy
- Event ordering guarantees
- Idempotency rules
- Schema governance

Location:
~~~text
docs/architecture/events/
~~~

---

### Identity

Defines:
- Tenant, account, and scope identity models
- Identity invariants
- Identity propagation semantics

Location:
~~~text
docs/architecture/identity/
~~~

---

### Observability

Defines:
- Required signals (metrics, logs, traces)
- Semantic attributes
- Cardinality rules
- Audit requirements

Location:
~~~text
docs/architecture/observability/
~~~

---

### Security

Defines:
- Trust boundaries
- Isolation requirements
- Authorization invariants
- Cryptographic expectations (conceptual, not tooling)

Location:
~~~text
docs/architecture/security/
~~~

---

## Governance Rules

1. **Normative Language**
   - MUST, MUST NOT, SHOULD, MAY are used deliberately.
   - Lowercase usage is non-normative.

2. **Immutability of Meaning**
   - Meaning must not change without explicit versioning.
   - Refactors that alter semantics are forbidden without review.

3. **Versioning**
   - Breaking architectural changes require:
     - New document version
     - Explicit migration notes
     - Compatibility statement

4. **Single Source of Truth**
   - Architecture meaning lives here.
   - Implementation repositories must conform.

---

## Change Policy

Any change to this directory:
- Is a system-level change
- Requires architectural review
- May impact multiple repositories

Treat this directory as the constitution of UNICOP.
