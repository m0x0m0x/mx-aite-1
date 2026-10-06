# E2 — Metric Reliability and Counter-Evidence

*Slice owner: designated skeptic. Scope: what large-sample measurements can and cannot support about on-chain/project legitimacy scoring. Project case studies are out of scope (owned by another agent). Access date for all retrievals: 2026-10-06. Sources listed in `E2_SOURCES.md`.*

**Reading rule for this document.** Three labels are used throughout and they are not interchangeable:

- **Measured** — a number produced by a named dataset or paper, with a denominator, that we retrieved from the primary source.
- **Asserted** — a claim whose primary source we could not retrieve; recorded here as a claim, never as evidence.
- **Practitioner-only** — a belief held in the practitioner literature with no retrievable measurement behind it. Treated as an assumption, not evidence.

---

## E2.1 Longitudinal dataset findings

*(Drafted incrementally. Section completed below as evidence is retrieved.)*

### E2.1.1 Developer activity — what the primary dataset actually publishes

**Electric Capital Open Dev Data** (Tier C, primary dataset, now public under CC BY 4.0) is the single largest longitudinal developer dataset available: continuous repository indexing since 2018, "hundreds of millions of commits … millions of developers", with a community-curated taxonomy of ecosystems and repos [E2-C1].

What the methodology doc actually defines, and what it therefore does *not* claim:

- **Monthly Active Developers (MAD)** = "the number of unique developers who made at least one commit in a given month". The doc states MAD "helps track ecosystem growth and developer retention over time" — it is presented as an *ecosystem-level* activity/growth measure, not as a validated project-survival predictor [E2-C1].
- **Developer segments**: full-time (10+ days/month), part-time (1–9 days/month), one-time (contributed once); newcomer (first month), emerging (2–12 months), established (12+ months) [E2-C1].
- **Commit attribution** is fingerprinting-based: original authors only, forks counted only for new code, all branches analysed, copy-pasted library code removed, and developers deduplicated by commit content across names/emails [E2-C1].

Two consequences that matter for scoring. First, this is a genuine strength: content-fingerprint dedup defeats the cheapest form of commit-count gaming (many identities, one committer), so Electric Capital commit counts are materially harder to inflate than naive `git log` counts. Second, and equally important: **the dataset is organised by ecosystem, not by project outcome.** There is no published survival or default label attached to it. MAD is a denominator-free popularity measure.

**Electric Capital 2024 Developer Report** (Tier C) — measured figures from the primary post [E2-C2]:

| Claim | Number |
|---|---|
| Commits / repositories analysed | 902 million commits, 1.7 million repositories |
| Developer growth since Ethereum launch (2015) | +39% per year |
| New developers in 2024 | 39,148 |
| Total developers YoY | −7% (marginally down) |
| Established developers (2+ yrs in crypto) | +27% YoY, all-time high |
| Share of code commits from established developers | 70% |
| Developers working on multiple chains | 1 in 3 (up from <10% in 2015) |
| Ecosystem taxonomy contributors | 829 people since inception; 339 in 2024 |
| Stablecoin supply / daily volume | $196B / $81B |

The 2024 "Established Developers +27% while total developers −7%" decomposition is the most useful structural fact here, and it is the direct empirical basis for treating **one-time contributors as noise** rather than signal: the population is a churn-heavy pyramid whose top is growing. That is a composition fact, not a survival statistic — the report does not publish a per-project retention-to-24-months figure.

**Finding (E2.1-F1, Measured, Tier C):** the primary crypto developer dataset is large, transparent, and now fully open, but it is *descriptively* organised. We could not retrieve any published cohort table from Electric Capital giving, e.g., "% of projects with <Y commits over 12 months were inactive at 24 months". Any such statistic in circulation should be treated as UNSOURCED until a primary table is produced.

### E2.1.2 Revenue, fees, and token-holder accrual

DeFiLlama's own definitions doc (Tier C, primary) is worth quoting precisely because it establishes that **three different "revenue" numbers exist and are routinely conflated** [E2-C3]:

- **Fees** = "total fees paid by users when using the protocol" — "equivalent to what would traditionally be considered Revenue in most off-chain businesses", i.e. top-line, "regardless of where those fees end up".
- **Revenue** = "subset of fees that the protocol collects for itself, usually going to the protocol treasury, the team or distributed among token holders"; excludes fees paid to LPs; "closer to Gross Income".
- **Holders Revenue** = "subset of revenue that is distributed to tokenholders by means of buyback and burn, burning fees or direct distribution to stakers"; "similar to Dividends and Buybacks".

The same doc discloses two measurement traps directly [E2-C3]:

- **TVL is price-confounded.** "A protocol's TVL might go down even if more assets are deposited… just looking at the TVL chart is not the best way to see if a protocol is receiving deposits or money is exiting, as that info gets mixed with price movements." DeFiLlama therefore publishes a separate **USD Inflows** metric (per-asset balance deltas × price, summed) and states the failure mode explicitly: "If a protocol has all of its TVL in ETH and one day ETH price drops 20% while there are no new deposits or withdrawals, TVL will drop by 20% while USD inflows will be $0."
- **Loop-driven TVL inflation.** Active loans "are excluded from TVL by default to account for looping strategies which can artificially inflate TVL."
- **Active Addresses** counts only direct interactions, "meant to help measure stickiness/loyalty of users".

**1kx Onchain Revenue Report H1 2025** (Tier D — recognised research house; primary dataset of its own, 1,244 protocols, 2020 through Q3 2025, sourced from Dune/Token Terminal/DeFiLlama, valuations from CoinGecko) [E2-D1]:

| Claim | Number |
|---|---|
| Protocols in dataset | 1,244 (2020–Q3 2025) |
| Protocols with >$1M annualised fee revenue (2025 YTD) | ~400 |
| Protocols passing **>$10M in value to token holders** | **20** |
| Revenue concentration | Top 20 protocols = 70% of revenue |
| Quarterly onchain fees at 2021 peak | $9.2B (Ethereum ~40%) |
| Application fee growth YoY (2025) | +126% |
| Transaction cost decline by 2025 | ~−90% vs 2021 |
| Projected 2026 onchain fees | $32B+, +60% YoY, all growth attributed to applications |

The 20-of-1,244 figure (1.6%) is the load-bearing number for this report. It is a *count of protocols*, not a share of fee dollars, and 1kx's own scope note excludes protocol rewards, staking yields and reserve interest from "onchain fees" [E2-D1] — so it is a conservative denominator for the numerator it measures. The report explicitly says topics "such as value accrual to token holders or protocol treasuries are only briefly discussed", i.e. this is not a study of whether accrual predicts anything.

### E2.1.3 Token death: the only directly predictive model we found, and how it fails

**Kuehn & Adnan, "Blockchain Lifecycle Prediction — Dead Coins"** (arXiv 2610.01379, Tier D preprint, Oct 2026) [E2-D2] is the closest thing in the retrieved literature to an actual survival model. Its abstract reports:

- **Base rate**: "over 52 percent of all tokens launched since 2021 ceasing to trade by early 2025."
- **Design**: LSTM on 90-day sequences of daily reference price and estimated market cap, 82 assets (41 alive / 41 dead), Coin Metrics data 2020–2026, strictly chronological train/test split to prevent look-ahead bias.
- **Result, best case on held-out test**: ROC AUC 0.98.
- **Result on unseen data**: "ROC AUC decreased between 0.59 and 0.65."

This is the single most important measurement in the slice for calibration, and the paper reports it against itself. A model with in-sample-adjacent discrimination of 0.98 that degrades to 0.59–0.65 out of sample is a coin flip. It also contains an admission that constrains the whole field: "the practical infeasibility of a multi-stage lifecycle model under current data conditions", and the diagnostic that failures look like "gradual value erosion and elevated volatility in the months preceding inactivity, rather than sudden catastrophic collapse" — i.e. the signal, when it exists, is *already price and volume*, arriving late.

Note also that the input features are price and market cap. **Not one off-chain, project-quality, or governance feature entered the model.**

### E2.1.4 Governance concentration

