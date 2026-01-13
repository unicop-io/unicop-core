
# UNICOP Documentation

This directory contains **public-facing documentation** for the UNICOP project.

The goal of this documentation is to explain **what UNICOP is**, **why it exists**, and **how it is intended to be used and evaluated** by engineers, architects, and technical decision-makers.

This is **not** the place for internal R&D notes, exploratory ideas, or unfinished design drafts.

---

## Scope of This Directory

Content in `docs/` is:

- stable
- externally readable
- suitable for GitHub
- aligned with the current architectural direction
- written for engineers outside the core UNICOP team

Anything experimental, speculative, or subject to rapid iteration **does not belong here**.

---

## Document Index

### 📌 Public Roadmap
- [`roadmap.md`](./roadmap.md)  
  A lightweight, public roadmap describing:
  - current focus areas
  - near-term milestones
  - long-term direction  
  This roadmap intentionally avoids deep implementation detail.

---

### 🧱 Architecture (Authoritative)
- [`./architecture/`](./architecture/)  
  Canonical architectural documents:
  - control-plane invariants
  - governance models
  - IAM strategy
  - UI strategy (architectural intent)
  - system boundaries and contracts  

If you want to understand **how UNICOP actually works**, this is the most important directory.

---

### 📖 Glossary (if present)
- [`glossary.md`](./glossary.md) 
  Canonical definitions of terms used across UNICOP documentation.

---

## What Is Explicitly Not Here

The following are **not** stored in `docs/`:

- internal roadmap drafts
- monetization whitepapers
- exploratory R&D documents
- competing or unresolved design alternatives
- implementation notes tied to active refactors

Those live in the `roadmap/` directory or in internal working documents.

---

## Reading Order (Suggested)

1. [`../docs/roadmap.md`](../docs/roadmap.md)
2. [`./architecture/README.md`](./architecture/README.md)
3. Core architecture documents referenced from there

---

## Stability Contract

Documents in this directory change **slowly** and **intentionally**.  
Breaking conceptual changes must be reflected consistently across architecture documents before being exposed here.

This directory represents UNICOP’s **public technical posture**.
