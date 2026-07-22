# Monthly Routine

Follow all global rules in `CLAUDE.md`. This prompt only covers today's steps.

1. Read all weekly reports from the current month in `reports/weekly/`.
2. Synthesize a strategic rollup: durable vs. transient trends, enterprise adoption signals, field-level shifts.
3. Identify 1–3 PoC candidates (see `PoC Candidate` flag in `config/scoring.yaml`) with a feasibility note each.
4. Identify material relevant to active/upcoming NSTC proposal cycles, organized by proposal section.
5. Fill `templates/monthly_report.md` and save as `reports/monthly/YYYY-MM.md`.
6. Sync the report to Notion per `config/notion.yaml` (monthly database), linking the month's weekly pages.

Output: the completed monthly report, plus a short confirmation of the Notion sync status.
