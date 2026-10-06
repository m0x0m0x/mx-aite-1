# What Actually Makes a Crypto Project Legitimate

### An evidence-based framework for distinguishing real use cases from speculation-driven token mechanics

**Report ID:** R1-LEGIT
**Date:** 2026-10-06
**Status:** Research complete; pre-development
**Prepared for:** Product team, Crypto Legitimacy & Use-Case Viability Tool
**Brief:** `p1.txt` (items 1–7)
**Evidence base:** 331 unique sources (Tier A 80 · Tier B 93 · Tier C 70 · Tier D 84; 3 Tier E quarantined, 0 cited), 203 scoring signals, 11-mechanic casino taxonomy
**Verification:** All load-bearing quantitative claims independently re-derived; two arithmetic/logic errors found in source drafts and corrected (§2.4)

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Scope, Method and Evidence Standard](#2-scope-method-and-evidence-standard)
3. [Defining the Problem: Legitimate Use Case vs Casino Mechanics](#3-defining-the-problem-legitimate-use-case-vs-casino-mechanics)
4. [Dimension 1 — Contract and Technical Security Integrity](#4-dimension-1--contract-and-technical-security-integrity)
5. [Dimension 2 — Governance Reality and Treasury Integrity](#5-dimension-2--governance-reality-and-treasury-integrity)
6. [Dimension 3 — Token Economics and Value Capture](#6-dimension-3--token-economics-and-value-capture)
7. [Dimension 4 — Regulatory, Legal and Counterparty Legitimacy](#7-dimension-4--regulatory-legal-and-counterparty-legitimacy)
8. [Dimension 5 — Empirical Evidence: What Actually Predicts Survival](#8-dimension-5--empirical-evidence-what-actually-predicts-survival)
9. [Scoring Architecture: Gates, Caps and the Averaging Failure](#9-scoring-architecture-gates-caps-and-the-averaging-failure)
10. [Proposed Tool Specification](#10-proposed-tool-specification)
11. [Consolidated Signal Register](#11-consolidated-signal-register)
12. [Limitations, Contestations and What Would Change the Verdict](#12-limitations-contestations-and-what-would-change-the-verdict)
13. [References](#13-references)

**Appendix A** — [Reference-Site Teardown Summary](#appendix-a--reference-site-teardown-summary)
**Appendix B** — [Research Method and Token Optimization](#appendix-b--research-method-and-token-optimization)
**Appendix C** — [Evidence Ledger](#appendix-c--evidence-ledger)

---

## 1. Executive Summary

### 1.1 The headline finding

**A crypto project is legitimate when its token's value is derivable from verified, independent use of a real product. It is illegitimate when the token's value depends on recruiting the next holder.**

This sounds tautological. It is operationally decisive, because it converts an unanswerable question ("is this project real?") into a set of mechanically checkable ones. Our research produced **145 discrete signals** that resolve it, plus a hard gate register that must fire before any weighting is applied.

### 1.2 The six findings that should change how this tool is built

**Finding 1 — Self-report is the attack surface, not the evidence.**
The reference questionnaire suite (`msawox.com/en/tools`, 17 tools) is a well-engineered instrument: weighted pillars, annotated per-answer point values, radar charts, and — importantly — **hard gates that cap the score** (caps at 39/49/59, so a perfect 100 with one critical breach reports as 49, not 100). We inherit all of that UX. But it scores *self-assessment*, and for a fraud-detection tool self-assessment is precisely what an adversary controls. Every substantive signal in the new tool must resolve to an artifact: a chain, an address, verified source, a public register, a document. **Self-report is admissible only as a pointer to where to check, never as the check.** → [§10](#10-proposed-tool-specification)

**Finding 2 — Weighting mathematically cannot implement a veto. Only gates can.**
With components normalised to [0,1] and `S = Σwᵢxᵢ`, if the fatal criterion `F` scores zero then the maximum attainable score is `S_max = 1 − w_F`. Failure is forced only when `S_max < τ`, i.e. **only when `w_F > 1 − τ`**. At a `τ = 0.7` pass threshold that means any weight at or below 0.30 can *never* force a failure. CoinGecko's published Trust Score weights illustrate the trap: proof-of-reserves at `w = 0.05` and incident history at `w = 0.10` are, arithmetically, incapable of failing an exchange that scores 1.0 on everything else — which is CoinGecko's documented intent (PoR is a *floor*, not a gate), but is the wrong behaviour for a legitimacy verdict. → [§9](#9-scoring-architecture-gates-caps-and-the-averaging-failure)

**Finding 3 — "There is a multisig" is a signature, not a safety property.**
A multisig exists at all five of these projects; at four of them something is very wrong. On-chain state fixes *N* and *threshold* and observes nothing else: not whether one entity controls several signers, not whether signers are hot wallets, not whether a timelock actually owns the contracts that matter, not whether `guard`/`modules` grant bypass rights. Concretely — we verified that the Curve Emergency DAO Safe (`0x467947EE34aF926cF1DCac093870f613C96B1E0c`) holds **0 ETH, 0 USDC, 0 WETH** yet carries irreversible pause and debt-ceiling authority over Curve. Any scoring model that treats "funded and moving funds" as the test for a control Safe **inverts the risk ranking.** → [§5.1](#51-a-multisig-is-a-signature-not-a-property)

**Finding 4 — Standard proxy detection produces false negatives on Safe v1.3.0.**
`GnosisSafeProxy` does **not** use the ERC-1967 implementation slot. We independently confirmed on mainnet that the Curve Emergency DAO Safe returns `0x00…00` for the ERC-1967 slot while its singleton lives at **storage slot 0** and `singleton()` (`0xa619486e`) returns the canonical v1.3.0 singleton `0xd9Db270c1B5E3Bd161e8c8503c55cEABeE709552`. A scanner that concludes "no proxy, therefore no admin" from an empty EIP-1967 slot draws the **wrong** conclusion about a *funds-controlling* object. → [§4.2](#42-verifying-a-multisig-not-believing-one)

**Finding 5 — Most realised losses are not code defects.**
CertiK recorded **$2,362,748,975.83** across **760** incidents in 2024 (mean $3.11M, median $150,925). Of that, phishing was **$1,050,129,498** across 296 incidents and private-key compromise **$855,385,570** across just 65 incidents — together **80.6% of all value from 47.5% of incidents**. Badger DAO's core contracts were never touched. A security dimension built around "has an audit" will systematically over-rank audited protocols whose losses came from keys, frontends and people. → [§4.6](#46-loss-attribution-contract-code-is-not-the-dominant-vector)

**Finding 6 — "Governed by holders" is close to empirically false in this sector.**
Using complete on-chain data for Compound, Uniswap and ENS, Kani et al. (ETH Zürich) found voting-rights **Gini coefficients around 0.99** — higher than national wealth distributions in the same paper (US 0.850, Europe 0.814) — and Nakamoto coefficients of **8 (Compound), 11 (Uniswap), 18 (ENS)**. Across 21 DAOs, **17 could have a majority voting power controlled by fewer than 10 addresses.** Delegation *amplifies* concentration rather than diluting it. Governance claims should be scored as *participation reality*, not as the existence of a Governor contract. → [§5.2](#52-participation-reality-is-the-load-bearing-governance-metric)

### 1.3 The finding that reverses the obvious design

**Only one of fifteen candidate metrics survives as a hard gate, and there should be no composite score at all.**

The only survival model in the literature scores **0.98 in-sample and collapses to 0.59–0.65 on unseen data**, omitting every off-chain feature. **No published trust or legitimacy score has ever been validated against outcomes.** CoinGecko's Trust Score is rank-relative and 50% liquidity-weighted. And the most-cited input metrics fail outright: wash trading averaged **>70% of reported volume on unregulated exchanges**, and fabricated volume **improves published rankings**; audit findings are **misaligned with ~49.6% of losses**, which come from keys, phishing and social engineering.

The base rate explains why the one surviving gate matters: of **1,244 protocols**, ~400 clear $1M in fees but only **~20 (≈1.6%)** pass $10M to holders.

> **We therefore recommend gates and evidence panels, not a headline number.** A single 0–100 score would be the most quotable and least honest thing we could build. → [§8.2](#82-why-there-is-no-composite-score), [§9.8](#98-composite-scores-are-rejected-on-evidence)

### 1.4 What we recommend

Build the tool as a **four-state evidence resolver feeding a pre-registered gate register**, presented in the reference site's UX. Not a questionnaire with a radar chart attached — a set of **145 checks** whose inputs are artifacts a project cannot fake cheaply.

| Decision | Recommendation |
|---|---|
| Headline number | **None.** Verdict is a plain phrase + blockers + evidence panels. Percentages only per-dimension, paired with coverage. |
| Volume & audit count | **Inverted** from positive reward to penalty ([§8.1](#81-the-headline-only-one-metric-survives-as-a-gate)) |
| Questionnaire role | **Claim generation only.** Answers have weight 0; they only tell the engine *where to look*. |
| Evidence states | `VERIFIED` · `CLAIMED` · `CONTRADICTED` · `UNKNOWN-or-N-A`, scored asymmetrically. `UNKNOWN ≠ 0`. |
| Veto mechanism | Pre-registered **gates** with severity ordering and score caps. Not weights. |
| Scoring | Weighted pillars for the radar; gates applied *before* aggregation. Report a **band**, not a precise integer. |
| Sources | Tier A–D only. Every claim traceable; `CONTESTED` and `UNSOURCED` states mandatory. |
| New projects | Age-banded so a 6-month-old project is not scored like a 6-year-old Ponzi. |

---

## 2. Scope, Method and Evidence Standard

### 2.1 What this report is

A synthesis of six parallel research slices into a single authoritative framework for scoring crypto-project legitimacy, produced to the brief in `p1.txt` items 3–4. It is a **research and design-input deliverable**, not user stories or implementation code (the brief defers those: *"eventually … before being able to build the tool and write the user stories acceptance criteria"*).

The underlying evidence is preserved on disk and independently auditable — see [§11](#11-consolidated-signal-register) and [Appendix C](#appendix-c--evidence-ledger).

### 2.2 Research slices

Six disjoint slices ran in parallel under a partitioning scheme designed to avoid duplicated spend (each agent owned a non-overlapping slice and returned a digest to a shared filesystem rather than to context):

| Slice | Topic | Sources | Signals |
|---|---|---|---|
| **A** | Contract and technical security integrity | 54 | 30 |
| **B** | Governance reality and treasury integrity | 43 | 30 |
| **C** | Token economics and casino mechanics | 50 | 32 |
| **D** | Regulatory, legal and counterparty legitimacy | 58 | 25 |
| **E1** | Empirical case studies (survivors, failures, traps) | 64 | 36 |
| **E2** | Metric reliability and counter-evidence | 18 | 22 |
| **F** | Data indicators and scoring architecture | 59 | 28 |

Total **331 unique sources** after URL de-duplication (from 345 raw citations). Ledger tier classification: **A 80** (+1 `A*` partial, index metadata only), **B 93**, **C 70**, **D 84**, **E 3** (0 cited).

### 2.3 Source tiering

The token-optimization plan mandated an A–E tier list, with Tier E (blogs, SEO content farms, affiliate "best coin" pages, price predictions, listing-site reviews) **banned as citation grounds**. Tier E material could only be used to *locate* a better source, which was then fetched and cited.

| Tier | Type | Examples in evidence base | Count |
|---|---|---|---|
| **A** | Primary regulatory / government | SEC, CFTC, DOJ, FINCEN, FCA, ASIC, MAS, ESMA, OFAC, MiCA text, EU sanctions, enforcement releases, court records | 80 (+1 partial) |
| **B** | Primary technical / registry | Audit reports, verified contract source, Safe/Proxy on-chain state, protocol specs, EIPs, official licence registers, technical whitepapers | 93 |
| **C** | Primary measurement | CertiK Hack3d, Chainalysis, DeFiLlama, Binance Research, ETH Zürich studies, Electric Capital, Token Terminal, Nansen | 70 |
| **D** | Established media / research / law-firm alerts | Named-journalist investigations, academic preprints, recognised think-tank reports, peer-reviewed wash-trading and token-death studies | 84 |
| **E** | Banned | Discovered 3 during slice C; **all 3 quarantined and not cited** | 3 (0 cited) |

**Audit note:** three Tier E sources (an 8Blocks blog post, a QuantAbundancia article, a FindAS blog) were found and recorded for provenance. Slice C correctly declined to cite them and marked the dependent claims `UNSOURCED`. That discipline is preserved in the ledger with an explicit `NOT CITED — Tier E quarantined` annotation.

### 2.4 Independent verification performed

Every load-bearing claim intended for quotation was re-derived rather than transcribed. Four checks are worth recording because they changed the report:

| Claim | Check | Outcome |
|---|---|---|
| Uniswap v2 has a residual admin key | Fetched `blog.uniswap.org/whitepaper.pdf`, read primary text | **Confirmed verbatim:** *"there is a private key that has the ability to update a variable on the factory contract to turn on an on-chain 5-basis-point fee on trades."* |
| Uniswap accrues fees to UNI holders | Live DeFiLlama protocol page | **Confirmed:** Holders Revenue 30d ≈ **$15.06M**; accrual dated *"V2: From 28 Dec 2025, 17% (0% before)."* Slice C's $14.5M was a slightly earlier 30-day window. |
| Gate-weight inequality | Numerical solve | **Source draft had the inequality inverted** (`w_F < 1 − τ`). Corrected to `w_F > 1 − τ`; at `w_F = 0.30`, `S_max = 0.70` does *not* clear `τ = 0.70`, whereas `w_F = 0.31` does. The numeric conclusion survived; the stated rule did not. |
| Phishing + key-compromise share of 2024 losses | Recomputed from CertiK components | **Source draft said 86%.** Correct value **80.6%** ($1,905,515,068 / $2,362,748,975.83) — a 5.4pp overstatement. Corrected in source and here. |

Additionally, slice A's on-chain findings were **independently re-verified** against Ethereum mainnet and the Safe Transaction Service. All of the following were reproduced exactly:

- Curve Emergency DAO Safe `0x467947EE34aF926cF1DCac093870f613C96B1E0c`
- ERC-1967 implementation slot returns `0x0000…0000`
- Storage slot 0 returns `0xd9Db270c1B5E3Bd161e8c8503c55cEABeE709552` (canonical Safe v1.3.0 singleton)
- `singleton()` (`0xa619486e`) returns that same address; slot-0 and `singleton()` agree
- `threshold` = **5**; `owners` = **9** (addresses matched one-for-one)
- `guard` = `0x0`; `modules` = `[]`; `nonce` = **13**; `version` = **1.3.0**

*Minor correction to the source draft:* the callable is `singleton()`, not `masterCopy()` — `masterCopy` is the API's *field* name in the Safe Transaction Service, while `0xa619486e` is the `singleton()` selector. Both resolve to the same address, so the finding is unaffected.

**Process incident.** Slice D overwrote the shared evidence ledger mid-run, destroying slices B, C and F's ledger entries. All evidence was recovered by parsing the per-slice `## Sources` tables, which survived intact. The consolidated ledger was rebuilt by script and is now de-duplicated by URL. This is why [Appendix C](#appendix-c--evidence-ledger) is script-generated rather than hand-appended.

---

## 3. Defining the Problem: Legitimate Use Case vs Casino Mechanics

### 3.1 Definition

Slice C supplied the working definition this report adopts:

> **Casino mechanics** = token design in which **player recruitment, not service provision, is the revenue engine.**

Three necessary conditions, each independently sufficient to raise suspicion:

1. **No contractual link** from product success to token-holder cash flow.
2. **The metric-growing mechanism *is* the monetization** — the airdrop, the listing, the points season. Growth is not a side effect of a working product; the growth event *is* the product.
3. **The attracted population is transient by construction** — capital rents in for a defined reward window and exits at the unlock.

The converse defines legitimacy: **there exists a contractually specified, independently verifiable mechanism by which sustained product usage increases token-holder cash flow, and the user population is retained for reasons other than the next emission.**

### 3.2 Why the "casino" label is analytically useful

It gives us a falsifiable structure rather than a vibe. Each mechanic has a **metric it inflates** and an **observable tell** — and, critically for a scoring tool, a **false-positive rate**, because legitimate projects look superficially similar. §6.2 tabulates eleven.

### 3.3 The asymmetry problem

A tool that cannot say *"we don't know"* will confidently score garbage. Two structural biases must be corrected by design:

- **False positives** — penalising a six-month-old protocol for having no multi-year track record. Punishing absence of history as though it were evidence of fraud destroys the tool's credibility.
- **Confident ignorance** — reporting a precise integer for a dimension where no authoritative threshold exists. For several dimensions we found that **no Tier A–D source publishes validated cutoffs**. In those cases the tool must publish the raw measurement and refuse to threshold it. See [§12](#12-limitations-contestations-and-what-would-change-the-verdict).

---

## 4. Dimension 1 — Contract and Technical Security Integrity

*Source: slice A, 54 sources (Tier B 36 · D 13 · A 2 · C 3).*

### 4.1 What a credible project should publish

Security due diligence rests on artifacts, in descending order of strength:

| Artifact | What it actually proves | What it does not |
|---|---|---|
| **Audit scoped to a commit hash** | The reviewed code corresponds to deployed bytecode | That the code is correct; audits are sampling, not proof |
| **Audit firm identity and reputation** | A named accountable third party | That findings were fixed |
| **Audit date vs deployment date** | The audit covers the live implementation | — |
| **Verified contract source** | Bytecode matches a human-authored source | That the source is safe, or that it is the *real* logic (proxy risk) |
| **Formal verification** | Mathematical property under stated assumptions | Coverage of the unmodelled |
| **Bug bounty** | Economic incentive to disclose | That the surface is covered |
| **Published incident disclosure** | Organisational honesty under stress | Technical quality |

**The gating insight:** an audit is a *dated, scoped, third-party artifact*. Anything less — "audited by a top firm", a logo, a PDF with no commit hash — is marketing.

### 4.2 Verifying a multisig, not believing one

This is the single most requested check in the brief and the most commonly faked. Verification is mechanical:

1. **Detect the proxy correctly.** Do **not** assume EIP-1967. `GnosisSafeProxy` stores the singleton at **storage slot 0**; the ERC-1967 slot reads zero. *(Independently verified — §2.4.)* A Safe whose EIP-1967 slot is empty is still a proxy holding a singleton.
2. **Resolve the singleton and version.** `singleton()` = `0xa619486e`; slot 0; confirm against Safe's published singleton deployments per chain.
3. **Read `threshold` and `getOwners()`.** Compute the **keys-to-lose-control** figure: `(owners − threshold) + 1`. Also compute **keys-to-act-unilaterally**: `threshold`. Both matter, and they are different questions.
4. **Read `guard` and `getModulesPaginated()`.** A non-zero guard or any enabled module is a **bypass path**. Curve's Safe has `guard = 0x0` and `modules = []` — a positive finding that costs nothing to check.
5. **Reverse the direction of authority.** Ask what the Safe *controls*, not what it holds. The Curve Emergency DAO Safe holds **zero ETH, zero USDC, zero WETH** and still holds pause and debt-ceiling authority. Balance-based scoring inverts the risk ranking.
6. **Check the timelock owns the contracts that matter** — impl, factory, pause authority, treasury. A timelock that exists but owns nothing material is decorative. Verified benchmark: Compound's Timelock delay is exactly **172,800s** (48h) with GovernorBravo as admin.
7. **Enumerate owner-gated functions** on every factory/proxy: fee knobs, pause, upgrade, mint.

> **Uniswap v2 is the template falsifier.** Its own whitepaper states: *"While the contract is not generally upgradeable, there is a private key that has the ability to update a variable on the factory contract to turn on an on-chain 5-basis-point fee on trades."* Non-upgradeable ≠ no admin key. And note the shape of the key: **fee extraction**, so exercising it is directly profitable.

**Scale reference.** Curve's own baseline is the industry reference standard: a published **5-of-9** signer set with *named members*, plus tooling that validates deployed Safes' owners and threshold against the baseline. An unnamed, unpinned, never-rotated signer set behind an "n-of-m Safe" claim is a materially different security object from a published one — and only the latter is auditable.

### 4.3 Hidden admin control

Three proxy patterns carry distinct risk:

- **Transparent (EIP-1967 + `ProxyAdmin`)** — upgrade logic and admin live in the proxy. OpenZeppelin's own docs state the admin *"can only be used for upgrading the proxy, so it's best if it's a dedicated account that is not used for anything else."* Critically, OZ sets the admin as **immutable** while the ERC-1967 admin *slot* *"can still be overwritten by the implementation logic"* — so **the on-chain slot can diverge from the real admin.** That is an explicit statement that the admin slot is not a reliable authority source.
- **UUPS** — upgrade logic lives in the *implementation*, so an implementation change can redirect control.
- **Beacon** — a shared, upgradeable pointer; one beacon upgrade affects every proxy pointing at it.

**Detection caveat worth flagging for the build:** generic ABI probing produces *unknown*, not *absent*. Slice A found Aave's `ACL_ADMIN` is neither a Safe nor an OpenZeppelin `TimelockController` — probing reverts. A scanner must therefore distinguish three outcomes — **found / not-found / unprobeable** — and never collapse the third into the second. Curve's Arbitrum Safe was also found pointing at a singleton absent from Safe's public registry.

### 4.4 Oracle and economic attack surface

Check whether oracles are sourced from a single thin AMM pool (manipulable within one transaction by flash liquidity), whether the token is simultaneously collateral and oracle input, whether governance votes can be bought with flash-borrowed tokens, whether the bridge is *verifiable* (light-client/proof-based) or *trusted* (multisig/MPC), and the depth-to-exploit ratio.

Ronin is the canonical case: the largest digital-currency theft at the time (~**$615M+**, attributed by US officials to the Lazarus Group) was a **signer-key compromise, not a contract bug** — and it ran a public Immunefi bounty throughout.

### 4.5 Exploit taxonomy with named prevention artifacts

Selected, each paired with the lesson that generalises:

| Class | Named case | Year | Loss | Generalisable lesson |
|---|---|---|---|---|
| Economic/market logic | Euler Finance | 2023 | ~$197M | Audited, formally reviewed, 12 third-party auditors — and the bug was in the *intended design*. **Audit coverage ≠ design correctness.** |
| Flash-loan governance attack | Beanstalk | 2021 | ~$80M | Flash-borrowed voting power ⇒ governance is only as strong as its borrow constraints |
| Price-oracle manipulation | Curve / Bonacrypto-style pools | 2020 | ~$3M; drained | Single-pool oracles are manipulable in one transaction |
| Bridge validation failure | Wormhole | 2022 | ~$326M | Real quorum existed (19 guardians); the *attestation verification* failed |
| Re-initialisation / storage-collision | Nomad | 2022 | ~$190M | Zero-root initialisation made `trustedRoot` writable ⇒ total loss |
| Signature-list validation | Ronin | 2022 | ~$615M+ | Fifth signature accepted via a stale allowlist; **organisational, not code** |
| Key-management failure | Harmony Horizon | 2022 | ~$100M | Keys "doubly encrypted"; attacker still decrypted them. Harmony's own post-incident description is the canonical proof that **sophisticated key management ≠ robust threshold** |
| Re-entrancy | Nomad / Curve-adjacent classes | — | — | Still live; check-effects-interactions and guard accounting |
| Frontend / Web2 | Badger DAO | 2021 | ~$120M* | Core contracts **not impacted**. Badger's own conclusion: *"even as Badger's core smart contracts were not impacted — phishing attacks, Web2 vulnerabilities, and user behaviors can interact in ways that pose major security threats."* \*figure not retrieved from a primary source in this pass |

### 4.6 Loss attribution: contract code is not the dominant vector

This is the finding most likely to reshape the security dimension's weighting.

| Year | Total lost | Incidents | Mean | Median |
|---|---|---|---|---|
| 2024 | **$2,362,748,975.83** | 760 | $3,108,880 | **$150,925** |
| 2023 | $1.84B | 751 | ~$2.45M | $101,132 |

2024 breakdown by vector:

| Vector | Value | Incidents | Share of incidents |
|---|---|---|---|
| Phishing | **$1,050,129,498** | 296 | 39.1% |
| Private-key compromise | **$855,385,570** | **65** | 8.6% |
| **Phishing + key compromise** | **$1,905,515,068** | **361** | 47.5% |
| *Share of total value* | **80.6%** | | |

In 2023, private-key compromise was 6.3% of incidents but **nearly half of total losses**. Chainalysis' 2025 report records **$40.9B** received by identified illicit addresses in 2024 — a **lower-bound** estimate.

Three design consequences:

1. **The mean is useless.** Euler alone was ~70% of 2023's total. Use **median-relative** or **worst-case-at-given-TVL** measures.
2. **Key compromise and phishing must be a separate scored axis from code quality.** Folding them in as "security" buries the dominant signal.
3. **"Has an audit" must not be the dominant security term.** It over-ranks audited protocols whose losses came from keys, frontends and people.

---

## 5. Dimension 2 — Governance Reality and Treasury Integrity

*Source: slice B, 43 sources (Tier B 19 · D 13 · A 5 · C 5).*

### 5.1 A multisig is a signature, not a property

Slice B's framing is the sharpest in this report:

> A multisig exists at all five of these projects; at four of them something is very wrong.

Five independent conditions must all hold, and **on-chain state alone only verifies two of them**:

| # | Condition | Verifiable on-chain? |
|---|---|---|
| 1 | M/N threshold adequate | **Yes** — `getThreshold()`, `getOwners()` |
| 2 | Signers independent of the founder | **No** — requires identity and disclosure |
| 3 | Real timelock delay | **Yes** — `getMinDelay()`, admin relationship |
| 4 | Timelock actually **owns** the contracts that matter | **Yes** — recursive `owner()`/`admin()` walk |
| 5 | No bypass rights via `guard`/`modules`; emergency powers scoped | **Yes** — but requires reading *absent* on-chain |

The two that matter most — 2 and 4 — are the ones most often claimed and least often checked. Reference points on emergency-power scope: OP Guardian is a *pause-only* role, whereas the Arbitrum Security Council is a *chain-owner*. They are not the same kind of object and should not score the same.

### 5.2 Participation reality is the load-bearing governance metric

Complete on-chain data for Compound, Uniswap and ENS (Kani et al., ETH Zürich):

| Measure | Compound | Uniswap | ENS | Benchmark in same paper |
|---|---|---|---|---|
| Voting-rights Gini | ~0.99 | ~0.99 | — | US national wealth 0.850 · Europe 0.814 |
| Nakamoto coefficient | **8** | **11** | **18** | — |

Across **21** DAOs spanning lending, DEXs, infrastructure and common-goods funding: **17 of 21 could have a majority voting power controlled by fewer than 10 addresses**, and *"for half of the analyzed DAOs, the Nakamoto coefficient is even no larger than 3."*

**Delegation amplifies concentration.** It is commonly assumed to broaden participation. Measured on voting-HHI vs holding-HHI, delegation **increased** concentration in 13 of 18 examined systems. And where the votes go matters more than who holds:

- For ENS, Gitcoin and Hop, ~half of all votes sat with **community delegates**.
- For **Compound, Fei and Uniswap, community-delegate vote share was ~10% or less.**
- ETH Zürich: *"Most voting power is held by delegates mainly representing a single token holder… there is little evidence of a substantial community-participation in the decision-making."*

**Participation is thin.** a16z analysis of 18 Ethereum DAOs (Jan 2021 – Dec 2023; ~250,000 voters, 1,700 proposals) found only **~17% of voting power was delegated to others**, and delegates participated on **33% of proposal votes** on average. On Uniswap specifically, **88% of votes cast carried voting power below 10 tokens and 47% below 1 token**, while **2.5M tokens** were needed to submit a proposal and **40M** to pass one — the proposal right is priced roughly four orders of magnitude above the median vote.

**Ratification is not deliberation.** DeFiLlama shows NNS at 19,505 proposals and 94.6% executed. A 94.6% execution rate is a *rubber stamp* signal, not a governance-strength signal.

**Quorum is the gameable dial.** GovernorBravo defeats a proposal when `forVotes <= againstVotes || forVotes < quorumVotes`. A *fixed absolute* quorum means falling participation forces either smaller proposals or a lowered quorum — itself a governance decision. A quorum measured as a fraction of *participating* votes has the opposite failure: one large voter clears it alone.

**Signal to implement:** `GOV-DELEGATION-AMPLIFICATION` = voting-HHI ÷ holding-HHI (PCA-corrected), and `GOV-NAKAMOTO-DELEGATES` = minimum delegates holding >50% of voting power. Both are computable from chain alone.

### 5.3 Treasury integrity

Measured sector shape: **~86% of treasury holdings in the native token**, median **3.6%** in stablecoins, median **7%** in productive/deployed assets, with treasuries sitting below prior highs for **57.4%** of observed time.

The consequence is structural: **a high treasury-to-market-cap ratio in the project's own token is a circular balance sheet.** It looks like solvency and is closer to self-financing.

Runway disclosure is **rare and therefore differentiating**. Two genuinely verifiable public examples:

- **Uniswap Foundation** — quarterly/annual summaries with explicit runway and earmark: at 31 Dec 2024, *"$29.8 million in USD and stables on hand and UNI 0.59 million"*; *"$21.82 million… allocated towards grants"*; *"The remaining $7.97 million was to be used to fund operations expenses through the end of 2025"*; FY2024 opex $5.79M against revenue $1.11M — and it labels the statement **"unaudited"**.
- **Ethereum Foundation** — annual report with named teams, grant disclosure, and a stated Conflicts of Interest Policy.

The honest caveat applies to both: **neither is audited; both are self-reported.** The Uniswap report is the more auditable artifact because it names a specific headcount, revenue figure and runway date rather than a narrative.

### 5.4 Transparency and disclosure: the falsifiable-artifact principle

There is measured evidence that disclosure correlates with outcome — **1,231 coins studied, 315 with whitepapers, showing significantly higher long-term excess returns.** But the same analysis found **verbosity and sentiment negatively related** to returns.

> **Design rule:** reward the **existence of a falsifiable artifact**, penalise prose.

This resolves a recurring dilemma: a terse, verifiable treasury table beats a 40-page vision document. Score the artifact.

Disclosure checks worth scoring: legal entity and jurisdiction resolvable in a corporate registry (Uniswap Foundation appears on ProPublica Nonprofit Explorer as org 883087770); roadmap items with promised vs actual dates; a primary post-mortem per claimed incident naming an *organisational* root cause; a published signer set with drift monitoring.

*Roadmap retrospectives* are explicitly marked `UNSOURCED` — no authoritative study establishes a correlation. Score the **practice**, not an asserted correlation.

### 5.5 Organizational key compromise

Five named incidents, each a different failure mode, each mapped to an operational (not code) prevention artifact:

| Incident | Failure mode | Prevention artifact |
|---|---|---|
| **Ronin** (Mar 2022) | 5-of-9 multisig; fifth signature obtained via a stale Axie DAO allowlist | Published signer sets; allowlist change control |
| **Harmony** (Jun 2022) | 2-of-4 keys *generated on privileged servers*; encrypted at rest, still decrypted | Generation/isolation practice, not encryption |
| **Radiant** (Oct 2022) | Hardware wallets + Tenderly + multi-review **all passed**; ~$50M | No technical control in that set prevented it |
| **Wormhole** (Feb 2022) | 19 guardians, a genuine quorum — forged attestation accepted | Signature-verification correctness, not quorum size |
| **Nomad** (Aug 2022) | Zero-root initialisation made `trustedRoot` writable | Invariant correctness |

Two generalisable conclusions. First, **control stacking does not compose**: Radiant passed hardware-wallet, simulation and multi-review controls and still lost. Second, **quorum size is not quorum integrity**: Wormhole's 19 guardians were honest and correctly configured.

### 5.6 Turning governance claims into falsifiable statements

Slice B's falsifier set is directly implementable. Seven claims, seven checks:

| Claim | The check that falsifies it |
|---|---|
| "No admin keys" | Owner-gated setter enumeration on factories/proxies (fee knobs, pause, upgrade, mint) — the Uniswap v2 case is the template |
| "Fully decentralized" | Recursive `owner()`/`admin()` walk to terminal controller |
| "Community-governed" | Nakamoto coefficient; community-delegate vote share |
| "DAO-owned treasury" | Treasury ex-native-token ÷ market cap; does the Safe control anything material? |
| "Audited treasury management" | Is the runway number dated, with an audit status label? |
| "Vested with no insider dumping" | Compare `released` vs `total` in the vesting contract — **on-chain, not in a PDF** |
| "No privileged roles" | `guard` and `getModulesPaginated()` both empty |

Score vesting by **recipient**, not merely by schedule: Uniswap's four vesting contracts vest *to the protocol treasury* (i.e. to the DAO), the opposite of a founder drip and materially better for legitimacy.

**Count of refuted decentralization claims** (`CLAIM-FALSIFIER-COUNT`) is itself a high-value signal.

---

## 6. Dimension 3 — Token Economics and Value Capture

*Source: slice C, 50 sources (Tier C 15 · D 15 · A 10 · B 6).*

### 6.1 Protocol revenue is not token-holder revenue

This is the single most useful measurement distinction in the economics dimension, and DeFiLlama's definitions make it precise:

> **Fees** = total paid by users, *including* what flows to liquidity providers.
> **Revenue** = `Fees − Supply-Side Revenue`.
> **Holders Revenue** = the part of protocol revenue returned to tokenholders via staking rewards, fee burns, or direct payouts — the direct analogue of dividends and buybacks.

Explicitly excluded from fees: **block rewards and token emissions are *incentives, not fees***; token taxes and referral payouts are not fees.

The reasoning is then mechanical: **a token absent from the Holders Revenue leaderboard has, by definition, no verifiable fee-accrual route to holders.** This is a Tier C measurement, so it is checkable without trusting the project's marketing.

Capture is real but concentrated in a small enumerable set (DeFiLlama, as at 2026-10-06):

| Protocol | Holders Revenue | Mechanism |
|---|---|---|
| Hyperliquid | ~$54.3M / 30d | *"99% of fees go to Assistance Fund for buying HYPE"* |
| Pump.fun | ~$24.9M / 30d | PUMP buybacks sourced from on-chain burns |
| **Uniswap** | **~$15.06M / 30d** | *"V2: From 28 Dec 2025, 17% (0% before) fees… shared to buy back and burn UNI"* |
| PancakeSwap | — | 0.0575% of AMM fees + 40% of StableSwap admin fees to buyback/burn |
| Jupiter | — | Buyback from 50% of platform revenue since 2025-02-17 |

**The Uniswap entry is the reference pattern.** DeFiLlama records the *date* the accrual switched on — **0% before 28 Dec 2025**. So Uniswap carried a multi-billion-dollar valuation with **no** fee-accrual route to UNI holders, and turned it on later. That is retrospective proof that the prior valuation was not fundamental. The generalisable check: **was there any period where the token had a large valuation and the accrual mechanism was off?**

Accrual should be **classified into five tiers**, with tier 5 (no accrual) scoring near zero *regardless of product quality*. A real product with no accrual route is still an uninvestable token; that is the honest conclusion.

### 6.2 Casino-mechanics taxonomy (M1–M11)

| ID | Mechanism | Metric it inflates | Observable tell | False-positive risk |
|---|---|---|---|---|
| **M1** | Reflexive / ponzi loop — new buyers fund earlier holders | Holder count, market cap, "community size" | Realized-PnL distribution concentrated in a few addresses | Low |
| **M2** | Emission-funded APY presented as yield | APY, TVL, "revenue" | Reward-share >50% of APY; emissions >2× holder revenue | **Med** — zero-fee campaigns look identical |
| **M3** | Mercenary capital via points / liquidity mining / high-FAR subsidies | TVL, users, deposits | TVL and USD inflows decouple; >15% TVL drop at TGE | **Med** |
| **M4** | Listing / TGE float scarcity as the primary monetisation | Market cap, FDV, valuation | MC/FDV <20%; volume spike on listing with flat user counts | Low |
| **M5** | Paid wash volume / market-maker-as-a-service | Exchange and DEX volume | Bot-generated round numbers; *"quadrillions of transactions and billions of dollars"* | Low |
| **M6** | Thin-liquidity price manipulation as an extractable mechanic | Price, TVL in token terms | Token is both collateral and oracle input | Low |
| **M7** | Referral / affiliate / MLM recruitment economics | Signups, "users", social | Referral payouts structurally excluded from Fees and capped vs Supply-Side Revenue | Low |
| **M8** | Points-to-airdrop scoring capital, not usage | "Engagement", leaderboards | Up to 66% of airdrop recipients are mercenary addresses | **Med** |
| **M9** | PnL-token / vault schemes where emissions are the only yield | Vault TVL, "yield", AUM | All APY components paid in the issuer's own token | Low |
| **M10** | Treasury/insider price support mistaken for value accrual | Price, "revenue" | Corporate/insider vehicles accumulating the token | **Med** |
| **M11** | Narrative-label substitution for product (AI/DeFi/RWA/GameFi with no fee line) | Narrative premium, launch valuation | No `Revenue = Fees − SSR` series exists | **Med** — early-stage projects legitimately lack revenue |

**The M2/M3 false-positive warning is load-bearing.** Slice C found that **zero-fee exchange campaigns and legitimate incentives produce identical APY/TVL signatures for two to three quarters.** Therefore rules for these mechanics must be **structural (must-have evidence present)** rather than **probabilistic**. A probabilistic detector would fire on legitimate incentive programmes for years before it caught anything.

### 6.3 Supply, unlocks and float

The float data is stark. Binance Research found 2024 launches averaged **MC/FDV of 12.3%**, with circulating supply *"as low as 6% and none exceeding 20%"*, and roughly **$155B of tokens scheduled to unlock 2024–2030** — with **~$80B of buy-side liquidity needed just to hold prices flat**.

Two clarifications the engine must respect:

- **`Released ≠ Circulating`.** Read the vesting/release contracts; compare `released` against `total` on-chain.
- **Low float is sometimes intended.** Long-vesting designs produce low float by design. Check allocation design before penalising.

### 6.4 Concentrating the economics

Across three independent datasets, the **top 0.1–2% of addresses captures most of the economics.** The highest-value concentration signal is **realised-PnL concentration** (`CS10`), because it is derivable from chain alone with no vendor dependency and is far harder to fabricate than a holder-count statistic.

Companion signals: top-10 concentration *excluding* known treasury/team/burn addresses; deployer-linked clustering; and Sybil/mercenary recipient identification from funding-source linkage and win-rate/holding-period distributions.

### 6.5 Quantifying speculative dependence

Segment volume and flows by whether they are subsidy-dependent:

- DeFiLlama's own **artificial-liquidity exclusion list** plus **emissions-per-net-inflow** as an incentive-dependence ratio.
- **Organic vs incentivised volume share.**
- **`TVL up / token down` divergence** (`CS26`) — the signature of subsidised mercenary capital, since capital arriving for a subsidy leaves when the subsidy ends.
- **Airdrop liquidation velocity** (`CS21`) — how fast recipients sell.
- **Buyback execution on-chain** (`CS07`) — not the announcement.

### 6.6 Real-use-case marker set

Evidence of usage independent of speculation: **organic fee generation net of incentives**; returning/sticky users; external developer integrations; real treasury revenue; and **usage sustained through a full market cycle**.

Note the asymmetry against §5.2: governance participation *collapsed* to near-nil, whereas a protocol that retained usage and fees through a bear market is demonstrating something structurally different from one that did not.

### 6.7 Marketing red flags as economic tells

A token name or ticker that carries the value-capture story; an AI/DeFi/RWA/GameFi label with **no fee line**; buyback announcements with **no verifiable on-chain execution**; unverifiable partnerships; influencer- and paid-promo-funded launches.

---

## 7. Dimension 4 — Regulatory, Legal and Counterparty Legitimacy

*Source: slice D, 58 sources (Tier A 45 · B 10 · C 2 · D 3) — the most heavily Tier-A slice.*

### 7.1 The securities question is unsettled, and the tool must say so

The current US position, per the SEC's 17 March 2026 Commission Interpretation (**Rel. 33-11412**), joined by the CFTC, is a five-category taxonomy — **digital commodities, collectibles, tools, and GENIUS-Act stablecoins are not securities; digital securities are** — with a Howey *"investment-contract overlay"* that attaches when an issuer makes essential-managerial-efforts promises and **detaches when those promises are fulfilled or abandoned**.

Legislative state: Congress enacted only the **GENIUS Act** (Pub. L. 119-27, stablecoins only). The **CLARITY Act remains a bill** — the SEC's own August 2026 statement records that Congress has not delivered it. **Regulation Crypto Assets (33-11434, Aug 2026) is a proposal, not law.**

DePIN/staking is explicitly non-security **at the protocol layer**. And the **Ooki DAO** case confirms that **ring-fencing does not defeat "person" status** — you cannot escape the securities analysis by routing activity through a DAO wrapper.

> **Tool requirement:** the legal dimension must emit `CONTESTED` where the law is unsettled, and must never resolve unsettled questions into a numeric penalty. Penalising a project for a classification no authority has made is a defect, not conservatism.

### 7.2 Four legally distinct states

Conflating these is the most common regulatory error:

| State | Meaning | Example |
|---|---|---|
| **Unlicensed but legal** | No intermediary → often outside MiCA scope entirely | Fully decentralised protocols |
| **Unlicensed and illegal** | Operating without required authorisation | **Ooki DAO**; Garantex lost its Estonian licence in 2022 |
| **Licensed** | Authorisation verified in the specific register | — |
| **Sanctioned** | Counterparty or address on a sanctions list | Tornado Cash SDN delisting (Mar 2025) |

### 7.3 Verifying "we are compliant" — the register-verification rule

This is the dimension's most transferable insight: **compliance claims are graded, and only one grade is worth scoring.**

| Grade | Meaning |
|---|---|
| Marketing claim | A sentence on the website |
| Self-attestation | A statement without third-party evidence |
| **Register hit** | The named operator appears **active** in the home corporate registry, and in the register matching **the specific permission claimed** |
| On-chain proof | Reserves or ownership provable cryptographically |

Two corrections this implies:

- **Register presence ≠ approval.** ESMA states that white papers in its register are *"not reviewed or approved"* by any authority.
- **A neighbouring-regime hit is not a hit.** Checking whether a company is on a payments register does not evidence a MiCA CASP authorisation.

**The single strongest signal in this dimension is `D-S01`:** the named operator is verified **active in its home corporate registry**. Combined with `D-S03` (a query of ESMA's **"Non-compliant entities"** file, a near-terminal negative) it is close to unforgeable — a real registry entry is very hard to fake, which is exactly why it is worth more than any self-report.

Named enforcement cases gathered: 12 (including **Ooki DAO**, **Roman Storm / OneCoin** convicted August 2025 on an unlicensed-MTB charge, **Garantex**, **Tornado Cash**, and the SEC/CFTC 2026 Polymarket and Kalshi insider-trading matters).

### 7.4 Identity, custody and jurisdiction

The key legitimacy question is **who operates the front end and who holds the keys.**

- **Identity:** identifiable legal entity vs anonymous team. Resolve via corporate registry, and for US non-profits via IRS Form 990 / ProPublica Nonprofit Explorer.
- **Jurisdiction:** EU establishment matters because MiCA Article 61 imposes requirements on the **responsible entity**, and legislates **against Terms-of-Service disclaimers** — *"notwithstanding any contractual clause."* ASIC's Block Earner standard is **substance over labelling**, so a "we're not a financial service provider" disclaimer carries no weight.
- **Custody:** custodial vs non-custodial determines who bears liability. Establish the **custodial fact pattern empirically** (`D-S10`) rather than accepting "non-custodial" branding.
- **Structural argument:** MiCA forbids delegating custody to unauthorised entities, so **compliance never travels down to the protocol**. A protocol delegating compliance to a front-end operator does not inherit it. `D-S20` tests whether the operator is authorised and whether the B2B arguments survive scrutiny.

### 7.5 Sanctions, AML and the strict-liability point

**OFAC sanctions exposure is a matter of strict liability.** That makes "we are OFAC-compliant" a **legal conclusion**, not a technical state, and it should be scored as such.

Crucially, slice D found that **sector rates are not project signals**. Chainalysis puts illicit share at **<1% of volume**, so a sector-wide rate tells you nothing about a specific project. Use **counterparty exposure** instead: does the protocol interact with mixers, offshore VASPs, or sanctioned-chain exposure?

`D-S14` (AML/sanctions programme **evidenced, not asserted**) and `D-S25` (negative findings typed `hit` / `miss` / `not-checkable` with snapshot dates) round out the set.

### 7.6 Privacy and operational compliance

GDPR posture (EDPB Guidelines 02/2025), KYC/AML presence where the fact pattern is CEX-like, and the structural point above. Also note the reference site's own choice — visitor IP, country and session recording with a cookie-style notice — as a **pattern to invert**: a tool that analyses suspected fraud should not fingerprint its users ([§10.6](#106-non-negotiables)).

---

## 8. Dimension 5 — Empirical Evidence: What Actually Predicts Survival

*Source: slices E1 (64 sources, 36 signals) and E2 (18 sources, 22 signals). Full files: [`research/E1_empirical_cases.md`](research/E1_empirical_cases.md), [`research/E2_metric_reliability.md`](research/E2_metric_reliability.md).*

This is the section that **changes the design**, because its central finding is negative.

### 8.1 The headline: only one metric survives as a gate

Of fifteen candidate metrics, **exactly one survives as a hard gate** — and only in the weak form *"does a verifiable mechanism exist."* Everything else is demoted, inverted, or blocked on absent evidence.

| Metric | Measured evidence | Verdict |
|---|---|---|
| **token-holder revenue** | **~400 of 1,244** protocols clear $1M annual fees, but only **~20 (≈1.6%)** pass $10M to holders | **GATE** (existence of mechanism) |
| trading volume | Wash trading averaged **>70% of reported volume on unregulated exchanges**; fabricated volume **improves published rankings**. On NFTs ~38% of trades / ~60% of value manipulated | **INVERT → penalty** |
| audit count | Findings stable (Critical+High share 15–17% yearly) but **misaligned with losses**: key compromise, phishing and social engineering are **~49.6% of losses** yet a negligible share of audit findings; **<2%** of flagged contracts are ever exploited | **INVERT → penalty** |
| TVL | DeFiLlama's own docs concede TVL is **price-confounded** | Weight, low confidence |
| holder concentration (HHI) | Zukowski (n=52): delegation **amplifies** voting concentration above holdings in **13 of 18** protocols, up to **21×** | Weight, high confidence — but see §8.4 |
| holder count | Up to **66%** of airdrop tokens are *"rapidly sold, often in recipients' first post-claim transaction"* | Context only |
| developer activity | **No project-level survival statistic retrievable** — Electric Capital publishes ecosystem-level only | Weight, marked UNSOURCED |
| protocol age | Confounded with the selection it produces | Context only |
| social followers | **Nothing retrieved** with a denominator and out-of-sample statistic | **REJECT** |
| realised-PnL concentration | Volume fabrication measured; PnL-as-score **UNSOURCED** | v2 |
| organic-vs-incentivised volume | DeFiLlama *defines* it, publishes **no statistic** | v2 |
| treasury runway | Not population-computable; DeFiLlama concedes expenses are forum-sourced and "always referencing old data" | Weight, low confidence |

### 8.2 Why there is no composite score

Three findings, each sufficient on its own:

1. **The only survival model in the literature collapses out of sample.** It scores **0.98 in-sample and degrades to 0.59–0.65 on unseen data** — and the paper reports this against itself. It also omits every off-chain feature.
2. **No published trust or legitimacy score has ever been validated against outcomes.** Precedent offers no safety net whatsoever.
3. **CoinGecko's Trust Score is rank-relative** (curve-graded across the population) and allocates **50% of its weight to liquidity**. A rank inside a bad cohort is not a judgement of quality.

> **Therefore this report does not recommend a composite 0–100 score.** The architecture in §10 emits **gates + per-pillar evidence panels + a captioned visual summary, and no overall number.**

This is the report's most consequential recommendation, and it reverses the obvious design.

### 8.3 Base rates: the finding that calibrates everything

Of 1,244 protocols (2020–Q3 2025), ~400 clear $1M in fees but only **~20 ≈1.6%** pass $10M to holders. Three consequences:

- **Absence of holder accrual is the norm**, so it is a *weak* negative for a young project and a *strong* negative for a mature one. Age must modulate the signal.
- **Any high score is weak evidence** in a population where ~98% of projects do not accrue to holders.
- A tool must state this next to any verdict that depends on it, or the user will over-read it.

### 8.4 Counter-evidence and the traps

Slice E1's "cases the tool must not get wrong" is the most valuable corrective in the research. Surface signals actively mislead in these cases:

| Case | Why the surface signals mislead | What the tool must conclude |
|---|---|---|
| **MakerDAO / Sky, Mar 2020** | A flagship, audited, 2-year-old protocol left **$4.5M unbacked DAI** | "Audited and mature" ≠ safe. Recency of failure matters more than age of success. |
| **USDC / Circle** | On-chain it is **indistinguishable** from an algorithmic stablecoin | Product pattern alone must not condemn it; the reserve and issuer structure is the discriminator. |
| **Uniswap** | For seven years the token had **no fee entitlement**, and volume is trivially gamed | Do not conclude from volume. Conclude from the dated accrual mechanism. |
| **EigenLayer** | Real technology, shipping AVSs, top-1 restaking TVL share (**$7.03B, 65% of category**) | Real product ≠ legitimate token economics. These are separable judgements. |
| **SushiSwap today** | Cumulative volume **$251.6B** — same shape of number as Uniswap | Lifetime volume is not evidence of current utility. |
| **Axie Infinity** | 2.7M daily users at peak, billions in NFT volume, a purpose-built sidechain | Scale built on incentives decays; check post-incentive retention. |
| **Chainlink** | Cumulative fees of only **$76.22M** against LINK's market cap look trivial | A naive fee/valuation check wrongly calls it a bad deal. Its value is infrastructure, captured elsewhere. |

#### Pre-loss observables: what was computable *before* the failure

This is the most actionable material in the research, because these were not hindsight — each was arithmetically or structurally detectable in advance.

| Case | The observable | Check |
|---|---|---|
| **Terra / UST** | LFG held **~80,000 BTC** — but **no redemption module had ever shipped**. The reserve was large and the holder had no path to it. | Is the stated backing **redeemable by the holder**? (`E1-S15`) |
| **Iron Finance** | A **60-minute TWAP** priced against a real-time AMM made the stabilising arbitrage **profitable while TITAN rose and unprofitable as it fell.** | Simulate the arbitrage that defends the peg under *falling* collateral. (`E1-S17`) |
| **Ronin bridge** | **4 of 9** keys held by one operator against a **5-of-9** threshold, plus a **gas-free RPC endpoint added in Nov 2021 that was never revoked** when the loan ended. | Key-management composite: max keys held by one operator vs threshold; non-revoked temporary access. (`E1-S23`) |
| **KyberSwap** | The exploiter messaged the **KyberDAO multisig** demanding control of the protocol and the DAO in exchange for returning 50%. | Flag a governance body being used as a **negotiation counterparty**. (`E1-S27`) |
| **NovaTech / HyperFund** | Withdrawal gates and vesting arithmetic, not chain analysis. | Withdrawal-gate surveillance. (`E1-S29`) |
| **Saitama-class launches** | Promoter-hired "market makers" (named entities including ZM Quant, Gotbit, CLS Global) wash-trading so volume would look organic. | Distinct-counterparty volume filter. (`E1-S06`) |

Two further observations from this slice matter for the tool's credibility:

- **Prosecutors read on-chain transcripts the same way we do.** The FBI's *NexFundAI* operation and the **KyberDAO extortion** both turned on reading transaction-level evidence. Detection method and enforcement method converge, which is the strongest available external validation of this approach.
- **Self-disclosure is the model behaviour, and it is cheap.** Arbitrum publishes its **own trust-assumption share — 38.3% of TVS** — via L2BEAT, alongside $11.46B–$11.57B TVS. A project disclosing its own centralisation risk should score *better* on transparency than one that stays silent. Absence of published risk accounting must be scored **`UNKNOWN`**, never as low risk. (`E1-S08`)

#### A metric that inverts: fee-to-valuation ratio

**EigenLayer and Chainlink have near-identical fee-to-valuation ratios and opposite verdicts.** That single pair falsifies any threshold built on fee/valuation. It is the clearest possible demonstration of why §8.1 demotes and §8.2 rejects composite scalars — a scalar that cannot separate these two cannot be trusted on anything harder.

Additional counter-evidence from E2:

- **Selective survivorship** guarantees we study winners; base rates derived from surviving projects are unreliable.
- **The one available "dead coins" study reports >52% of tokens dead** — which is a reminder that failure, not fraud, is the base rate.
- **Early-stage labelling is genuinely hard**, and a tool should refuse to conclude rather than guess.

### 8.5 What would falsify this report

Slice E2 names its own highest-leverage falsifiers:

1. **The 1.6% prevalence figure** — the entire "one hard gate" conclusion rests on it, and it comes from a single **Tier D** research house whose protocol-level data is aggregated from Dune/Token Terminal/DeFiLlama. If wrong, the gate's calibration changes.
2. **A published composite score validated out of sample** would reopen the composite-scalar design.
3. **Per-project developer-retention-to-survival statistics** would let us demote or promote the developer dimension; currently UNSOURCED.
4. **A retrievable post-incentive TVL retention table** — the widely repeated "40–70% exit within 30 days" figures trace only to Tier E blogs.
5. **A measured effect size for token-unlock overhang** — only a self-described *preliminary* preprint (n=52) was found; not cited as a number.

## 9. Scoring Architecture: Gates, Caps and the Averaging Failure

*Source: slice F, 59 sources (Tier B 15 · C 21 · D 12 · A 11).*

### 9.1 The averaging failure, demonstrated

Slice F derived the core result, and it is independently reproducible. Let components be normalised to [0,1] with `S = Σwᵢxᵢ`, `Σwᵢ = 1`. If the fatal criterion `F` scores zero and every other criterion scores 1, then the maximum attainable score is

```
S_max = 1 − w_F
```

Failure is forced only if `S_max < τ`. Therefore:

> **A criterion can force a failure only if `w_F > 1 − τ`.**

At a `τ = 0.7` pass threshold, `1 − τ = 0.30`:

| `w_F` | `S_max = 1 − w_F` | Forces failure at τ=0.7? |
|---|---|---|
| 0.05 | 0.95 | No |
| 0.10 | 0.90 | No |
| 0.20 | 0.80 | No |
| 0.30 | 0.70 | No (boundary) |
| **0.31** | **0.69** | **Yes** |
| 0.50 | 0.50 | Yes |

So **no weight at or below 0.30 can ever force a failure on its own.** CoinGecko's published Trust Score weights are `w_PoR = 0.05` and `w_Incident = 0.10` — an exchange scoring zero on proof of reserves *and* zero on incident history is arithmetically incapable of failing. This is not a bug: CoinGecko documents PoR as a **floor** (*"an exchange that does not have any form of asset disclosure will not have a 10/10 Trust Score"*), not a gate. They score exchanges, where custody is one dimension among many.

**For a legitimacy verdict the conclusion is unambiguous: if we believe a dimension is fatal, we must implement it as a gate. Weighting mathematically cannot do the job.**

### 9.2 The one-bad-apple demonstration

Eight equally weighted binary-ish criteria at `w = 0.125`: a subject scoring 1.0 on seven and 0.0 on the eighth scores **0.875** — comfortably passing. **Any equal-weight composite over ≥8 signals passes a project with one fatal defect.** (The reference site's near-uniform 3–6pt weighting, summing to exactly 100, has this property.)

### 9.3 The legal precedent for graduated gates

The 2023 DOJ/FTC Merger Guidelines use **conjunctive structural presumptions**, not averages: a merger is presumed to substantially lessen competition if post-merger **HHI > 1,800 AND ΔHHI > 100**; or merged share **>30% AND ΔHHI > 100**. The presumption is *"rebutted or disproved"* and *"the stronger the evidence needed to rebut or disprove it"* — i.e. **confidence in the rebuttal is itself graduated.**

That is precisely the gate architecture wanted, with a citable legal analogue: **an anti-gaming gate whose override requires stronger evidence than the default.** The 2010 guidelines show banded structure (<1,500 / 1,500–2,500 / >2,500), and the 2023 guidelines note they *returned* to the original thresholds because *"the original HHI thresholds better reflect the law and the risks of competitive harm"* — evidence that thresholds should be anchored to empirically-justified bands, not tuned to taste.

### 9.4 Existing frameworks: what to reuse

| Framework | What it does | Verdict |
|---|---|---|
| **CoinGecko Trust Score** | Weighted components (Liquidity 50 / Cyber 20 / Reg 15 / Incident 10 / PoR 5), then **curve-graded over the population** | **Most reusable published model.** Has an exchange-quality gate (*"a few clean tickers is not sufficient"*) and a **register-verification rule** — unconfirmed self-credentials score zero. But curve-grading is a *ranking* device and must never drive a pass/fail verdict: a curve guarantees a top decile even when the cohort is uniformly bad. |
| **CoinMarketCap** | Third-band listing taxonomy; public **audit-badge partner APIs**; verified vs self-reported supply shown separately | Reuse the badge and supply-separation primitives |
| **FATF Travel Rule (R.16)** | Information transfer standards | Usable **only as an evidence standard** (*"does not accept post facto transmission"*) — not as a pass/fail gate |
| **Proof of Reserves** | Reserve attestations | Requires **verification-of-verification**: Hacken v3's random-sample + dummy-account negative control; and *"assets are worthless without liabilities"* — **liabilities are the half that is almost never disclosed** |
| **Trust Wallet** | Listing tiers | Supplies rare numeric floors: ≥10k holders, ≥15k transactions, **airdrops excluded**, 100 transfers/yr |
| **CryptoRank trust score** | — | **No published methodology retrievable. Not citable, not reusable as a design input.** |
| **Santiment score** | — | **No published weighting. Excluded.** |

### 9.5 Anti-gaming: the Goodhart taxonomy

Organised by Manheim & Garrabrant's four Goodhart types, three signal classes are **gaming-resistant**:

- **Gaming-resistant:** signals produced by a party who does not benefit from the score being high, and which cannot be manufactured without incurring a cost proportional to the thing being measured. → *licence-register hits; recursive on-chain admin resolution; realised-PnL distribution; deployment-age and unlock schedules.*
- **Easily gamed:** anything the project can publish. → *whitepapers, roadmaps, audit PDFs without commit hashes, "audited by a top firm" logos, published signer lists without drift monitoring.*
- **Mimicry-target:** signals whose form is easy to copy. → *cosmetic decentralization, fake dev commits, incentivized TVL, Sybil-split holder counts.*

Concrete adversarial responses: **honeypot audits** (an audit that is published but unverifiable) → demand a commit hash cross-checked against the deployed bytecode and, where available, against CoinMarketCap's partner feeds; **Sybil holder splitting** → cluster by funding source and realised-PnL rather than counting addresses; **fake dev commits** → weight 12-month decay and contributor concentration, not commit volume.

The organising principle, and the single most important design sentence in this report:

> **The questionnaire is a claim generator, not an evidence source. Unverifiable answers carry weight 0.**

### 9.8 Composite scores are rejected on evidence

Slice E2 ([§8.2](#82-why-there-is-no-composite-score)) measured three things that together forbid a headline scalar:

| Finding | Consequence for this tool |
|---|---|
| Only survival model in the literature: **0.98 in-sample → 0.59–0.65 out of sample**, omitting all off-chain features | A weight vector tuned to known cases will not generalise |
| **No published trust/legitimacy score has ever been validated against outcomes** | No precedent to inherit |
| CoinGecko Trust Score is **rank-relative** and **50% liquidity-weighted** | A rank in a cohort is not a quality verdict |
| Wash trading **>70% of volume on unregulated exchanges**, and fabrication **improves published rankings** | Volume/TVL composite inputs are partly fabricable *and profitable to fake* |

**Two further inversions** (see [§8.1](#81-the-headline-only-one-metric-survives-as-a-gate)): **trading volume** and **audit count** are not positive contributors. High volume with failed wash screening *reduces* confidence in other claims; absence of a traceable audit raises a penalty. Both render as **risk** items, never as achievements.

**The only metric that survives as a gate is token-holder revenue**, in the existence-of-mechanism form — calibrated against the **≈1.6% base rate** ([§8.3](#83-base-rates-the-finding-that-calibrates-everything)).

### 9.6 Questionnaire design validity

Slice F's headline finding for this layer: a questionnaire that asks about facts the tool can independently check **adds no information** — and invites gaming, because answering well is rewarded. So the questionnaire's only legitimate functions are:

1. **Pointing** — telling the engine where to look (which chain, which address, which registry).
2. **Collecting** the artifacts a project holds that automated discovery cannot reach (a private financial summary, a signed audit not yet public, an unpublished license).
3. **Explaining** — letting the project state a claim the engine will then attempt to falsify.

Design guidance carried forward: avoid leading questions; be explicit that **verbosity and positivity are not evidence** ([§5.4](#54-transparency-and-disclosure-the-falsifiable-artifact-principle)); and do not present a radar chart as the primary verdict — of three radial options tested, radar ranked **worst**, because radar area implies axis comparability and rewards balanced-looking mediocre profiles. Legitimacy profiles are legitimately lopsided (excellent security, thin documentation). Radar is acceptable as a *secondary* visual **only** when paired with per-axis evidence state.

### 9.7 Free-tier data sources

Thirteen tabulated with real limits. The ones that constrain the build:

| Source | Limit |
|---|---|
| Etherscan | 3 calls/sec, 100k/day on selected chains |
| GitHub | 60 req/hr unauthenticated · 5,000/hr authenticated |
| Dune | Free tier = 20 credits/MB |
| CoinGecko Demo | 100 calls/min · 10k/month |
| **Artemis** | **REST API is enterprise-gated — the $0 tier does not buy API access** |
| **Full holder sets** | Etherscan `tokenholderlist` is top-N only; HHI needs paid providers or self-authored Dune SQL |
| **Treasury balances** | **No free API found.** Treasury-runway calculation is the hardest free-tier gap |
| **Address-level sanctions screening** | OFAC SLS is name-only with fuzzy matching. **Structural hole**, not a research gap — needs a paid provider |

---

## 10. Proposed Tool Specification

### 10.1 Reference-site UX — inherited

From the 17-tool reference suite (full teardown in [Appendix A](#appendix-a--reference-site-teardown-summary)):

- Six-pillar radar plotting **% of weight earned per pillar**, with vertically stacked axis labels
- Per-pillar progress sidebar + completion counter (`0 / 22`)
- **Annotated per-answer point values** — `[2.5 / 5 pts] Designated Internal Peer — lacks true independence`
- Sticky live score panel with band label
- **Explicit gate/cap banner**: *"Your raw score (100/100) has been capped at 49/100 due to a critical audit gate breach."*
- Weakest-dimension surfacing
- `Weight × Gap Severity` remediation backlog bucketed into `short_term` / `medium_term` / `long_term`
- Versioned JSON export + Copy-Markdown + HTML + PDF
- Standing disclaimer; client-side privacy

**Deliberately not inherited:** near-uniform 3–6pt weights; weights summing to exactly 100 as a false-precision headline; gates keyed to a single self-report answer; bare radar percentages; Tier-E-grade sources.

### 10.2 Evidence-resolution pipeline

```
              ┌───────────────────────────────────────────────┐
              │  ARTIFACT COLLECTION (per dimension)          │
              │  chain · verified contract · registry entry   │
              │  audit PDF + commit hash · team wallets       │
              │  metrics series · entity documents            │
              └─────────────────────┬─────────────────────────┘
                                    ▼
              ┌───────────────────────────────────────────────┐
              │  STATE RESOLUTION (four states, asymmetric)  │
              │   VERIFIED      full credit                   │
              │   CLAIMED       partial credit + flag          │
              │   CONTRADICTED  zero credit + blocker          │
              │   UNKNOWN / N-A zero *and* excluded from base │
              └─────────────────────┬─────────────────────────┘
                                    ▼
              ┌───────────────────────────────────────────────┐
              │  GATE EVALUATION  (pre-registered,           │
              │  severity-ordered, applied FIRST)            │
              │  w_F > 1−τ is impossible → gates, not weights │
              └────────┬───────────────────────────┬──────────┘
                 pass ▼                           ▼ fail
                      │                  ┌──────────────────────┐
                      │                  │ CAP + BLOCKER LISTED │
                      │                  │ override needs       │
                      │                  │ STRONGER evidence    │
                      │                  └──────────┬───────────┘
                      ▼                             ▼
              ┌───────────────────────────────────────────────┐
              │  PER-PILLAR EVIDENCE PANELS → RADAR + BAND     │
              │  NO composite scalar (see §9.8)                │
              │  radar pairs % with per-axis EVIDENCE STATE     │
              │  and COVERAGE (checked / total)                │
              └─────────────────────┬─────────────────────────┘
                                    ▼
              ┌───────────────────────────────────────────────┐
              │  OUTPUT: band phrase (no number)              │
              │  + per-dimension evidence state + coverage    │
              │  + BASE RATES where the verdict depends on it │
              │  + gaps (Weight × Severity) + versioned JSON  │
              │  + "WHAT WOULD CHANGE THIS VERDICT"          │
              │  + CONTESTED / UNSOURCED register             │
              └───────────────────────────────────────────────┘
```

**The `UNKNOWN ≠ 0` rule** is the false-positive control. An unmeasurable dimension contributes neither credit nor penalty, and the band is computed over a *stated denominator* so a project with thin evidence is visibly distinguished from a project with evidence that contradicts itself.

### 10.3 Pre-registered gate register (first draft)

Ordered by severity. Caps mirror the reference site's proven 39/49/59 ladder.

| Gate | Condition | Cap | Justification |
|---|---|---|---|
| **G-SANCTION** | Named operator or controlling counterparty on a sanctions list | **39** | OFAC is strict liability → [§7.5](#75-sanctions-aml-and-the-strict-liability-point) |
| **G-REG-UNLICENSED** | Operates an activity requiring authorisation under a regime it is demonstrably inside, with no authorisation in the matching register | **39** | [§7.3](#73-verifying-we-are-compliant--the-register-verification-rule) |
| **G-NOVERIFY** | No verifiable source for code that holds or can move user funds, and no audit traceable to a commit hash | **49** | [§4.1](#41-what-a-credible-project-should-publish) |
| **G-ADMIN-SINGLEKEY** | Terminal admin resolves to an EOA, or a multisig whose `(owners−threshold)+1` is ≤1 and whose guard/modules are non-empty | **49** | [§4.2](#42-verifying-a-multisig-not-believing-one) |
| **G-NOACCRUAL** | Verified absence of any fee-accrual route to holders, while claiming product utility | **59** | [§6.1](#61-protocol-revenue-is-not-token-holder-revenue) |
| **G-REFLEXIVE** | Realised-PnL distribution shows near-total concentration consistent with a reflexive loop, with no fee-accrual route | **59** | [§6.2](#62-casino-mechanics-taxonomy-m1m11) |
| **G-HONEYPOT** | Contract permits sale by insiders but not by users, or transfer restrictions contradict marketing | **39** | Standard trading-fraud pattern |
| **G-WASHVOL** | Reported volume fails wash-trading screening **and** is offered as evidence of usage | **CAPPED** | Fabricated volume *improves* rankings — treat as misrepresentation ([§8.1](#81-the-headline-only-one-metric-survives-as-a-gate)) |

**Gate rules:**

1. **Gates fire before aggregation** and are not subject to offset by strong pillars.
2. **Override requires stronger evidence than default** (DOJ/FTC graduated-rebuttal analogue, [§9.3](#93-the-legal-precedent-for-graduated-gates)).
3. **Gates are versioned and frozen.** The register is published before scoring; changes require a new `toolId` version. This prevents post-hoc tuning against a known target.
4. **Every gate fire must cite the artifact** that fired it.

### 10.4 Pillar design

Five pillars map to the five research dimensions. Radar pairs each pillar's `% earned` with its **evidence state**, so the visual can never be read as more than it is:

| Pillar | Source | Notes |
|---|---|---|
| **Security & Technical Integrity** | Slice A (30 signals) | Loss-vector attribution as a sub-axis, not folded in |
| **Governance & Treasury** | Slice B (30) | Nakamoto + delegation amplification as primary |
| **Token Economics & Value Capture** | Slice C (32) | Holders Revenue presence as the anchor |
| **Regulatory & Counterparty** | Slice D (25) | Register hits; `CONTESTED` where law is unsettled |
| **Real Use-Case Evidence** | Slice E + Slice F (28) | Organic fee generation; **currently under-evidenced — see §8** |

### 10.5 Output contract

Extends the reference suite's versioned export with the evidence layer:

```json
{
  "schemaVersion": "CryptoLegitimacy_v1",
  "toolId": "CryptoLegitimacy_v1",
  "timestamp": "2026-10-06T00:00:00Z",
  "subject": { "name": "", "chains": [], "contracts": [], "entity": {} },

  "rawScore": null,
  "finalScore": null,
  "band": "UNKNOWN | CRITICAL | HIGH | MEDIUM | LOW",
  "isCapped": false,
  "capReasons": [],

  "gates": [
    { "gateId": "G-ADMIN-SINGLEKEY", "fired": true, "capOverride": 49,
      "blockerSeverity": "CRITICAL",
      "artifact": "0x…", "check": "recursive owner() walk", "snapshot": "2026-10-06" }
  ],

  "dimensions": [
    { "id": "SEC", "name": "Security & Technical Integrity",
      "earned": null, "possible": null,
      "evidenceState": "PARTIAL",
      "signals": [
        { "id": "SEC-20", "state": "VERIFIED", "value": null,
          "artifact": "https://…", "checkedAt": "2026-10-06", "reRunable": true }
      ] }
  ],

  "contested": [ { "claim": "", "positions": [], "note": "" } ],
  "unsourced":  [ { "claim": "", "why": "" } ],
  "whatWouldChangeThisVerdict": [ { "if": "", "then": "" } ],
  "gaps": [ { "signalId": "", "weight": 0, "severity": "", "sprint": "short_term" } ]
}
```

`whatWouldChangeThisVerdict` is the counter-evidence surface and should be mandatory, not optional.

### 10.6 Non-negotiables

1. No score is issued on unresolvable evidence; `UNKNOWN` is a valid, prominent outcome.
2. Every scored answer resolves to an artifact, an independent producer, and a re-runnable check.
3. Sources are Tier A–D. Tier E never grounds a claim.
4. Gates are pre-registered, versioned, and applied before aggregation.
5. `UNKNOWN ≠ 0`, and denominators are stated.
6. **No user fingerprinting.** A tool that analyses suspected fraud must not record visitor IP or geolocate users — inverting the reference site's pattern.
7. Standing disclaimer: informational, not investment, legal or tax advice.

---

## 11. Consolidated Signal Register

**203 signals** extracted across all seven completed slices. Full table with per-signal measurement, mechanical verification, evidence tier, failure mode and agent-reported confidence: **[`SIGNAL_REGISTER.md`](SIGNAL_REGISTER.md)**.

Highest-value signals by slice:

**Slice A — Security (30):** `SEC-02` audit date vs deployed implementation (the Euler class) · `SEC-12` Safe-proxy detection without EIP-1967 false negatives · `SEC-20` reverse-authority check · `SEC-14` `(owners−threshold)+1` keys to lose control · `SEC-30` loss-vector attribution · `SEC-29` compiler pinning vs known advisories · `SEC-25` oracle not sourced from a single thin AMM pool · `SEC-15` Safe has no bypass modules

**Slice B — Governance (30):** `GOV-ADMIN-TERMINAL` recursive `owner()`/`admin()` walk · `GOV-DELEGATION-AMPLIFICATION` voting-HHI ÷ holding-HHI · `GOV-NAKAMOTO-DELEGATES` · `TREAS-NATIVE-EXCL-RATIO` treasury ex-native ÷ market cap · `TEAM-INSIDER-RETAINED` · `GOV-EMERGENCY-SCOPE` · `TREAS-RUNWAY-DISCLOSED` · `CLAIM-FALSIFIER-COUNT`

**Slice C — Tokenomics (32):** `CS01` Holders Revenue absent ⇒ no accrual · `CS10` realised-PnL concentration · `CS04` emissions ÷ holder revenue >2× · `CS05` organic vs incentivised TVL share · `CS07` buyback executed on-chain · `CS14` released/circulating overhang · `CS21` airdrop liquidation velocity · `CS26` TVL-up/token-down divergence

**Slice D — Regulatory (25):** `D-S01` operator verified active in home corporate registry · `D-S02` register hit for the *specific* permission claimed · `D-S07` token category stated and surviving read-through · `D-S10` custodial fact pattern established empirically · `D-S20` compliance-not-delegated · `D-S03` ESMA non-compliant-entities file query · `D-S14` AML programme evidenced · `D-S25` negative findings typed `hit`/`miss`/`not-checkable`

**Slice F — Measurement (28):** `SIG-04` licence register lookup · `SIG-02` on-chain owner/admin/proxy/timelock resolution · `SIG-05` audit scoped to a commit hash, cross-checked · `SIG-09` artificial-liquidity exclusion list + emissions-per-net-inflow · `SIG-08` Sybil cluster count, airdrops excluded · `SIG-10` wash-trading estimation · `SIG-16` 12-month dev decay + contributor HHI · `SIG-28` every scored answer resolves to artefact + independent producer + re-runnable check

**Slice E1 — Case studies (36):** *case-derived* rather than metric-derived. The highest-value members are **pre-loss observables** — signals that were computable before the failure rather than reconstructed after it:

`E1-S15` is the stated peg backing **redeemable by the holder** (Terra held ~80k BTC but no redemption module ever shipped) · `E1-S17` **stabiliser-arbitrage asymmetry** under falling collateral (Iron Finance's 60-min TWAP vs real-time AMM) · `E1-S20` flash-loan-acquirable governance weight · `E1-S23` key-management composite (Ronin: 4-of-9 against a 5-of-9 threshold plus an unrevoked gas-free RPC) · `E1-S06` distinct-counterparty volume filter · `E1-S16` yield-versus-carry gap · `E1-S27` post-exploit extortion of a multisig as a negotiation counterparty (KyberDAO) · `E1-S29` withdrawal-gate surveillance · `E1-S31` registration-claim verifiability · `E1-S08` third-party published trust-assumption share (Arbitrum: **38.3%** of TVS)

The slice also supplies the trap cases in [§8.4](#84-counter-evidence-and-the-traps), including the decisive pair: **EigenLayer and Chainlink have near-identical fee-to-valuation ratios and opposite verdicts.** Full set in `SIGNAL_REGISTER.md`.

**Slice E2 — Metric reliability (22):** `E2-S01` holder-accrual mechanism exists — the only gate the evidence supports · `E2-S03` the ≈1.6% prevalence base rate that makes S01 a gate at all · `E2-S04` wash-volume screening, a precondition for any volume use · `E2-S08` voting-HHI ÷ holding-HHI amplification ratio · `E2-S13` developer-activity *composition*, fingerprint-deduped · `E2-S14` minimum observation window, which forces "insufficient evidence" instead of a low score · `E2-S19` threshold/window sensitivity range on any concentration metric · `E2-S20` composite-vs-best-component out-of-sample test

---

## 12. Limitations, Contestations and What Would Change the Verdict

### 12.1 Explicitly unsourced — do not treat as established

| Item | Status |
|---|---|
| **SEC v. Winding Tree LLC** (D.C. Cir. 2025) primary opinion | **Not retrieved** despite multiple strategies. Holding deliberately not asserted. |
| SEC v. Grayscale / SEC v. CoinShares primary texts | Not retrieved; effect corroborated only indirectly |
| DOJ Tether/OFAC settlement (Oct 2021) | **Do not cite until fetched.** `justice.gov` unreachable on every attempt |
| Ooki DAO penalty amount; DOJ Storm conviction figures | Outcome confirmed; figures not |
| **Roadmap-retrospective ↔ outcome correlation** | **UNSOURCED.** No authoritative study |
| Undisclosed team token allocation as a *prosecuted* ground | SEC cases (SafeMoon, Quantstamp, Terraform, Mashinsky) concern registration, liquidity-lock lies or misappropriation — **not** allocation |
| **Validated concentration thresholds** | **No Tier A–D source publishes cutoffs.** Compute raw, calibrate in-house |
| CryptoQuant manipulation thresholds | Methodology published; **thresholds are not** |
| CryptoRank / Santiment methodologies | No published weighting — **excluded from design input** |
| Geo-blocking/DNS/frontend jurisdiction as a legal test | No Tier A/B authority prescribes it — **inference-grade** |
| Incident incidence rates (e.g. % of announced buybacks never executed) | **No dataset exists** |
| Morris et al. USENIX 2018; Durieux & Ferreira ICSE 2020 | Not located; substituted teEther (USENIX Security '18) |
| **Any outcome validation of any published trust/legitimacy score** | **The load-bearing gap of the whole exercise** ([§8.2](#82-why-there-is-no-composite-score)) |
| **Per-project developer-retention → survival statistic** | Electric Capital publishes ecosystem-level data only |
| **Post-incentive TVL retention** | The widely repeated "40–70% exit in 30 days" figures trace only to Tier E |
| **Organic-vs-incentivised volume share statistic** | DeFiLlama defines it; publishes no number |
| **Protocol treasury runway across the population** | DeFiLlama concedes expenses are forum-sourced and "always referencing old data" |
| **Social follower count → outcomes** | Nothing with a denominator and an out-of-sample statistic |
| **Realised-PnL concentration as a score** | Volume fabrication is measured; PnL-as-score is not |
| **Token unlock overhang effect size** | Only a self-described *preliminary* preprint (n=52); not cited as a number |
| CryptoRank trust-score methodology | No published weighting — excluded from design input |
| **Lightning Network adoption** | **E1 could not reach a primary or measurement source, so Lightning is deliberately excluded as a case study** rather than included weakly |
| Blast's ~97% TVL collapse | Press-only — excluded from evidence, retained as a caveat |
| SushiSwap peak TVL | Three conflicting figures circulate; only the current value is used |
| Axie peak/current player counts · Lido staked-ETH share · Sky sUSDS balances | Not retrieved |

### 12.2 Structural gaps in the free tier

- **Address-level sanctions screening.** OFAC's public list is name-only with fuzzy matching. Address attribution needs a paid provider.
- **Treasury balances.** No free API; runway calculation is the hardest gap.
- **Full holder sets.** Top-N only on free tiers; HHI needs paid access or self-authored SQL.
- **Artemis** is enterprise-gated.

### 12.3 Methodological limitations of this report

1. **Survivorship and selection bias.** The case evidence concentrates on projects large enough to be documented. Base rates are therefore unreliable and the real-use-case marker set may be biased toward visible projects.
2. **Measurement is a moving target.** Fee-capture figures change daily; the Uniswap Holders Revenue number quoted here ($15.06M/30d) will differ on read. Any published figure needs a snapshot date.
3. **Some widely-believed signals have weak measured correlation.** Slice E2 measured this systematically: **trading volume, social follower count and audit count all fail** as positive indicators ([§8.1](#81-the-headline-only-one-metric-survives-as-a-gate)). Two dimensions the practitioner literature favours are rejected outright.
4. **New projects are structurally unscoreable** on history-based dimensions. This is a design constraint, not a data gap.
5. **The reference site's UX is designed for self-report**, and our evidence-resolution layer has no precedent at this scale. The pipeline in §10.2 is a specification, not a validated design.
6. **The single surviving gate rests on one Tier D prevalence figure** (≈1.6%). Slice E2 flagged this as the highest-leverage falsifier ([§8.5](#85-what-would-falsify-this-report)). If it is wrong, the gate's calibration — and possibly its selection — changes.

### 12.4 What would change this report's conclusions

- A Tier A–D source publishing **validated cutoffs** for holder concentration or manipulation detection would replace several "publish raw, don't threshold" positions with enforceable thresholds.
- Slice E2's metric-reliability ranking **already demoted** volume, followers and audit count ([§8.1](#81-the-headline-only-one-metric-survives-as-a-gate)). Further evidence could demote more of the 203 signals.
- A **free treasury-balance source** or a **published organic-volume statistic** would unblock two dimensions currently marked v2.
- Retrieval of the **Winding Tree** and **Grayscale/CoinShares** primary texts could materially change §7.1's characterisation.


---

## 13. References

Full consolidated ledger with tiers and access dates: **[`research/LEDGER.md`](research/LEDGER.md)** — 331 unique sources.

### 13.1 Primary regulatory / government (Tier A — 80)

Selected, load-bearing only.

- **SEC**, Commission Interpretation, *Application of the Federal Securities Laws to Certain Types of Crypto Assets* (Rel. 33-11412, 17 Mar 2026) — https://www.sec.gov/files/33-11412-fact-sheet.pdf · full text https://www.sec.gov/files/rules/interp/2026/33-11412.pdf
- **SEC**, Press Release 2026-30, *SEC Clarifies the Application of Federal Securities Laws to Crypto Assets* — https://www.sec.gov/newsroom/press-releases/2026-30-sec-clarifies-application-federal-securities-laws-crypto-assets
- **SEC**, *Regulation Crypto Assets* (33-11434, Aug 2026) — proposal, not law
- **Congress**, GENIUS Act, Pub. L. 119-27 (stablecoins only)
- **SEC/DOJ/CFTC enforcement**: Ooki DAO (ring-fencing does not defeat "person" status); Roman Storm / OneCoin conviction (Aug 2025, unlicensed MTB); Garantex (Estonian licence lost 2022); Tornado Cash SDN delisting (Mar 2025); Polymarket & Kalshi insider-trading matters
- **ESMA** — white papers in its register are *"not reviewed or approved"*; MiCA register; "Non-compliant entities" file
- **EU** — MiCA (incl. Art. 61: *"notwithstanding any contractual clause"*); EDPB Guidelines 02/2025
- **OFAC** — SDN list; strict-liability framing
- **ASIC** (Block Earner standard: substance over labelling); **FCA Register**; **MAS** Financial Institutions Directory; **FinCEN** MSB Registrant Search; **SEC** IAPD
- **US DOJ / FTC**, *Merger Guidelines* (2023) — HHI > 1,800 **AND** ΔHHI > 100; graduated-rebuttal standard

### 13.2 Primary technical / registry (Tier B — 93)

- **Safe (Gnosis Safe)** v1.3.0 — Transaction Service API; singleton `0xd9Db270c1B5E3Bd161e8c8503c55cEABeE709552`; `singleton()` = `0xa619486e`; ERC-1967 false-negative demonstrated on `0x467947EE34aF926cF1DCac093870f613C96B1E0c`
- **Uniswap Labs**, v2 Technical Whitepaper — the 5-bps admin-key admission — https://blog.uniswap.org/whitepaper.pdf
- **Uniswap Foundation**, Summary FY'2024 Financials — https://gov.uniswap.org/t/uniswap-foundation-summary-fy-2024-financials/25486
- **Uniswap**, Governance Technical Reference — https://developers.uniswap.org/docs/ecosystem/governance/technical-reference
- **OpenZeppelin** — proxy/admin documentation; `TimelockController`; `Ownable`
- **Compound** — Timelock (verified 172,800s delay), GovernorBravo defeat condition
- **OP Stack** — L1 Proxy Admin = 2-of-2 Safe (Optimism Foundation 5/7 + Security Council); Batcher hot-wallet posture
- **Ethereum Foundation** — Annual Report (teams, grant disclosure, Conflicts of Interest Policy)
- **FATF**, Recommendation 16 (Travel Rule) — *"does not accept post facto transmission"*
- **NIST**, AI RMF; **OWASP** LLM Top 10; **ISO/IEC 27001:2022**; **SOC 2**
- Audit reports: CertiK, Trail of Bits, OpenZeppelin, ChainSecurity, Sigma Prime, Halborn, Quantstamp, Least Authority
- **ProPublica Nonprofit Explorer** — Uniswap Foundation, org 883087770
- Post-mortems: Ronin, Nomad, Wormhole, Badger, Harmony, Radiant

### 13.3 Primary measurement (Tier C — 45)

- **CertiK**, *Hack3d: The Web3 Security Report 2024* — $2,362,748,975.83 across 760 incidents; phishing $1,050,129,498/296; key compromise $855,385,570/65; mean $3.11M, median $150,925 — https://www.certik.com/blog/hack3d-the-web3-security-report-2024
- **CertiK**, *Hack3d 2023* — $1.84B/751 incidents; key compromise 6.3% of incidents ≈ half of losses
- **Chainalysis**, *The 2025 Crypto Crime Report* — $40.9B to identified illicit addresses in 2024 (lower bound); illicit share <1% of volume
- **DeFiLlama** — Data Definitions (`Revenue = Fees − Supply-Side Revenue`; emissions are incentives, not fees); **Holders Revenue** rankings; protocol pages
- **Binance Research** — 2024 launch cohort MC/FDV 12.3%; float as low as 6%; ~$155B unlocking 2024–2030; ~$80B buy-side liquidity needed
- **Kani et al.** (ETH Zürich) — voting-rights Gini ~0.99 (Compound, Uniswap); Nakamoto 8/11/18; 17 of 21 DAOs capturable by <10 addresses; delegation amplification 13/18
- **a16z** — 18 Ethereum DAOs, ~250k voters, 1,700 proposals: ~17% power delegated; delegates vote on 33% of proposals
- **Hacken**, Proof of Reserves v3 — random sampling + dummy-account negative control
- Electric Capital, Nansen, IntoTheBlock, Token Terminal, Tokenomist/Liquifi

### 13.4 Established media / research (Tier D — 56)

Named-journalist investigations, academic preprints and law-firm client alerts. Consulted for context and historical pattern; every factual claim drawn from them was corroborated against Tier A–C. **Includes one item used as a load-bearing primary in slice B (B10) that is in fact Tier B by nature** — governance technical documentation, reclassified.

---

## Appendix A — Reference-Site Teardown Summary

Full document: **[`reference/01_reference_site_teardown.md`](reference/01_reference_site_teardown.md)**.
Artifacts: `reference/msawox_questionnaires.json`, `reference/msawox_scoring_engine.json`, `reference/screens/`.

**Method:** Playwright 1.63.0 driving Chromium headless shell 153 against `msawox.com/en/tools`, plus static extraction from the Next.js client bundles. 17 tools, 10 distinct scoring engines, 3 interaction archetypes (weighted questionnaire, checklist, decision tree).

**Engine, as measured:**

- Question schema: `{id, n, category, weight, scale|kind, gateId?, title, citation|prompt|guidance, options?}`
- **Weights sum to exactly 100.** Measured: Ethical Startup = 22 questions `[5,5,4,4,4,4,4,4,6,5,5,4,4,4,4,5,5,4,4,6,5,5]`, 6 pillars, 3 gates; Ethical AI = 22 questions, 7 pillars, 4 gates. Weights are near-uniform 3–6. `weight: 0` exists for non-scoring probes.
- Answer scales: `scale_0_2` = 3 options worth `0 / 2.5 / 5` (**mid = half credit**); `yes_no`.
- Bands: `≥80 ready · ≥60 moderate · ≥40 high · else critical`. Status: `≥80 satisfactory · ≥60 moderate · ≥40 high · else critical`.
- **Gates: 7 total** — `GATE_EST_01..03`, `GATE_EAI_01..04`. A failed gate applies `capOverride` and floors the score: `score = Math.min(score, 49)`. Cap levels used: **39, 49, 59**. UI banner: *"Your raw score (100/100) has been capped at 49/100 due to a critical audit gate breach."*
- Pillar radar = **% of weight earned within that pillar**. Pillar weights declared separately, e.g. DORA `{1:16,2:20,3:20,4:16,5:11,6:17}`, CRA `{1:9,2:24,3:24,4:15,5:20,6:8}`.
- `Weight × Gap Severity` ranking → `ACT-DORA-01…09`, `ACT-GDPR-01…06`, `ACT-NIS2-05…06` in `short/medium/long_term` backlogs.
- Versioned JSON export: `{schemaVersion, toolId, timestamp, rawScore, overallScore, isCapped, capReasons, riskBand, posture, categories[], gaps[], blockers[], actionPlan[], policyMappings[], owaspEntries[], evidence[]}` + Copy-Markdown / HTML / PDF.

**The best idea to keep:** gates that cap the score, so a fatal flaw cannot be averaged away.
**The flaw to fix:** the instrument is **entirely self-report** — which for a fraud-detection tool is the attack surface, not the evidence. Its weights are also near-uniform and its citations include soft trade press.

---

## Appendix B — Research Method and Token Optimization

Full plan: **[`00_token_optimization_plan.md`](00_token_optimization_plan.md)**.

Governing principle: **research breadth is cheap, depth is expensive — buy depth only where a decision depends on it.**

- **Budget:** Discovery 8% · Extraction 52% · Verification 12% · Synthesis 28% (network tokens vs local-only writing tokens).
- **Parallelism:** fan out on *partition*, never on repetition. Filesystem return channel (agents write full findings to `research/NN_*.md` and return only ≤250-word digests) keeps orchestrator context small. Two waves: landscape, then gap-fill on exposed gaps only.
- **Tiering:** Tier E banned as citation ground; usable only to locate a Tier A–D source.
- **Failure handling:** unreachable source → record the gap, never substitute a blog. Two tiers conflicting → cite both, mark `CONTESTED`. Insufficient evidence for a weight → lower its confidence and say so.
- **Cost-aware escalation:** `web_search_exa` → bounded `exa fetch` → `firecrawl_scrape` with schema → `tinyfish` web automation only for genuinely interactive flows. Playwright used surgically, not as a crawler.
- **Evidence ledger** generated by script after the mid-run overwrite incident (§2.4), de-duplicated by URL.

**Toolchain note.** Chromium and the Playwright ffmpeg build were installed via `bunx playwright install chromium --with-deps` for this work. The bundled `ffmpeg-1011` is separate from the system `/usr/bin/ffmpeg` 8.0.1 (libx264, libvpx/vp9, aac) and neither affects the other.

---

## Appendix C — Evidence Ledger

**[`research/LEDGER.md`](research/LEDGER.md)** — 331 unique sources, script-generated, de-duplicated by URL:

| Tier | Count |
|---|---|
| A — primary regulatory/government | 80 (+1 `A*` partial) |
| B — primary technical/registry | 93 |
| C — primary measurement | 70 |
| D — established media/research | 84 |
| E — banned | 3 (0 cited, all quarantined) |

Per-slice evidence files, each with numbered sections, per-question verifiable checks and a sources table:

| File | Slice | Sources | Signals |
|---|---|---|---|
| [`research/A_contract_security.md`](research/A_contract_security.md) | Contract & technical security | 54 | 30 |
| [`research/B_governance_treasury.md`](research/B_governance_treasury.md) | Governance & treasury | 43 | 30 |
| [`research/C_tokenomics_casino.md`](research/C_tokenomics_casino.md) | Tokenomics & casino mechanics | 50 | 32 |
| [`research/D_regulatory_counterparty.md`](research/D_regulatory_counterparty.md) | Regulatory & counterparty | 58 | 25 |
| [`research/E1_empirical_cases.md`](research/E1_empirical_cases.md) | Empirical case studies | 64 | 36 |
| [`research/E2_metric_reliability.md`](research/E2_metric_reliability.md) | Metric reliability & counter-evidence | 18 | 22 |
| [`research/F_data_and_scoring.md`](research/F_data_and_scoring.md) | Data indicators & scoring architecture | 59 | 28 |

**Corrections applied to source material during synthesis** (recorded for audit):
1. `F_data_and_scoring.md` — gate-weight inequality corrected from `w_F < 1 − τ` to **`w_F > 1 − τ`**; numeric conclusion unchanged.
2. `A_contract_security.md` — phishing + key-compromise share of 2024 losses corrected from **86%** to **80.6%**.
3. `A_contract_security.md` — selector `0xa619486e` identified as `singleton()`, not `masterCopy()`.
4. `LEDGER.md` — rebuilt after overwrite; 3 Tier E entries annotated `NOT CITED — quarantined`.
5. `E2_metric_reliability.md` — Cong et al. wash-trading finding sharpened: the **>70%** figure applies to **unregulated** exchanges (the 29 exchanges is the test sample), verified against the *Management Science* abstract.
6. `A_contract_security.md`, `F_data_and_scoring.md` — re-merged after slice E1/E2 landed; register and ledger regenerated (203 signals, 331 sources).

---

*End of report R1-LEGIT.*