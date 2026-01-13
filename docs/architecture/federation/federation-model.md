# Federation Model

## Navigation
- [Overview](#overview)
- [Shard Model](#shard-model)
- [Ownership and Authority](#ownership-and-authority)
- [Failure Isolation](#failure-isolation)
- [Global Views](#global-views)

---

## Overview

Federation scales UNICOP horizontally by partitioning ownership across many independent shards.
Federation is a **deployment topology** that preserves the declarative and event-driven architecture.

---

## Shard Model

A **shard** is a complete, isolated UNICOP stack. It is logically and operationally independent.

A shard:
- Owns a set of scopes
- Is authoritative within those scopes
- Exposes a shard-local API for the resources it owns
- Does not depend on other shards for correctness

---

## Ownership and Authority

A shard owns one or more scopes.

Examples of scopes:
- `account:123456789012`
- `tenant:acme-prod`
- `region:us-east-1`

Within its scope, the shard is the absolute source of truth.

Prohibited behaviors:
- Cross-shard database queries
- Cross-shard orchestration jobs
- Cross-shard “business logic calls” between shards

---

## Failure Isolation

Shard failures are isolated by design:
- A shard failure impacts only the scopes it owns
- Other shards continue to operate normally

Federation guarantees:
- Bounded blast radius
- Independent maintenance windows
- Region/tenant/account isolation (as defined by scope assignment)

---

## Global Views

Global views (e.g., “all resources across all scopes”) are produced by **fan-out aggregation**.
Canonical semantics:
- Global views MUST be derived from shard-scoped queries
- Global views MUST NOT require a central mega-database

Fan-out may be performed by:
- A client (UI/CLI) issuing multiple scoped requests
- A dedicated aggregator service (implementation detail; not specified in unicop-core)

Back to top: [Federation Model](#federation-model)

---
