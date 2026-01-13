# UNICOP — Roadmap (Public / Lightweight)

*A high-level view of where UNICOP is going, why the sequence matters, and what is intentionally deferred.*

This document is **public-facing**.  
It describes **direction and sequencing**, not internal mechanics or R&D detail.

---

## Table of Contents

- [Purpose](#purpose)
- [Guiding Principles](#guiding-principles)
- [Current Focus (Now)](#current-focus-now)
- [Next Milestones](#next-milestones)
- [Deferred (Intentional Non-Goals for Now)](#deferred-intentional-non-goals-for-now)
- [What This Roadmap Is Not](#what-this-roadmap-is-not)
- [Relationship to Internal Documents](#relationship-to-internal-documents)
- [Summary](#summary)

---

## Purpose

The purpose of this roadmap is to communicate:

- **what UNICOP is actively building**
- **what comes next**
- **what is deliberately postponed**
- **why the order matters**

It is designed for:
- senior engineers
- architects
- platform and SRE leaders

This is **not** a promise of dates.  
It is a statement of **engineering intent and prioritization**.

[↑ Back to top](#unicop--roadmap-public--lightweight)

---

## Guiding Principles

UNICOP development follows strict ordering constraints:

- **Architecture before UX**
- **Declarative models before automation**
- **Bounded reconciliation before scale**
- **Observability before convenience**
- **One deployment model before variants**

Anything that violates these principles is deferred.

[↑ Back to top](#unicop--roadmap-public--lightweight)

---

## Current Focus (Now)

### 1. Core Control Plane Foundations

Active work is focused on stabilizing:

- NetBox-based declarative models (authoritative desired state)
- Clear object boundaries and ownership
- Deterministic relationships and constraints
- Snapshotting and bounded inventory
- Event emission as a first-class signal

This establishes **relational truth** as the system’s foundation.

---

### 2. Event Spine and Execution Wiring

In parallel:

- Kafka as the durable audit and event spine
- Event schemas stabilized
- Deterministic event ordering per domain
- Event → execution dispatch via EDA and AWX

The goal is **honest, observable state transitions**, not speed.

---

### 3. Reconciliation (MVP)

Current reconciliation goals:

- bounded per-root reconciliation
- explicit success/failure outcomes
- no silent retries
- no hidden side effects

This is the minimum viable **control loop**, not a full self-healing engine.

[↑ Back to top](#unicop--roadmap-public--lightweight)

---

## Next Milestones

### 4. Security and Governance Planes

Once the core loop is stable:

- Identity integration (OIDC / SSO)
- Secrets and PKI as first-class systems
- Minimal, declarative IAM governance
- Temporary human access with TTL
- Explicit drift detection policies

Security is treated as **foundational**, not an add-on.

---

### 5. Federation-Ready Architecture

Without changing core logic:

- support multiple independent control-plane instances
- clear ownership boundaries (accounts, tenants, regions)
- stateless routing based on scope
- failure isolation by design

Federation is a **deployment concern**, not a rewrite.

---

### 6. Observability and Auditability

Before any user-facing UX expansion:

- per-root reconciliation state
- drift visibility
- event timelines
- failure classification
- operator-first diagnostics

If operators cannot understand failure, the system is incomplete.

[↑ Back to top](#unicop--roadmap-public--lightweight)

---

## Deferred (Intentional Non-Goals for Now)

The following are **explicitly postponed**:

- a unified end-user UI
- self-service catalogs
- graphical workflow builders
- multi-deployment distributions
- SaaS / hosted offering
- marketplace integrations

Deferral is intentional to avoid architectural debt.

[↑ Back to top](#unicop--roadmap-public--lightweight)

---

## What This Roadmap Is Not

This roadmap is **not**:

- a feature checklist
- a marketing plan
- a delivery schedule
- a promise of timelines
- a comparison matrix

Those belong elsewhere.

This document exists to show **discipline and sequencing**.

[↑ Back to top](#unicop--roadmap-public--lightweight)

---

## Relationship to Internal Documents

This public roadmap is derived from **much deeper internal material**, including:

- detailed architecture contracts
- IAM governance models
- federation and sharding designs
- UI strategy and phased rollout plans
- desired-state projection and reconciliation mechanics
- monetization and product strategy drafts

Those documents exist, but are **not published at this stage**.

[↑ Back to top](#unicop--roadmap-public--lightweight)

---

## Summary

UNICOP is being built as a **long-lived control plane**, not a convenience tool.

The roadmap reflects that reality:

- correctness before velocity
- constraints before flexibility
- observability before abstraction
- one clean path forward, not many half-finished ones

Everything else can wait.

[↑ Back to top](#unicop--roadmap-public--lightweight)
