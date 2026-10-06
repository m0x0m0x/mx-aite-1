# F — Measurable Data Indicators, Data-Source APIs, and Scoring/Assessment Frameworks

**Slice owner:** subagent F (measurement & tooling layer)
**Date accessed:** 2026-10-06
**Scope note:** this file covers *how to measure and how to aggregate*. Security, tokenomics and regulatory theory are owned by other slices; where they appear here it is strictly as an **evidence standard** or a **measurable input**, not as legal analysis.

---

## F1 Existing scoring frameworks — comparative teardown

### F1.1 CoinGecko Trust Score (the most reusable published model found)

CoinGecko publishes a full methodology page, which is rare and valuable. Current composition (Basilisk update, May 2026):

| Component | Weight |
|---|---|
| Liquidity | 50% |
| Cybersecurity | 20% |
| Regulation | 15% |
| Incident | 10% |
| Proof of Reserves | 5% |

Design features worth stealing, verbatim from their own documentation:

- **Weighted sum, then curve-graded over the population.** "The final Trust Score is not a simple weighted sum. Instead, the combined score is graded on a curve consisting of the full population of scored exchanges." Result: a *relative* standing, not a fixed scale. **Weakness for us:** a curve guarantees a top decile even when the whole cohort is bad, so it is a ranking device and must never be used for a pass/fail verdict.
- **An exchange-level quality gate.** "An exchange-level quality gate ensures that just maintaining a small number of clean trading pairs, while the rest of the tickers on the exchange exhibiting anomalous activity or poor liquidity is not sufficient to receive a high score." This is a **gate layered on top of a weighted score** — the single most directly transferable mechanism in the whole file. It is the industry equivalent of "one fatal red flag cannot be averaged away."
- **Register verification, not self-report.** Three steps: identify the claimed credential + issuing authority + jurisdiction → check it against the official register → classify on an oversight spectrum. **"Self-reported credentials that cannot be independently confirmed are not scored."** Only the *highest-ranked verified* credential counts. This is the template for verification-of-verification.
- **Benchmark-set deviation scoring.** Each subject is compared against a reference set of high-confidence-truthfulness venues on volume consistency and order-book depth range, rather than against absolute thresholds. **Weakness:** contamination of the reference set poisons the whole distribution.
- **Excluded-but-displayed attributes.** Team Presence and API coverage are shown but explicitly *excluded from the score* and may be added later. Useful pattern: separate "evidence we display" from "evidence we score."
- **Deliberately minimised manual review.** Human review may catch in-context anomalies, "care is taken to minimize human intervention in determining the final liquidity score."
- **Versioned methodology.** TrustScore 1 (May 2019) → 2 (Sep 2019) → Cybersecurity (Jul 2020) → Team Presence & Incidents (Nov 2020) → PoR (Jan 2023) → Basilisk (May 2026). Weekly recalculation "balances responsiveness to real changes against the ranking volatility that more frequent recalculation would create."
- **The deprecation of a proxy is the lesson.** Basilisk *removed* SimilarWeb web-traffic normalisation because "Most trading now happens through mobile apps and APIs, making web traffic an increasingly unreliable signal." Proxy metrics decay; ask what you actually want.
- **Outsourced component = opaque input.** The Cybersecurity Score is "evaluated by CORE3, a Risk Infrastructure & Intelligence Platform powered by Hacken" — an unpublished third-party score injected at 20% weight. **Weakness:** a 20% weight on a formula we cannot inspect or reproduce.

The older per-pair methodology page separately documents a *pair-level* Trust Score using web traffic (SimilarWeb), orderbook spread and ±2% depth, volume, trade frequency and outlier checks, coloured Green/Yellow/Red. It explicitly says the pair score "is not final and changes in real-time."

### F1.2 The averaging failure mode, demonstrated from CoinGecko's own published weights

This is the concrete mathematical motivation for F4. With components normalised to [0,1] and S = Σ wᵢxᵢ, Σwᵢ = 1, let F be a fatal criterion and τ the passing threshold. If x_F = 0 (total failure on the fatal criterion), the maximum attainable score is S_max = 1 − w_F. Failure is forced only when S_max < τ, i.e. 1 − w_F < τ, i.e.

> **a fatal criterion can only ever force a failure if w_F > 1 − τ.**

CoinGecko's published weights: w_PoR = 0.05, w_Incident = 0.10. For any reasonable τ (say 0.7, i.e. "above 7/10"), 1 − τ = 0.30, and both 0.05 and 0.10 are far *below* it — and since a large weight is what forces failure, being below the threshold means they cannot force it. So under a linear model an exchange with **zero** proof of reserves and **zero** recorded incidents is arithmetically incapable of failing. (Verified numerically: w_F = 0.30 gives S_max = 0.70, which does not clear τ = 0.70; w_F = 0.31 gives 0.69 and does.) This is not a bug — CoinGecko's own documentation confirms the intent: PoR is a *floor* ("an exchange that does not have any form of asset disclosure will not have a 10/10 Trust Score"), not a gate. The lesson for our tool is not that CoinGecko is wrong (they are scoring exchanges, where custody is one dimension among many); it is that **if we believe a dimension is fatal, we must implement it as a gate, because weighting mathematically cannot do the job.**

### F1.3 CoinMarketCap — Listings Criteria and the Transparency/Data Alliance

Official criteria define three listing states:

- **Unverified** — DEX pair pages created by automated on-chain processes, not reviewed.
- **Verified** — "manually reviewed by the CMC team… on a best-efforts basis. **Verification does not guarantee project quality.** It simply means that CMC believes that the information originates from the rightful source."
- **Tracked** — meets Section B guidelines *and* exhibits strength across Section C factors.

Section C is an eight-factor holistic framework: (1) Trading Volume & Market Pairs; (2) Community Interest; (3) Traction/Progress; (4) Team; (5) Product/Market Fit; (6) Impact & Practicality; (7) Uniqueness & Innovation; (8) Project Longevity & Activity. CMC states this is **"not simply a matter of ticking off a checklist or hitting predefined thresholds, as we benchmark submissions against others in the cohort."** **Weakness:** a purely holistic, unpublished weighting is not reproducible or auditable; "Product/Market Fit" and "Impact & Practicality" as written are not measurable.

Two elements are highly reusable:

- **Audit badges are pulled from partner APIs, not granted by CMC.** Public feed endpoints: `https://cmc-api.hacken.io/`, `https://www.fairyproof.com/fairyproof_data/cmc.json`, `https://al.quantstamp.com/api/cmc`, `https://cmc.certik-skynet.com/v1`, `https://www.coinscope.co/api/audit/cmc`. The badge means "a named third party published a finding for this address" — it carries no severity, date, or remediation status. This is a machine-readable third-party-anchor mechanism we can query directly.
- **Verified and self-reported circulating supply are displayed separately**, and self-reported CS has "no ranking implications." **Pattern to adopt: never blend a verified figure and a self-reported figure into one number.**

The Data Accountability & Transparency Alliance publishes partner criteria that are unusually operational: operating ≥1 year; ≥2,000 active users or demonstrable traction; ≥2 meaningful partnerships outside the operating group; **≥1 technical implementation peer-reviewed by credible institutions**; ≥50 citations in major crypto media in the past year. These are testable.

### F1.4 Trust Wallet — the only numeric on-chain listing thresholds found

Official listing requirements include measurable floors:

- **Minimum 10,000 holders and 15,000 transactions. "Airdropped tokens are excluded from these counts."** This is a directly implementable anti-Sybil haircut on holder metrics and should be adopted verbatim in spirit.
- Completed **full audit by a reputable security firm**; live site with white paper, roadmap, tokenomics, use case; active socials with a **responsive support team**; "Accounts with fake followers or bots will be rejected"; originality/no cloning of established projects or stablecoins.
- A three-state **token status taxonomy**: `active` (meets circulation requirements), `spam` ("distributed to a large number of recipients that have no inherent value or has been verified as a dishonest scheme or fraud"), `abandoned` ("very low activity — below 100 token transfers a year, migrated to mainnet or to a new contract"). That is a clean three-band scheme with an explicit liveness floor.
- **Caveat disclosed on their own docs:** submissions carry a non-refundable PR fee and "Payment of the Pull Request Fee does not guarantee your asset will be approved." A paid submission route weakens the signal; we must treat Trust Wallet status as one input, not a verdict.

### F1.5 FATF — Travel Rule / Recommendation 16 as an *evidence standard*, not a score

FATF Recommendation 16 (Payment Transparency), applied to VA transfers via INR.15, requires that originating VASPs "obtain and hold required and accurate originator information and required beneficiary information on VA transfers, submit the above information to the beneficiary VASP or financial institution (if any) **immediately and securely**," and make it available to authorities. The 2021 VA/VASP Guidance defines "account number" for VA purposes (e.g. wallet address) and clarifies "immediately and securely."

Three reusable points:

1. **FATF explicitly rejects a weak implementation:** the guidance states FATF "does not accept post facto transmission travel rule data." So the scored item is *demonstrated* transmission, not claimed policy.
2. FATF's 2025 Best Practices in Travel Rule Supervision report identifies "good practices … in terms of assessing Travel Rule Compliance Tools" — i.e. FATF itself treats *tool assessment* as a scoring problem, and notes the "sunrise issue" (uneven cross-border adoption) as a known evasion path.
3. FATF guidance ¶14(g) lists as a VASP risk factor "Whether the VASP implements the 'travel rule' or not and how effectively it has mitigated the 'sunrise issue'." Counterparty-hygiene is an accepted risk axis.

