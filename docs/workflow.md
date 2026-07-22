# Workflow

This document describes the operational detail of running EKI Research Intelligence day to day.

## Cadence Overview

| Cadence | Trigger | Input | Output | Prompt |
|---|---|---|---|---|
| Daily | Every day (manual or scheduled) | `config/sources.yaml`, `config/topics.yaml` | `reports/daily/YYYY-MM-DD.md` | `prompts/daily.md` |
| Weekly | Every 7th day | Past 7 daily reports | `reports/weekly/YYYY-Www.md` | `prompts/weekly.md` |
| Monthly | Calendar month end | All weekly reports for the month | `reports/monthly/YYYY-MM.md` | `prompts/monthly.md` |

## Daily Sequence

```mermaid
sequenceDiagram
    participant U as User
    participant CC as Claude Code
    participant Src as Sources
    participant Repo as Repo (reports/)
    participant Notion as Notion

    U->>CC: Run prompts/daily.md
    CC->>CC: Load CLAUDE.md global rules
    CC->>Src: Query per config/sources.yaml + topics.yaml
    Src-->>CC: Candidate findings
    CC->>CC: Filter to last 24-48h, score per scoring.yaml
    CC->>Repo: Write reports/daily/YYYY-MM-DD.md
    CC->>Notion: Create/update Daily page
    CC-->>U: Report + sync confirmation
```

## Weekly Sequence

1. Trigger weekly synthesis (manually, or on a 7-day schedule).
2. Claude Code reads the last 7 files in `reports/daily/`.
3. Cross-report synthesis: themes, contradictions, trend direction.
4. Re-scoring pass: findings may be promoted if the week's full context changes their assessed value.
5. Write `reports/weekly/YYYY-Www.md`; sync to Notion, linking source daily pages.

## Monthly Sequence

1. Trigger monthly rollup at month end (manually, or scheduled).
2. Claude Code reads all `reports/weekly/*.md` files for the month.
3. Strategic synthesis: durable vs. transient trends, PoC candidates, NSTC-relevant material.
4. Write `reports/monthly/YYYY-MM.md`; sync to Notion, linking source weekly pages.

## Running a Routine

Routines are invoked as ordinary Claude Code sessions inside this repository. There is no CLI wrapper (yet — see `docs/roadmap.md` Phase 2). A typical invocation:

```text
# From within the repository, with CLAUDE.md auto-loaded:
"Run today's daily routine per prompts/daily.md"
```

Claude Code will load `CLAUDE.md` automatically as project instructions; the cadence prompt is passed explicitly (or referenced by name) to select which routine runs.

## Handling Gaps

- **Missed daily run:** Do not backfill fabricated findings for the missed date. Either skip the date explicitly (note the gap in the next weekly report) or run a best-effort catch-up scoped to the missed window, clearly labeled as a catch-up run.
- **Missed weekly run:** The next weekly run should widen its daily-report window to cover the full unsynthesized period, noted explicitly in the report.
- **Notion sync failure:** The Markdown file in `reports/` is authoritative regardless. Retry the sync on the next run; do not block report creation on sync success.

## Review Cadence

Because reports accumulate quickly, periodically review `config/topics.yaml` and `config/sources.yaml` (suggested: every monthly rollup) to prune stale topics/sources and add newly relevant ones, keeping the signal-to-noise ratio high.
