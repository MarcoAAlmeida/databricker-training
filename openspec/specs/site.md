# Spec: Documentation Site

## Overview

The project MUST publish a static documentation site via MkDocs deployed to GitHub Pages.

## Theme

- The site MUST use the Material for MkDocs theme.
- The site MUST apply a custom Databricks brand palette:
  - Primary and accent color: `#FF3621` (Databricks red)
  - Text color: `#1B2431` (dark navy)
  - Code background: `#1B2431`, code foreground: `#F5F5F5`
- The site MUST use DM Sans for body text and DM Mono for code.
- The site MUST display the Databricks logo (`simple/databricks`) as the site icon.

## Navigation

The site MUST include the following top-level pages:

| Page | File | Purpose |
|------|------|---------|
| Home | `docs/index.md` | Three-item validation checklist (connectivity, plugins, GitHub) |
| References | `docs/references.md` | Curated links with icons |
| OpenSpec | `docs/openspec.md` | OpenSpec workflow documentation |

## Footer Social Links

The site MUST display the following social links in the footer:

- GitHub repo (`fontawesome/brands/github`)
- Databricks workspace (`simple/databricks`)
- AWS Console us-east-2 (`fontawesome/brands/aws`)

## Features

- The site SHOULD enable `navigation.instant` for fast page transitions.
- The site SHOULD enable `navigation.top` (back-to-top button).

## Markdown Extensions

- The site MUST support `attr_list` for attribute annotations.
- The site MUST support `pymdownx.emoji` using Material/TwEmoji for icon rendering.
