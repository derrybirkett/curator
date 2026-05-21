# curator

Autonomous overnight scanner — files GitHub issues for dead exports, oversized files, and stale placeholders. Never modifies code.

Part of the [Delta Suite](https://monospace.studio/ideas/product-labs/delta-suite): build → observe → gate.

## What it does

Curator runs on a schedule, scans your codebase for cleanup candidates, and files labelled GitHub Issues. It operates at the **observe** end of the observe → propose → act spectrum — surfaces debt so humans decide what to act on.

| Scope | What it looks for |
|---|---|
| `dead-exports` | Exported symbols with no references in the codebase |
| `oversized-files` | Files that have grown too large and likely need splitting |
| `stale-placeholders` | TBD, Coming soon, lorem ipsum, and old year markers |

## Install

Add as a git submodule:

```bash
git submodule add https://github.com/derrybirkett/curator .curator
```

Then wire up the GitHub Actions workflow from `config.yml`.

## Configuration

Edit `config.yml` to set dry-run mode, confidence threshold, max issues per run, and which scopes are active.

Kill switch: create `.curator-pause` at your repo root to halt all runs immediately.

## Safety

- `contents: read` only — never writes to files or commits
- Max 5 issues per run, with deduplication across runs
- Dry-run mode on by default for the first 14 nights