**Zukowski, "Auditing governance concentration beyond token allocation: a live-governance study of 52 token protocols"** (Frontiers in Blockchain, Tier D, published 2026-08-05) [E2-D3] is the strongest governance measurement retrieved. Holder snapshots collected March–May 2026 via Dune Analytics + Helius DAS API; HHI computed over top-(1,000 + #PCAs) holders after exclusions; 133 protocol-controlled-address exclusions across 38 protocols; plus an exchange-custody completion audit excluding 64 Nansen-labelled CEX deposit wallets across 21 protocols.

Measured findings [E2-D3]:

| Claim | Number |
|---|---|
| Sample | 52 token protocols (50 in the powered regression) |
| Holding HHI: sector contrast | DePIN HHI above DeFi, Cohen's d = 0.65 (voter-inclusive treatment); 0.75 under uniform staking-aggregation exclusion |
| Holding HHI max in sample | Livepeer 0.199 of record; 0.033 under uniform staking exclusion |
| Voting vs holding concentration | 13 of 18 sampled protocols **amplify** (>1.0x); 5 disperse |
| Amplification examples | Curve veCRV 15x, Balancer veBAL 21x, Frax veFXS 11.4x (distinct ve-token class) |
| Dispersion examples | ENS 0.48x, GMX 0.87x, HNT 0.26–0.39x, JUP 0.12x, LPT 0.27x |
| 12-month vs full-history voting HHI | Spearman ρ = 0.87 (n=12); no classification flips |
| Regressions that are **null** | Allocation, maturity, and float specifications are all null |
| Subsidy regression | Significant in levels but driven entirely by Livepeer |

Two results here are directly load-bearing for this report.

First, **delegation concentrates governance above token holdings in 13 of 18 cases**, often by an order of magnitude. A tool that measures holder concentration on-chain and reports "decentralisation" is measuring the wrong object: vote-escrowed systems are an order of magnitude more concentrated than the holder HHI suggests. This is a measured, quantified failure mode of holder-HHI-as-decentralisation-metric.

Second, **the nulls are the finding**: token allocation structure, protocol maturity and float do *not* predict holding concentration, while the authors' own balance-based "insider wallet" measure does not survive their tautology check (Section 4.4) and the subsidy result is driven entirely by one protocol out of 52. Concentration is real and measurable; most of the things practitioners assume drive it do not.

### E2.1.5 Coin-market "trust scores": what they are and what they are not

**CoinGecko Trust Score** (Tier C, primary methodology doc) is explicitly and exclusively an **exchange** quality score, not a project or token legitimacy score [E2-C4]:

| Component | Weight |
|---|---|
| Liquidity | 50% |
| Cybersecurity | 20% |
| Regulation | 15% |
| Incident | 10% |
| Proof of Reserves | 5% |

Critically for calibration: "the final Trust Score is not a simple weighted sum. Instead, the combined score is graded on a curve consisting of the full population of scored exchanges, before being assigned a final Trust Score. This means an exchange's final Trust Score reflects its **relative standing among peers**, rather than a number on a fixed scale." It is recalculated weekly.

Two consequences. (a) CoinGecko's score is **rank-relative**, so it carries no absolute meaning and cannot be ported into a tool as an absolute probability of anything. (b) Its largest component is liquidity — trading volume, orderbook depth, spread — i.e. 50% of it is exactly the family of metrics that a volume-wash attack targets. The methodology doc is admirably candid about this: "in the crypto markets, these are not always the case, as exchanges may artificially inflate their reported trading volume", which is why liquidity is scored by **consistency against a trusted reference benchmark set** plus an exchange-level quality gate ("maintaining a small number of clean trading pairs… is not sufficient to receive a high score") and manual review. The anti-gaming reasoning is present and measurable — but the anti-gaming is *concentrated in the liquidity sub-score*, and it was built for centrally-controlled orderbooks, not for a DEX where volume can be manufactured by a handful of related wallets.

The doc also verifies regulatory credentials against official registers and excludes unconfirmable self-reported claims [E2-C4]. That is a hard, mechanically checkable standard — and notably one that has **no analogue** in any of the on-chain project metrics in our table.

No CoinGecko project-level or CryptoRank trust score was retrieved with a published predictive or outcome-validation study. See E2.3.6.

### E2.1.6 Volume authenticity — the strongest measured evidence in the whole slice

Two papers give hard numbers on how much reported trading volume is fabricated.

**Cong, Li, Tang & Yang, "Crypto Wash Trading"** — NBER Working Paper 30783 (2022), published *Management Science* 69(11), 6427–6454 (Tier D, peer-reviewed) [E2-D4]. Tests 29 cryptocurrency exchanges. Finding: "abnormal first-significant-digit distributions, size rounding, and transaction tail distributions on unregulated exchanges reveal rampant manipulations"; the quantified wash trading on each unregulated exchange "averaged over **70% of the reported volume**". Fabricated volumes ("trillions of dollars annually") improve exchange *ranking*, temporarily distort prices, and correlate with exchange characteristics such as age and userbase, market conditions, and regulation. Regulated exchanges "feature patterns consistently observed in financial markets".

**Falk, Tsoukalas & Zhang, "Can AI Detect Wash Trading? Evidence from NFTs"** (arXiv 2311.18717, Tier D preprint, v3 Mar 2025) [E2-D5]. Uses public on-chain NFT data for direct rather than statistical estimation, across three major exchanges: "**~38% (30–40%) of trades** and **~60% (25–95%) of traded value** likely involve manipulation, with significant variation across exchanges." Crucially, the paper re-assesses the indirect methods of Cong et al.: roundedness-based regressions are "most promising, though still error-prone in the NFT setting", and the authors build an AI estimator to reduce exchange- and trade-level estimation errors.

**Why this dominates the volume row.** Two independently-designed studies, different markets (CEX spot/perpetuals vs NFT), different methods (first-digit/statistical vs on-chain graph), agree that **reported volume is the single most fabricable headline metric in crypto**, with the fabrication rate in the tens of percent and, on unregulated venues, above 70%. Any scoring model that gives volume a positive weight has to carry an equivalent wash-detection penalty or it is measuring the counterparty's willingness to spend money on fake volume. Note also that Cong et al. find wash volumes *improve ranking* — i.e. the metric is gameable specifically because it is a published ranking input.

### E2.1.7 Security: what audits do and do not predict

Three findings, all Tier D, all pointing the same way.

**Beyer, "The Audit Gap in Blockchain Security"** (arXiv 2606.15465, Apr 2026, Oak Security) [E2-D6]. Dataset: **23,818 public audit findings from 22 security firms** vs **218 real-world exploit incidents** (rekt.news), aggregate losses ≈ **US$7.76B**, window 1 Jan 2022 – 27 Mar 2026.

- Audit findings are *stable* across the window: the Critical+High share stays in a **15–17% band in every complete year**.
- The two distributions are *categorically misaligned*: "private-key compromise, phishing, and social-engineering vectors account for approximately **49.6% of cumulative losses** yet represent a negligible share of published audit findings."
- Realised losses are heavy-tailed: **top 8 incidents = 50.6%** of cumulative dollar losses; **top 20 = 71.4%**.
- The author's own analytical convention, quoted: "audit outputs and exploit outputs describe different populations" — presented in parallel, not compared as like samples.
- Reported in related work: Perez & Livshits found that of **23,327 contracts flagged as vulnerable by six analysis tools, fewer than 2% were ever exploited**, and genuinely at-risk funds were concentrated in a small number of contracts.

**Bourveau, Brendel & Schoenfeld, "Decentralized Finance (DeFi) assurance: early evidence"** (*Review of Accounting Studies* 29, 2209–2253, 2024, open access) [E2-D7]. Hand-coded sample of **~8,500 smart contract audit reports**; audits are pervasive, voluntary, unregulated, with "no private or public standards" governing them, and inputs/outputs "differ substantively from those of conventional financial audits". Critically for scoring: the paper finds "the **market reacts positively to the release of these audit reports**, suggesting that these reports are value-relevant." That is a price/attention reaction — it is evidence that an audit *announcement* moves markets, not that audits reduce incident probability.

**David, Zhou, Qin, Song, Cavallaro & Gervais, "Do you still need a manual smart contract audit?"** (arXiv 2306.12338, Tier D preprint) [E2-D8]. Benchmark of **52 previously compromised DeFi contracts** (≈$1B combined losses), 38 vulnerability classes: GPT-4 and Claude identify the correct vulnerability type in **40%** of cases with a high false-positive rate; best-case true-positive rate on mutation-tested synthetic contracts 78.7%. Included here for one reason: **every one of the 52 contracts in the vulnerability-detection benchmark was a contract that had been compromised** — i.e. the benchmark is by construction a set of real misses.

**Synthesis for the audit-count row.** Audit *count* measures procurement activity by teams who are generally able to pay for it, i.e. it is largely a proxy for having money and caring about reputation. The best available evidence says the categories of loss that dominate (keys, phishing, social engineering ≈ 49.6% of dollars) are **outside what code audits examine at all**, and the correlation between flagged vulnerability and actual exploitation is under 2%. Audit count should not be a hard gate.

### E2.1.8 Holder counts and airdrop-driven address inflation

**Messias, Yaish & Livshits, "Airdrops: Giving Money Away Is Harder Than It Seems"** (arXiv 2312.02752, rev. Aug 2026, Tier D) [E2-D9]. "First comprehensive empirical study of nine major airdrops across Ethereum and Layer-2 ecosystems." Finding: "a substantial share of tokens — **up to 66% in some cases** — are rapidly sold, often in recipients' first post-claim transaction", driven by "airdrop farmers" who optimise eligibility criteria. The Arbitrum case study illustrates "how short-term activity spikes fail to translate into sustained user involvement".

The implication for scoring is direct and structural: the cheapest way for a protocol to increase its address count is to airdrop to addresses that will sell immediately and never return. Holder count is therefore **partly a purchased metric**, and address counts on L2s are further contaminated by bundler/sequencer address inflation.

Related: a CHI 2026 study on airdrop-hunter/Sybil definitions reports that "empirical studies report that **over 60% of airdropped tokens**…" are associated with Sybil behaviour in the datasets it constructs [E2-D10] — cited here as corroborating, with the caveat that we read only the abstract/snippet of that item.

### E2.1.9 Decentralisation measurement itself is contested

**Ovezik, Karakostas, Milad, Kiayias & Woods, "SoK: Measuring Blockchain Decentralization"** (arXiv 2501.18279; published as a chapter in *Lecture Notes in Computer Science* / ACAC 2025, Tier D) [E2-D11]. This is a systematisation + empirical validation of the measurement layer itself, and its findings undercut naive use of any single decentralisation index:

- "the importance of decentralization is undermined by the lack of a widely accepted methodology to measure it."
- "the seemingly innocuous choices performed during data extraction, such as the **size of estimation windows** or the application of **thresholds that affect the resource distribution**, have important repercussions when calculating the level of decentralization."
- Exploratory factor analysis: "in Proof-of-Work (PoW) blockchains, **participation on the consensus layer is not correlated with decentralization**, but rather captures a distinct signal, unlike in Proof-of-Stake (PoS) systems, where the different metrics align under a single factor. These findings challenge the long-held assumption within the blockchain community that **higher participation drives higher decentralization**."
- Governance note in the same paper: the absence of an accepted decentralisation methodology was directly litigated — "in the proceedings of the SEC vs. Ripple case, where the SEC argued that the XRP token was centralized… Ultimately, the lack of a widely accepted methodology for assessing decentralization led to Ripple being exonerated from most accusations."

Combined with Zukowski's finding that voting HHI amplifies holding HHI in 13/18 protocols [E2-D3], the conclusion for our table is that **Nakamoto coefficient / voting concentration is real and measurable, but its value depends on an explicit, published choice of resource, window and threshold** — and one influential study finds the naive version is uncorrelated with decentralisation even in PoW.

### E2.1.10 What we could NOT retrieve as primary

Recorded so that downstream agents do not mistake absence for a negative result. Each is `UNSOURCED` in this report and must not be cited as fact.

| Widely-repeated claim | Status |
|---|---|
| "% of incentivised TVL that leaves within 30 days of emission cuts" (often cited as 40–70%, or 70–90% within 14 days) | **UNSOURCED.** Only Tier E/blog and vendor-marketing pages retrieved (beelaa.com, LinkedIn, chainscorelabs, growgami). No DeFiLlama or Artemis primary table. Excluded from E2.2 as a number. |
| A DeFiLlama or Artemis published "stickiness"/"organic share" statistic | **UNSOURCED.** DeFiLlama publishes `Token Incentives` as a definition [E2-C3] but we retrieved no published retention study. |
| Protocol-by-protocol treasury runway in months | **UNSOURCED.** DeFiLlama's `Expenses` field is self-described as lagging and incomplete: "We collect this data mainly from annual protocol reports on their forums, so it's always referencing old data and will not be up-to-date in realtime" [E2-C3]. Runway is therefore not mechanically computable across the population. |
| A CryptoRank trust-score methodology document | **NOT RETRIEVED.** No official methodology page surfaced; only news/marketing. CryptoRank's scoring is treated as **UNSOURCED** and is not used as evidence anywhere. |
| Per-project Electric Capital retention cohort tables | **NOT RETRIEVED** (see E2.1-F1). |
| A measured correlation between social follower count and token returns | **NOT RETRIEVED.** Nothing with a denominator and an out-of-sample statistic. Treated as practitioner-only. |
| A measured correlation between realised-PnL concentration and outcomes | **NOT RETRIEVED.** Wash-trading volume effects are measured [E2-D4, E2-D5]; PnL-concentration-as-a-score is not. |
| Any published validation of a published trust score against future outcomes | **NOT RETRIEVED** — see E2.3.6. |

---

## E2.2 Metric reliability ranking

**How to read this table.** "Claimed predictive value" is what the practitioner literature asserts. "Measured evidence" is what we could actually retrieve, with the number. Where the two differ, the gap *is* the finding. `Evidence quality` refers to the tier of the retrieved source, and where the answer is "no measurement exists" the evidence tier is `none` — that is a stronger statement than a low tier.

Gameability is scored `Low / Medium / High / Trivial` for an adversary who knows the metric is being scored and can spend, or fabricate, resources to move it.

| metric | claimed predictive value | measured evidence (with number) | evidence quality (tier) | how gameable | gameability mechanism | recommend use as |
|---|---|---|---|---|---|---|
| developer commits & active developers | Strong. "The single best leading indicator of project survival." | **No project-level survival statistic retrieved.** Available: Electric Capital Open Dev Data defines MAD and content-fingerprinted dedup [E2-C1]; 2024 report gives ecosystem-level figures — 902M commits / 1.7M repos, established devs +27% YoY and **70% of commits** while total devs **−7%**, 39,148 new devs [E2-C2]. Kuehn & Adnan's only predictive model uses **price and market cap, no dev features** [E2-D2]. | **C for the dataset; none for the predictive claim** | **Low** if computed with content-fingerprint dedup; **Trivial** if computed as raw `git log` counts | Raw commit counts: squash-merge spam, trivial dependency-bump commits, bot committers, multi-identity splitting. Electric Capital's fingerprinting removes copy-paste and dedups identities, which blocks the cheap versions. Residual gaming: legitimate-looking but value-destroying commits; offloading work to monorepos or vanity commits; moving development to private repos (invisibility risk, the inverse failure). | **Weighted dimension**, and only on a fingerprint-deduped source. Never a hard gate. Must be read as a *composition* measure (share from established vs one-time devs) rather than an absolute count. |
| TVL | Strong. "Real capital at risk = skin in the game = real users." | DeFiLlama's own doc concedes TVL is **price-confounded**: "If a protocol has all of its TVL in ETH and one day ETH price drops 20% while there are no new deposits or withdrawals, TVL will drop by 20% while USD inflows will be $0" — which is why they publish USD Inflows separately [E2-C3]. Also: TVL "is meant to be a proxy for the skin in the game", with exclusions for cycled lending, own tokens, vesting tokens, double-counting and unproductive assets [E2-C3, E2-D12]. LPT = 0.199 HHI, the highest holding concentration in the 52-protocol governance sample [E2-D3]. No retrieved study shows TVL predicts token survival or returns. | **C for the metric definition and its caveats; none for predictive value** | **Medium** | Borrow-and-reloop (DeFiLlama excludes by default, but third-party scorers that sum raw numbers do not); self-minted token pairs; one-sided LP with no trading; points-farming deposits; price inflation of the deposited asset. | **Weighted dimension only**, and *only* as DeFiLlama-constructed TVL (never self-summed), always paired with USD Inflows and an incentive-adjustment. Use price-change-adjusted or inflow-based variants if available. |
| trading volume | Strong. "Liquidity and usage = real demand." | **The most thoroughly falsified metric in the table.** Cong et al.: tests across 29 exchanges found wash trading "averaged more than **70% of the reported volume**" **on unregulated exchanges**, and fabricated volumes "improve exchange ranking" [E2-D4]. Falk et al.: "**~38% (30–40%) of trades** and **~60% (25–95%) of traded value** likely involve manipulation" across three NFT exchanges, and indirect detection methods remain "error-prone" [E2-D5]. | **D, peer-reviewed (Management Science) + D preprint**, and the evidence is *against* the metric | **Trivial** | Self-trading between related wallets; matched internal orders; volume rebates; wash trading via connectivity clusters. Cong et al. document that this specifically manipulates published *ranking* metrics — i.e. exactly the use we would put it to. | **Reject as a positive score input.** Permissible only as a *negative* input (wash-volume penalty) and as context. Never a gate. |
| holder count | Moderate. "Distribution breadth = adoption." | Up to **66%** of tokens in airdrops are "rapidly sold, often in recipients' first post-claim transaction" across nine major airdrops [E2-D9]; corroborating work reports >60% of airdropped tokens in Sybil-farmer datasets [E2-D10]. No retrieved study shows holder count predicts survival. DeFiLlama deliberately reports **Active Addresses** (direct interactions only) instead [E2-C3]. | **D, preprints; none for the predictive claim** | **Trivial** | Airdrop to sybil-farmed addresses is the cheapest growth hack in crypto. L2 bundler/sequencer address inflation multiplies apparent users. Exchange-deposit address aggregation is opaque. | **Reject.** If a user-breadth measure is needed at all, use retained-cohort/active-address measures, not lifetime address counts. |
| holder concentration (HHI) | Strong. "High HHI = insiders control = risk." | **Measured and quantified as a *mis-specification* risk.** Zukowski (n=52): delegation amplifies voting concentration above token holdings in **13 of 18** sampled protocols; ve-token amplification reaches **15x (Curve veCRV), 21x (Balancer veBAL), 11.4x (Frax veFXS)**; dispersion cases down to JUP 0.12x, LPT 0.27x, ENS 0.48x [E2-D3]. Study also reports 133 PCA exclusions across 38 protocols plus 64 exchange-custody exclusions across 21 protocols — i.e. **naive HHI is materially wrong without entity resolution** [E2-D3]. | **D, peer-reviewed (Frontiers in Blockchain), live data** | **Medium** | Sybil-splitting across addresses; exchange-custody aggregation in the denominator (an exchange's 10M-user wallet reads as maximal concentration); excluding protocol-controlled addresses that are in fact beneficial owners; staking/lock wrappers that hide voting power. Zukowski shows results are *sensitive* to the staking treatment (Livepeer 0.199 vs 0.033). | **Weighted dimension, with mandatory entity resolution**, and it must be computed on **voting power**, not holdings. Report both holding-HHI and voting-HHI. Never a hard gate — JUP and LPT are large surviving systems with dispersing governance. |
| net protocol fees | Strong. "Real users pay real money." | DeFiLlama defines Fees as top-line and Revenue as the retained subset [E2-C3]. 1kx: **~400 protocols** with >$1M annualised fees (2025 YTD) out of 1,244 protocols; **top 20 protocols = 70% of all revenue** [E2-D1]. No retrieved study shows fees predict survival; fee levels are strongly asset-price-driven (1kx: 2021 peak quarterly fees $9.2B, Ethereum ~40%) [E2-D1]. | **C for the definition; D for distribution; none for prediction** | **Medium** | Fee *events* (one-off airdrop-season spikes); routing volume through the protocol's own market maker; token-price-driven denominators; airdrop campaigns that generate one month of swap fees. 1kx explicitly excludes off-chain fees and non-user-paid income from its fee totals [E2-D1], so scope discipline matters. | **Weighted dimension.** Require multi-period (≥4 quarters) persistence to avoid single-event artefacts. Always keep Fees, Revenue and Holders Revenue as three separate columns — the conflation is the most common error in the space. |
| **token-holder revenue** | Very strong in practitioner belief. "This is the only real test: does the token capture anything?" | **The strongest *discriminating* number we found, and it is a rarity statistic.** 1kx (1,244 protocols, 2020–Q3 2025): **~400** protocols with >$1M annualised fees, but only **20** protocols passed **>$10M in value to token holders** [E2-D1] — ~1.6% of the sample. DeFiLlama defines Holders Revenue precisely (buyback-and-burn, fee burn, direct staking distribution) [E2-C3]. | **D (1kx primary dataset of 1,244 protocols) for the prevalence figure; C for the definition.** No predictive/outcome study retrieved. | **Low-Medium** | Hardest of all metrics to fake sustainably, because it requires paying out. But: token buybacks are financed from treasury sales, not earnings (circular); "distributed" can mean a one-off airdrop to stakers; Hyperliquid routes **97% of trading fees** to an Assistance Fund that burns HYPE — a real sink but also excluded from the study's subsidy regression as an outlier [E2-D3]; accounting classification of what counts as "holder revenue". | **Hard gate** — this is the one recommendation where the evidence supports gating, because the base rate of *not* having it is 98.4%. Gate = "does any verified mechanism route measurable value to holders, sustained ≥2 quarters", **not** "does it exceed a dollar threshold" (which would gate out everything legitimate and early). |
| protocol age / time since launch | Strong, via survivorship logic. "Older = survived more." | **The metric is confounded with the selection it produces.** 1kx's 1,244-protocol dataset spans 2020–Q3 2025, so its 20 holder-revenue protocols are necessarily old by construction [E2-D1]. Kuehn & Adnan's dead-coin model needed a 90-day history window — i.e. **age is a precondition for being scorable at all** [E2-D2]. Zukowski's powered regression reports **maturity as a null predictor** of HHI [E2-D3]. | **none for the predictive claim; the null and the confounds are measured** | **Medium** | Waiting is free. Age is the one metric an adversary gains from *not* acting, which makes it attractive as a scoring input and useless as one — but it also means the *inverse* (recent launch) is heavily populated by rug-adjacent projects, so penalising age naively punishes honest early-stage work. | **Context only — never a positive score component, and never a penalty on its own.** Its legitimate use is as an *evidence-sufficiency* gate: below some age, the tool must say "insufficient evidence" rather than score low. See E2.3.4. |
| audit count | Strong. "Audited = safe." | Findings are stable (Critical+High share **15–17% every complete year**) but the categories are misaligned with losses: private-key compromise, phishing and social engineering "account for approximately **49.6% of cumulative losses** yet represent a negligible share of published audit findings" [E2-D6]. Reported in [E2-D6]: of **23,327 contracts flagged vulnerable by six tools, fewer than 2% were ever exploited**. Bourveau et al. (n≈8,500 audit reports) find reports are "value-relevant" in the sense that "the market reacts positively to the release of these audit reports" — a market reaction, not an incident-rate reduction [E2-D7]. All 52 contracts in the LLM-audit benchmark were ones that had been breached [E2-D8]. | **D, mixed: preprint [E2-D6] + peer-reviewed (*Review of Accounting Studies*) [E2-D7] + preprint [E2-D8]** | **High** | Buying a cheap report from a name-brand shop; repeated re-audits of the same commit inflating a count; an audit that certifies a small surface while the vulnerable path lives in a proxy/bridge/oracle. The [E2-D7] finding that there are "no private or public standards" for SCAs is the mechanism: nothing stops anyone publishing a report. | **Reject as a score.** Permissible as **context** (does an audit exist; is the auditor identifiable; does the report cover the deployed contracts) and as a hard gate only in the narrow form "**the deployer contract is unaudited *and* unverified**" — never as a graded count. |
| social follower count | Strong in practitioner belief. "Community size = traction." | **Nothing retrieved.** No study with a denominator and an out-of-sample statistic linking follower count to token outcomes was found. Meanwhile the only verified coin-level model [E2-D2] uses price and market cap only, and fails out of sample (AUC 0.59–0.65). Follower counts are not verified against any identity by any retrieved primary source. | **none — practitioner-only** | **Trivial** | Paid followers, bot farms, engagement pods, follower buying is a listed service. Bought followers are cheaper and faster than any real growth metric. | **Reject.** If social is used at all, use *verified, non-farmable* activity (e.g. forum contributions by accounts with a verifiable history) and never raw follower counts. |
| realised-PnL concentration | Strong. "If one wallet makes all the P&L, it's insider extraction." | **UNSOURCED as a metric.** Related *volume*-fabrication evidence is strong and measured [E2-D4, E2-D5], but no retrieved study measures realised-PnL concentration as a predictor of anything. PnL is also not directly observable on-chain without a chosen price convention and without attributing each wallet's positions. | **none** | **Medium-High** | Attributing PnL to entities requires the same clustering that concentration metrics are trying to evade; a whale can split across wallets. Conversely a single profitable market-maker wallet is normal and healthy. | **Reject as a score.** Context only, if at all. Note that it is *derivable* from the same clustering that powers holder-HHI, so it inherits all of HHI's entity-resolution error. |
| organic-vs-incentivised volume share | Strong, and the intuitive fix for the volume problem. | **UNSOURCED.** DeFiLlama defines `Token Incentives` as tokens allocated via liquidity mining or incentive schemes [E2-C3], which is the raw input — but no published study computes and reports an organic-share statistic, and no DeFiLlama/Artemis retention table was retrieved. | **none** | **Medium** | The measurement is only as good as the incentives dataset, which is protocol-reported (emissions schedules are self-declared) and lags reality. Misstated emissions → wrong organic share. | **Context, and the target of future work.** High conceptual merit and it is the correct correction to the volume row, but we cannot yet cite a measurement, so it must not carry weight in v1. Flagged as `UNSOURCED`, not as low-value. |
| vesting/unlock overhang | Strong. "Upcoming unlocks are forced selling." | Only retrieved item is a **preliminary** study of **52 token unlock events on Binance** (Kim, SSRN 6632838, Tier D preprint, self-described "preliminary evidence"), not fetched to primary text. No pooled sample, no effect size retrieved. | **D but preliminary and not retrieved to primary** | **Medium** | Cliff schedules are largely fixed on-chain and hard to move, so this metric is comparatively honest — the gaming is in *choosing* cliffs, and in classifying unlocks as "team" vs "ecosystem". A 12-month cliff concentrates supply into a single future date. | **Weighted dimension**, computable and hard to fake, but treat the predictive claim as unproven. Always report next-12-month unlock as % of circulating supply, not absolute token counts. |
| treasury runway | Strong. "Can they afford to keep operating?" | **Not mechanically computable across the population from a primary source.** DeFiLlama's `Expenses` (salaries, audits) is collected "mainly from annual protocol reports on their forums, so it's always referencing old data and will not be up-to-date in realtime like other DefiLlama data" [E2-C3]. Treasury is tracked as a separate field, excluding the protocol's own tokens by default [E2-C3]. No measured link from runway to survival retrieved. | **C for the components; none for computability at population scale or for prediction** | **Medium-High** | The numerator (treasury) is transparent, but the denominator (burn) is self-reported and stale; teams choose when to disclose a report. A treasury denominated in the protocol's own token reads as large while being worthless. | **Weighted dimension, with the denominator marked unavailable more often than not.** Show treasury *composition* (stablecoins vs own token) as context — that distinction is the whole signal. Never a gate: too much disclosure noise. |
| Nakamoto coefficient / voting concentration | Strong. "High coefficient = genuinely decentralised." | Measured and **qualified**. Zukowski: voting-vs-holding amplification in **13 of 18** protocols; results rank-stable between 12-month and full-history windows (Spearman ρ = 0.87, n=12) [E2-D3]. Ovezik et al. (SoK): "the seemingly innocuous choices… such as the size of estimation windows or the application of thresholds that affect the resource distribution, have important repercussions"; in PoW, consensus-layer participation "is not correlated with decentralization" and the field's assumption that "higher participation drives higher decentralization" is challenged [E2-D11]. SoK: the absence of an accepted methodology was litigated in SEC v. Ripple [E2-D11]. | **D, peer-reviewed (Frontiers in Blockchain) + D (SoK, ACAC chapter)** | **Medium** | Choosing a flattering threshold (33% vs 51%), a flattering resource (holdings vs votes vs multisig signers), and a flattering window; counting a Gnosis Safe 3-of-5 as three entities. Zeng/Sokolov-style estimates also depend on entity attribution. | **Weighted dimension, published with its methodology.** Never a gate: Ovezik et al. show the metric is not comparable across resources and systems without stated choices. |

### E2.2 notes on the ranking itself

Three rows deserve explicit promotion or demotion beyond the table:

1. **Only one metric survives as a hard gate: token-holder revenue**, and only in the existence-of-mechanism form. Every other metric's evidence either points the wrong way (volume, follower count), fails to be predictive at all (age, holder count, audits, treasury runway), or is real-but-specification-sensitive (HHI, Nakamoto, vesting). One gate out of fifteen is the honest yield.
2. **Two rows should be inverted from "positive" to "negative" inputs**: trading volume and audit count become penalties (wash-volume exposure, unaudited-and-unverified deploy) rather than rewards.
3. **Two rows are blocked on evidence, not on merit**: organic-vs-incentivised volume share and realised-PnL concentration. Both are conceptually right and both fail on retrievable measurement. They belong in v2 with a measurement pipeline attached, not in v1 with an invented weight.

---

## E2.3 Counter-evidence and base rates

This section is not a caveats appendix. Each item below is a reason to *reduce* what the tool is willing to say, and several of them are strong enough that a reasonable reviewer would ship a worse-looking scorecard on purpose.

### E2.3.1 Selective survivorship is not a hypothesis to test; it is a property of the sample

Every longitudinal dataset we retrieved that is big enough to be useful is also built from surviving, indexed, publicly-repository-having projects. Electric Capital's Open Dev Data explicitly depends on a "community-curated taxonomy" with 829 contributors adding repositories since inception [E2-C2] — a project nobody has heard of, or one that never opened a repo, is not in the denominator at all. 1kx's revenue dataset covers 1,244 protocols *that have identifiable fee revenue* [E2-D1]. Kuehn & Adnan's dead-coin study is the only one that deliberately balances alive against dead (41/41), and it is also the only one whose model fails [E2-D2].

The concrete consequence: **every "X predicts survival" claim in this space is estimated on the survivors' covariate distribution.** A high developer count looks predictive partly because the counterfactual — projects with low developer counts that survived — is systematically under-sampled from public indexers relative to projects with low developer counts that died. This inflates the apparent effect size of exactly the metrics that are easiest to observe on a surviving project.

Design implication for the tool: any survival statistic we compute must be reported with an explicit statement of which projects are in the denominator, and every derived statistic must be checked for the "surviving project observed on GitHub, dead project observed on nothing" asymmetry before it is used at all.

### E2.3.2 Widely-believed signals with weak or absent measured correlation

Stated plainly, with the evidence that undermines each:

| Belief | What we actually measured |
|---|---|
| "Reported volume proves real usage" | Wash trading averaged **>70% of reported volume** on unregulated exchanges, and fabricated volumes specifically **improve published rankings** [E2-D4]. On NFTs, ~38% of trades / ~60% of value manipulated [E2-D5]. |
| "More audits = safer" | The loss categories audits can see are not the loss categories that dominate: key compromise, phishing and social engineering ≈ **49.6% of cumulative losses**, "a negligible share of published audit findings" [E2-D6]. Flagged-vulnerable-but-unexploited: **>98%** of 23,327 flagged contracts [E2-D6]. Audits are pervasive, voluntary and standardised by nobody [E2-D7]. |
| "An audit announcement is a quality signal" | What is measured is a market reaction to disclosure [E2-D7], not a change in incident probability. These are different claims and only the first is supported. |
| "More holders = broader distribution" | Up to **66%** of airdropped tokens sold in the recipient's first post-claim transaction [E2-D9]. |
| "On-chain HHI shows how centralised governance is" | Delegation amplifies voting concentration above holdings in **13 of 18** protocols, by up to **21x** [E2-D3]. |
| "Higher participation = higher decentralisation" | In PoW, consensus participation "is not correlated with decentralization" — it captures a distinct signal [E2-D11]. |
| "Distribution / maturity / float explain concentration" | All three specifications are **null** in the 52-protocol study [E2-D3]. |
| "A strong social following signals a real community" | No measurement retrieved with a denominator and an out-of-sample statistic. |
| "A model can spot failing projects early" | Best held-out ROC AUC 0.98 → **0.59–0.65 on unseen data** [E2-D2]. And "multi-stage lifecycle model under current data conditions" is documented as practically infeasible [E2-D2]. |

The pattern across all nine: the belief survives because it is *directionally plausible and unfalsifiable at the level of individual projects*, and because in a survivorship-filtered sample the observable correlates of success line up with the observable correlates of existing.

### E2.3.3 Known, specific weaknesses of on-chain quality metrics

1. **Price contamination.** TVL is denominated in a volatile asset. DeFiLlama's own worked example: a 20% ETH drop makes TVL fall 20% "while USD inflows will be $0" [E2-C3]. Any TVL-based score silently inherits the token's beta.
2. **Self-reporting and staleness.** Operating expenses are gathered from forum reports and are "always referencing old data" [E2-C3]. Runway is therefore not population-computable.
3. **Three different "revenue" numbers.** Fees, Revenue and Holders Revenue are distinct and routinely conflated [E2-C3]. The gap between them is where the entire scoring question lives: ~400 protocols clear $1M in fees, 20 clear $10M to holders [E2-D1].
4. **Entity resolution is unresolved.** Zukowski needed 133 PCA exclusions across 38 protocols and a further 64 exchange-custody exclusions across 21 protocols to compute HHI meaningfully [E2-D3]. Any tool that does not replicate this overstates concentration by construction.
5. **Measurement choices are load-bearing and under-reported.** Window size and threshold choice "have important repercussions" [E2-D11]; Livepeer's HHI moves 0.199 → 0.033 under a different staking treatment [E2-D3].
6. **Metric gaming is *known to work against ranking* specifically.** Cong et al. document that wash volumes improve exchange ranking [E2-D4] — the adversarial case is not hypothetical, it is measured against the exact use we would make of these numbers.
7. **The reference population is not stable.** DeFiLlama tracks 7,000+ protocols and 500+ chains; thresholds calibrated on a 2023 population are not calibrated on the 2026 population. 1kx's own sector taxonomy is "consolidated" from DeFiLlama, TokenTerminal, CoinGecko and Messari [E2-D1] — four taxonomies, merged, with the merge unreproducible from the report.
8. **Coverage of the "denominator" is unknown.** There is no authoritative census of crypto projects against which coverage can be measured.

### E2.3.4 Labelling an early-stage project illegitimate: what a tool must refuse to conclude

The hard case is a project with 8 months of history, 2 developers, $400k TVL, no token-holder revenue, and a clean, unlocked contract. Under a naive weighted score that reads as "weak". The correct reading is "**insufficient evidence**", and the distinction is not cosmetic.

Reasons, each tied to something we measured:

- **The only survival model available needs a 90-day observation window and still fails out of sample** [E2-D2]. At 8 months, a project is inside the window where the literature's own best attempt does not discriminate.
- **Age is a precondition for scoring, not evidence of quality.** Kuehn & Adnan's design requires history [E2-D2]; 1kx's 20 holder-revenue protocols are old *by construction of the window* [E2-D1]. Scoring a project below the data horizon is scoring missing data.
- **The mechanisms that would justify a low score are exactly the ones that take time to become observable.** Vesting cliffs, unlock overhang, treasury drain and governance capture are all lagging indicators by nature.
- **A negative conclusion is asymmetric in cost.** A false "illegitimate" label on an honest early-stage project is unrecoverable; a "not enough evidence yet" label costs the user one quarter of waiting.
- **Zukowski's nulls are a warning about small samples.** With n=50 covariates-complete protocols, allocation/maturity/float all came back null [E2-D3] — and the one significant result was "driven entirely by Livepeer" out of 52 [E2-D3]. Single-protocol dependencies in a small-sample regression are the normal case, not the exception.

**Concrete refusals the tool must implement (recommended, and derived from the above):**

1. Below a minimum observation window (recommend: no scoring verdict at all below ~12 months of history, or below N quarters of complete fee data), return **"insufficient evidence"** — not a low score.
2. Never output a probability of being legitimate. Nothing in the retrieved evidence supports calibration to a probability. Publish confidence intervals and evidence counts instead.
3. Refuse any statement about a project's *intent* (fraud, scam, rug) from on-chain quality metrics. Intent is not on-chain. The measured fraud data is complaint-based and loss-based [E2-A2], not project-level.
4. Distinguish "no mechanism found" from "mechanism found but zero payments". These are completely different states and collapsing them is the single most common error in this space.
5. Never rank. Rank requires a complete, comparable population; §E2.3.3(8) says we do not have one. Report metrics and their measurement quality.

### E2.3.5 Base-rate discipline — why a high score in this population is weak evidence

The base rates:

- **>52%** of tokens launched since 2021 had ceased trading by early 2025 [E2-D2].
- Of 1,244 protocols with fee data 2020–Q3 2025, **~400** exceeded $1M annualised fees and **20** (≈1.6%) passed $10M to token holders [E2-D1].
- Revenue is extremely concentrated: **top 20 protocols = 70% of revenue** [E2-D1].
- Realised losses are heavy-tailed: **top 8 incidents = 50.6%** of cumulative dollar losses, top 20 = 71.4% [E2-D6].
- In the US, crypto complaints: **181,565 complaints, >$11B losses** in 2025, of 1,008,597 total IC3 complaints and >$17.7B in cyber-enabled fraud losses [E2-A2].

The calibration argument, stated as Bayes rather than rhetoric. Let a project's prior probability of being long-lived-and-non-rug be **p**, which the 52% token-death figure alone puts well below 0.5 for anything launched since 2021. A scoring tool observes S metrics and produces "high score". The quantity that matters is the **positive likelihood ratio** — how many times more likely a high score is under "long-lived and honest" than under "dead or fraudulent". Our retrieved evidence supplies **no** validated likelihood ratio for any metric. What it supplies instead is:

- For volume: a measured fabrication rate of 30–70%+ [E2-D4, E2-D5] — i.e. the *likelihood* of the observation is high under both hypotheses, so the likelihood ratio approaches 1 and the metric carries almost no information. This is the formal version of "high volume means nothing".
- For audits: prevalence is universal and unstructured [E2-D7]; flagged contracts are exploited <2% of the time [E2-D6]. Again, near-unit likelihood ratio.
- For holder count: the count is largely *purchased* by the project [E2-D9] — so it is evidence of spending, not of health.
- For token-holder revenue: the base rate of 20/1,244 makes absence of the mechanism genuinely informative [E2-D1]. Its presence is what carries information.

The practical consequence: **in a low-base-rate population, most "good" scores are uninformative, and the informative signals are the rare, expensive, hard-to-fake ones.** A tool that averages fifteen weak signals into a composite number actively destroys information, because it converts a set of near-likelihood-ratio-1 observations into a confident-looking scalar. This is the strongest single argument in this report for a **gate-plus-evidence-panel design instead of a score**.

Formally: with 15 metrics of average likelihood ratio ~1.05, multiplying them yields a "combined" likelihood ratio around 2.1 (1.05^15) — which sounds impressive and is arithmetically meaningless, because the metrics are neither independent nor individually calibrated, and the one metric that *is* informative gets diluted to 1/15 weight. Aggregation of uncalibrated indicators manufactures false confidence. That is the specific failure mode a legitimacy score is most likely to have, and it is why E2.4 treats "no combined scalar" as a falsifiable claim.

### E2.3.6 Have published "trust scores" ever been validated against outcomes? No.

Plainly: **we found no study validating any published crypto trust score, legitimacy score, or quality index against subsequent outcomes.** Not CoinGecko's Trust Score, not CryptoRank's, not any project-ranking composite we encountered.

The retrieved CoinGecko methodology is instructive precisely because it is unusually candid about what it is and is not [E2-C4]:

- It is an **exchange** score, not a project score. Scope error is the easiest way to accidentally cite it as project-level evidence.
- It is **rank-relative by construction**: the combined score "is graded on a curve consisting of the full population of scored exchanges… an exchange's final Trust Score reflects its relative standing among peers, rather than a number on a fixed scale." **A rank-relative score is not portable and has no fixed-scale meaning.** Any tool importing it must not treat "Trust Score 8" as "8 out of 10 good".
- Its largest component is **50% Liquidity** — the very family of metrics Cong et al. measure as up to 70%+ fabricated on unregulated venues [E2-D4]. CoinGecko's answer is to score liquidity by consistency against a trusted benchmark set plus an exchange-level quality gate plus manual review; that is a real mitigation, and it is *specific to centrally-controlled orderbooks with institutional liquidity*. It does not transfer to a DEX where volume can be manufactured by a handful of related wallets.
- 20% Cybersecurity, 15% Regulation, 10% Incident, 5% Proof of Reserves. Only the Regulation component has a hard verification standard: credentials are checked against the official register, and "self-reported credentials that cannot be independently confirmed are not scored" [E2-C4].
- Recalculated weekly — so scores are volatile snapshots, and no documented backtest exists.

CryptoRank: **no official methodology document was retrieved.** Its scoring is UNSOURCED here and is not used as evidence anywhere in this report.

Why the absence matters more than it looks. A trust score that has never been validated against outcomes is a **presentational artefact**: it converts a set of chosen indicators into a number that carries unearned authority. Worse, the components are chosen for *availability and marketability* (there must be an exchange, there must be a headline number, it must update weekly), and the ranking-relative design means the score measures position in a vendor's panel rather than an absolute property. The report should treat every published composite score as an **input selection to be audited, never an output to be reported**.

### E2.3.7 Two claims in the wider report that this slice weakens

- Any statement that "developer activity is the strongest predictor" must be downgraded to "the most measured and least gameable *descriptive* indicator", because no project-level survival statistic was retrievable (§E2.1-F1) and the one predictive model in the literature omitted it and still failed [E2-D2].
- Any statement that "revenue proves value capture" is wrong as stated. The measured distinction is fees vs revenue vs holders revenue [E2-C3], and only the third is the relevant quantity [E2-D1].

---

## E2.4 What would falsify this report

Stated as specific, checkable findings. Each is phrased so that a published number could settle it either way. Ordered by how much each would change the conclusions.

**F1. A validated project-survival model that includes developer metrics.** If a peer-reviewed or working-paper study with a proper dead-alive-balanced sample, a chronological split, and reported out-of-sample AUC showed that a *project-level* developer-activity feature (e.g. commits-over-12-months, deduplicated developers) adds materially to out-of-sample discrimination — say AUC ≥ 0.70 on genuinely unseen data — then §E2.2's demotion of developer metrics from "strongest leading indicator" to "descriptive dimension" would be wrong, and §E2.3.1's survivorship critique would need weakening. **Current counter-evidence:** the only predictive model we found used price and market cap only and scored 0.59–0.65 out of sample [E2-D2].

**F2. A primary, reproducible post-incentive TVL retention figure.** The recurring "40–70% of incentivised TVL exits within 30 days" claim is UNSOURCED here. If DeFiLlama, Artemis or a peer-reviewed paper publishes a table of pre/post-emission TVL for a defined cohort with stated methodology, and retention turns out to be **>80%**, then (a) the organic-share row moves from UNSOURCED to a high-value weighted dimension, and (b) the sticky-capital critique of TVL weakens materially. If the primary number lands at **<50%**, TVL should be demoted further — to a negative input only.

**F3. Wash-trading rates materially lower on-chain than on unregulated CEX.** Cong et al.'s >70% figure is for unregulated *exchanges* [E2-D4]; Falk et al.'s ~38%/60% is for NFT markets [E2-D5]. If a primary measurement on *major DEX spot markets* found wash share materially below, say, 20%, the volume row would move from "reject as positive input" to "weak positive input with a penalty" and the "reject" recommendation in §E2.2 would be reversed. This is the single most likely finding to change.

**F4. A published trust score with a documented backtest.** If CoinGecko, CryptoRank or any comparable provider published an out-of-sample evaluation — e.g. "top-decile Trust Score projects had X% survival at 24 months vs Y% for the bottom decile", with a stated cohort — then §E2.3.6's central claim ("no published trust score has been validated against outcomes") is false as stated and must be corrected. Note this claim is about *existence of published validation*, not about the scores' quality; a weak result would still be a real result and would be a substantial addition to the literature.

**F5. Audit *type and scope* predicts incident rates even though count does not.** §E2.2 rejects audit count but this does not imply audits are useless. If a study showed that audits of a *named reputable firm* covering the *deployed, non-upgradeable* contracts reduce realised exploit incidence by a measurable margin (with a stated base rate), then the audit row should be promoted from "reject as score / context" to a **weighted dimension**, and the recommendation in §E2.2 ("reject as a score") would need revision. Currently: 49.6% of losses are in categories audits do not examine [E2-D6], and <2% of flagged contracts are exploited [E2-D6].

**F6. Holder concentration predicting adverse outcomes.** §E2.2 recommends HHI as a weighted dimension and explicitly *not* a gate, on the basis that large surviving systems exist with dispersed (JUP 0.12x) or concentrated (LPT 0.27x holding HHI) governance [E2-D3]. If a study showed concentrated HHI *does* predict failure or hostile outcomes within a cohort, the "never a gate" recommendation would be wrong. Also: if voting-HHI proved to predict something that holding-HHI does not, holder HHI as a proxy for governance risk would have to be retired entirely.

**F7. Token-holder revenue prevalence is much higher than 20/1,244.** §E2.2 promotes token-holder revenue to the **only hard gate**, resting on the 1.6% base rate [E2-D1]. If a measurement with a broader or more careful protocol census found, say, 30%+ of live protocols with sustained holder accrual, then absence of the mechanism would no longer be strong evidence of anything and the single-gate recommendation would collapse to zero gates. This is the highest-leverage falsifier, because the entire E2.2 conclusion rests on one prevalence figure.

**F8. Follower count / social breadth has predictive value net of bought engagement.** §E2.2 rejects it as practitioner-only. Any study with a verified-identity design (excluding purchased accounts) and a reported out-of-sample statistic would require re-admission. Conversely, a measured demonstration that follower counts are ~trivially purchasable at published unit prices would strengthen the rejection.

**F9. Treasury runway is computable at population scale.** §E2.2 notes DeFiLlama's own disclosure that expenses data is stale and forum-sourced [E2-C3]. If a primary source with timely, mechanically-derived protocol operating costs existed and correlated with survival, the runway row would move from "weighted dimension with often-missing denominator" to a real gate candidate.

**F10. The §E2.3.5 aggregation argument is wrong.** §E2.3.5 asserts that averaging many near-uninformative metrics manufactures false confidence, and that a gate-plus-panel design is therefore required. This would be falsified by a demonstration that a composite of the fifteen metrics **does** outperform the best individual metric on a properly labelled, out-of-sample cohort — i.e. that the indicators carry partially independent, individually-calibrated information that survives aggregation. Nobody has shown this for this metric set; if someone does, the design recommendation changes and the skeptic's position is wrong.

**F11. Nakamoto coefficient turns out to be well-defined.** Ovezik et al. show measurement choices have large repercussions and that PoW participation is uncorrelated with decentralisation [E2-D11]. If a consensus methodology emerged and were adopted, the Nakamoto row could move from "weighted dimension with published methodology" to a comparable, hard gate. Note the SoK's own governance observation: the absence of an accepted method was material in SEC v. Ripple [E2-D11] — which cuts both ways, since a single standardised definition could equally enable a regulator to treat high scores as dispositive.

---

## Scoring signals extracted

| signal_id | what it measures | how to verify mechanically | evidence tier | failure mode | confidence |
|---|---|---|---|---|---|
| E2-S01 | Existence of a verifiable mechanism routing value to token holders (buyback-and-burn, fee burn, direct staking distribution), sustained ≥2 quarters | Parse the token contract + fee router; confirm a burn/distribution path; sum realised holder revenue over ≥2 quarters from DeFiLlama `holders-revenue` and cross-check on-chain | C definition [E2-C3] + D prevalence [E2-D1] | Mechanism present but never executed (declared fee switch, dormant router); "holder revenue" funded by treasury token sales rather than earnings | **High** — the only row supporting a gate |
| E2-S02 | Share of airdropped / claimed tokens sold in the first post-claim transaction | For each claim event, compute time-to-first-dispose from on-chain transfers | D [E2-D9] | Genuine long-term recipients look identical to farmers on a per-claim basis; requires cohort analysis to separate them | **High** (as a manipulation detector), Low as a quality predictor |
| E2-S03 | Holder-revenue prevalence as a base rate: what fraction of comparable live protocols have any holder accrual | Recompute on the current protocol universe; compare against 20/1,244 ≈ 1.6% [E2-D1] | D [E2-D1] | Denominator is protocol census-dependent; 1kx window is 2020–Q3 2025 | **High** for the order of magnitude, Low for the exact figure — see E2.4-F7 |
| E2-S04 | Whether reported volume survives wash-trading screening | Cong et al. tests: first-significant-digit distribution, size rounding, transaction tail distribution [E2-D4]; or on-chain connectivity clustering (Falk et al. [E2-D5]) | D, peer-reviewed + preprint | Estimation error: indirect methods are "error-prone in the NFT setting" [E2-D5]; a whale market-maker legitimately dominates | **High** that screening is necessary, **Medium** on the threshold |
| E2-S05 | Wash-volume share estimate per protocol (and the *direction* of the adjustment) | Estimate share of volume attributable to linked wallets; report as a range, not a point | D [E2-D4, E2-D5] | Under-detection on new venues; over-flagging of legitimate LP/market-maker flow | **Medium** |
| E2-S06 | TVL purity: share of TVL surviving DeFiLlama's own exclusions (borrowed, own tokens, vesting, double-counted, unproductive) | Recompute from raw contract balances against DeFiLlama's documented exclusion list [E2-C3] | C [E2-C3] | Exclusions are DeFiLlama's judgement calls; third-party scorers almost never replicate them | **High** |
| E2-S07 | USD inflows vs TVL change (deposits separated from price effect) | DeFiLlama's own method: per-asset balance deltas × price, summed [E2-C3] | C [E2-C3] | Depends on the underlying balance data; unavailable for many protocols | **High** where computable |
| E2-S08 | Voting-HHI / holding-HHI amplification ratio (does governance concentrate above token ownership?) | Compute holding HHI and voting HHI (Tally delegates / Snapshot vote-weighted / VSR lockup-weighted); report the ratio [E2-D3] | D [E2-D3] | Requires the same entity resolution as HHI; ratios are threshold- and venue-dependent | **High** as a *disclosure* item, Medium as a score |
| E2-S09 | Entity-resolution completeness before any concentration statistic is published | Count and disclose PCA exclusions, exchange-custody exclusions, bridge/vesting labels — Zukowski needed 133 across 38 protocols + 64 across 21 [E2-D3] | D [E2-D3] | Silent under-counting inflates apparent concentration by construction | **High** |
| E2-S10 | Share of cumulative security losses falling outside code-auditable categories | Replicate the [E2-D6] taxonomy against a current incident set; baseline: key compromise + phishing + social engineering ≈ 49.6% of losses vs negligible share of audit findings | D preprint [E2-D6] | Incident taxonomies are fragmented and disagree (the paper's own stated limitation); rekt.news-derived | **Medium-High** |
| E2-S11 | Exploit-rate given a positive static-analysis flag | Percent of tool-flagged vulnerable contracts later exploited; baseline: <2% of 23,327 [E2-D6] | D preprint, reported in [E2-D6] | Undercounts exploits that are unreported; flag population is tool-specific | **Medium** |
| E2-S12 | Whether a deployer contract has an *identifiable* audit from a *named* firm, and whether it covers the deployed (not just pre-deploy) code | Resolve the audit report to a firm, a commit hash, and the deployed bytecode | D [E2-D7] (no standards exist) | Reports exist but certify a trivial surface; proxy/bridge/oracle paths uncovered | **Medium** — usable as a gate in the narrow "unaudited **and** unverified" form only |
| E2-S13 | Developer-activity *composition*: share of commits from established (12+ month) developers | Electric Capital Open Dev Data `eco_mads` tenure segments and contribution ranks [E2-C1] | C [E2-C1] | Requires the full fingerprint-dedup pipeline; raw GitHub counts are trivially gamed | **High** for the metric's value, **none** for its predictive claim (E2.4-F1) |
| E2-S14 | Minimum observation window before any verdict is issued | Compare the project's data horizon to the model's usable input window (90-day sequences in [E2-D2]) | D [E2-D2] + C [E2-C1] | A tool that scores anyway converts missing data into a low score | **High** |
| E2-S15 | Base-rate context attached to every output: fraction of the comparable universe with the same metric profile | Recompute the universe denominator each run; publish it | D [E2-D1, E2-D2, E2-D6] | Silent denominator inflation recreates survivorship bias at scoring time | **High** |
| E2-S16 | Next-12-month unlock overhang as % of circulating supply, split team/ecosystem/investor | Read on-chain vesting contracts / published schedules; annualise | D preliminary, not retrieved to primary [see E2.1.10] | Team-vs-ecosystem classification is self-declared; cliff timing is chosen by the issuer | **Medium** for the number, **Low** for the predictive claim |
| E2-S17 | Treasury composition: share in stablecoins vs the protocol's own token (runway *quality* proxy) | Read treasury addresses; value each component at current prices; report the split, not just the total | C [E2-C3] | Treasury address set is often incomplete; own-token treasuries are circular | **Medium** — more reliable than runway itself |
| E2-S18 | Staleness of the protocol's own operating-cost disclosure (proxy for whether any cost data exists at all) | Date the latest forum/annual report; compute disclosure lag | C [E2-C3] | Teams disclose opportunistically; a fresh report may still be wrong | **Medium** |
| E2-S19 | Decentralisation-measurement sensitivity: recompute HHI / Nakamoto under ≥2 thresholds and ≥2 windows and report the spread | Run the [E2-D3] and [E2-D11] robustness protocol; report the range | D [E2-D3, E2-D11] | A single-point number conceals the sensitivity the SoK documents | **High** — this is a *meta*-signal and the cheapest thing to implement |
| E2-S20 | Whether a composite score beats its best single component out of sample (the E2.3.5 test) | Periodic, pre-registered: score a labelled cohort, compare composite AUC to best-component AUC | none — this signal does not yet exist | Untested; this report predicts the composite will NOT outperform, but that prediction is itself unverified | **Low confidence in the prediction; High confidence the test is worth running** |
| E2-S21 | Verification status of a provider-published trust/legitimacy score before it is imported (does it publish a method? a backtest? is it rank-relative?) | Read the provider methodology page; check for backtest, stated denominator, and whether the score is rank-normalised | C [E2-C4] (CoinGecko only) | Rank-relative scores imported as absolute values [E2-C4]; CryptoRank method unavailable (UNSOURCED) | **High** as a process control |
| E2-S22 | Redemption/sell-through of token claims as a manipulation signal, reported per cohort not per project | Cohort-level first-dispose timing from on-chain transfers, as in [E2-D9] | D [E2-D9] | Requires a claim-event dataset the tool does not otherwise have | **Medium** |

---

*End of E2. Sources: `E2_SOURCES.md`. This file contains no project case studies by design.*
