# Changelog

All notable changes to the structure and instruction content of this repository are documented here. This is a documentation/config repository, not a versioned software package, so entries describe **maintainability and consistency changes**, not feature releases.

## [Unreleased] — 2026-07-22 — Maintainability & Duplication Review

A senior-engineer-style review of the initial scaffold, focused on removing duplicated content between `CLAUDE.md`, `prompts/`, and `config/`, and restoring the single-source-of-truth boundaries the architecture was supposed to enforce (see `docs/architecture.md` § Instruction Layer / Configuration Layer).

### Fixed — Duplication

- **`CLAUDE.md` §9–11 (Daily/Weekly/Monthly Routine) duplicated `prompts/daily.md`, `prompts/weekly.md`, `prompts/monthly.md` almost verbatim.** Both files carried the same numbered step lists, so any change to a cadence's workflow required editing two files in lockstep — the exact drift risk `CLAUDE.md`'s own intro paragraph warned against. Collapsed into a single new **§9 Routine Cadence**, which states only the invariants that hold across all three cadences (trigger conditions, the "each cadence consumes only the output of the one below it" rule, write/sync targets) and explicitly defers step-by-step workflow to the prompt files. Sections renumbered: Reliability Rules 12→10, Evaluation Principles 13→11, Translation Principles 14→12.
- **`config/notion.yaml`'s `sync_rules` block restated four behavioral rules already present in `CLAUDE.md` §7** (no-duplicate-pages, mirror-exactly, relation timing, retry-on-failure). Config files should hold structure/parameters, not behavioral rules. Removed `sync_rules` from `notion.yaml`; `CLAUDE.md` §7 is now the sole owner of Notion sync behavior, and `notion.yaml` carries a one-line pointer comment instead.
- **`config/scoring.yaml`'s `rationale_requirement` restated the "every score needs a rationale" rule** that already exists in `CLAUDE.md` §10 (Reliability Rules) — and, in the file header comment, cited the *wrong* section number (§13, Evaluation Principles, instead of the correct §12/now §10). Replaced the restated rule text with a pointer to `CLAUDE.md` §10 and fixed the stale reference.
- **`docs/workflow.md`'s "Weekly Sequence" and "Monthly Sequence" sections re-enumerated the same steps as `prompts/weekly.md` / `prompts/monthly.md`**, a third copy of the same checklist. Trimmed both to a short paragraph covering only what isn't already in the prompt (trigger condition, input scope) with an explicit note that exact steps live in the prompt file.
- **`docs/workflow.md`'s "Handling Gaps → Notion sync failure" bullet restated the same retry rule now centralized in `CLAUDE.md` §7.** Replaced with a pointer.
- **`prompts/daily.md` hardcoded the three composite flag names** (`PoC Candidate`, `NSTC Relevant`, `High Priority`) inline. If a flag is renamed or added in `config/scoring.yaml`, the prompt would silently go stale. Changed to reference `config/scoring.yaml` generically rather than enumerate flag names.

### Changed — Cross-References

- Updated all `CLAUDE.md §N` references across `config/scoring.yaml`, `config/sources.yaml`, `docs/architecture.md`, and `.github/PULL_REQUEST_TEMPLATE.md` to match the renumbered sections.
- `docs/architecture.md`'s two references to `config/notion.yaml`'s (now-removed) `sync_rules` key updated to point to `CLAUDE.md` §7.

### Clarified — Ownership, Not Removed

- The six-domain EKI taxonomy appears in three places (`CLAUDE.md` §3 short definition, `config/topics.yaml` machine-readable slugs/keywords, `docs/research_domains.md` long-form rationale). Reviewed and judged **intentional layering, not harmful duplication** — each serves a different audience (LLM self-containedness, machine query scoping, human rationale) and the wording is not copy-pasted between them. Added an explicit "Canonical source note" to `CLAUDE.md` §3 stating `config/topics.yaml` wins on any naming disagreement, so future edits don't have to re-derive this judgment call.

### Verification

- All four `config/*.yaml` files re-validated with `yaml.safe_load` after edits — no syntax errors introduced.
- Grepped the full repository for `§\d+` and `sync_rules` after every edit to confirm no dangling cross-references remain.

## [Initial] — 2026-07-22 — Repository Scaffold

Initial creation of the `eki-research-intelligence` repository: `CLAUDE.md` global instructions, cadence prompts (`prompts/daily.md`, `weekly.md`, `monthly.md`), configuration (`config/topics.yaml`, `sources.yaml`, `scoring.yaml`, `notion.yaml`), report templates, architecture/workflow/roadmap/research-domain documentation, a worked bilingual sample daily report, and GitHub issue/PR templates.
