# Migration State Machine

## Navigation
- [States](#states)
- [Allowed Transitions](#allowed-transitions)
- [Enforcement Matrix](#enforcement-matrix)
- [Transition Semantics](#transition-semantics)

---

## States

Canonical states (v1):

~~~text
STABLE
QUIESCING
LOCKED
PIVOTING
~~~

Definitions:

- **STABLE**
  - Normal operation (read/write allowed)

- **QUIESCING**
  - Preparing for migration (read/write allowed)
  - Used to synchronize data and reach a safe point

- **LOCKED**
  - Read-only allowed
  - Mutating methods rejected

- **PIVOTING**
  - No access allowed
  - Used during final mapping update / cutover

---

## Allowed Transitions

Canonical ordered transitions:

~~~text
STABLE -> QUIESCING -> LOCKED -> PIVOTING -> STABLE
~~~

No other transitions are permitted in v1.

---

## Enforcement Matrix

| State | GET / HEAD / OPTIONS | POST / PUT / PATCH / DELETE |
|---|---|---|
| STABLE | ALLOWED | ALLOWED |
| QUIESCING | ALLOWED | ALLOWED |
| LOCKED | ALLOWED | REJECT (423) |
| PIVOTING | REJECT (503) | REJECT (503) |

Only the router enforces this matrix.

---

## Transition Semantics

Canonical semantics for GD-driven migration:

1. GD sets scope to `QUIESCING`.
2. External migration tooling synchronizes state between shards (out of scope).
3. GD sets scope to `LOCKED`.
4. External tooling completes final reconciliation (out of scope).
5. GD sets scope to `PIVOTING`.
6. GD atomically updates ScopeKey → ShardID mapping.
7. GD sets scope to `STABLE`.

Back to top: [Migration State Machine](#migration-state-machine)

---
