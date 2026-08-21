# Megeredchian Claude Code plugins

A private plugin marketplace for Claude Code. This repo is both the marketplace and the plugin.

## What's in it

**`design-system`** — IBM Design Language and Carbon expertise, as one skill:

- `ibm-design-language` — tokens (colour, type, spacing, motion, grid), the 2x Grid, the UI shell and global header, the seventeen universal patterns, status indicators and data visualization, and where Carbon v12 is heading.
- Bundles `carbon-tokens.css`, a drop-in token layer covering the White and Gray 100 themes.
- Bundles `check_contrast.py`, which measures colour pairs against WCAG thresholds and knows the Carbon palette by name.

## Install

```
/plugin marketplace add <your-github-user>/<this-repo>
/plugin install design-system@megeredchian
```

Then `/plugins` to confirm. The skill appears as `design-system:ibm-design-language` and also triggers on its own when the work calls for it.

## Update

Bump `version` in **both** `.claude-plugin/marketplace.json` and `plugins/design-system/.claude-plugin/plugin.json`, commit, push. Then, on any machine:

```
/plugin marketplace update megeredchian
/plugin update design-system@megeredchian
```

Version is what triggers an update. If you don't bump it, nothing changes for anyone.

## Layout

```
.claude-plugin/marketplace.json          the catalogue
plugins/design-system/
  .claude-plugin/plugin.json             the plugin manifest
  skills/ibm-design-language/
    SKILL.md                             decision layer
    references/                          loaded on demand
    assets/carbon-tokens.css
    scripts/check_contrast.py
```

Adding a second skill later means one more folder under `skills/`. Nothing else changes.

## Validate before pushing

```bash
claude plugin validate ./plugins/design-system
claude plugin validate .
```
