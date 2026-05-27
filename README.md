# curator

**[derrybirkett.github.io/curator](https://derrybirkett.github.io/curator/)**

A read-only LLM agent that scans a repository for cleanup candidates and files GitHub Issues. Never modifies code.

Part of the [Delta Suite](https://monospace.studio/ideas/product-labs/delta-suite): build → observe → gate. Also part of the [Moirai](https://github.com/derrybirkett/moirai) agent ecosystem (planned).

## What it does

Curator runs on a schedule, scans your codebase for cleanup candidates, and files labelled GitHub Issues. It operates at the **observe** end of the observe → propose → act spectrum — surfaces debt so humans decide what to act on.

| Scope | What it looks for |
|---|---|
| `dead-exports` | Exported symbols with no references in the codebase |
| `oversized-files` | Files that have grown too large and likely need splitting |
| `stale-placeholders` | TBD, Coming soon, lorem ipsum, `[TODO]`, and old year markers |

## What this repo contains

Just the agent definition — prompt, scope-detection logic, config schema, and sane defaults. No workflow files, no config values, no host-specific wiring. Wiring lives in the consuming repo; eventually [Moirai](https://github.com/derrybirkett/moirai) will install and orchestrate it.

```
AGENT.md         Persona and behavioural spec
PROMPT.md        Scanning instructions executed by the LLM
scopes/          Detection logic per category
schema.json      JSON Schema for the consuming repo's config.yml
defaults.yml     Default values for every config field
CHANGELOG.md     Versioned changes (pin a tag in your consuming repo to lock behaviour)
docs/            Pages site source — derrybirkett.github.io/curator
```

## Install

Add as a git submodule:

```bash
git submodule add https://github.com/derrybirkett/curator .curator
```

Then drop a config file at `.github/agents/curator/config.yml` (validated against `schema.json`, overriding `defaults.yml`) and wire a workflow that invokes the prompt with `AGENT_CONFIG_PATH` set to that file. See the [monospace.studio reference wiring](https://github.com/derrybirkett/monospace.studio/blob/main/.github/workflows/nightly-curator.yml) for a working example.

Once Moirai is built, `moirai install curator` will do the wiring for you.

## Configuration

The consuming repo's `config.yml` overrides anything in `defaults.yml`. Common knobs:

- `dry_run: true` — write findings to workflow summary only, file no issues
- `min_confidence` — filter out lower-confidence candidates (`high` | `med` | `low`)
- `max_issues_per_run` — hard cap per run
- `scan_paths` / `exclude_paths` — paths to scan and ignore
- `scopes.<name>.enabled` — toggle individual scopes

Kill switch: create `.curator-pause` at your repo root to halt all runs immediately.

## Safety

- `contents: read` only — never writes to files or commits
- Hard cap on issues per run, with deduplication across runs (skips files that are open or recently closed `[curator]` issues)
- Files in any `curator:wontfix`-labelled issue are skipped permanently
- Dry-run mode available for first-night validation
- Kill switch: `.curator-pause` at repo root

## License

MIT — see [LICENSE](LICENSE).
