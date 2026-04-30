# AGENTS.md

This file provides guidance to AI coding agents (e.g. ChatGPT Codex) when working with this repository.

## Project Overview

BSR CodePet — a pixel-art "tamagotchi" that reflects your GitHub commit activity. The page reads data from `data.json`, which is updated automatically by a GitHub Actions workflow. The pet's tier and mood evolve based on commit count over the last 30 days. **Single `index.html` file, zero dependencies, no build step — deployed via GitHub Pages.**

## Development Setup

No installation needed. Open the file directly:

```bash
xdg-open index.html    # Linux
open index.html        # macOS
start index.html       # Windows
```

To test data changes, edit `data.json` locally and reload the page.

## Common Commands

No build / test / lint pipeline. Manual verification in the browser.

GitHub Actions workflows handle stats updates and deployment automatically:
- `update-stats.yml` — runs daily, updates `data.json` via the GitHub API and commits to `main`.
- `deploy-pages.yml` — triggers on push to `main`, publishes to GitHub Pages.

## Code Style

- Single file, vanilla HTML5 + CSS + JS. Do not introduce new dependencies, build steps, or transpilers.
- Keep functions small and focused on a single responsibility.
- Write clear, self-documenting code; only add comments where the logic is non-obvious.
- Do not add unnecessary error handling, logging, or abstractions.

## Architecture Notes

- **Tiers:** determined by commit count (Scrap → Core → Forge → Arc → Apex). Each tier has a distinct colour.
- **Mood:** driven by commit count thresholds (0 = sleeping, ≥5 = charging, ≥15 = active, ≥30 = turbo, ≥60 = APEX).
- **Data source:** `data.json` — fields: `user`, `commits`, `window_days`, `streak`, `repos`, `top_lang`, `updated_at`.
- **Auth:** the `update-stats.yml` workflow requires a `GH_PAT` repository secret.

## Git Workflow

Only two persistent branches exist: `develop` (active work) and `main` (stable releases).

- **Humans:** always commit directly to `develop`. Never commit or push to `main`.
- **AI agents (Claude, Codex, etc.):** open pull requests from a short-lived `claude/*` or `codex/* ` branch. The PR must always target `develop`, never `main`. Delete branch after merge.

Do not create feature or topic branches.
