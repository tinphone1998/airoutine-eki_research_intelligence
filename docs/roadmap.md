# Roadmap

Phased plan for evolving EKI Research Intelligence from a manual-assisted routine into a semi-automated research infrastructure.

## Phase 1 — Manual-Assisted Routines (Current)

**Status:** Active

- Daily/weekly/monthly routines run as Claude Code sessions, invoked manually.
- Sources queried live at run time (no local cache/index).
- Reports written directly to `reports/` and mirrored to Notion.
- Paper database exists only as structured entries within the Notion database.

**Goal:** Validate the report structure, scoring rubric, and bilingual format against real research needs before investing in automation.

## Phase 2 — Automated Ingestion & Local Paper Database

**Status:** Planned

- Populate `scripts/` with source-polling utilities (arXiv API, OpenAlex API, GitHub trending) to pre-fetch candidate findings before a session starts, reducing live-fetch latency and rate-limit exposure.
- Introduce a local, file-based paper database (e.g., structured JSON/YAML or SQLite under a new `data/` directory) as the canonical dedup/index layer, with Notion as a generated view rather than the sole structured store.
- Add a lightweight CLI entry point (`scripts/run_daily.py` or similar) to standardize routine invocation and logging.

**Trigger to begin:** Once Phase 1 has produced at least 4–6 weeks of reports and the rubric/template have stabilized.

## Phase 3 — Cross-Report Semantic Search & Trend Dashboards

**Status:** Planned

- Build lightweight embedding-based search over the accumulated `reports/` history to answer questions like "what have we tracked on GraphRAG since Q1?"
- Generate a rolling trend dashboard (score distributions over time, topic volume trends) from the local paper database.
- Explore a static-site generator (e.g., MkDocs) to publish `reports/` and `docs/` as a browsable site for collaborators.

**Trigger to begin:** Once the local paper database (Phase 2) has enough entries (~200+) that manual browsing of `reports/` becomes inefficient.

## Phase 4 — NSTC Proposal Generation Pipeline

**Status:** Planned

- Feed monthly rollups directly into a structured NSTC proposal drafting pipeline: background/motivation sections generated from durable-trend findings, methodology sections informed by tracked PoC candidates.
- Maintain a proposal-cycle calendar cross-referenced against monthly rollup dates so relevant material surfaces automatically ahead of submission deadlines.
- Extend bilingual support to full proposal-document formatting conventions (NSTC template compliance).

**Trigger to begin:** Ahead of the next active NSTC submission cycle, once Phase 3 dashboards make it easy to identify which monthly rollups are proposal-relevant.

## Non-Goals

- This project will not attempt full-text ingestion/storage of copyrighted papers — only metadata, links, and original summaries.
- This project will not replace peer review or independent verification of findings; it accelerates awareness and triage, not validation.
