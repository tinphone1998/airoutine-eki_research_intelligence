# Weekly Routine

Follow all global rules in `CLAUDE.md`. This prompt only covers today's steps.

1. Read all daily reports from the past 7 days in `reports/daily/`.
2. Identify recurring themes, contradictions, and trend acceleration/deceleration across the week.
3. Re-rank the week's findings; promote any whose score changed given the full week's context.
4. Fill `templates/weekly_report.md` and save as `reports/weekly/YYYY-Www.md`.
5. Sync the report to Notion per `config/notion.yaml` (weekly database), linking the week's daily pages.
6. Update the running watchlist of open questions carried into next week.

Output: the completed weekly report, plus a short confirmation of the Notion sync status.