The **FATF Virtual Assets Red Flag Indicators** (Sept 2020, built from 100+ case studies contributed by FATF Global Network members) gives a regulator-grade taxonomy of transaction, transaction-pattern, anonymity, sender/recipient, source-of-funds and geographic red flags. Reusable as an interpretation checklist for on-chain patterns — **not** as a legitimacy score. FATF is an AML supervisor's framework; it says nothing about whether a protocol has a real product.

### F1.6 Proof of Reserves — attestation vs on-chain proof

The strongest treatment is Nic Carter et al., *Proof of Reserves: The Practitioner’s Guide to an Emerging Standard*:

- **Proof of assets is nearly worthless on its own; the binding number is liabilities.** "a digital asset platform may have experienced a loss, or management may be attempting to defraud customers, may underreport liabilities to give the impression the platform is fully reserved." The taxonomy distinguishes *Proof of Platform Reserves* (PoPR), where the claim is assets ≥ liabilities and the customer can verify their own balance is inside the liability set.
- **Merkle proofs are the verification-of-verification primitive**: a Merkle Root lets each customer confirm their Account Leaf links to the root, "demonstrating inclusion within the PoPR," while preserving privacy.
- The guide also covers zero-knowledge variants (Provisions; Boneh/Bünz/Bonneau/Clark) and the evidential requirements for **proof of control** over reserve assets.

Hacken's published PoR Audit Methodology v3 is the concrete procedure, and three of its steps are directly stealable:

- The Client Liability Report is generated from a **production replica database**, with the auditor observing the extraction scripts and reconciling totals.
- The Merkle root is built from that data, and the auditor **randomly samples 10 PoR user IDs** and cryptographically tests inclusion.
- A **dummy-account negative control** is run to confirm only valid records entered the tree.
- Collateralisation ratio = in-kind assets ÷ client liabilities, standardised to USD.

**Known weaknesses to encode in our scoring:** PoR is point-in-time; the entity chooses the in-scope wallet set; internal DB integrity is assumed; omitted liabilities are only detectable through sampling; and the disclosed snapshot boundary ("any tokens outside of the scope or subsequent transactions made after the snapshot are not included"). Treat PoR as a **recency-bounded** evidence item.

**OKX's published implementation is the strongest live specimen we found.** It uses zk-STARK (via FRI) over an encrypted Merkle trace table, with three explicit constraints: (1) the claimed total equals the sum of all account balances; (2) **every account has non-negative net equity** (positive net equity); (3) **every account's total balance is included** in the calculation. It publishes the list of wallet addresses with signed "I am an OKX address" messages, an open-source zk-STARK validator, and a downloadable per-report archive with report IDs and dates (e.g. report IDs 499955235 / 500137125 / … with dates, "zk-STARK v2"). That is a genuinely user-verifiable artefact set. **Caveat:** it verifies OKX's *account ledger*, not that OKX is solvent against off-ledger obligations, and it is entirely self-published.

### F1.7 ISO/IEC 27001:2022 and SOC 2 / AICPA Trust Services Criteria

**ISO/IEC 27001:2022** (Edition 3, 2022-10) is an ISMS *requirements* standard: "Conformity with ISO/IEC 27001 means that an organization or business has put in place a system to manage risks related to the security of data owned or handled by the company." ISO's own survey notes it accounts for "almost a fifth of all valid certificates to ISO/IEC 27001" (ISO Survey 2021). **Strength:** accredited third-party certification → mechanically verifiable by certificate number, certifying body and scope. **Weakness:** it certifies *process*, not outcome; scope statements can be narrow; it is silent on smart-contract and economic-security risk.

**AICPA 2017 Trust Services Criteria (with Revised Points of Focus 2022)** covers five categories — Security, Availability, Processing Integrity, Confidentiality, Privacy — and explicitly applies at the level of "an entire entity; a subsidiary, division, or operating unit… a function relevant to the entity's… objectives; or a particular type of information." **That scope flexibility is the key verification hazard:** a SOC 2 report may cover a single support tool, not the product that holds user funds.

CoinGecko's Basilisk note states that in the near term it is "looking at adding SOC2 Type 2 and ISO27001 certifications as a factor for cybersecurity." That gives us a citable, current, industry-endorsed design intent. **Our adoption rule (our proposal):** an operational-security signal only scores if the submission states (a) attestation type (SOC 2 Type 2 vs Type 1; ISO 27001 certificate vs statement of applicability), (b) **period covered** for Type 2, (c) the **scope statement** verbatim, (d) auditor identity and accreditation, and (e) a bridge letter tying the audited entity to the exact legal entity and domain that operates the protocol. Anything less = unverified claim, weight 0. This mirrors CoinGecko's three-step register verification.

### F1.8 Where I found no reusable methodology (honest gaps)

