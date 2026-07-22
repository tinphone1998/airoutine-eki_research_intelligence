# scripts/

Automation and utility scripts supporting the research routines.

**Status:** Empty in Phase 1 (see `docs/roadmap.md`). Currently all research work is performed directly by Claude Code within a session, using its own fetch/search tools per the instructions in `CLAUDE.md` and `prompts/`.

## Planned Contents (Phase 2)

| Script | Purpose |
|---|---|
| `poll_sources.py` | Pre-fetch candidate findings from `config/sources.yaml` ahead of a daily session, to reduce live-fetch latency and rate-limit exposure. |
| `dedupe_papers.py` | Cross-check new findings against the local paper database by DOI/URL before they're added, per `CLAUDE.md` §8. |
| `run_daily.py` / `run_weekly.py` / `run_monthly.py` | Standardized entry points that assemble the right context (`CLAUDE.md`, cadence prompt, relevant prior reports) for a routine invocation. |
| `notion_sync.py` | Push a completed Markdown report into the Notion databases described in `config/notion.yaml`, idempotently (update-in-place, never duplicate). |

## Conventions (once populated)

- Scripts should be runnable independently of any specific LLM session — they prepare/verify data, they do not replace the research judgment performed by Claude Code.
- No secrets in scripts or their default config — read credentials from environment variables (see `.gitignore` for excluded local config patterns).
- Prefer small, single-purpose scripts over a monolithic runner, so individual pieces of automation can be adopted incrementally.
