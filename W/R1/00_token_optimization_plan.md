# Token Optimization Plan — Crypto Legitimacy Assessment Tool Research

**Date:** 2026-10-06
**Scope:** Governs how research tokens are spent for the *Crypto Legitimacy & Use-Case Viability* tool (brief: `p1.txt`).
**Governing principle:** research *breadth is cheap, depth is expensive.* Buy depth only where a decision depends on it.

---

## 1. Token Budget Baseline

| Pool | Purpose | Allocation | Mechanism |
|---|---|---|---|
| **P1 — Discovery** | Map the landscape, find candidate primary sources | 8% | 1 cheap broad search per subdomain |
| **P2 — Extraction** | Pull facts + figures from *already chosen* authoritative pages | 52% | `maxCharacters`-limited fetches on vetted URLs only |
| **P3 — Verification** | Confirm claims that would change a scoring weight | 12% | Targeted fetches on named documents (audits, safes, licenses) |
| **P4 — Synthesis** | Write the report | 28% (of a much larger local-only budget) | Local file I/O, no network |

P4 is deliberately large: synthesis is the deliverable, and writing tokens are the cheapest
tokens per unit of user value. We spend network tokens on *facts* and local tokens on *prose*.

## 2. Source Tiering (authoritative > blog)

Fetch priority, strictly enforced. Tier C and D sources are **banned** as citation grounds.

| Tier | Type | Examples | Allowed use |
|---|---|---|---|
| **A** | Primary regulatory | SEC, CFTC, ESMA, MiCA text, FATF, FCA, MAS, DOJ/SEC enforcement releases, court filings | Legal status, enforcement precedent, licence verification |
| **B** | Primary technical | Audit PDFs (CertiK/Trail of Bits/OpenZeppelin/Maker/Sigma Prime/ChainSecurity), verified contract source (Etherscan verified source, GitHub), Safe/Timelock on-chain state, protocol docs/specs | Security claims, multisig/timelock *evidence*, architecture |
| **C** | Primary measurement | DeFiLlama, Artemis, Nansen, IntoTheBlock, Dune dashboards, Token Terminal, CoinGecko/CryptoRank methodology pages, Electric Capital dev report | TVL, volume quality, dev activity, holder distribution |
| **D** | Established media/research | academic preprints, law-firm client alerts, recognised think-tank reports, named-journalist investigative pieces | Context, historical pattern |
| **E** | Blogs, SEO content farms, airdrop lists, "best coin to buy" sites, price predictions, affiliate reviews | — | **Discovery only. Never cited as a source of fact.** |

**Rule:** every factual claim in the final report carries a Tier A–D URL. Tier E may only
point to a Tier A–D source that we then fetch and verify.

## 3. Parallelism Strategy (the "spawn many agents" requirement, made token-efficient)

Parallelism *increases* total tokens but *decreases* wall-clock. We therefore parallelise only where
the sub-tasks are genuinely independent, and we constrain every sub-task by a contract:

- **Fan-out on partition, not on repetition.** Each agent owns a disjoint slice of the taxonomy
  (security / tokenomics / governance / regulatory / empirical / tooling-mechanics). No agent is asked
  for the same topic twice.
- **Filesystem return channel, not message return channel.** Agents write full findings to
  `research/NN_*.md` with citations and return only a ≤250-word structured digest. This keeps the
  orchestrator's context small while preserving full evidence on disk for synthesis.
- **Hard source discipline per agent.** Each agent gets the A–E tier list. Agents are told to refuse
  to write unsourced assertions rather than pad their output.
- **Two waves.** Wave 1 = landscape (parallel). Wave 2 = gap-fill on questions Wave 1 exposed
  as contested or under-sourced. Wave 2 is deliberately small and only spends tokens on known gaps.
- **Concurrency cap** matching available research MCP capacity to avoid rate-limit thrash and
  truncated fetches (a truncated fetch is a wasted fetch).

## 4. Context-Window Discipline

| Rule | Implementation |
|---|---|
| Never bulk-read a fetched page into orchestrator context | Agents absorb the page; orchestrator reads only digests + final selected files |
| Pin extraction caps | `web_fetch_exa.maxCharacters` ≈ 6–8k per page, not default-full-page |
| Scraped web artefacts go to disk | Playwright output written to `reference/`, read via targeted greps |
| Prune before synthesis | After Wave 1, drop redundant digests; keep only gap-tracked items |
| Single synthesis pass | Report is written once, from the evidence files, with the brief's section order |

## 5. Cost-Aware Tool Selection

- **Cheap → expensive escalation.** `web_search_exa` (discovery, small) → `exa fetch` (bounded
  extraction) → `firecrawl_scrape` w/ schema (only for JS-heavy pages) → `tinyfish` web automation
  (only for interactive quiz flows where the payload is inherently behind JS interaction).
- **Playwright is a targeted tool, not a crawler.** Used for: (a) the reference site's interactive
  questionnaire/radar rendering, (b) verifying on-chain contract pages that block plain fetch.
  Not used for bulk crawling.
- **No redundant re-fetch.** A URL fetched once and recorded in an evidence ledger is not fetched
  again; gaps are resolved by a different, higher-tier source instead.

## 6. Evidence Ledger

Each agent appends to `research/LEDGER.md`: URL, tier, date accessed, and the specific claim it
supports. Final report references are generated from the ledger, so every citation is traceable and
duplicates collapse automatically.

## 7. Failure Handling

| Failure | Response |
|---|---|
| Source unreachable / paywalled | Record as unreachable; do not substitute a blog; note the gap |
| Two tiers conflict | Cite both; mark the claim *contested* in the report |
| Fetch truncated | Re-fetch with higher cap **once**; else record as insufficient |
| Research server out of credits | Degrade to already-collected evidence; never invent to fill a gap |
| Insufficient evidence for a scoring weight | Lower the weight's confidence and say so explicitly in the report |

## 8. Definition of Done

1. `00_token_optimization_plan.md` — this file.
2. `reference/` — reverse-engineered reference-site tool inventory.
3. `research/` — evidence files + ledger, Tier A–D only.
4. `01_crypto_legitimacy_research_report.md` — authoritative report, header + numbered jumpable TOC
   + references, per brief item 4.
5. Every scoring characteristic traces to ≥1 Tier A–D citation or is explicitly flagged as
   practitioner heuristic.