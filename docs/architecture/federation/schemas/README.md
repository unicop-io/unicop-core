# Federation Schemas (Index)

## Navigation
- [Versioning](#versioning)
- [v1 Schemas](#v1-schemas)

---

## Versioning

Schemas are versioned by directory:
~~~text
schemas/v1/
schemas/v2/
...
~~~

The canonical federation spec in `unicop-core` defines only:
- Names
- Fields
- Semantics of fields and states
- Service contracts

Implementations must conform.

---

## v1 Schemas

- [v1/scope.proto](v1/scope.proto)
- [v1/shard.proto](v1/shard.proto)
- [v1/federation.proto](v1/federation.proto)
- [v1/directory.proto](v1/directory.proto)
- [v1/router.proto](v1/router.proto)

Back to top: [Federation Schemas (Index)](#federation-schemas-index)

---
