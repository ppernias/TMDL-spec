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
TMDL for team collaboration. These extensions were not designed first â they emerged from
real deployment needs. The key insight is that they address *different levels of system
complexity*, not different domains of the same complexity level.

This insight leads to the three-level taxonomy.

---

## The three-level taxonomy

### Level 1 â AssistantSpec: ADL 2.0

**What it describes**: A single LLM-based assistant entity.

**File structure**: One YAML file (or a small chain via `extends`).

**Runtime requirement**: The specification is compiled into text injected into the model's
context window. The runtime manages one entity's context.

**The central question it answers**: *What is this assistant?*

**Example**: A tutoring assistant named Ada, a research co-author named Marco, a customer
support assistant for a specific product.

```yaml
adl_version: "2.0"
metadata:
  name: "Ada"
  description: "Educational tutor for prompt engineering"
identity:
  type: tutor
  persistent: true
role:
  primary_function: educational_tutor
  responsibilities:
    - "Guide learners without giving answers"
boundaries:
  must_not:
    - "Provide complete solutions to assessed tasks"
  defer_to_human:
    - "Official grading and certification"
```

---

### Level 2 â Managed Knowledge Entity: TMDL

**What it describes**: A single entity whose knowledge base is externally managed,
evolves during work, and may be shared with other teammates.

**File structure**: An entity spec (like ADL 2.0) plus a knowledge manifest declaring
the team's shared knowledge environment.

**Runtime requirement**: The runtime manages both the context window AND an independent
knowledge store â loading, versioning, and updating documents independently of the
entity specification.

**The central question it answers**: *What is this entity and what is its live knowledge environment?*

**What distinguishes Level 2 from Level 1**: Knowledge in Level 1 (ADL 2.0) consists
of static references injected into the context window. Knowledge in Level 2 (TMDL) has
a *lifecycle*:

- It **existed before** the entity was configured (pre-work documents, prior research)
- It **changes during** operation (in-progress papers, meeting notes, generated artefacts)
- It **may be shared** between multiple teammates on the same project
- It **requires independent runtime management** â the runtime must load, update, and
  version documents independently of the entity spec

**Example**: Marco, a research co-author, who needs access to the current draft of the
ADL 2.0 paper, the TDL technical reference, research notes, and bibliography â all of
which evolve as the research progresses.

```yaml
tmdl_version: "1.0"
extends_adl: "2.0"
metadata:
  name: "Marco"
identity:
  type: teammate
  persistent: true
knowledge_manifest:
  shared:
    - id: paper-adl2
      source: papers/adl_2_0/current_draft.md
      status: in-progress        # pre-existing | in-progress | generated
      access: read-write         # read | read-write | create
      scope: shared              # private | shared
  private:
    - id: working-notes
      source: notes/marco_notes.md
      status: generated
      access: read-write
      scope: private
```

---

### Level 3 â SystemSpec: TDL

**What it describes**: A multi-component educational system with architecturally distinct
functional layers, each in a separate file with a specific role.

**File structure**: A `course_manifest.yaml` (entry point) plus an engine, an instructional
model, one or more learning sequences, content files, and rubrics.

**Runtime requirement**: The runtime resolves a package of files, validates all
cross-references, and orchestrates execution across components. The Course Manifest is
the only file the runtime needs to find â it resolves all others.

**The central question it answers**: *How is this tutoring system organized and how do its
components coordinate?*

**What distinguishes Level 3 from Level 2**: The components are not all "knowledge for
the entity." They have **differentiated functional roles** that the runtime interprets
separately:

- The **engine** handles session mechanics (commands, state tracking)
- The **instructional model** encodes pedagogical strategy (events, transitions)
- The **learning sequence** instantiates pedagogy for a specific lesson
- The **content files** provide domain knowledge
- The **rubrics** define assessment criteria

**Example**: A complete course on prompt engineering with 2 lessons, each following a
Bloom-based pedagogical model, with shared rubrics and independent content files.

```
prompt_engineering_101/
âââ course_manifest.yaml       # entry point
âââ engine.yaml                # session mechanics
âââ instructional_model.yaml   # bloom-4step pedagogy
âââ learning_sequence_01.yaml  # lesson 1 structure
âââ learning_sequence_02.yaml  # lesson 2 structure
âââ rubric_prompt_quality.yaml # shared assessment
âââ content_01.md              # lesson 1 content
âââ content_02.md              # lesson 2 content
```

---

## The manifest as the key architectural primitive

Both TMDL (Level 2) and TDL (Level 3) introduce a **manifest** â a coordination file
that declares what components exist, how they relate, and what the runtime must manage.

This is not coincidental. The manifest is the structural element that separates higher
levels from Level 1:

| Level | Manifest type | What it coordinates |
|-------|---------------|---------------------|
| Level 1 | None | A single specification file |
| Level 2 | Team knowledge manifest | Knowledge documents and their lifecycle |
| Level 3 | Course manifest | Engine, pedagogy, sequences, content, rubrics |

In the **agentic paradigm**, both types of manifest evolve from passive declarations into
active orchestration specifications:

- Level 2 manifest â coordinates stateful agents sharing a persistent knowledge workspace
- Level 3 manifest â orchestrates specialized agents (pedagogy agent, assessment agent,
  content agent) with defined roles and authority boundaries

---

## Why this justifies three papers

The three languages address three genuinely distinct problems:

| Paper | Language | Core contribution |
|-------|----------|------------------|
| ADL 2.0 | Level 1 | A minimal, validated, domain-agnostic core with mandatory boundaries |
| TMDL | Level 2 | A knowledge lifecycle model for teammates working in evolving project contexts |
| TDL | Level 3 | A six-layer package architecture for decoupled, reusable, pedagogically validated tutoring systems |

Each paper can stand alone because each language solves a different problem. Together,
they form a coherent framework because all three share the same core structural vocabulary
and the same agentic evolution path.

---

## Shared structural vocabulary

All three languages use the same concepts, even when the implementation differs:

| Concept | ADL 2.0 | TMDL | TDL |
|---------|---------|------|-----|
| Identity | `identity` | `identity` | `tutor_profile` |
| Role | `role` | `role` | Instructional Model + Learning Sequence |
| Knowledge | `knowledge` (static) | `knowledge` + manifest (dynamic) | Resource layer |
| Tools/Commands | `tools` | `tools` | Engine layer |
| Boundaries | `boundaries` | `boundaries` | Pedagogical rules P1âP7 |
| Collaboration | `collaboration` | `collaboration` + team protocols | Tutor profile style |
| Inheritance | `extends` (spec inheritance) | `extends` (spec inheritance) | `extends` (model conformance) |

---

## Reading guide

To understand the ADL family:

1. **Start with ADL 2.0**: `SPECIFICATION.md` and `docs/ADL2_technical_reference.md`
   explain the core language, boundary design, and inheritance rules.

2. **Then read TMDL**: https://github.com/ppernias/TMDL-spec â understand how the
   knowledge lifecycle model extends ADL 2.0's static references into a managed
   knowledge environment.

3. **Then read TDL**: https://github.com/ppernias/tdl-spec â understand how the
   course manifest and six-layer architecture extend ADL 2.0's single-file model
   into a multi-component system.

4. **Read `docs/adl_architectural_reflections.md`** for the full architectural
   analysis connecting all three languages to the agentic paradigm.