- **CryptoRank** — no official methodology document for its trust/legitimacy score surfaced in this wave. Searches returned only third-party site-analysis blogs (Tier E, discovery only, not cited). **Do not model our design on it.** Its public score is treated as an opaque vendor number.
- **Santiment** — no official published weighting methodology located.
- **Coinbase verification/attestation programme** — no official methodology document retrieved. I am not asserting what Coinbase publishes.
- **Nansen reserves pages** — identified only indirectly (Hacken's methodology doc names Nansen and DefiLlama as the places reserve data appears). These are **project-submitted** datasets, not independent audits.

---

## F2 Free / authoritative data sources with API access

Covered below and in the `## Data source table`. Key reliability judgement, signal by signal:

**Genuinely free and mechanical (Tier A/B):** OFAC Sanctions List Service (names, not addresses), GitHub REST API (60/hr unauthenticated IP-based; 5,000/hr authenticated PAT; 15,000/hr for GitHub Apps on GitHub Enterprise Cloud orgs; search endpoints are more restrictive; Git LFS has its own bucket), Etherscan API V2 (Free tier: **3 calls/second, up to 100,000 calls/day, selected chains only**, no PRO endpoints; Lite 5 cps/100k; Standard 10 cps/200k; 60+ EVM chains under one key selected by `chainid`), DeFiLlama Free API (**no auth**, `https://api.llama.fi`, 31 endpoints).

**Free but quota-bound:** CoinGecko Demo key (100 calls/min, 10,000 calls/month) vs keyless (**~10–30 calls/min CoinGecko, ~10 calls/min GeckoTerminal**, IP-shared; CoinGecko's own docs say it "is not suitable for production workloads, scheduled polling, or high-frequency updates"). All requests including 4xx/5xx count toward the limit. Paid plans start at $35/mo.

**Free but not really free at scale:** Dune's Free plan charges **20 credits per MB exported** (Analyst 10, Plus 2). Execution endpoints are credit-based; metadata endpoints are free. Max result size 32 GB with truncation. The deeper issue is epistemic, not financial: a Dune number is **SQL that someone wrote** — it is curated, not canonical, and we are trusting an anonymous author.

**Self-reported / unverifiable — flagged explicitly:**
- CoinGecko and DeFiLlama both price tokens largely from the same upstream feeds (DeFiLlama: "Almost all tokens are priced using CoinGecko's API"). Circular dependency between "independent" providers.
- DeFiLlama adapters are community-maintained per project; inclusion/exclusion logic varies by project and can change.
- Artemis exposes a `internal_data_source` and `source` field per metric (e.g. `24H_VOLUME` → `Source: Coingecko`), which is genuinely useful provenance — but the **REST API is enterprise-gated**: their API-key doc says "If there is no API key listed here, it means you are not an Enterprise customer and will need to upgrade." The $0 tier is the Analyst product, not the API.
- Nansen covers Address Current/Historical Balances, Address Transactions, **Address Counterparties**, **Address Related Wallets**, Address PnL & Trade Performance, **Address Labels**, Historical Top Holders, Who Bought/Sold, Smart Money netflows/holdings, Portfolio, and a Permissionless Rewards (Points) API. `Address Related Wallets`, `Address Labels` and `Historical Top Holders` are precisely the primitives our Sybil-clustering and ex-treasury concentration metrics need. **Pricing not published on the docs pages I read → unknown. Treat as paid.**
- TRM Labs, Chainalysis, Solidus Labs: no free tier documented in this wave → unknown; commercial contract.
- GoPlus publishes a rich API surface (Token Security, Malicious Address — explicitly "Free, timely, and comprehensive", NFT Security, Approval Security, dApp Security Info, Signature Data Decode, Phishing Site Detection, Token Security for Solana (Beta), EVM/Solana Transaction Simulation). **I could not retrieve a documented rate limit or endpoint path table** → unknown. It is an opaque automated risk score; use as a triage flag, never as a weighted score input.

**Critical negative finding for sanctions screening:** the OFAC Sanctions List Service is an **entity/name** service. Its search tool "employs fuzzy logic on its name search field." **OFAC does not publish on-chain address screening.** We cannot screen a wallet address against OFAC with Tier A data; on-chain attribution requires a commercial chain-analytics provider or our own heuristics. Do not let the report imply otherwise.

---

## F3 Quantitative indicators with formulas

Full table in `## Implementable metrics`. Definitions and their provenance:

**M01 `hhi_holders`** — HHI = Σ sᵢ², sᵢ = address_balanceᵢ / Σ address_balance, over non-zero holders. **Recognised standard:** DOJ/FTC Herfindahl–Hirschman Index (sum of squares of market shares; approaches 10,000 in a monopoly, ~0 when many equal firms). **Our extension:** applying a firm-market-share index to token holder distribution is *our* proposal; the standard is defined for market concentration. Always report on both a 0–1 and 0–10,000 scale.

**M02 `hhi_ex_treasury`** — same sum of squares after excluding labelled non-economic holders: treasury, team/foundation, vesting contracts, burn addresses (0x0…/0xdead), staking/LP contracts, bridge contracts, and CEX omnibus addresses. **Depends entirely on label quality** (Etherscan Address Tags, Nansen Address Labels). An unlabelled insider wallet survives the exclusion; treat as medium confidence.

**M03 `top10_ex_treasury` + Gini** — Σ top-10 sᵢ after the same exclusion. Report alongside the Gini coefficient G = Σᵢ Σⱼ |xᵢ − xⱼ| / (2n²μ), which is less sensitive to the single-largest-holder artefact.

**M04 `holders_tvs_divergence`** — elasticity e = (Δ% monthly holders) ÷ (Δ% monthly TVL) over a common window (90d recommended). Banding: e ≥ 2 with TVL rising but M12 organic volume flat = participant growth is farmed; e < 0.5 with TVL rising = TVL concentrating into fewer hands. **Our own proposal**; no published standard located.

**M05 `fee_to_holder_revenue`** — ratio = (revenue actually accruing to token holders, i.e. buybacks + distributions **observed as executed on-chain transfers**) ÷ (net protocol fees). DeFiLlama supplies fees and revenue separately via `/overview/fees` and `/summary/fees/{protocol}`; we must not rely on dashboard fields for the holder-accrual leg — that must come from traced transfers.

**M06 `emission_adjusted_yield`** — headline APY = annualised(rewards_paid + fees + native_emissions) ÷ pool_TVL; real yield RY = annualised(rewards paid in assets **not** minted by the protocol and **not** the chain's native token) ÷ pool_TVL; report RY/APY. When rewards are denominated in a token the protocol itself prints, the ratio → 0, which is the finding. **Our own proposal**, built on DeFiLlama `/pools` and `/chart/{pool}`. Caveat: use DeFiLlama's core-TVL figure (borrowed coins, protocol's own tokens, Staking, Pool2 and non-circulating/vesting tokens are excluded) or you double-count.

**M07 `unlock_overhang_dtwu`** — proximity-weighted 90-day unlock overhang = Σᵢ uᵢ·wᵢ where uᵢ = unlock amount as a fraction of circulating supply, dᵢ = days until that cliff, wᵢ = (90 − dᵢ)/90 for dᵢ ≤ 90 and 0 otherwise. Report also the undiluted version (Σuᵢ over 90d) so the weighting is inspectable. **Our own proposal.** Input: DeFiLlama Pro `/api/emissions` and `/api/emission/{protocol}` — but treat as a *claimed* schedule until it is reconciled against a deployed vesting contract's methods and a timelock/multisig that can actually change it.

**M08 `dev_decay_rate`** — fit ln(rolling-90d commits) against t over 12 months; k = exp(slope) − 1, so k < 0 is decay. Filters: commits attributed to emails outside the org/contributor set, mass reformatting, non-substantive paths, and single-author months. **The formula is our own**; the practice of filtering to count "real" developers is the Electric Capital convention.

**M09 `contributor_concentration`** — HHI over commit authors in 90d **plus** 12-month code-ownership HHI from git blame. Same recognised HHI standard. Cheap-to-game with a single prolific committer, which is precisely why we pair it with M08.

**M10 `wash_trading_estimate`** — Cong, Li, Tang & Yang, *Crypto Wash Trading*, Management Science 69(11):6427–6454 (2023), NBER WP 30783. Method: (a) compare first-significant-digit distribution against **Benford's law**; (b) **roundness ratio** — classify a trade as "round" if the last non-zero digit of its size is below 100 basis units, fit a pooled log(unrounded volume)/log(round volume) ratio on a *regulated reference cohort*, then estimate wash volume as (observed unrounded volume) − (benchmark ratio × observed round volume). Their finding: fabricated volume averaged **over 70% of reported volume** on unregulated exchanges (median 79.1%; >53.4% even on Tier-1), and **70% wash trading moves an exchange's CMC rank up by ~46 positions** — a direct demonstration that a ranking metric *is* the gaming target. Corroborating method: Chen, Lin & Wu, *Do cryptocurrency exchanges fake trading volumes?*, Physica A 586 (2022) — combine off-chain trade-size counts with on-chain transaction counts to detect reported-vs-settled mismatch. **Our extension:** the CEX roundness/Benford tests do not transfer to AMMs; for DEX/AMM we substitute a common-funder self-trade detector (M11).

**M11 `sybil_cluster_count`** — build the address ← funding-source bipartite graph; cluster addresses by common funding edges; report **clusters, not addresses**, as the independence-adjusted holder base. Theory backing: SybilGuard admits O(√n log n) sybils per attack edge and breaks down entirely once attack edges reach Ω(√n / log n) (~15,000 in a million-node network); SybilLimit improves this to O(log n); Gatekeeper (Tran, Bhargava, Lee, Reingold, Weintraub) to O(log k), which is optimal when k is constant. **The actionable conclusion:** the binding constraint is **the number of attack edges, not the number of sybil identities**. So the scored quantity should be "how many *distinct external funding edges* feed the top-holder set," not "how many addresses." Combine with Trust Wallet's rule: exclude airdropped recipients from holder counts.

**M12 `organic_volume_ratio`** — organic volume = Σ volume from addresses that (a) have held ≥30 days median, (b) share **no** funder with the protocol treasury or known market-maker addresses, (c) are not in the top decile of trades-per-address-age (bot signature). Ratio = organic / total. **Our own proposal.** Honest constraint: address-level volume is only available from a commercial provider (Nansen/Artemis/Chainalysis) or from SQL we author on Dune — i.e. this metric is expensive on the free tier, and we should say so rather than pretend otherwise.

**M13 `treasury_runway_months`** — treasury_value_usd ÷ net_monthly_burn_usd, where net_monthly_burn = (monthly opex + monthly buyback) − (monthly revenue retained by the protocol). **Treasury holdings are the binding data gap:** fees/revenue are free on DeFiLlama but treasury balances are Pro-gated or vendor. Report runway in *both* USD-at-current-price and at 50% price, because a token-denominated treasury's runway halves when the token halves.

**M14 `sybil_aided_holder_growth`** = Δholders_90d ÷ Δ(external funding edges)_90d. If holder count grows 10× while distinct funding edges grow 2×, the growth was manufactured. **Our own proposal.**

**M15 `incentive_efficiency`** = emissions_paid_usd ÷ net_tvl_inflow_usd in the window. Capital efficiency of the incentive programme: high emissions per dollar of *net* inflow is the incentivised-TVL signature. DeFiLlama's documented USD Inflows metric (per-asset balance difference × price, summed, which removes the price-effect artefact in raw TVL) and Pro `/api/inflows`.

**M16 `reserve_coverage`** = verified_onchain_assets ÷ independently_verified_claims, where claims are verified by Merkle-leaf self-verification plus the auditor's random-sample and dummy-account negative control (F1.6). Band by report recency.

---

## F4 Scoring architecture

### F4.1 Why a pure weighted sum fails, per the MCDA literature

- Compensatory methods (SAW, TOPSIS, AHP) permit full offset: "in compensatory techniques, poor performances of a strategy in some criteria can be compensated for by high performances in some other criteria; therefore, the aggregated performance of a strategy might not reveal its weakness areas." Non-compensatory ELECTRE III produced rankings with **lower sensitivity to criterion weights** than SAW and AHP, and the study concludes "the rankings obtained by the ELECTRE III method are more reliable" (Banihabib 2018).
- The veto problem is named directly: "the Decision Maker may prefer not to select alternatives which have a very low performance in whatever criterion. In contrast, such an alternative may have the best overall evaluation, since the additive model may compensate this low performance in one of the criteria as a result of high performance in other criteria… the results obtained from a numerical simulation show that it is **not so rare for a veto of the best alternative to occur in the additive model**" (de Almeida et al., *Additive-Veto Models For Choice And Ranking Multicriteria Decision Problems*, Applied Mathematical Modelling 30(6), 2013).
- **Our recommendation:** a **two-layer architecture**. Layer 1 is a non-compensatory gate/outranking pass on a small set of *fatal* criteria (funded contract is honeypotted or unverified-source; multisig/timelock claim cannot be confirmed on-chain; claims a licence not on the register; reserve report fabricated; sanctions exposure on a Tier A list). Layer 2 is a compensatory weighted total over the non-fatal criteria, **reported as a band, not a continuum**. Gate criteria never enter the weighted sum, so they cannot be averaged away (see the F1.2 arithmetic).

### F4.2 Gating precedent from Tier A

The 2023 DOJ/FTC Merger Guidelines use **conjunctive structural presumptions**, not averages: a merger is presumed to substantially lessen competition if post-merger HHI > 1,800 **AND** ΔHHI > 100; or merged-firm share > 30% **AND** ΔHHI > 100. Crucially the presumption is "rebutted or disproved" and "**the stronger the evidence needed to rebut or disprove it**" — i.e. confidence in the rebuttal is itself graduated. That is exactly the gate architecture we want, with a citable legal-analogue: **an anti-gaming gate whose override requires stronger evidence than the default.** The 2010 guidelines additionally show banded structure (<1,500 / 1,500–2,500 / >2,500) and the 2023 guidelines note they *returned* to the original thresholds because "the original HHI thresholds better reflect the law and the risks of competitive harm" — evidence that thresholds should be anchored to empirically-justified bands, not tuned to taste.

### F4.3 The specific mathematics of averaging a critical flaw into a passing score

Restating F1.2 as a design rule: **for a criterion of weight w to be able to force a failure at threshold τ, we require w > 1 − τ.** Therefore:

- With τ = 0.7, any criterion we call fatal must be implemented as a gate, because no weight at or below 0.30 can force a fail on its own.
- If instead we want a criterion to be *strongly* penalising but reliably *not* fatal, its weight must sit at or below 1 − τ — i.e. ≤ 0.30 — with a smaller weight being safer. Note the asymmetry: the closer w approaches 1 − τ from below, the more likely a total failure on that criterion alone drags the aggregate under τ anyway, so "strongly penalising but never fatal" is intrinsically fragile. That fragility is the argument for gates, not a tuning preference.
- **Corollary (the "one bad apple" demonstration):** with 8 equally weighted criteria at w = 0.125, a subject scoring 1.0 on seven criteria and 0.0 on the eighth scores 0.875 — comfortably passing. Any equal-weight composite over ≥8 binary-ish signals will pass a project with one fatal defect. This is a structural argument for gates, not a tuning preference.
- **Mitigation without gates:** use a convex aggregation (power mean / CES, ρ > 1) or a multiplicative form S = Π xᵢ^{wᵢ}, which is *non-compensatory by construction* because a zero zeroes the product. **Our proposal:** product aggregation for the fatal subset, sum aggregation for the remainder. This is the MCDA "non-compensatory" idea expressed in closed form and is cheap to implement.

### F4.4 Curve grading: what it buys and what it costs

CoinGecko's Basilisk curve is the industry precedent for replacing a fixed scale with population-relative grading. **Cost:** a curve guarantees the top decile is populated even in a uniformly bad cohort; a score is not comparable across time because the population changes; and it is unfit for a verdict. **Use it for:** ranking projects against a peer cohort. **Never use it for:** "does this project pass." Our design should produce both, clearly separated: a **band verdict** (gate-clean / gated-out, plus a band on the total) and a **cohort percentile**.

### F4.5 Evidence-confidence weighting

The concrete rule we can lift is CoinGecko's: *"Self-reported credentials that cannot be independently confirmed are not scored."* Our three-tier extension (our proposal):

| Tier | Definition | Multiplier |
|---|---|---|
| **Verified** | We or a disinterested third party re-ran the check against primary data: register lookup, deployed bytecode hash, on-chain state, re-queried public API, published verifier run against a published artefact | ×1.0 |
| **Asserted** | A real artefact exists and is inspectable, but no independent confirmation (vendor self-report, project-posted dashboard, self-published attestation) | ×0.4 (our chosen value; tuned, disclose it) |
| **Unverifiable** | Self-report, screenshot, blog post, marketing PDF, unverifiable "audit published" claim | ×0.0 |

Rationale for keeping "asserted" at a non-zero rather than zero value: unlike CoinGecko's binary, it preserves the *information* that an artefact exists (weakly legitimate) without letting it compete with verified evidence. We attempted to align this to the ISO/IEC 25012 data-quality characteristic model but **could not retrieve the standard text** (ISO's OBP returned an unrelated standard), so we are not citing it; treat this as a practitioner design.

### F4.6 What to publish and what to keep private

**Publish:** every component's raw value, every component's evidence tier, the band thresholds, the gate criteria and their trigger state, the versioned score history, and the recalculation cadence (CoinGecko uses weekly, explicitly to damp ranking volatility — this also fixes the "hedgehog" problem where a project simply resubmits until it gets a lucky pass). **Do not publish:** the exact weight vector. Weights are the attack surface; publishing them converts our score into an optimisation target (F5). The transparency/gaming trade-off is real, and the resolution is to publish *inputs and outputs* while keeping the *mapping* private — auditable but not directly optimisable.

---

## F5 Anti-gaming design

### F5.1 Threat taxonomy

Using Manheim & Garrabrant, *Categorizing Variants of Goodhart's Law* (arXiv:1803.04585), which distinguishes four failure modes: **Regressional** (selecting for an imperfect proxy also selects for noise — "tails come apart"), **Extremal** (selection pushes the subject into a region where the old relationship no longer holds; two mechanisms, model insufficiency and change of regime), **Causal** (the evaluator's own intervention changes the relationship — their windmill/wind-speed example), and **Adversarial** (an agent with goals different from the evaluator's causes the collapse). **Adversarial Goodhart is our threat model, since the project is an adversary with an incentive to maximise our score.**

### F5.2 Attack-by-attack design

**(a) "Audit published but unverifiable" (Adversarial).** Defences: require the audit to resolve to a named auditor + report URL + **the exact commit hash or deployed bytecode the audit covered**; cross-check against independent public feeds (the CMC partner endpoints: Hacken, FairyProof, Quantstamp, CertiK, Coinscope — if a claim of audit does not appear in any of those feeds, that is negative evidence); require a **findings-disposition table** with a public commit reference per finding; require recency, and treat a major recorded incident as an explicit zero (CoinGecko's incident component scores zero for a major incident). Our add: **a stated audit that produces no findings list is scored as unverifiable, not as clean** — absence of findings evidence is not evidence of absence.

**(b) Sybil-splitting of holders (Adversarial).** Defences from M11 + the Sybil-resistance theory: cluster on funding-source edges; score *clusters*; count distinct **attack edges** as the binding quantity; exclude airdropped recipients (Trust Wallet rule); require that holder count never alone crosses a band — it must be paired with at least one cost-to-forge signal.

**(c) Fake dev commits (Adversarial + Regressional).** Defences: contributor-level HHI (M09); require ≥2 distinct identified humans in the 90d window and ≥K over 12m; substantive-path weighting (`src/`, `contracts/` only); entropy of files touched per author (formatters touch hundreds of files with a one-line diff); measure 12m trend (M08), never 1 month; check issue/PR responsiveness ratio against the same repo. **Weakness we must state:** anyone can hire five real part-time engineers for six months. Dev activity is a *hygiene* signal, never a legitimacy proof.

**(d) Incentivised TVL (Causal Goodhart — the emissions *cause* the capital, so the proxy is caused, not merely correlated).** This is the textbook Causal Goodhart case: DeFiLlama's own documentation now states it is "improving the Total Value Locked (TVL) metric by removing unproductive or artificial liquidity" and enumerates the cases: "Liquidity pools with a few providers and no trading activities… Assets deposited into lending pools with a few lenders and no borrowers… Assets deposited into yield/staking pools from a few depositors only to earn points or rewards… Wrapper assets that lack verified or provable backing… Assets without real user deposits or that can no longer be withdrawn by users." **We should implement exactly this exclusion list as a filter, plus M15 (emissions per $ of net inflow), M06 (real-yield ratio) and M12 (organic volume ratio).**

**(e) Cosmetic decentralization (Adversarial).** Never count addresses; count **control**. Ask for facts, not adjectives: the multisig address, its threshold, its signer set, whether a timelock fronts it and with what minimum delay, and whether any proposal has actually traversed the timelock. CoinGecko's register-verification pattern applies unchanged: if a control claim cannot be confirmed on-chain, it scores zero.

**(f) Verification-of-verification.** Every scored claim must resolve to (i) a primary artefact, (ii) an independent producing party, (iii) a check *we* can re-run. The Merkle-proof + random-sample + dummy-account negative control from Hacken's methodology is the model — it verifies the *process* rather than the assertion. Nic Carter's deeper version: verify **liabilities**, because assets can be shown while liabilities are hidden.

### F5.3 Which signals are resistant, and why

- **Cost-to-forge** — gaming requires spending a non-recyclable resource and leaves a permanent trace: on-chain transaction fees, real value at risk behind a multisig with an enforced timelock, a real user loss from an unrecovered exploit, verifiable on-chain liabilities. These resist because the cost is sunk and the trace is immutable.
- **Third-party-anchored** — an independent party with no stake in our score must produce the artefact: a regulatory register entry, an accredited audit report, a signed open-source release. These resist because we do not grade them.
- **Reputation-lagged** — signals with a long time constant resist because a year of history cannot be manufactured in a week: M08 dev decay, 12-month fee retention, 12-month code-ownership HHI. **Design corollary (our proposal): make low-confidence signals decay fast and high-confidence signals decay slowly.** Fast-decaying weak signals cannot be farmed; slow-decaying strong signals are immune to a re-optimisation sprint.
- **Explicitly cheap-to-fake → must be gated, never sufficient alone:** holder count, social follower count, headline APY, raw TVL, GitHub stars, existence of an audit PDF, number of team members.

---

## F6 Questionnaire-to-score design

### F6.1 The validity problem, stated plainly

A self-report questionnaire asking a project team to describe its own legitimacy is the **highest-social-desirability context survey methodology has**. Krumpal, *Determinants of social desirability bias in sensitive surveys: a literature review*, Quality & Quantity 47(4):2025–2047 (2013), establishes that respondents "underreport socially undesirable activities and overreport socially desirable ones," and that the bias depends on perceived sensitivity, the privacy of the situation, the fraction of the population engaged, and design features. Documented mitigations, per that review: **randomised response technique**; **self-administration** (absence of an interviewer reduces bias — repeatedly demonstrated across sensitive domains); **confidentiality assurances / careful wording**; the **bogus pipeline** (raising the respondent's subjective probability of being caught lying); emphasising the importance of the study; and manipulating the survey situation.

**The design conclusion that follows, and it is the strongest recommendation in this file:** the questionnaire must be **claim generation, not evidence collection**. Every answer is auto-checked against a public source; an answer that cannot be checked is surfaced to the user as *unverifiable — weight 0* and contributes nothing. Make the questionnaire **self-administered** with no interviewer and no live video call; add an explicit, honestly-worded verification notice ("we verify a random sample of your answers against public sources") which is both the bogus-pipeline technique and simply truthful about what the tool does.

### F6.2 Radar / spider charts: the evidence is against them

The reference site in the brief uses radar charts. The measurement literature says do not.

- Albo, Lanir, Bak & Rafaeli, *Off the Radar: Comparative Evaluation of Radial Visualization Solutions for Composite Indicators* (2015): a controlled experiment comparing radar against flower-charts (OECD Better Life style) and circle-charts found **"the Radar chart was the least effective and least liked,"** while the alternatives performed "mixed and dependent on the task." They note radar charts are used for composite indicators "although in dispute" and that "no empirical evidence on Radar's effectiveness" had existed.
- Stephen Few (2005), *Keep Radar Graphs Below the Radar*: positions along a linear scale are easier to compare than positions along radial axes; radar "isn't clear where it begins and where it ends or whether it should be read clockwise or counterclockwise," so it cannot support ranking; and viewers "tend to prefer polygons with symmetrical shapes," which flatters mediocre-but-even profiles over strong-but-lopsided ones. *(Practitioner source — used as illustration, not as authority.)*
- Feldman, *Filled Radar Charts Should not be Used to Compare Social Indicators*, Social Indicators Research 111(3):709–712 (2012) — cited within Albo et al.
- Kantabutra & Tangmanee (2025): perceptual bias grows with the number of radial axes and data series, with a recommended range of 5–7 series; a related 2019 smartphone study (157 business students, 16-item comprehension quiz) found comprehension driven by interactions rather than main effects — i.e. radar reading is fragile and task-dependent.

**False precision.** Rendering a composite of weak ordinal signals as a decimal number claims measurement precision we do not have. Three remedies (our proposal): (1) report a **band** plus the component table with raw values and evidence tiers; (2) publish a **weight-sensitivity note** — Banihabib (2018) shows compensatory methods "exhibit a high dependency to the weights of some dominant criteria," so any band claim should state whether it survives plausible weight perturbation; (3) keep radar, if the reference site requires it for look-and-feel, as a **non-load-bearing decorative view** clearly subordinate to the band-and-table display.

### F6.3 Which questions discriminate and which merely feel good

**Discriminating** (each has an independent, mechanical, falsifiable verification target):

1. "Give the deploy address of your main contract." → check bytecode, verified source, proxy pattern, owner/admin on a block explorer.
2. "Give the multisig/Safe address." → verify threshold, signer count and distinctness, and whether a timelock fronts it.
3. "Give the repository URL **and the release tag that was audited**." → check the tag exists and that the audit references it. This one question collapses the "audit published but unverifiable" failure mode into a single verifiable artefact.
4. "Give your licence number, issuing authority and jurisdiction." → CoinGecko's register-verification pattern. A project that cannot name a registerable credential loses the *point*, not merely the question.
5. "Name the addresses you consider treasury, and the vesting contract address." → feeds M01/M02/M07, and disagreement with third-party labels is itself a signal.
6. "What were your net protocol fees in the last 90 days, and where is the on-chain proof?" → check against DeFiLlama `/summary/fees/{protocol}`.
7. "Was any token distributed by airdrop? To how many addresses, funded from how many distinct sources?" → feeds M11/M14 directly and adopts Trust Wallet's exclusion rule.
8. "Which of your claims have an independent third party who has verified them?" → forces the verified/asserted/unverifiable split at the source.
9. "If your SOC 2 / ISO 27001 report is scoped to a subset, quote the scope statement." → turns a badge into a verifiable claim or an explicit admission.

**Merely feels good** (high desirability pull, no falsifiable target — display them, but weight 0):

1. "How innovative is your technology?" — no falsifiable target.
2. "Rate your team's strength 1–10." — unfalsifiable.
3. "How decentralised is your project?" — the answer is a *set of on-chain facts*, not an opinion. Ask for the facts.
4. "Do you believe your project will still exist in five years?" — a textbook socially-desirable item.
5. "How committed is your team to the community?" — unfalsifiable.

**The governing rule:** every scored item must be phrased as a **verifiable fact request with an artefact**, never as a rating. Derived rules: (i) pair every positive claim with a **falsification route** ("name the register where a regulator lists you"); (ii) exclude any item whose truthful answer is socially desirable and whose false answer is unfalsifiable; (iii) label unverifiable items as weight-0 *in the UI* so the tool is honest about its own validity rather than silently absorbing the claim; (iv) disclose the sampling-based verification notice.

**Use-case claims become scoreable only when decomposed into machine-checkable properties.** CoinMarketCap's category criteria show the right instinct and the right trap: for AI, "AI must play a direct and instrumental role in the project's ecosystem/value chain," and their own illustration that merely "partnering with or doing a basic integration with tools such as ChatGPT and OpenAI may not necessarily suffice — that would be tantamount to saying that infrastructure providers like exchanges, AWS and ISPs that use OpenAI are AI companies." For DeFi they require protocols to be "decentralized, non-custodial, smart-contract enforced, on-chain traceable, permissionless." Those are properties a machine can partly test. The trap is the immediately adjacent category they also document: a blockchain merely being "well-suited to" a use case is not enough, and pivots after launch must clear a higher bar, with **launch positioning weighted heavily compared to post-launch pivots**. So: ask *which* verifiable property implements the claimed use case, and weight pre-launch commitments above post-hoc narratives.

---

## Sources

| Ref | Tier | Title | URL | Date accessed |
|---|---|---|---|---|
| F1 | C | CoinGecko — Trust Score Methodology | https://support.coingecko.com/hc/en-us/articles/36442561461657-Trust-Score-Methodology | 2026-10-06 |
| F2 | C | CoinGecko — Trust Score (Basilisk Update) | https://support.coingecko.com/hc/en-us/articles/57645752611865-Trust-Score-Basilisk-Update | 2026-10-06 |
| F3 | C | CoinGecko — Methodology (version history of Trust Score) | https://www.coingecko.com/en/methodology | 2026-10-06 |
| F4 | C | CoinGecko — Trust Score 3.0: Proof of Reserves (Assets & Liabilities) | https://support.coingecko.com/hc/en-us/articles/14570182625817-Trust-Score-3-0-Proof-of-Reserves-Assets-Liabilities | 2026-10-06 |
| F5 | C | CoinMarketCap — Listings Criteria (Section B/C framework, Unverified/Verified/Tracked) | https://support.coinmarketcap.com/hc/en-us/articles/360043659351-Listings-Criteria | 2026-10-06 |
| F6 | C | CoinMarketCap — Category-Specific Listings Criteria (audit badge partner APIs) | https://support.coinmarketcap.com/hc/en-us/articles/360045595471-Category-Specific-Listings-Criteria | 2026-10-06 |
| F7 | C | CoinMarketCap — Supply (Circulating, Total, Max); verified vs self-reported CS | https://support.coinmarketcap.com/hc/en-us/articles/360043396252-Supply-Circulating-Total-Max | 2026-10-06 |
| F8 | C | CoinMarketCap — Data Accountability & Transparency Alliance partner criteria | https://coinmarketcap.com/academy/article/data-accountability-and-transparency-alliance | 2026-10-06 |
| F9 | C | Trust Wallet — New Asset Listing Acceptance Guidelines (10k holders / 15k tx, airdrops excluded, audit, bots) | https://developer.trustwallet.com/developer/new-asset/requirements | 2026-10-06 |
| F10 | C | Trust Wallet — Repository details / token status taxonomy (active/spam/abandoned) | https://developer.trustwallet.com/developer/new-asset/repository_details | 2026-10-06 |
| F11 | B | Hacken — Proof of Reserves Audit Methodology v3 (liability report, Merkle sampling, dummy control) | https://www.scribd.com/document/807508281/Proof-of-Reserves-Methodology-v3 | 2026-10-06 |
| F12 | B | Nic Carter et al. — Proof of Reserves: The Practitioner's Guide (PoPR, liabilities, Merkle, ZK) | https://niccarter.info/wp-content/uploads/Proof-of-Reserves-.pdf | 2026-10-06 |
| F13 | B | BIP-127 — proof-of-reserves transaction standard | https://github.com/kcw-grunt/bips/blob/a2645cdc421d13842f4ea180338264942330abff/bip-0127.mediawiki | 2026-10-06 |
| F14 | C | OKX — Proof of Reserves (zk-STARK constraints, Merkle, open-source validator, report archive) | https://www.okx.com/en-us/proof-of-reserves | 2026-10-06 |
| F15 | C | OKX — Proof of Reserves downloads (report IDs, dates, zk-STARK v2) | https://www.okx.com/en-eu/proof-of-reserves/download | 2026-10-06 |
| F16 | A | FATF — Updated Guidance for a Risk-Based Approach to VA and VASPs (2021) | https://www.fatf-gafi.org/content/dam/fatf-gafi/guidance/Updated-Guidance-VA-VASP.pdf | 2026-10-06 |
| F17 | A | FATF — Best Practices in Travel Rule Supervision (2025) | https://www.fatf-gafi.org/content/dam/fatf-gafi/recommendations/Best-Practices-Travel-Rule-Supervision.pdf | 2026-10-06 |
| F18 | A | FATF — Virtual Assets Red Flag Indicators (Sept 2020) | https://www.fatf-gafi.org/content/dam/fatf-gafi/reports/Virtual-Assets-Red-Flag-Indicators.pdf | 2026-10-06 |
| F19 | A | US DOJ/FTC — Herfindahl-Hirschman Index (definition, bands) | https://www.justice.gov/atr/herfindahl-hirschman-index | 2026-10-06 |
| F20 | A | US DOJ/FTC — 2023 Merger Guidelines (conjunctive structural presumptions) | https://www.justice.gov/atr/merger-guidelines/applying-merger-guidelines/guideline-1 | 2026-10-06 |
| F21 | A | US DOJ/FTC — Horizontal Merger Guidelines (2010), HHI computation + bands | https://www.justice.gov/sites/default/files/atr/legacy/2010/08/19/hmg-2010.pdf | 2026-10-06 |
| F22 | A | ISO/IEC 27001:2022 — Information security management systems, Requirements | https://www.iso.org/standard/27001 | 2026-10-06 |
| F23 | A | AICPA & CIMA — 2017 Trust Services Criteria (with Revised Points of Focus 2022) | https://www.aicpa-cima.com/resources/download/2017-trust-services-criteria-with-revised-points-of-focus-2022 | 2026-10-06 |
| F24 | A | AICPA — 2017 TSC full text (Security/Availability/PI/Confidentiality/Privacy scope levels) | https://assets.ctfassets.net/rb9cdnjh59cm/72xv4p67HVXKp6CjWmjkPk/1cdbfa19f6307e2720396b66a6194dc9/trust-services-criteria-updated-copyright.pdf | 2026-10-06 |
| F25 | A | OFAC — Sanctions List Service (fuzzy name search, SDN + non-SDN downloads) | https://ofac.treasury.gov/sanctions-list-service | 2026-10-06 |
| F26 | B | Etherscan — API Overview (V2, 60+ chains, tokenholderlist, getsourcecode, tags) | https://docs.etherscan.io/apis/api-overview | 2026-10-06 |
| F27 | B | Etherscan — Rate Limits (Free 3 cps / 100k per day, selected chains) | https://docs.etherscan.io/resources/rate-limits | 2026-10-06 |
| F28 | B | GitHub — REST API rate limits (60/hr unauth, 5,000/hr authed, 15,000/hr GHEC apps) | https://docs.github.com/en/rest/using-the-rest-api/rate-limits-for-the-rest-api | 2026-10-06 |
| F29 | B | Dune — API overview (base URL, SDKs, metadata endpoints free) | https://docs.dune.com/api-reference/overview/introduction | 2026-10-06 |
| F30 | B | Dune — Billing (Free 20 credits/MB exported, 32GB result cap) | https://docs.dune.com/api-reference/overview/billing | 2026-10-06 |
| F31 | B | Dune — Get Execution Result (X-Dune-Api-Key, sampling/filtering/pagination) | https://docs.dune.com/api-reference/executions/endpoint/get-execution-result | 2026-10-06 |
| F32 | B | GoPlus — Security API documentation index | https://docs.gopluslabs.io/ | 2026-10-06 |
| F33 | B | Nansen — API documentation index (Address Labels, Related Wallets, Historical Top Holders, Points API) | https://docs.nansen.ai/api | 2026-10-06 |
| F34 | C | DeFiLlama — API docs (free vs Pro base URLs, 31/38 endpoints, $300/mo Pro) | https://defillama.com/docs/api | 2026-10-06 |
| F35 | C | DeFiLlama — Data definitions & metrics glossary (USD Inflows, App Fees vs App Revenue) | https://docs.llama.fi/analysts/data-definitions | 2026-10-06 |
| F36 | C | DeFiLlama — Methodology (TVL exclusions: borrowed coins, own tokens, Staking, Pool2, native staking) | https://docs.llama.fi/ | 2026-10-06 |
| F37 | C | DeFiLlama — What to include as TVL (including artificial/unproductive liquidity exclusions) | https://docs.llama.fi/list-your-project/what-to-include-as-tvl | 2026-10-06 |
| F38 | C | DeFiLlama — Adapters discussion #434 (composability double-counting problem statement) | https://github.com/DefiLlama/DefiLlama-Adapters/discussions/434 | 2026-10-06 |
| F39 | C | CoinGecko — API errors and rate limits (Demo 100 cpm; keyless IP pool) | https://docs.coingecko.com/docs/errors-and-rate-limits | 2026-10-06 |
| F40 | C | CoinGecko — Keyless public API (~10–30 cpm; not for production) | https://docs.coingecko.com/docs/keyless-public-api | 2026-10-06 |
| F41 | C | CoinGecko — API pricing (Demo free 100 cpm / 10k per month; paid from $35/mo) | https://www.coingecko.com/en/api/pricing | 2026-10-06 |
| F42 | B | CoinGecko — API introduction (Demo vs Pro reference, onchain via GeckoTerminal) | https://docs.coingecko.com/reference/introduction | 2026-10-06 |
| F43 | C | Artemis — Pricing ($0 Analyst tier; API/Snowflake on enterprise) | https://about.artemis.ai/pricing | 2026-10-06 |
| F44 | B | Artemis — API reference (per-metric `source` / `internal_data_source` provenance; API key enterprise-gated) | https://www.artemis.ai/docs/api-reference/core-artemis-assets/list-available-metrics-for-assets-by-symbol | 2026-10-06 |
| F45 | B | Artemis — API key documentation ("not an Enterprise customer and will need to upgrade") | https://www.artemis.ai/docs/artemis-api/api-key | 2026-10-06 |
| F46 | D | Manheim & Garrabrant — Categorizing Variants of Goodhart's Law (arXiv:1803.04585) | https://arxiv.org/abs/1803.04585v3 | 2026-10-06 |
| F47 | D | de Almeida et al. — Additive-Veto Models for Choice and Ranking MCDA Problems (Applied Mathematical Modelling 30(6)) | https://ideas.repec.org/a/wsi/apjorx/v30y2013i06ns0217595913500267.html | 2026-10-06 |
| F48 | D | Banihabib — Comparison of Compensatory and non-Compensatory MCDM models (ELECTRE III weight-sensitivity) | https://iranarze.ir/wp-content/uploads/2018/06/E7692-IranArze.pdf | 2026-10-06 |
| F49 | D | Krumpal — Determinants of social desirability bias in sensitive surveys (Quality & Quantity 47(4)) | https://ideas.repec.org/a/spr/qualqt/v47y2013i4p2025-2047.html | 2026-10-06 |
| F50 | D | Cong, Li, Tang & Yang — Crypto Wash Trading (NBER WP 30783; Management Science 69(11)) | https://www.nber.org/papers/w30783 | 2026-10-06 |
| F51 | D | Chen, Lin & Wu — Do cryptocurrency exchanges fake trading volumes? (Physica A 586, 2022) | https://www.sciencedirect.com/science/article/abs/pii/S0378437121006786 | 2026-10-06 |
| F52 | D | Albo, Lanir, Bak & Rafaeli — Off the Radar: Comparative Evaluation of Radial Visualization Solutions | https://exa.ai/library/publication/n830v4ng3fw | 2026-10-06 |
| F53 | D | Kantabutra & Tangmanee — Perceptual Bias in Mobile Radar Chart Visualization (2025) | https://exa.ai/library/publication/m24bx6s0mqj | 2026-10-06 |
| F54 | D | Evaluating Design Features that Enhance Radar Chart Comprehension on Smartphones (2019) | https://www.aasmr.org/jsms/Vol14/No.7/Vol.14.No.7.31.pdf | 2026-10-06 |
| F55 | D | Keep Radar Graphs Below the Radar — Stephen Few (2005) [practitioner, illustration only] | https://perceptualedge.com/articles/dmreview/radar_graphs.pdf | 2026-10-06 |
| F56 | D | Nguyen, Tran, Bethke, Westmoreland, Nelson, Barve — Combating Sybil attacks in cooperative systems (Gatekeeper, SumUp, Credo) | https://cs.nyu.edu/media/publications/tran_nguyen.pdf | 2026-10-06 |
| F57 | D | SybilLimit: A Near-Optimal Social Network Defense against Sybil Attacks (NUS) | https://www.comp.nus.edu.sg/~yuhf/yuh-sybillimit.pdf | 2026-10-06 |
| F58 | D | Baghaee et al. — Sybil in the Haystack (Algorithmic 16(1):34, MDPI) | https://www.mdpi.com/1999-4893/16/1/34 | 2026-10-06 |
| F59 | A | FATF/OECD — API evangelist OpenAPI spec for OFAC SLS search & lists endpoints | https://raw.githubusercontent.com/api-evangelist/department-of-the-treasury/refs/heads/main/openapi/department-of-the-treasury-search-api-openapi.yml | 2026-10-06 |

---

## Data source table

| source | what it gives | API | auth | reliability caveat | tier |
|---|---|---|---|---|---|
| DeFiLlama Free | TVL (`/protocols`, `/protocol/{p}`, `/v2/chains`), prices, stablecoins, yields (`/pools`, `/chart/{pool}`), DEX volume (`/overview/dexs`, `/summary/dexs/{p}`), perp OI, fees & revenue (`/overview/fees`, `/summary/fees/{p}`) | `https://api.llama.fi` | **None** (Pro is `$300/mo` at `pro-api.llama.fi/{KEY}`) | Free rate limit stated only as "Standard" — **exact number not documented on the page read**. Adapters are community-maintained per project. Prices "almost all… using CoinGecko's API" → circular with CoinGecko. TVL excludes borrowed coins, protocol's own tokens, Staking, Pool2, non-circulating/vesting tokens, native chain staking. Token unlocks (`/api/emissions`, `/api/emission/{p}`) and inflows are **Pro-only** | C |
| CoinGecko | Market data, prices, market cap, categories; exchange volumes; GeckoTerminal onchain (200+ chains, 1,800+ DEXes) | `https://api.coingecko.com/api/v3`, `https://api.geckoterminal.com/api/v2` | Demo key free; keyless available | Demo = 100 calls/min + 10,000 calls/month; keyless ≈10–30 cpm shared per IP, ≈10 cpm GeckoTerminal, "not suitable for production… scheduled polling". All requests incl. 4xx/5xx count toward the limit. Same price source feeds DeFiLlama | C |
| Etherscan | Balances, txns, token transfers, internal txs, gas, logs, `tokenholderlist`, `getsourcecode`, `fundedby`, `getaddresstag`, `dailytx`; 60+ EVM chains via `chainid` | `https://api.etherscan.io/v2/api` (and per-chain hosts) | API key; **Free tier: 3 calls/sec, 100,000 calls/day, selected chains only**, no PRO endpoints | `tokenholderlist` returns top-N only, not the full holder set. Address tags and metadata are **vendor heuristics**, not ground truth — treat as labels to be cross-checked, never as proof of ownership | B |
| Dune | Arbitrary onchain SQL over curated tables; execute + results | `https://api.dune.com/api/v1/...`, header `X-Dune-Api-Key` | API key; Free plan exists | Free plan still charges **20 credits per MB exported** (Analyst 10, Plus 2); 32 GB result cap with truncation. **Epistemic caveat dominates:** a Dune number is SQL an anonymous author wrote — curated, versioned by that author, and not canonical. Cite the dashboard/query, never "Dune says" | B |
| GitHub REST | Commits, contributors, blame, releases, tags, repo metadata | `https://api.github.com` | None (60 req/hr, IP-based) or PAT (5,000 req/hr) | 15,000 req/hr for GitHub Apps on GitHub Enterprise Cloud orgs. Search endpoints more restrictive. Commit counts are trivially forgeable; contributors must be filtered (see M08/M09) | B |
| OFAC Sanctions List Service | SDN and Consolidated non-SDN lists as JSON/XML/PDF; structured search with `matchScore` | `https://sanctionslistservice.ofac.treas.gov/api` (lists) and `/api` search | None; free | **Entity/name screening only — OFAC does not publish on-chain address screening.** Name search uses fuzzy logic → false positives on crypto project names. Cannot be used to screen a wallet address with Tier A data | A |
| GoPlus | Token Security, Malicious Address, NFT Security, Approval Security, dApp Security Info, Phishing Site Detection, Transaction Simulation (EVM/Solana) | documented at `docs.gopluslabs.io`; endpoint paths **not retrieved** | Malicious Address API stated "Free"; others unknown | **Rate limit and exact endpoint paths unknown** (docs fetch failed). Automated risk score with unpublished weighting → opaque. Triage flag only, never a weighted score input | B |
| Nansen | Address balances/history, transactions, counterparties, **related wallets**, **address labels**, **historical top holders**, DEX trades, who bought/sold, smart-money flows/holdings, Portfolio, Points API | `docs.nansen.ai/api` endpoint index | API key | **Pricing unknown / not published on the docs read** → treat as paid. `Address Related Wallets` and `Address Labels` are exactly the Sybil-clustering and ex-treasury primitives, so this is the single highest-value paid dependency. Nansen also surfaces project-submitted reserve data → self-reported, not an audit | B |
| Artemis | Protocol fundamentals: DAU, txns, TVL, fees, revenue, stablecoin flows, per-asset metric metadata incl. `source` / `internal_data_source` | REST `/data/api/*`, `/supported-metrics/`, key via `ARTEMIS_API_KEY` env or `api_key` query | API key, **enterprise-gated** | `$0` tier is the Analyst product, not the REST API. Provenance fields are a genuine plus (e.g. `24H_VOLUME → Source: Coingecko`) and we should reuse that pattern in our own data model | B/C |
| CoinMarketCap audit-badge feeds | Third-party assertion that a named auditor published findings for an address | `https://cmc-api.hacken.io/`, `https://www.fairyproof.com/fairyproof_data/cmc.json`, `https://al.quantstamp.com/api/cmc`, `https://cmc.certik-skynet.com/v1`, `https://www.coinscope.co/api/audit/cmc` | None (public) | Presence-only: no severity, no report date, no remediation status, no commit hash. Absence of a badge is weak negative evidence, not proof of no audit | C |
| OKX Proof of Reserves | zk-STARK (FRI) proof of account ledger over encrypted Merkle trace; three constraints (sum, non-negative net equity, inclusion); published wallet addresses with signed messages; open-source validator; per-report archive with IDs/dates | `https://www.okx.com/en-us/proof-of-reserves` | None to view/download | **Entirely self-published.** Verifies OKX's account ledger, not solvency against off-ledger obligations. Point-in-time; snapshot boundary disclosed. Use as an exemplar of *verification design*, not as an independent signal about OKX | C |
| Trust Wallet token repository | Token status `active` / `spam` / `abandoned`; listing thresholds (≥10,000 holders, ≥15,000 transactions, airdrops excluded) | public GitHub repo, `repository_details` | None to read | Paid, non-refundable submission fee and maintainers reserve the right to reject → a paid route weakens the signal; treat as one input, not a verdict | C |
| Chainalysis / TRM Labs / Solidus Labs | Onchain entity attribution, address risk, sanctions exposure | commercial APIs | Commercial contract; **no free tier documented** | Unknown rate limits and pricing. Required for real address-level sanctions attribution (which OFAC does not cover). Budget as a paid line item or state the gap | C |
| Raw JSON-RPC (public nodes) | Block/log/trace data, the substrate for any self-computed metric | chain-specific JSON-RPC | Usually rate-limited public endpoints | Cheapest ground truth, highest engineering cost. For M12/M11-class address-level analysis this is the only route that is fully auditable by us | B |
| ISO/IEC 25012-style data-quality characteristics | Characteristic-based data-quality scoring | n/a | n/a | **Not sourced** — ISO OBP returned an unrelated standard on fetch. Do not cite; our verified/asserted/unverifiable scheme stands on its own | — |
| CryptoRank trust score | opaque vendor score | none public | n/a | **No official methodology located.** Not reusable as a design input | — |
| Santiment scores | social/sentiment/development metrics | commercial API | unknown | **No published weighting methodology located.** Discovery hint only | — |

---

## Implementable metrics

| metric_id | name | precise formula | needed inputs | caveats |
|---|---|---|---|---|
| M01 | `hhi_holders` | HHI = Σᵢ sᵢ², sᵢ = balanceᵢ / Σ balance, over non-zero holders. Report 0–1 and ×10,000 | Full holder set with balances (Etherscan `tokenholderlist` = top-N only; Nansen Historical Top Holders; Dune SQL) | **Standard:** DOJ/FTC HHI. **Extension to holder distribution is ours.** CEX omnibus addresses collapse many holders into one |
| M02 | `hhi_ex_treasury` | Σᵢ sᵢ² after excluding labelled treasury / team / vesting / burn (0x0, 0xdead) / staking / LP / bridge / CEX omnibus addresses | M01 inputs + address labels (Etherscan `getaddresstag`, Nansen Address Labels) + project-declared treasury addresses | Medium confidence: labels are vendor heuristics; an unlabelled insider wallet survives. Always publish the raw (M01) and adjusted (M02) pair |
| M03 | `top10_ex_treasury` + Gini | Top10 = Σ(top-10 sᵢ) post-exclusion. Gini G = ΣᵢΣⱼ\|xᵢ−xⱼ\| / (2n²μ) | as M02 | Gini is less sensitive to a single whale than top-1; report both |
| M04 | `holders_tvs_divergence` | e = (Δ% holders, 90d) ÷ (Δ% TVL, 90d); bands: e≥2 with M12 flat = farmed participants; e<0.5 with TVL up = concentration | Holder series + DeFiLlama `/protocol/{p}` TVL history | **Our proposal.** Window choice is arbitrary — state it; do not report a single number |
| M05 | `fee_to_holder_revenue` | (buybacks + distributions **observed as executed transfers** to holders) ÷ net protocol fees | DeFiLlama `/summary/fees/{p}` (fees vs revenue) + on-chain trace of holder-directed transfers | DeFiLlama already separates Fees from Revenue — reuse that boundary. The holder-accrual leg must be traced, never read off a dashboard field |
| M06 | `emission_adjusted_yield` | APY = annualised(rewards + fees + native emissions) ÷ pool TVL; RY = annualised(rewards in assets **not** minted by the protocol and **not** the chain's native token) ÷ pool TVL; report RY/APY | DeFiLlama `/pools`, `/chart/{pool}` | **Our proposal.** Use DeFiLlama **core** TVL; using Staking/Pool2/borrowed TVL double-counts. Ratio → 0 is the finding, not a data error |
| M07 | `unlock_overhang_dtwu` | Σᵢ uᵢ·wᵢ with uᵢ = unlock ÷ circulating supply, wᵢ = (90−dᵢ)/90 for dᵢ≤90 else 0. Publish unweighted Σuᵢ over 90d alongside | DeFiLlama Pro `/api/emissions`, `/api/emission/{p}` + deployed vesting contract | **Our proposal.** Treat schedule as *claimed* until reconciled against a deployed contract and a timelock that can change it. Schedules are routinely revised |
| M08 | `dev_decay_rate` | k = exp(slope of ln(rolling-90d commits) vs t over 12m) − 1; k<0 = decay. Filter non-org emails, reformat-only commits, non-substantive paths | GitHub API commits (5,000 req/hr authed) | **Formula is ours.** One-month windows are forgeable; always 12m. Bots are trivial to add, hence M09 |
| M09 | `contributor_concentration` | HHI over 90d commit authors + 12m code-ownership HHI from blame | GitHub commits + blame | **HHI standard**; the 12m ownership view is ours. A single prolific committer defeats the 90d view alone |
| M10 | `wash_trading_estimate` | Roundness ratio: trade is "round" if last non-zero digit < 100 bp; fit pooled log(unrounded)/log(round) on a **regulated reference cohort**; wash_vol = observed_unrounded − ratio × observed_round. Cross-check first-significant-digit distribution vs Benford | CEX trade-size data (proprietary or vendor) | **Published method:** Cong, Li, Tang & Yang (Management Science 2023; NBER 30783), which found >70% of reported volume on unregulated exchanges. Assumes legitimate traders on the unregulated venue resemble those on the regulated benchmark (paper states this explicitly). **Does not transfer to AMMs** — use M11 self-trade detection instead |
| M11 | `sybil_cluster_count` | Cluster addresses by common funding-source edges (connected components at ≥K shared funders). Score **clusters**, not addresses; report distinct external funding edges into the top-holder set as the binding quantity | Full funding graph — needs paid provider or own Dune/RPC indexing | Sybil theory: bound scales with **attack edges** (SybilGuard O(√n log n) → SybilLimit O(log n) → Gatekeeper O(log k)). Free-tier infeasible; flag the cost |
| M12 | `organic_volume_ratio` | organic = Σ vol from addresses with median holding ≥30d, sharing no funder with treasury/MM addresses, and not in the top decile of trades-per-address-age; ratio = organic ÷ total | Address-level volume (paid provider or Dune SQL) | **Our proposal.** Expensive on the free tier — state the cost rather than implying parity with M04 |
| M13 | `treasury_runway_months` | runway = treasury_value_usd ÷ net_monthly_burn; burn = (monthly opex + monthly buyback) − (monthly revenue retained). Report at 100% and 50% token price | Treasury balances (Pro/vendor) + DeFiLlama revenue | **Data gap:** treasury holdings are not freely available. A token-denominated treasury's runway halves when the token halves — always publish the stressed figure |
| M14 | `sybil_aided_holder_growth` | Δholders_90d ÷ Δ(external funding edges)_90d | as M11 | **Our proposal.** Ratio >> 1 means participant growth was manufactured |
| M15 | `incentive_efficiency` | emissions_paid_usd ÷ net_tvl_inflow_usd (window) | DeFiLlama USD Inflows (per-asset balance delta × price) / Pro `/api/inflows` + emissions | Use **net** inflows, not TVL delta, or a token price move masquerades as inflow (DeFiLlama documents this artefact). High emissions per net inflow dollar = incentivised TVL signature |
| M16 | `reserve_coverage` | verified_onchain_assets ÷ independently_verified_claims, where claims are confirmed by Merkle-leaf self-verification **plus** auditor random sample **plus** dummy-account negative control | Published PoR files + wallet-address ownership messages | **Method:** Hacken PoR Methodology v3 / Nic Carter PoPR. Point-in-time only; the entity scopes the wallet set. Band by report recency (>180 days = stale) |
| M17 | `fee_retention_12m` | fees_t ÷ fees_{t−12m} (rolling, net of token-price effect by using USD net inflow terms where possible) | DeFiLlama `/summary/fees/{p}` history | **Our proposal.** A slow, reputation-lagged metric — deliberately hard to fake quickly |
| M18 | `audit_finding_remediation` | Σ over reports of (findings with a public commit reference closing them) ÷ (total findings), weighted by auditor tier and report recency | Audit reports + project repos (GitHub API) | Verification of "we fixed it" is much weaker than verification of "it was found". Absence of a findings table is scored as *unverifiable*, not clean |

---

## Scoring signals extracted

| signal_id | what it measures | how to verify mechanically | evidence tier | failure mode | confidence |
|---|---|---|---|---|---|
| SIG-01 | Contract source is genuinely deployed and verified | Fetch bytecode hash + verified source via Etherscan `getsourcecode`; recompute compile determinism where metadata allows | B | Source published but not the deployed bytecode; verified source differs from runtime code | High |
| SIG-02 | Upgrade/admin control is real and constrained | Read owner/admin/proxy slots on-chain; resolve multisig threshold, signer count and timelock minimum delay from contract state | B | Cosmetic decentralization: threshold 1, signers related, timelock bypassable | High |
| SIG-03 | Timelock has actually been exercised | Historical proposal/execute records through the timelock, with timestamps ≥ minimum delay | B | Timelock exists but never used, so its behaviour is untested | Medium |
| SIG-04 | Claimed licence/registration is real | Look up the credential in the issuing authority's official register (CoinGecko three-step pattern); exclude if no public register exists | A | Self-reported credential; register lookup never performed | High |
| SIG-05 | Audit exists, is scoped, and findings are dispositioned | Resolve auditor + report URL + **the exact commit hash or bytecode audited**; cross-check against CMC partner badge feeds; count findings with public remediation commits | B/C | "Audit published but unverifiable"; scope mismatch; stale audit on rewritten code | Medium-high |
| SIG-06 | Holder concentration, raw | M01 over full holder set | B | CEX omnibus addresses dominate; dust and burn addresses inflate apparent distribution | Medium |
| SIG-07 | Holder concentration, excluding treasury/team/burn | M02 with label cross-check | B | Insider wallet unlabelled; labels differ across vendors | Medium |
| SIG-08 | Participant count is Sybil-resistant | M11 cluster count + Trust Wallet's airdrop-exclusion rule + M14 | B | Sybil-splitting; airdrop recipients counted as users | Medium |
| SIG-09 | TVL is economically real, not emitted | Apply DeFiLlama's own artificial-liquidity exclusion list; M15; M06 | C | Incentivised TVL, few providers and no trading, few lenders and no borrowers, point-farming deposits, unbacked wrappers | Medium-high |
| SIG-10 | Reported volume is not manufactured | M10 roundness/Benford tests on CEX; common-funder self-trade detection on AMM | D | Wash trading — measured at >70% of reported volume on unregulated exchanges by a peer-reviewed study | Medium |
| SIG-11 | Organic (non-incentivized) volume share | M12 | B/C | Costs paid-tier data; holding-period and funder heuristics can be evaded | Low-medium |
| SIG-12 | Protocol earns real fees, sustained | M17 + DeFiLlama `/summary/fees/{p}`; verify against on-chain fee accrual | C | Fee farming, fee routing to a contract the team controls, seasonal spikes | Medium-high |
| SIG-13 | Token holders actually receive economic value | M05, with holder-direction transfers traced on-chain | C | Buyback announcements with no executed transfers | Medium |
| SIG-14 | Yield is emission-adjusted | M06 real-yield / headline-APY ratio | C | Headline APY denominated in a token the protocol prints | Medium |
| SIG-15 | Unlock overhang is small and near-term-weighted | M07 plus on-chain vesting-contract reconciliation | C | Published schedule silently changed; cliff hidden behind a long linear tail | Medium |
| SIG-16 | Developer activity is sustained and distributed | M08 12m decay + M09 contributor and ownership HHI | B | Fake commits, single-committer repos, one-month bursts | Medium |
| SIG-17 | Custody/liability claims are verifiable | M16 with Merkle self-verification + random sample + dummy negative control; compare to OKX's published constraint set | B/C | Underreported liabilities; self-published PoR; stale snapshot | Medium |
| SIG-18 | Sanctions/geographic exposure | OFAC SLS name screening against declared entities and named key people (fuzzy — expect false positives) | A | **On-chain address screening is not provided by OFAC**; a name match does not bind a wallet | Low (as sole signal) / High (as a gate on named entities) |
| SIG-19 | Counterparty payment-transparency hygiene | Evidence of originator/beneficiary data transmitted with transfers (FATF R.16 standard; FATF does not accept post-transmission data) | A | "We are Travel Rule compliant" as an unevidenced policy claim | Medium |
| SIG-20 | Operational security maturity | SOC 2 Type 2 / ISO 27001 certificate: type, **period**, verbatim scope statement, auditor, entity/domain bridge letter | A | Narrow scope covering a support tool rather than the custody path; Type 1 substituted for Type 2 | Medium-high |
| SIG-21 | Team and investor claims are corroborated | Cross-check named investors/advisors against their own public disclosures; check LinkedIn/consortium records | C | Unverifiable prestige signalling; consortium membership that never funded | Low |
| SIG-22 | Governance participation is real | On-chain proposal count, turnout, quorum, and whether voting power is delegated out of a few wallets | B | Vote-buying; empty proposals; governance theatre | Medium |
| SIG-23 | Contract-verification coverage of the system | Sourcify-style metadata verification plus Etherscan verification across all deployed contracts, including proxies and periphery | B | Only the audited contract is verified; periphery and upgrade paths unverified | Medium-high |
| SIG-24 | Public roadmap claims match shipped releases | GitHub releases/tags vs dated roadmap commits; time-to-release distribution | B | Roadmap items quietly dropped; dates slip with no consequence | Medium |
| SIG-25 | Cohort position (not a verdict) | Curve-grade component total against a defined peer cohort, as CoinGecko does | C | Curve grading guarantees a top decile in a uniformly bad cohort; non-comparable over time | Medium (ranking only) |
| SIG-26 | Data-source provenance is explicit | Per-metric `source` / `internal_data_source` field on every input, as Artemis publishes | B/C | Silent mixing of self-reported and verified figures | High (as a process control) |
| SIG-27 | Score stability across recomputation | Recompute on a fixed published cadence from frozen inputs; report version history | C | Re-submission lottery; weight drift between runs | High (as a process control) |
| SIG-28 | Questionnaire claims survive verification | Every scored answer resolves to an artefact + independent producer + re-runnable check; unverifiable answers shown as weight-0 | A/D (method) | Social-desirability bias; leading self-report; unverifiable "audited"/"decentralised"/"compliant" claims | Medium — **this signal is the tool's main integrity control** |