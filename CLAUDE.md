# CLAUDE.md — Global Instructions for EKI Research Intelligence

This file is the standing instruction set for Claude Code when operating inside the `eki-research-intelligence` repository. It applies to **every** routine (`prompts/daily.md`, `prompts/weekly.md`, `prompts/monthly.md`) and to any ad-hoc research task run in this repo. Cadence-specific prompt files stay intentionally short — they describe *what* to do today; this file describes *how* to do it, always.

If a cadence prompt and this file conflict, this file wins unless the user explicitly overrides it for the current session.

---

## 1. Project Vision

Build a durable, compounding research intelligence system for **Enterprise Knowledge Intelligence (EKI)** — one that gets more valuable every week it runs, because each report builds on the structured memory of the ones before it. The system should let a single researcher maintain literature awareness, enterprise product awareness, and proposal-readiness at a level normally requiring a small team.

The repository is both the **tool** (prompts, config, templates) and the **output** (the accumulated reports) — the two are versioned together so that the reasoning behind every scored finding is reconstructable later.

## 2. Research Objectives

1. Maintain continuous, low-latency awareness of AI research relevant to enterprise knowledge systems.
2. Track how academic advances translate (or fail to translate) into enterprise products.
3. Build a standing corpus of scored, categorized findings usable for literature review sections of papers and proposals.
4. Identify proof-of-concept (PoC) opportunities where current lab-stage research is enterprise-ready.
5. Continuously surface material relevant to NSTC (國家科學及技術委員會) research proposal cycles.
6. Produce a bilingual knowledge trail (English + Traditional Chinese) usable both internationally and domestically.

## 3. Enterprise Knowledge Intelligence (EKI) — Definition

**Enterprise Knowledge Intelligence** is the discipline of applying AI systems — LLMs, retrieval architectures, knowledge graphs, and autonomous agents — to how organizations **capture, structure, retrieve, govern, and act on knowledge**. It sits at the intersection of:

- **Information Retrieval & RAG** — retrieval-augmented generation, hybrid search, re-ranking, grounding.
- **Knowledge Representation** — knowledge graphs, ontologies, ontology-grounded LLMs, ontology-free structured memory.
- **Enterprise Search & Assistants** — copilot-style assistants over internal document stores, permissions-aware retrieval.
- **Agentic Workflows** — multi-step, tool-using agents that act on enterprise knowledge (not just answer questions about it).
- **Knowledge Governance** — provenance, access control, hallucination mitigation, auditability of AI-derived answers.
- **Knowledge-Worker Productivity** — measurable impact of AI copilots on research, engineering, legal, and analyst workflows.

Every finding tracked by this repository should be classifiable against at least one of these six sub-domains (see `docs/research_domains.md`).

## 4. Research Scope

**In scope:**
- Peer-reviewed and preprint papers on RAG, knowledge graphs, enterprise agents, LLM reliability/grounding, and knowledge-worker productivity.
- Enterprise AI product launches, feature updates, and technical blog posts from major vendors (Microsoft, Google, Salesforce, ServiceNow, Glean, Notion, Anthropic, OpenAI, etc.).
- Open-source tooling relevant to enterprise RAG/agent stacks (orchestration frameworks, vector databases, evaluation harnesses).
- Academic venues: NeurIPS, ICML, ICLR, ACL, EMNLP, NAACL, SIGIR, CIKM, KDD, and enterprise-track workshops.

**Out of scope (unless explicitly requested):**
- General consumer AI news with no enterprise/knowledge angle.
- Pure hardware/infrastructure stories (chips, datacenters) without a knowledge-systems angle.
- Speculative commentary/opinion pieces without a primary source (paper, product doc, or official announcement).

## 5. Writing Style

- Lead with the finding, not the source. State what changed or what was shown before naming who did it.
- Every claim about a paper or product must be traceable to a primary source (link + date).
- Prefer concrete numbers (benchmark deltas, latency, cost) over adjectives ("significant," "powerful").
- Use tables for comparative information (multiple papers/products against the same criteria).
- Use Mermaid diagrams only when they clarify a structure or flow that prose would make harder to follow — never decoratively.
- Keep daily entries terse (2–5 sentences per finding). Save synthesis and narrative for weekly/monthly reports.
- No filler openers ("In today's rapidly evolving AI landscape..."). Start with substance.

## 6. Bilingual Rules

All reports (daily, weekly, monthly) are bilingual: **English primary, Traditional Chinese (繁體中文) secondary.**

- Structure: each major section has an English block followed immediately by its Traditional Chinese counterpart, clearly labeled (`### English` / `### 中文`), rather than interleaving sentence-by-sentence.
- The English version is the source of truth; the Chinese version is a faithful research-register translation, not a casual paraphrase.
- Technical terms (model names, benchmark names, architecture names) stay in English in both versions on first mention, with a Chinese gloss in parentheses only if the term is not already common in Taiwanese AI research usage (e.g., `檢索增強生成 (Retrieval-Augmented Generation, RAG)`).
- Scores and metrics are never translated (numbers and units are language-neutral).
- Titles of papers/products stay in their original language; do not translate proper nouns or product names.

See §12 for detailed translation principles.

## 7. Notion Workflow

Notion is the durable, searchable home for all reports; the Markdown files in `reports/` are the git-versioned source of truth that get mirrored there.

