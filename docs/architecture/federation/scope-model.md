# Scope Model

## Navigation
- [Scope Definition](#scope-definition)
- [Supported Scope Types](#supported-scope-types)
- [ScopeKey Format](#scopekey-format)
- [Scope Encoding in URIs](#scope-encoding-in-uris)
- [Constraints](#constraints)

---

## Scope Definition

A **scope** is the minimal unit of ownership and routing in federation.
Routing decisions are based on the scope extracted from the URI.

---

## Supported Scope Types

Canonical scope types (v1):
- `account`
- `tenant`
- `region`

Additional scope types may be added only by updating:
- `schemas/v1/scope.proto`
- This document
- The router extraction rules in `routing-contract.md`

---

## ScopeKey Format

A **ScopeKey** is the canonical, globally unique routing key:

~~~text
<scope_type>:<scope_id>
~~~

Examples:
~~~text
account:123456789012
tenant:acme-prod
region:us-east-1
~~~

Constraints:
- `scope_type` is a canonical identifier (lowercase ASCII)
- `scope_id` is treated as opaque by the router except for extraction/parsing rules
- ScopeKeys are immutable once created

---

## Scope Encoding in URIs

All routable APIs MUST embed scope in the URI path.

Canonical patterns (v1):

### Account-scoped
~~~text
/api/{provider}/accounts/{account_id}/{resource...}
~~~

### Tenant-scoped
~~~text
/api/tenants/{tenant_id}/{resource...}
~~~

### Region-scoped
~~~text
/api/regions/{region_id}/{resource...}
~~~

Router extraction MUST be possible from the URI alone.

---

## Constraints

1. Routing MUST NOT depend on headers, cookies, or request bodies.
2. The router MUST be able to extract exactly one ScopeKey from any routable request.
3. If a request does not match any routable pattern, it is either:
   - Non-federated (implementation-defined), or
   - Rejected as invalid for federation (see routing contract)

Back to top: [Scope Model](#scope-model)

---
