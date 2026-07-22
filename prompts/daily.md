# Daily Routine

Follow all global rules in `CLAUDE.md`. This prompt only covers today's steps.

1. Query sources in `config/sources.yaml`, scoped to topics in `config/topics.yaml`, for items published/updated in the last 24–48 hours.
2. Score every retained finding using `config/scoring.yaml`, with a one-line rationale per dimension.
3. Fill `templates/daily_report.md` and save as `reports/daily/YYYY-MM-DD.md`.
4. Sync the report to Notion per `config/notion.yaml` (daily database).
5. Flag any finding matching a composite flag (`PoC Candidate`, `NSTC Relevant`, `High Priority`) for weekly follow-up.

Output: the completed daily report, plus a short confirmation of the Notion sync status.
