# UNICOP — Glossary

This glossary defines terms as they are used **within UNICOP**.  
Where a term has a broader industry meaning, the definition below reflects
its **specific meaning and constraints inside the UNICOP architecture**.

---

## Table of Contents

- [Authority](#authority)
- [Audit Spine](#audit-spine)
- [Bounded Reconciliation](#bounded-reconciliation)
- [Control Plane](#control-plane)
- [Desired State](#desired-state)
- [Drift](#drift)
- [Event](#event)
- [Event Spine](#event-spine)
- [Execution Plane](#execution-plane)
- [Identity Plane](#identity-plane)
- [Intent](#intent)
- [Observed State](#observed-state)
- [Reconciliation](#reconciliation)
- [Root](#root)
- [Schema](#schema)
- [Snapshot](#snapshot)
- [Source of Truth (SSOT)](#source-of-truth-ssot)
- [Stratification](#stratification)
- [Time-to-Context](#time-to-context)
- [Truth](#truth)
- [Trust TTL](#trust-ttl)
- [Workload Boundary](#workload-boundary)
- [Summary](#summary)

---

## Authority

The system or component whose view of a resource is considered **canonical**.

In UNICOP:
- NetBox is the authoritative source of *intent* and *desired state*
- Cloud providers are authoritative for *temporary reality*
- Kafka is authoritative for *historical fact*

Authority is explicit and scoped. There is no implicit authority.

[↑ Back to top](#unicop--glossary)

---

## Audit Spine

The immutable, time-ordered record of intent, action, and outcome.

In UNICOP, the audit spine is implemented using Kafka.
It records:
- declared intent
- reconciliation attempts
- execution results
- failures and retries
- drift observations

The audit spine is append-only and replayable.

[↑ Back to top](#unicop--glossary)

---

## Bounded Reconciliation

A reconciliation model where actions are constrained to an explicit,
predefined **Root** (domain of authority).

Instead of reconciling “everything”:
- UNICOP reconciles one Root at a time
- Roots are locked atomically
- Unrelated Roots may reconcile concurrently

This prevents split-brain behavior and unbounded blast radius.

[↑ Back to top](#unicop--glossary)

---

## Control Plane

The system responsible for:
- defining intent
- enforcing constraints
- coordinating change
- recording history

UNICOP is a control plane.
It does not directly execute work everywhere; it **decides what should happen,
in what order, and under what authority**.

[↑ Back to top](#unicop--glossary)

---

## Desired State

The state of infrastructure **as declared by humans or automation**.

In UNICOP:
- Desired state lives in NetBox
- Desired state is validated before execution
- Desired state may temporarily diverge from reality

Desired state is not overwritten by execution failures.

[↑ Back to top](#unicop--glossary)

---

## Drift

Any divergence between:
- desired state (NetBox)
- observed state (cloud, DNS, infrastructure)

Drift is normal and expected.

In UNICOP:
- Drift is detected
- Drift is recorded
- Drift is treated as a signal, not an error

[↑ Back to top](#unicop--glossary)

---

## Event

A discrete, immutable record describing a state transition or observation.

Examples:
- resource.created
- resource.updated
- resource.marked_for_delete
- reconciliation.failed
- drift.detected

Events are facts. They are never modified or deleted.

[↑ Back to top](#unicop--glossary)

---

## Event Spine

The transport and persistence layer for events.

In UNICOP, the event spine is Kafka.
It provides:
- ordering guarantees
- durability
- replay capability
- decoupling between producers and consumers

[↑ Back to top](#unicop--glossary)

---

## Execution Plane

The layer that performs real-world actions.

In UNICOP:
- AWX executes actions
- Ansible playbooks perform provider operations
- Execution is always driven by declared intent

The execution plane does not define truth.

[↑ Back to top](#unicop--glossary)

---

## Identity Plane

The system responsible for authentication, authorization, and identity propagation.

In UNICOP:
- Keycloak is the identity plane
- Humans and systems authenticate via OIDC/SAML
- Identity context flows into audit events

[↑ Back to top](#unicop--glossary)

---

## Intent

A human or automated declaration of *what should exist*.

Intent:
- is explicit
- is validated
- is recorded
- may fail to execute

Intent is never inferred from execution.

[↑ Back to top](#unicop--glossary)

---

## Observed State

The state reported by external systems:
- cloud providers
- DNS servers
- infrastructure APIs

Observed state may be:
- delayed
- inconsistent
- incomplete

UNICOP records observed state but does not treat it as authoritative intent.

[↑ Back to top](#unicop--glossary)

---

## Reconciliation

The process of attempting to align reality with desired state.

Reconciliation may:
- succeed
- partially succeed
- fail

All outcomes are recorded.
Reconciliation does not erase history.

[↑ Back to top](#unicop--glossary)

---

## Root

The smallest unit of consistency and locking.

Examples:
- a VPC
- a DNS zone
- an account / region pair
- an IAM boundary

A Root:
- is reconciled atomically
- is locked during reconciliation
- defines a blast radius

[↑ Back to top](#unicop--glossary)

---

## Schema

The formal definition of:
- resource types
- relationships
- constraints

In UNICOP:
- schema lives in NetBox models
- schema enforces invariants at intent-time
- schema prevents invalid states from being declared

[↑ Back to top](#unicop--glossary)

---

## Snapshot

A point-in-time, logical capture of desired state for a Root.

Snapshots are used to:
- ensure deterministic reconciliation
- avoid mid-run state changes
- support replay and debugging

[↑ Back to top](#unicop--glossary)

---

## Source of Truth (SSOT)

The system whose data is considered authoritative for a given domain.

UNICOP uses **multiple scoped sources of truth**, not a single global one.

Example:
- NetBox → intent
- Kafka → history
- Provider APIs → current reality

[↑ Back to top](#unicop--glossary)

---

## Stratification

The intentional separation of execution paths based on latency,
frequency, and complexity.

Examples:
- DNS updates may use lightweight reconcilers
- Heavy provisioning uses AWX workflows

Stratification prevents overloading the execution plane.

[↑ Back to top](#unicop--glossary)

---

## Time-to-Context

The time required for an engineer to answer:
- why a resource exists
- who authorized it
- what depends on it
- what will break if it is removed

UNICOP’s goal is to reduce Time-to-Context to a single query or view.

[↑ Back to top](#unicop--glossary)

---

## Truth

In UNICOP, truth is not a single value.

Truth consists of:
- declared intent
- observed reality
- recorded history

Conflicts between these are expected and explicit.

[↑ Back to top](#unicop--glossary)

---

## Trust TTL

A bounded time window during which observed state is considered fresh.

If no observation occurs within the Trust TTL:
- the state is marked stale
- confidence is reduced
- reconciliation or inspection is triggered

[↑ Back to top](#unicop--glossary)

---

## Workload Boundary

The explicit line where UNICOP stops managing state.

UNICOP manages:
- infrastructure
- identity
- DNS
- control surfaces

UNICOP does not manage:
- application logic
- request lifecycles
- internal application state

[↑ Back to top](#unicop--glossary)

---

## Summary

This glossary defines **how terms are used inside UNICOP**, not how they are
used casually or in marketing material.

Terms are expected to evolve, but definitions must remain explicit and consistent.

[↑ Back to top](#unicop--glossary)
