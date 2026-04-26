# Spec: CI/CD Pipeline

## Overview

The project MUST automatically build and deploy the documentation site on every push to `main`.

## Trigger

- The pipeline MUST run on push events to the `main` branch.

## Permissions

- The pipeline MUST have `contents: read`, `pages: write`, and `id-token: write` permissions.

## Build Job

- The build job MUST run on `ubuntu-latest`.
- The build job MUST check out the repository using `actions/checkout@v4`.
- The build job MUST set up Python 3.11.
- The build job MUST install dependencies via `pip install -r requirements.txt`.
- The build job MUST build the site with `mkdocs build --site-dir _site`.
- The build job MUST upload the `_site` directory as a GitHub Pages artifact.

## Deploy Job

- The deploy job MUST depend on the build job completing successfully.
- The deploy job MUST run on `ubuntu-latest`.
- The deploy job MUST target the `github-pages` environment.
- The deploy job MUST publish via `actions/deploy-pages@v4`.

## Dependencies

- `requirements.txt` MUST include `mkdocs` and `mkdocs-material`.
- `requirements.txt` SHOULD remain minimal — no unpinned transitive dependencies.
