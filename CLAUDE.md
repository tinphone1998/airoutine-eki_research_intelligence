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

> **Canonical source note:** this list is the conceptual definition, kept intentionally short and self-contained so it can be internalized without file-hopping. `config/topics.yaml` is the canonical machine-readable taxonomy (slugs, keywords, priority weights); `docs/research_domains.md` is the canonical long-form rationale for each domain. If the two disagree on domain names, `config/topics.yaml` wins.

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

- Configuration lives in `config/notion.yaml` (parent page, database name, page-naming conventions, properties). That file defines **structure only** — the behavioral rules below are defined here, once, and are not restated there.
- Every daily/weekly/monthly report creates exactly one Notion page as a row in its cadence's database, named per the naming pattern in `config/notion.yaml`.
- Never create a duplicate page for a date/period that already has one — if a page already exists, update it in place and note in the session output that an update (not a create) occurred.
- Relations between cadences (daily → weekly → monthly) must be set at page creation time, not backfilled later, to avoid orphaned pages.
- Notion pages mirror the Markdown template structure exactly (see `templates/`) so the two representations never drift.
- On sync failure, the Markdown file in `reports/` remains authoritative; retry the sync on the next run rather than blocking report creation on sync success.

## 8. Paper Database Workflow

The paper database is the cumulative record of every scored finding, intended to seed literature review sections and citation tracking.

- Every paper/product referenced in a daily report must be logged with: title, authors/org, venue or source, publication date, URL, one-line summary, and full score set (§ scoring).
- Deduplicate by canonical URL/DOI before adding a new entry — if a paper reappears (e.g., a preprint later published at a venue), update the existing entry rather than duplicating it.
- The paper database is maintained as structured entries inside the Notion database (see `config/notion.yaml`) and mirrored in `reports/` history; a local file-based export/index is a Phase 2 roadmap item (see `docs/roadmap.md`).
- When scores change materially on re-review (e.g., a paper's enterprise relevance becomes clearer after a product launch cites it), update the entry and note the revision date and reason.

## 9. Routine Cadence

The system runs three cadences. **Exact step-by-step workflow for each lives solely in its prompt file** (`prompts/daily.md`, `prompts/weekly.md`, `prompts/monthly.md`) — do not duplicate those steps here; this section defines only the invariants that hold across all three, so that a change to one cadence's steps never requires a matching edit in this file.

- **Daily** — runs once per day; sources new findings directly from `config/sources.yaml`.
- **Weekly** — runs every 7th day (or on demand); synthesizes across the preceding daily reports.
- **Monthly** — runs at calendar month end (or on demand); synthesizes across the month's weekly reports.
- Each cadence consumes only the output of the cadence below it — weekly reads daily reports, monthly reads weekly reports. Never re-query `config/sources.yaml` directly at the weekly or monthly level; re-ranking/re-scoring existing findings is expected, re-sourcing new ones is not.
- A finding's score may be revised at a later cadence in light of new context (e.g., a weekly synthesis raises a daily finding's Research Potential). Always state the prior score and the reason for the change — see §10 (Reliability Rules).
- All three cadences write into `reports/<cadence>/` using the matching `templates/<cadence>_report.md` and mirror to Notion per §7 and `config/notion.yaml`. This is invariant across cadences and enforced by each prompt; it is not restated per-cadence here.

## 10. Reliability Rules

- **No hallucinated sources.** If a claim cannot be traced to a fetched primary source, do not include it — state the gap instead of inventing a citation.
- **No fabricated scores.** Every score must be justified with a one-line rationale referencing the rubric in `config/scoring.yaml`.
- **Recency discipline.** Always state the actual publication/update date of a source; never imply something is "new" without checking its original date.
- **Source diversity.** Do not let a single source (e.g., arXiv alone) dominate a report if other configured sources are relevant to the topic — actively cross-check.
- **Explicit uncertainty.** When a finding is ambiguous (e.g., unclear if a technique generalizes beyond the benchmark shown), state the uncertainty rather than smoothing over it.
- **Version discipline.** If a paper has multiple arXiv versions, always cite the version actually reviewed and note if conclusions might differ by version.

## 11. Evaluation Principles

Every scored finding should be evaluated, not just summarized. Apply these principles alongside the rubric in `config/scoring.yaml`:

- Distinguish a genuine methodological contribution from an incremental benchmark improvement.
- Weigh reproducibility: is code/data released? Has anyone besides the original authors validated the result?
- Weigh enterprise applicability honestly: a paper can be scientifically excellent and enterprise-irrelevant, or vice versa — do not conflate the two axes.
- Treat vendor marketing claims (product blog posts, press releases) with a lower evidentiary bar than peer-reviewed claims, and label them as such.
- Prefer identifying *why* something matters (mechanism, implication) over restating *what* it does.

## 12. Translation Principles

- Translate meaning and register, not word-for-word structure — Traditional Chinese research writing favors shorter clauses; do not force English sentence structure onto the Chinese text.
- Maintain terminological consistency across all reports: once a term is translated/glossed a particular way (tracked informally via prior reports in `reports/`), reuse that same rendering rather than introducing variants.
- Use Traditional Chinese (繁體中文) exclusively — never Simplified Chinese.
- Numbers, units, model/product names, and benchmark names remain unchanged between English and Chinese sections.
- When a concept has no natural Chinese equivalent in current research usage, keep the English term and provide a brief explanatory gloss rather than coining an awkward translation.
- Chinese sections are for Taiwanese academic/enterprise readers (e.g., NSTC proposal context) — calibrate register accordingly (formal, precise, avoid colloquialisms).
