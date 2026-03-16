# CLAUDE.md — Knowledge index for AI tools

Read this file first before answering any question about this repository.

---

## What this repository is

TMDL (TeamMate Description Language) is a YAML-based specification language for defining
virtual teammates powered by LLMs. It is the **Level 2 Managed Knowledge Entity** in the
ADL family — extending ADL 2.0 with a knowledge lifecycle model and team knowledge manifest.

---

## The three-level taxonomy (critical)

| Level | Language | What it describes |
|-------|----------|-------------------|
| Level 1 | **ADL 2.0** | A single assistant entity |
| Level 2 | **TMDL** | A teammate + managed evolving knowledge |
| Level 3 | **TDL** | A complete multi-component tutoring system |

TMDL is Level 2 because its knowledge has a LIFECYCLE:
- Existed before the teammate was configured (pre-existing)
- Changes during operation (in-progress)
- Is created by the teammate (generated)
- May be shared between multiple teammates
- Requires independent runtime management beyond context injection

---

## TMDL structure (9 sections)

| Section | Required | Key fields |
|---------|----------|-----------|
| tmdl_version | yes | must be "1.0" |
| metadata | yes | id (required), name (required) |
| identity | yes | display_name, personality (both required) |
| role | yes | type, responsibilities (both required) |
| collaboration | yes | contribution_style (required) |
| tools | no | commands, options, decorators |
| context | no | team_members, timeline, resources |
| knowledge | no | + status, access, scope (new TMDL fields) |
| knowledge_manifest | no | shared, private knowledge environment |
| boundaries | yes | must_not (min 1), defer_to_human (min 1) |

---

## What's new vs ADL 2.0

Three structural additions define Level 2:

1. **knowledge.status** — pre-existing | in-progress | generated
2. **knowledge.access** — read | read-write | create
3. **knowledge.scope** — private | shared
4. **knowledge_manifest** — team knowledge environment (new section)
5. **context** — team members, timeline, resources (new section)
6. **collaboration.contribution_style** — required (analytical|supportive|generative|integrative|executive)
7. **collaboration.disagreement_approach** — defer|voice|dialogue|investigate
8. **role.boundaries.can_do / defers_to** — role-level constraints

---

## File map

| Task | Files |
|------|-------|
| Complete specification | docs/TMDL_technical_reference.md |
| ADL family context | docs/ADL_FAMILY.md |
| Validate a spec | spec/schemas/tmdl_schema.yaml |
| Working example (Marco) | examples/marco-researcher.yaml |
| Base example | examples/ana-analyst.yaml |

---

## Terminology

| Correct | Never use |
|---------|-----------|
| Level 2 — Managed Knowledge Entity | "enhanced assistant" |
| knowledge lifecycle | "dynamic knowledge" |
| knowledge manifest | "knowledge index" |
| contribution_style | "work style" |
| pre-existing / in-progress / generated | "static / dynamic" |
