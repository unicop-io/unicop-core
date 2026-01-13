# Routing Contract

## Navigation
- [Router Responsibilities](#router-responsibilities)
- [Prohibited Behavior](#prohibited-behavior)
- [Scope Extraction Rules](#scope-extraction-rules)
- [Error Semantics](#error-semantics)
- [Migration Enforcement](#migration-enforcement)

---

## Router Responsibilities

The router MUST:
1. Extract ScopeKey from the request URI (see [Scope Extraction Rules](#scope-extraction-rules)).
2. Resolve ScopeKey to `ShardID` via GD (see `api-spec.md`).
3. Enforce migration state as defined in `migration-state-machine.md`.
4. Forward the request to the resolved shard.

The router MUST be:
- Stateless (no persistent state)
- Horizontally scalable
- Deterministic in routing outcomes given the same GD state

---

## Prohibited Behavior

The router MUST NOT:
- Perform domain/business logic
- Mutate request/response payloads (except trace propagation and strictly internal routing metadata, implementation-defined)
- Persist state
- Assign scopes to shards
- Transition migration state (unless acting as an authorized GD client using GD APIs; the authority still remains GD)

---

## Scope Extraction Rules

A request is **routable** if and only if it matches one of the canonical URI patterns.

### Pattern A: Account-scoped
~~~text
/api/{provider}/accounts/{account_id}/...
~~~
Extract ScopeKey:
~~~text
account:{account_id}
~~~

### Pattern B: Tenant-scoped
~~~text
/api/tenants/{tenant_id}/...
~~~
Extract ScopeKey:
~~~text
tenant:{tenant_id}
~~~

### Pattern C: Region-scoped
~~~text
/api/regions/{region_id}/...
~~~
Extract ScopeKey:
~~~text
region:{region_id}
~~~

If a request matches multiple patterns (should not happen), the router MUST reject with `400`.

---

## Error Semantics

Routing error semantics (canonical):

- `400 Bad Request`
  - URI does not match required scope format
  - Ambiguous scope extraction (multiple matches)
  - Malformed scope identifiers

- `404 Not Found`
  - ScopeKey resolves to no known mapping in GD

- `423 Locked`
  - Migration state forbids non-idempotent methods for the scope (see migration matrix)

- `503 Service Unavailable`
  - GD is unavailable or times out
  - Migration state forbids all access (e.g., PIVOTING)
  - Resolved shard is unavailable (implementation may surface as 503)

The exact JSON body format is not specified in unicop-core.
Only status code semantics are canonical.

---

## Migration Enforcement

Migration enforcement MUST follow `migration-state-machine.md`.

The router MUST treat method categories as:

- Read-only: `GET`, `HEAD`, `OPTIONS`
- Mutating: `POST`, `PUT`, `PATCH`, `DELETE`

Back to top: [Routing Contract](#routing-contract)

---
