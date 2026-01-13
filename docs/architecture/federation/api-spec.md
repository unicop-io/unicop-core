# Federation API Specification (Canonical)

## Navigation
- [Overview](#overview)
- [Global Directory API](#global-directory-api)
- [Router Contract APIs](#router-contract-apis)
- [Schema Versioning Rules](#schema-versioning-rules)

---

## Overview

This document specifies the canonical APIs and schemas required for federation:
- Global Directory (GD) service contract
- Router-facing resolution calls
- Data structures used across federation components

This specification is expressed in protobuf (`schemas/v1/*.proto`) and is canonical.

---

## Global Directory API

The Global Directory API is defined in:
- `schemas/v1/directory.proto`

The GD MUST expose an API that supports:
- Resolving a ScopeKey to a ScopeMapping
- Getting shard metadata
- Listing shards (for health/aggregation planning)
- Updating mappings and states (authorized operations)
- Atomic state transitions for migration

The protobuf service is canonical; the transport (gRPC/HTTP2/etc.) is a deployment detail.

---

## Router Contract APIs

Router contract schemas are defined in:
- `schemas/v1/router.proto`

The router MUST rely on GD resolution (`ResolveScope`) as the authority for:
- `shard_id`
- `migration_state`

The router MUST enforce:
- `migration_state` enforcement matrix from `migration-state-machine.md`

---

## Schema Versioning Rules

- All federation schemas are versioned under `schemas/v1/`
- Breaking changes require a new version directory (`schemas/v2/`)
- Components may be in version skew; compatibility policy is defined outside unicop-core

Back to top: [Federation API Specification](#federation-api-specification-canonical)

---
