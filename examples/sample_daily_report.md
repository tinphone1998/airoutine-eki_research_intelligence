# Daily Report — 2026-07-21

> Notion: `Daily — 2026-07-21` | Sources scanned: 8 | Findings retained: 3

## Summary / 摘要

### English
Today's scan surfaced a strong signal in permissions-aware retrieval: a new arXiv preprint proposes a re-ranking method that enforces access-control constraints *during* retrieval rather than as a post-filter, directly addressing a gap flagged in last week's synthesis. Separately, Glean shipped a product update operationalizing a similar idea, suggesting the research and product tracks are converging faster than expected.

### 中文
今日掃描發現一項強烈訊號，與具權限感知能力之檢索技術相關：一篇新發表的 arXiv 預印本提出一種在檢索階段（而非事後過濾階段）即強制執行存取控制限制的重新排序方法，直接回應了上週綜整報告中指出的研究缺口。此外，Glean 同時發布了實現類似概念的產品更新，顯示研究與產品發展軌跡的匯流速度快於預期。

---

## Findings / 研究發現

### Finding 1: Access-Control-Aware Re-ranking for Enterprise RAG

**Source:** arXiv (academic) · **Date:** 2026-07-20 · **Link:** https://arxiv.org/abs/2607.xxxxx

**Topics:** Hybrid Search & Re-ranking, Permissions-Aware Retrieval

#### English
Proposes injecting document-level ACL metadata directly into the re-ranking scorer, rather than filtering retrieved candidates after the fact. Reports a 22% reduction in "over-retrieval" of documents outside a user's access scope versus a post-filter baseline, on a synthetic enterprise corpus with simulated permission boundaries. Code and synthetic benchmark released.

#### 中文
提出將文件層級的存取控制清單（ACL）元資料直接注入重新排序評分器，而非於檢索後再進行過濾。相較於事後過濾之基準方法，於具模擬權限邊界之合成企業語料庫上，該方法使超出使用者存取範圍之文件「過度檢索」情形減少 22%。程式碼與合成基準測試集皆已公開釋出。

**Scores:**

| Dimension | Score | Rationale |
|---|---|---|
| Academic Score | 4 | Novel integration point (scorer-level, not filter-level); code and benchmark released, supporting reproducibility. |
| Enterprise Score | 5 | Directly addresses permissioning, the most common reason enterprise RAG pilots stall (see `docs/research_domains.md` Domain 5). |
| Product Score | 3 | No vendor has shipped this exact mechanism yet, though Glean's update (Finding 2) is directionally close. |
| Research Potential | 4 | Opens a clear follow-on question: does this generalize beyond synthetic permission structures to real enterprise ACL complexity? |
| Implementation Difficulty | 3 | Requires reworking the re-ranking scorer and ACL metadata pipeline; moderate lift, code available as a starting point. |

**Flags:** NSTC Relevant

---

### Finding 2: Glean Ships "Scoped Retrieval" Feature

**Source:** Glean Engineering Blog (industry) · **Date:** 2026-07-19 · **Link:** https://www.glean.com/blog/scoped-retrieval-ga

**Topics:** Enterprise Copilots, Permissions-Aware Retrieval

#### English
Glean announced general availability of "Scoped Retrieval," which the blog post describes as enforcing permission boundaries earlier in the retrieval pipeline to reduce latency from post-hoc filtering. Vendor-reported metrics only (no independent benchmark); claims 30% latency reduction on large tenant deployments.

#### 中文
Glean 宣布「範圍檢索」（Scoped Retrieval）功能正式全面上線，其部落格文章描述該功能於檢索流程更早階段即強制執行權限邊界，以減少事後過濾所產生之延遲。此為廠商自行提供之數據（未經獨立基準測試驗證），宣稱在大型租戶部署中可降低 30% 延遲。

**Scores:**

| Dimension | Score | Rationale |
|---|---|---|
| Academic Score | 1 | Not a research contribution; vendor product announcement with no methodology detail. |
| Enterprise Score | 4 | Real production deployment at scale signals genuine enterprise demand for this exact problem. |
| Product Score | 5 | Already shipped and GA — as mature as this dimension gets. |
| Research Potential | 2 | Limited — no released methodology to build on; useful primarily as a market-validation data point. |
| Implementation Difficulty | 2 | Not applicable to us as a build target since it's a closed vendor feature; scored as difficulty to replicate the observed behavior. |

**Flags:** High Priority

---

### Finding 3: Long-Context Retrieval Tradeoff Study

**Source:** OpenReview (ICLR 2027 submission, academic) · **Date:** 2026-07-18 · **Link:** https://openreview.net/forum?id=xxxxxxxx

**Topics:** Long-Context vs. Retrieval Tradeoffs

#### English
Under double-blind review; evaluates whether 1M+ token context windows reduce the marginal value of retrieval for enterprise document QA. Finds retrieval still outperforms long-context-only approaches when the corpus exceeds ~5,000 documents, primarily due to attention dilution rather than context length limits per se. Uncertain how this generalizes beyond the two benchmark corpora tested.

#### 中文
目前處於雙盲審查階段；評估百萬級以上 token 之長上下文視窗，是否降低檢索技術於企業文件問答任務中之邊際價值。研究發現當語料庫文件數超過約 5,000 份時，檢索技術之表現仍優於僅依賴長上下文之方法，其主因為注意力稀釋（attention dilution），而非上下文長度本身之限制。惟此結論能否推廣至所測試之兩個基準語料庫以外的情境，仍屬未知。

**Scores:**

| Dimension | Score | Rationale |
|---|---|---|
| Academic Score | 3 | Solid empirical study but under review, not yet validated by the community; only two benchmark corpora tested. |
| Enterprise Score | 4 | Directly informs a real architecture decision enterprises face today (retrieval vs. long-context). |
| Product Score | 2 | Informative for architecture decisions but not itself a shippable feature. |
| Research Potential | 4 | Strong candidate for literature review support on the retrieval-vs-context-window debate. |
| Implementation Difficulty | 4 | Reproducing the study would require assembling a comparable large document corpus and two model configurations. |

**Flags:** NSTC Relevant

---

## Watchlist Carried Forward / 待追蹤事項

- Track whether the arXiv access-control re-ranking method (Finding 1) gets validated on real (non-synthetic) enterprise data in a follow-up paper.
- Monitor for other vendors following Glean's scoped-retrieval direction (Finding 2) — would strengthen the "durable trend" case in the next monthly rollup.

## Sync Status

- Notion sync: success
- Paper database entries added/updated: 3