- Configuration lives in `config/notion.yaml` (parent page, database name, page-naming conventions).
- Every daily report creates one Notion page as a row in the configured database, named per the `daily_page_naming` pattern.
- Every weekly/monthly report similarly creates one page each, linked back to the daily pages it synthesizes (use Notion relation properties where available).
- Never overwrite an existing Notion page silently — if a page for a given date/period already exists, update it explicitly and note in the session output that an update (not a create) occurred.
- Notion pages mirror the Markdown template structure exactly (see `templates/`) so the two representations never drift.

## 8. Paper Database Workflow

The paper database is the cumulative record of every scored finding, intended to seed literature review sections and citation tracking.

- Every paper/product referenced in a daily report must be logged with: title, authors/org, venue or source, publication date, URL, one-line summary, and full score set (§ scoring).
- Deduplicate by canonical URL/DOI before adding a new entry — if a paper reappears (e.g., a preprint later published at a venue), update the existing entry rather than duplicating it.
- The paper database is maintained as structured entries inside the Notion database (see `config/notion.yaml`) and mirrored in `reports/` history; a local file-based export/index is a Phase 2 roadmap item (see `docs/roadmap.md`).
- When scores change materially on re-review (e.g., a paper's enterprise relevance becomes clearer after a product launch cites it), update the entry and note the revision date and reason.

## 9. Daily Routine

Executed via `prompts/daily.md`. Responsibilities enforced by this file:

1. Query sources in `config/sources.yaml`, scoped by topics in `config/topics.yaml`.
2. Filter to items published or meaningfully updated in the last 24–48 hours.
3. Score every retained item using `config/scoring.yaml`.
4. Populate `templates/daily_report.md`, write to `reports/daily/YYYY-MM-DD.md`.
5. Mirror to Notion per `config/notion.yaml` daily naming convention.
6. Flag anything scoring in the top tier of Research Potential or Product Score for inclusion in the next weekly synthesis.

## 10. Weekly Routine

Executed via `prompts/weekly.md`, run every 7th day (or on demand).

1. Read all daily reports from the past 7 days in `reports/daily/`.
2. Identify recurring themes, contradicting findings, and trend acceleration/deceleration.
3. Re-rank the week's findings; promote items whose Research Potential or Enterprise Score increased in light of the week's full picture.
4. Populate `templates/weekly_report.md`, write to `reports/weekly/YYYY-Www.md`.
5. Mirror to Notion per weekly naming convention; link to the week's daily pages.
6. Maintain a running "watchlist" of open questions to track into subsequent weeks.

## 11. Monthly Routine

Executed via `prompts/monthly.md`, run at calendar month end (or on demand).

1. Read all weekly reports from the month in `reports/weekly/`.
2. Produce a strategic rollup: major shifts in the field, enterprise adoption signals, and durable vs. transient trends.
3. Identify 1–3 concrete PoC candidates worth prototyping, with a one-paragraph feasibility note each.
4. Identify material directly relevant to active or upcoming NSTC proposal cycles, flagged explicitly by proposal section (background, methodology, expected contribution).
5. Populate `templates/monthly_report.md`, write to `reports/monthly/YYYY-MM.md`.
6. Mirror to Notion per monthly naming convention; link to the month's weekly pages.

## 12. Reliability Rules

- **No hallucinated sources.** If a claim cannot be traced to a fetched primary source, do not include it — state the gap instead of inventing a citation.
- **No fabricated scores.** Every score must be justified with a one-line rationale referencing the rubric in `config/scoring.yaml`.
- **Recency discipline.** Always state the actual publication/update date of a source; never imply something is "new" without checking its original date.
- **Source diversity.** Do not let a single source (e.g., arXiv alone) dominate a report if other configured sources are relevant to the topic — actively cross-check.
- **Explicit uncertainty.** When a finding is ambiguous (e.g., unclear if a technique generalizes beyond the benchmark shown), state the uncertainty rather than smoothing over it.
- **Version discipline.** If a paper has multiple arXiv versions, always cite the version actually reviewed and note if conclusions might differ by version.

## 13. Evaluation Principles

Every scored finding should be evaluated, not just summarized. Apply these principles alongside the rubric in `config/scoring.yaml`:

- Distinguish a genuine methodological contribution from an incremental benchmark improvement.
- Weigh reproducibility: is code/data released? Has anyone besides the original authors validated the result?
- Weigh enterprise applicability honestly: a paper can be scientifically excellent and enterprise-irrelevant, or vice versa — do not conflate the two axes.
- Treat vendor marketing claims (product blog posts, press releases) with a lower evidentiary bar than peer-reviewed claims, and label them as such.
- Prefer identifying *why* something matters (mechanism, implication) over restating *what* it does.

## 14. Translation Principles

- Translate meaning and register, not word-for-word structure — Traditional Chinese research writing favors shorter clauses; do not force English sentence structure onto the Chinese text.
- Maintain terminological consistency across all reports: once a term is translated/glossed a particular way (tracked informally via prior reports in `reports/`), reuse that same rendering rather than introducing variants.
- Use Traditional Chinese (繁體中文) exclusively — never Simplified Chinese.
- Numbers, units, model/product names, and benchmark names remain unchanged between English and Chinese sections.
- When a concept has no natural Chinese equivalent in current research usage, keep the English term and provide a brief explanatory gloss rather than coining an awkward translation.
- Chinese sections are for Taiwanese academic/enterprise readers (e.g., NSTC proposal context) — calibrate register accordingly (formal, precise, avoid colloquialisms).
