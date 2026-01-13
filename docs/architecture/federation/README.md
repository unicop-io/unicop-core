# UNICOP Core — Federation Architecture (Canonical Specification)

## Navigation
- [Purpose](#purpose)
- [Scope](#scope)
- [Design Principles](#design-principles)
- [Terminology](#terminology)
- [Document Map](#document-map)
- [Schema Map](#schema-map)

---

## Purpose

This specification defines how UNICOP scales beyond a single control-plane instance by introducing:
- **Shared-nothing shards**
- **Global Directory (GD)** for Scope→Shard authority
- **Stateless routing semantics** based on scope encoded in the URI
- **Migration state machine** enforced at the router boundary

This is a **canonical, implementation-independent specification**.

---

## Scope

This specification defines:
- Federation semantics and invariants
- Shard ownership boundaries
- Scope model and ScopeKey format
- Router contract (semantics)
- Global Directory contract (authority + API)
- Migration state machine and enforcement matrix
- Canonical schemas (protobuf)

This specification explicitly does **not** define:
- Proxy choice (Envoy/NGINX/etc.)
- Storage backends (Redis/etcd/etc.)
- WASM/Lua/Go/Rust implementations
- Deployment topology, Helm charts, Kubernetes manifests

All implementation details belong outside `unicop-core`.

---

## Design Principles

### P1. Shared-Nothing Shards
Each shard is a complete, isolated UNICOP control-plane instance.
No shard may query another shard’s database, Kafka, caches, or internal APIs.

### P2. Scope-First Routing
Every request that must be routed to an owning shard MUST embed scope in the URI.
Routing MUST NOT depend on headers, cookies, or request bodies.

### P3. Single Source of Truth for Ownership
Scope ownership is defined only by the Global Directory (GD).
The router enforces GD state and forwards to the owning shard.

### P4. Router is Stateless
The router stores no persistent state.
Any caching is an implementation detail and does not change semantics.

### P5. Router is Enforcement Point
Migration state and write locks are enforced at the router boundary only.
Shards do not contain federation safety logic.

### P6. No Global Resource IDs
Resource IDs are only unique within a shard.
Global identity is always `(shard_id, local_resource_id)`.

---

## Terminology

| Term | Definition |
|---|---|
| Shard | A complete, isolated UNICOP control-plane instance owning a set of scopes |
| Scope | Minimal unit of ownership and routing (account, tenant, region, etc.) |
| ScopeKey | Canonical routing key in the form `<scope_type>:<scope_id>` |
| Global Directory (GD) | Authoritative registry for ScopeKey → Shard mapping and migration state |
| Router | Stateless component that routes requests based on ScopeKey and enforces migration state |
| Federation | Coordinated operation of multiple shards under the same logical UNICOP control plane |

---

## Document Map

- [federation-model.md](federation-model.md) — shard model, ownership, failure isolation
- [scope-model.md](scope-model.md) — scope types, ScopeKey format, URI encoding rules
- [global-directory.md](global-directory.md) — GD authority, data model, and contract semantics
- [routing-contract.md](routing-contract.md) — router responsibilities, prohibited behavior, error semantics
- [migration-state-machine.md](migration-state-machine.md) — migration FSM and enforcement matrix
- [api-spec.md](api-spec.md) — canonical APIs (protobuf service contracts) for federation components
- [schemas/README.md](schemas/README.md) — schema index and versioning rules

---

## Schema Map

Schemas are versioned under `schemas/v1/`:
- `scope.proto`
- `shard.proto`
- `federation.proto`
- `directory.proto`
- `router.proto`

Back to top: [README.md](#unicop-core--federation-architecture-canonical-specification)

---
