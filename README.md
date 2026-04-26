# Databricks Exploration

A monorepo for exploring Databricks capabilities across multiple subprojects. The repo doubles as its own AI-assisted development environment — all feature work is driven through an OpenSpec workflow that keeps design, specs, and implementation tasks co-located with the code.

---

## Subprojects

| Folder | Description |
|--------|-------------|
| `county-geocoder/` | API that resolves a (lat, lng) coordinate to a US county |
| `sam-analysis/` | Bulk extraction and analysis of wage data from SAM.gov raw text files |

Each subproject has its own `notebooks/`, source code, and `docs/` folder. All docs feed into a shared MkDocs site published to GitHub Pages.

---

## Shared Infrastructure

| Folder | Purpose |
|--------|---------|
| `docs/` | Top-level site pages (home, references) |
| `skills/` | Claude Code skills for Databricks-specific workflows |
| `openspec/` | Change management — proposals, specs, and tasks |
| `.claude/` | Claude Code settings and project memory |
| `.github/workflows/` | CI/CD — builds and deploys the MkDocs site on push to `main` |

---

## Development Process — OpenSpec

OpenSpec is a lightweight change management workflow that lives inside the repo. Instead of writing specs in a wiki or Jira and letting them drift from the code, every feature goes through a structured cycle:

```
/opsx:propose  →  proposal.md + design.md + tasks.md
/opsx:apply    →  work through tasks one by one
/opsx:archive  →  move completed change to archive
```

Each change lives in `openspec/changes/<change-name>/` and produces three artifacts:

| Artifact | Contains |
|----------|----------|
| `proposal.md` | What we're building and why — scope, goals, non-goals |
| `design.md` | How we're building it — architecture decisions, tradeoffs |
| `tasks.md` | Ordered checklist of implementation steps |

Long-lived behavioral baselines (things that must always be true) live in `openspec/specs/` and are referenced by changes that touch that domain.

### Example: Validating Databricks + S3 Integration

Suppose we want to build a POC that confirms our Databricks workspace can read raw files from S3 before committing to a full pipeline. The workflow would look like this:

**1. Propose the change**
```
/opsx:propose validate that a Databricks notebook can mount an S3 bucket
and read a sample file — this is a prerequisite for the sam-analysis pipeline
```

This generates a `proposal.md` scoping the POC (read one file, log its line count, no writes), a `design.md` capturing the auth approach (instance profile vs. access keys vs. Unity Catalog external location), and a `tasks.md` breaking it into steps:

- [ ] Configure S3 credentials in the Databricks workspace
- [ ] Create a notebook that mounts the bucket or uses `dbutils.fs`
- [ ] Read a sample file and print basic stats
- [ ] Confirm the result and document findings in the subproject docs

**2. Implement task by task**
```
/opsx:apply
```

Claude works through each task, reading the proposal and design for context. If a decision changes mid-implementation (e.g., we switch from `dbutils.fs.mount` to a Unity Catalog external location), that gets captured back in `design.md` rather than lost in conversation history.

**3. Archive when done**
```
/opsx:archive
```

The completed change moves to `openspec/changes/archive/` and the relevant spec in `openspec/specs/` is updated to reflect the new baseline.

---

## Getting Started

```bash
pip install -r requirements.txt
mkdocs serve        # preview the docs site locally
```

For Databricks CLI setup and workspace connectivity, see the [Databricks Core skill](skills/) or the official Databricks CLI docs.
