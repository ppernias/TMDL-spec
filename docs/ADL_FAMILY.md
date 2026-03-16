# The ADL Family
## Relationship between ADL 2.0, TDL, and TMDL

This document explains how the three languages in the ADL family relate to each other,
why each exists, and what architectural role each plays. It is the key document for
understanding why TDL and TMDL are not simply alternatives to ADL 2.0 but necessary
extensions at higher levels of complexity.

---

## Why three languages?

ADL 2.0 was designed as a minimal, domain-agnostic core. As it was applied in practice,
two domain-specific extensions became necessary: TDL for educational tutoring systems and
TMDL for team collaboration. These extensions were not designed first -- they emerged from
real deployment needs. The key insight is that they address *different levels of system
complexity*, not different domains of the same complexity level.

This insight leads to the three-level taxonomy.

---

## The three-level taxonomy

### Level 1 -- AssistantSpec: ADL 2.0

**What it describes**: A single LLM-based assistant entity.

**File structure**: One YAML file (or a small chain via `extends`).

**Runtime requirement**: The specification is compiled into text injected into the model's
context window. The runtime manages one entity's context.

**The central question it answers**: *What is this assistant?*

**Example**: A tutoring assistant named Ada, a research analyst named Alex, a customer
support assistant for a specific product.

### Level 2 -- Managed Knowledge Entity: TMDL

**What it describes**: A single entity whose knowledge base is externally managed,
evolves during work, and may be shared with other teammates.

**Runtime requirement**: The runtime manages both the context window AND an independent
knowledge store -- loading, versioning, and updating documents independently.

**What distinguishes Level 2**: Knowledge has a lifecycle:
- Existed before the entity was configured (pre-existing)
- Changes during operation (in-progress)
- May be shared between teammates
- Requires independent runtime management

### Level 3 -- SystemSpec: TDL

**What it describes**: A multi-component educational system with architecturally distinct
functional layers, each in a separate file.

**File structure**: A `course_manifest.yaml` (entry point) plus engine, instructional
model, learning sequences, content files, and rubrics.

**Runtime requirement**: Resolves a package of files, validates cross-references, and
orchestrates execution across components.

---

## The manifest as the key architectural primitive

Both TMDL (Level 2) and TDL (Level 3) introduce a **manifest** -- a coordination file
that declares what components exist, how they relate, and what the runtime must manage.

| Level | Manifest type | What it coordinates |
|-------|---------------|---------------------|
| Level 1 | None | A single specification file |
| Level 2 | Team knowledge manifest | Knowledge documents and their lifecycle |
| Level 3 | Course manifest | Engine, pedagogy, sequences, content, rubrics |

ADL 2.0 as a **document** defines the three-level taxonomy and the manifest concept for
all levels. ADL 2.0 as a **language** is Level 1 (no manifest). TDL and TMDL instantiate
what ADL 2.0 defines.

In the **agentic paradigm**, manifests evolve from passive declarations into active
orchestration specifications:

- Level 2 manifest -> coordinates stateful agents sharing a persistent knowledge workspace
- Level 3 manifest -> orchestrates specialized agents with defined roles and authority

---

## Why this justifies three papers

| Paper | Language | Core contribution |
|-------|----------|------------------|
| ADL 2.0 | Level 1 | Minimal, validated, domain-agnostic core with mandatory boundaries |
| TMDL | Level 2 | Knowledge lifecycle model for teammates in evolving project contexts |
| TDL | Level 3 | Six-layer package architecture for decoupled, pedagogically validated tutors |

---

## Shared structural vocabulary

| Concept | ADL 2.0 | TMDL | TDL |
|---------|---------|------|-----|
| Identity | `identity` | `identity` | `tutor_profile` |
| Role | `role` | `role` | Instructional Model + Learning Sequence |
| Knowledge | `knowledge` (static) | `knowledge` + manifest (dynamic) | Resource layer |
| Tools | `tools` | `tools` | Engine layer |
| Boundaries | `boundaries` | `boundaries` | Pedagogical rules P1-P7 |
| Inheritance | `extends` (spec) | `extends` (spec) | `extends` (model conformance) |

---

## Reading guide

1. **ADL 2.0**: `SPECIFICATION.md` and `docs/ADL2_technical_reference.md`
2. **TMDL**: https://github.com/ppernias/TMDL-spec
3. **TDL**: https://github.com/ppernias/tdl-spec
4. **Architecture**: `docs/adl_architectural_reflections.md`
