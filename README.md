# TMDL â TeamMate Description Language

**TMDL** is a YAML-based specification language for defining virtual teammates powered by LLMs that collaborate effectively with human teams.

TMDL is a **Level 2 Managed Knowledge Entity** within the ADL family: it extends ADL 2.0 with a knowledge lifecycle model and team knowledge manifest.

---

## The ADL family

| Language | Level | What it describes |
|----------|-------|-------------------|
| **ADL 2.0** | Level 1 â AssistantSpec | A single assistant entity |
| **TMDL** | Level 2 â Managed Knowledge Entity | A teammate with evolving knowledge |
| **TDL** | Level 3 â SystemSpec | A complete multi-component tutoring system |

TMDL is Level 2 because teammates work with knowledge that has a lifecycle â not static references. Knowledge existed before configuration, changes during work, may be shared between teammates, and requires independent runtime management.

---

## TMDL structure

Eight core sections plus the new knowledge_manifest:

| Section | Required | Purpose |
|---------|----------|---------|
| `tmdl_version` | yes | Must be "1.0" |
| `metadata` | yes | id, name, authorship |
| `identity` | yes | display_name, personality |
| `role` | yes | type, responsibilities |
| `collaboration` | yes | contribution_style, protocols |
| `tools` | no | commands, decorators |
| `context` | no | team, timeline, resources |
| `knowledge` | no | knowledge refs + lifecycle fields |
| `knowledge_manifest` | no | team shared knowledge environment |
| `boundaries` | yes | must_not, defer_to_human (inherited from ADL 2.0) |

---

## What's new in TMDL vs ADL 2.0

### Knowledge lifecycle model

Each knowledge entry gains three new fields:

```yaml
knowledge:
  - id: paper-draft
    name: "ADL 2.0 Paper Draft"
    source: "papers/adl2_draft.md"
    status: in-progress        # pre-existing | in-progress | generated
    access: read-write         # read | read-write | create
    scope: shared              # private | shared
```

### Team knowledge manifest

```yaml
knowledge_manifest:
  id: my-project-2026
  shared:
    - id: spec-reference
      source: "specifications/reference.md"
      status: pre-existing
      access: read
  private:
    - id: my-notes
      source: "notes/session_notes.md"
      status: generated
      access: read-write
      owner: my-teammate-id
```

---

## Quick example

```yaml
tmdl_version: "1.0"

metadata:
  id: marco-research-teammate
  name: "Marco"

identity:
  display_name: "Marco"
  personality: |
    Analytical research co-author specializing in educational AI.
    Always checks terminology consistency. Flags open questions.

role:
  type: researcher
  responsibilities:
    - "Draft academic paper sections in IEEE format"
    - "Review arguments for logical consistency"

collaboration:
  contribution_style: analytical
  interaction_mode: proactive

boundaries:
  must_not:
    - "Provide harmful or misleading information"
    - "Act autonomously on irreversible actions"
  defer_to_human:
    - "Final decisions on team direction and strategy"
    - "Decisions with external consequences"
```

---

## Validation

```bash
pip install pykwalify pyyaml
pykwalify -d my_teammate.yaml -s spec/schemas/tmdl_schema.yaml
```

---

## Documentation

- [Technical Reference](docs/TMDL_technical_reference.md) â complete spec, schemas, examples
- [ADL Family](docs/ADL_FAMILY.md) â relationship between ADL 2.0, TMDL, and TDL
- [ADL2-spec](https://github.com/ppernias/ADL2-spec) â Level 1 foundation
- [tdl-spec](https://github.com/ppernias/tdl-spec) â Level 3 SystemSpec

---

## License

CC-BY-4.0

## Citation

```
@misc{pernias2026tmdl,
  title={TMDL: A Description Language for Virtual TeamMates in Hybrid Human-AI Teams},
  author={PernÃ­as Peco, Pedro A. and Escobar Esteban, M. Pilar},
  year={2026},
  url={https://github.com/ppernias/TMDL-spec}
}
```
