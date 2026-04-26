# Databricks Training — Starter Repo

This repo is a minimal sandbox for validating three things before building anything real:

1. **Databricks connectivity** — can we reach the workspace and run a notebook?
2. **Plugin / extension configuration** — are the right tools (VS Code extension, Databricks CLI, etc.) installed and authenticated?
3. **GitHub integration** — is the repo linked to the workspace and syncing correctly?

Once these three are confirmed, this repo will grow into a full medallion lakehouse project.

---

## Folder Structure

| Folder | Intent |
|--------|--------|
| `data/` | Sample and seed files for future pipeline testing |
| `notebooks/` | Databricks notebooks (bronze / silver / gold layers) |
| `pipelines/` | Standalone Python pipeline scripts |
| `skills/` | Training guides for each project module |

All folders are currently empty placeholders.

---

## Connectivity Checklist

See `PROGRESS.md` for the step-by-step validation checklist.
