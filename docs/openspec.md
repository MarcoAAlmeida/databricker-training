# OpenSpec

[OpenSpec](https://github.com/Fission-AI/OpenSpec) is a spec-driven development framework that creates a structured layer between human intent and AI implementation. The idea: **agree on specs before writing code**, so AI assistants have a clear, versioned source of truth to work from.

## Why use it here

This project is in early stages — `notebooks/`, `pipelines/`, and `skills/` are all placeholders. As content grows, keeping track of *why* changes were made and *what behavior is expected* becomes critical. OpenSpec gives us that audit trail.

## The flow

```
/opsx:propose "feature idea"
      │
      ▼
  openspec/changes/<name>/
    ├── proposal.md   ← why + what
    ├── specs/        ← delta: ADDED / MODIFIED / REMOVED requirements
    ├── design.md     ← how (technical approach)
    └── tasks.md      ← checkbox list — drives /opsx:apply
      │
      ▼
/opsx:apply            ← AI reads tasks.md and implements
      │
      ▼
/opsx:archive          ← delta specs merge into openspec/specs/; change is done
```

## Commands reference

All `/opsx:*` commands run in **Claude Code** (this chat) — not in the terminal.

| Command | What it does |
|---------|--------------|
| `/opsx:propose "idea"` | Creates a new change folder with proposal, delta specs, design, and tasks |
| `/opsx:explore` | Investigate before committing to a direction |
| `/opsx:apply` | Reads `tasks.md` and implements each item, marking checkboxes as it goes |
| `/opsx:archive` | Merges delta specs into `openspec/specs/` and closes the change |
| `/opsx:continue` | Creates the next artifact based on what's missing |
| `/opsx:ff` | Fast-forwards through all planning artifacts at once |
| `/opsx:verify` | Verifies work against specs |

## Project structure

```
openspec/
├── specs/           ← current behavior, by domain (source of truth)
├── changes/         ← one folder per in-progress change
│   └── <name>/
│       ├── proposal.md
│       ├── specs/
│       ├── design.md
│       └── tasks.md
└── config.yaml      ← project context injected into every AI command
```

## Setup (no Node.js required)

The `openspec` CLI is only needed for `openspec init`, which just creates folders. Skip it and create the structure directly:

```bash
mkdir -p openspec/specs openspec/changes
```

Then add `openspec/config.yaml` with project context:

```yaml
project:
  name: databricks-training
  stack: MkDocs, Material for MkDocs, GitHub Pages, Python
  conventions:
    - Databricks brand palette (FF3621 red, 1B2431 navy, DM Sans/Mono)
    - Docs live in docs/, skills in skills/<area>/SKILL.md
    - requirements.txt stays minimal (mkdocs + mkdocs-material only)
```

## Delta specs format

Each change only captures what's *changing* — not the full spec. This prevents conflicts when multiple changes touch the same domain:

```markdown
## ADDED

- The system MUST provide a skill module for databricks-architecture
- Each skill MUST include a SKILL.md with objectives and references

## MODIFIED

- The References page SHOULD link to the architecture skill

## REMOVED

- (none)
```

When archived, these deltas are automatically merged into the relevant file under `openspec/specs/`.

## Initial specs (baseline)

Two spec files document the current state of this project:

- `openspec/specs/site.md` — the MkDocs site (pages, theme, social links)
- `openspec/specs/ci-cd.md` — GitHub Actions pipeline (build → deploy to Pages)

Future changes propose deltas on top of these baselines.
