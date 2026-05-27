# Changelog

All notable changes to the Curator agent.

The format is based on [Keep a Changelog](https://keepachangelog.com/), and this project adheres to [Semantic Versioning](https://semver.org/). Consuming repos pin a tag (e.g. `v0.1.0`) to lock behaviour.

## [0.1.0] — 2026-05-27

Initial extraction from the [monospace.studio](https://github.com/derrybirkett/monospace.studio) `shulkerbox` submodule into its own repo. This is the first piece of the [Moirai](https://github.com/derrybirkett/moirai) walking-skeleton plan — agent definition lives here; wiring and configuration live in consuming repos.

### Added
- `AGENT.md` — persona, trust level, output rules, what curator is not.
- `PROMPT.md` — full scanning instructions executed by the LLM.
- `scopes/dead-exports.md`, `scopes/oversized-files.md`, `scopes/stale-placeholders.md` — per-category detection logic.
- `schema.json` — JSON Schema for the consuming repo's `config.yml`.
- `defaults.yml` — sane defaults for every config field. Consuming repos only specify overrides.
- `README.md`, `LICENSE` (MIT).

### Changed (relative to the previous `shulkerbox/agents/curator/` location)
- Prompt now reads config from `$AGENT_CONFIG_PATH` (host-injected) instead of hard-coded `shulkerbox/agents/curator/config.yml`.
- `AGENT.md` exclude-paths language is no longer tied to the shulkerbox submodule — it points at the consuming repo's `exclude_paths` config.
- Issue body footer references "your repo's curator config" rather than a shulkerbox path.

### Removed
- The literal `config.yml` (with monospace.studio-specific values) does not ship in this repo. That belongs in the consuming repo.
