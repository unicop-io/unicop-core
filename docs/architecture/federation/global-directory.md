# Global Directory (GD)

## Navigation
- [Responsibility](#responsibility)
- [Authority Rules](#authority-rules)
- [Logical Data Model](#logical-data-model)
- [Consistency Semantics](#consistency-semantics)
- [GD API Contract](#gd-api-contract)

---

## Responsibility

The Global Directory (GD) is the single authoritative source for:
- ScopeKey → Shard mapping
- Migration state for a scope
- Shard metadata (identity and capabilities metadata, versioning metadata)

GD does NOT store:
- Domain resource graphs
- NetBox data
- Events
- Desired state payloads

---

## Authority Rules

1. Only GD may assign a scope to a shard.
2. Only GD may transition migration state for a scope.
3. The router MUST enforce GD migration state.
4. Shards MUST treat GD mapping as authoritative.
5. No component other than GD may claim ownership authority.

---

## Logical Data Model

The logical (implementation-independent) model is defined in protobuf schemas:
- `schemas/v1/directory.proto`
- `schemas/v1/federation.proto`
- `schemas/v1/shard.proto`
- `schemas/v1/scope.proto`

Key logical entities:
- `ScopeMapping` (ScopeKey → ShardID + MigrationState)
- `ShardMetadata` (ShardID → metadata)

---

## Consistency Semantics

GD MUST provide:
- Deterministic ScopeKey resolution
- Atomic mapping updates for a scope
- Linearizable semantics for migration state transitions (logical requirement)

Implementation details (e.g., datastore choice, replication technology) are explicitly out of scope for unicop-core.

---

## GD API Contract

The canonical GD API is specified in:
- `api-spec.md`
- `schemas/v1/directory.proto`

Back to top: [Global Directory (GD)](#global-directory-gd)

---
