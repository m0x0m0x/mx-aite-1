# Crypto Legitimacy Tool — Development Specification

### Single-file build spec handed to implementation

**Document ID:** DEV-SPEC-R1
**Date:** 2026-10-06
**Status:** Ready for development
**Parent research:** [`../01_crypto_legitimacy_research_report.md`](../01_crypto_legitimacy_research_report.md)
**Evidence base:** [`../SIGNAL_REGISTER.md`](../SIGNAL_REGISTER.md) (145 signals), [`../research/LEDGER.md`](../research/LEDGER.md) (298 sources)
**Primary design reference:** <https://www.msawox.com/en/tools/islamic-startup-readiness> — the **Islamic Startup Readiness Suite** (layout, tab component, sidebar, card and live-score panel are to be matched verbatim; see §9 and §11.5)
**Intended implementer:** local LLM coding agent
**Scope:** ONE tool with a tabbed result view — not a suite of tools (see [§1.0](#10-this-is-one-tool), [§9](#9-information-architecture-and-navigation))

---

## Table of Contents

**Part I — What we're building**
1. [Product definition](#1-product-definition)
2. [Target user: not a crypto expert](#2-target-user-not-a-crypto-expert)
3. [Personas and core journeys](#3-personas-and-core-journeys)
4. [Design principles](#4-design-principles)

**Part II — How it works**
5. [Methodology: the evidence pipeline](#5-methodology-the-evidence-pipeline)
6. [Scoring engine: gates, evidence states, no composite score](#6-scoring-engine-gates-evidence-states-no-composite-score)
7. [Pillar definitions](#7-pillar-definitions)
8. [Gate register](#8-gate-register)

**Part III — Build it**
9. [Information architecture and navigation](#9-information-architecture-and-navigation)
10. [User stories and acceptance criteria](#10-user-stories-and-acceptance-criteria)
11. [Visual design system](#11-visual-design-system)
12. [Radar chart specification](#12-radar-chart-specification)
13. [Plain-language glossary](#13-plain-language-glossary)
14. [Data sources and API integration](#14-data-sources-and-api-integration)
15. [Export contracts](#15-export-contracts)
16. [Privacy, security and non-functional requirements](#16-privacy-security-and-non-functional-requirements)

**Part IV — Ship it**
17. [Traceability matrix](#17-traceability-matrix)
18. [Build order and definition of done](#18-build-order-and-definition-of-done)
19. [Anti-requirements](#19-anti-requirements)

---

# Part I — What we're building

## 1. Product definition

### 1.0 This is ONE tool

**Deliver a single, unified tool — not a suite of separate tools or calculators.**

Like the reference site, it is **one product with a tabbed interface**: the tabs are *views onto the same assessment*, not separate tools with separate scores. A user picks a project once and moves between tabs of that one assessment. There is exactly **one verdict** and **one evidence set** for that project, visible from every tab.

The EPICs in [§10](#10-user-stories-and-acceptance-criteria) are **workstreams inside that one tool**, not separate tools. "EPIC 5 — Radar" means the radar *tab of the single tool*, not a radar product.

| Not this | This |
|---|---|
| 12 separate tools, each with its own score | One tool; tabs are views of one assessment |
| A quiz that yields a number, plus a chart, plus a list | One verdict surfaced consistently across every tab |
| A "trust score calculator" and separately a "security checker" | A single evidence model; each tab filters or presents it differently |

### 1.1 One-line description

A free web tool that takes a crypto project and tells an everyday investor **whether the evidence suggests it is a real, usable product — or whether its token exists mainly to attract buyers** — and shows its work.

### 1.2 The problem it solves

A non-expert cannot distinguish a functioning protocol from a well-marketed speculation scheme. Existing "trust scores" (CoinGecko, CryptoRank) are **unvalidated against outcomes** (see [§6.5](#65-why-we-do-not-ship-a-composite-score)), rank a token inside its own peer group rather than judging it, and present a single number that implies far more precision than exists.

### 1.3 What it is not

| Non-goal | Why |
|---|---|
| A price predictor or investment recommendation | We assess evidence, not future returns |
| An audit tool | We check whether audits exist and are scoped; we do not perform one |
| A legal opinion | Regulatory analysis is explicitly `CONTESTED` where unsettled ([report §7.1](../01_crypto_legitimacy_research_report.md)) |
| A due-diligence substitute for professionals | Standing disclaimer; it is a triage aid |
| A token price screener | Deliberately excluded — see [§19](#19-anti-requirements) |

### 1.4 Core product promise

> **Every statement we make links to the artifact we checked, and we tell you what we could not check.**

---

## 2. Target user: not a crypto expert

This is the **single most important constraint** in this document. The user does not know what a multisig, an audit, a timelock, HHI, or market cap/FDV is. They may have heard of "wallet" and "token". They are deciding where to put money they cannot afford to lose.

### 2.1 Consequences for design — binding rules

| # | Rule | Rationale |
|---|---|---|
| **R1** | **No unexplained jargon in primary copy.** Every technical term in visible text must be a glossary link on first use, with a plain-English definition. | Otherwise the tool confirms the user's ignorance instead of fixing it |
| **R2** | **Lead with a verdict in everyday words**, not a number. Primary verdict is a phrase + colour, e.g. *"Mixed — some real signs, some serious gaps."* | A non-expert cannot interpret 67/100, and the number implies precision we don't have |
| **R3** | **Every finding carries "What this means for you"** — one plain sentence connecting the finding to the user's decision. | Technical correctness without decision relevance is useless to this user |
| **R4** | **Show the evidence state as words, not shades.** `Verified` / `Unclear` / `Contradicted` / `Not enough information` — with icons, never colour alone. | Colour-only encoding fails ~8% of men; words also survive print and greyscale |
| **R5** | **Progressive disclosure.** Plain summary first; technical detail behind a labelled expand. | Default view must be readable in 30 seconds |
| **R6** | **Always state what we could NOT find.** "We could not verify X" is a first-class result, never hidden or shown as zero. | A tool that hides uncertainty will confidently mislead |
| **R7** | **Explain *why* a check matters** before showing its result — never a bare pass/fail. | An unexplained green tick is unfalsifiable to a lay reader |
| **R8** | **No false precision.** Never show a composite 0–100 score as the headline. Percentages only per-dimension, always paired with evidence state. | Out-of-sample discrimination collapses from 0.98 to ~0.6 for composite scores ([§6.5](#65-why-we-do-not-ship-a-composite-score)) |
| **R9** | **Warn before the verdict.** First run shows a short explainer: what the tool does, can't do, and that absence of evidence ≠ evidence of absence. | Prevents misreading the tool's first impression |
| **R10** | **Never use "safe", "secure", "legit", "scam", or "fraud" as a verdict word.** Use evidence-framed language. | These are legal/financial claims the tool cannot support |

### 2.2 Vocabulary register

**Use:** project · token · wallet address · evidence · verified · unclear · contradicted · not enough information · serious gap · red flag · blocker

**Avoid, or define on first use:** fully diluted valuation (FDV) · market cap · liquidity pool · total value locked (TVL) · emissions · staking yield · multisig · timelock · oracle · re-entrancy · rug pull · honeypot · APY · HHI · Nakamoto coefficient · decentralization score

---

## 3. Personas and core journeys

### 3.1 Persona A — "Priya", retail saver

45, has £8,000 to invest, saw a token on social media advertising 40× returns. Understands banking apps, not blockchains. **Needs:** to avoid obvious frauds. **Abandons if:** jargon-heavy or if the verdict isn't stated plainly in the first screen.

### 3.2 Persona B — "Tom", tech-curious professional

32, software engineer, owns some crypto, evaluating whether to add to a position. Reads technical content comfortably but isn't a security auditor. **Needs:** to see *why*, drill into evidence, export for further reading.

### 3.3 Persona C — "Dr. Osei", nonprofit treasurer

58, considering a crypto donation or grant. Needs defensible due diligence with citations for a board. **Needs:** exportable evidence trail, licence/entity verification.

### 3.4 Primary journey

```
LAND  →  explainer (what we can/cannot check)
      →  search & select project
      →  intake: confirm identity (chain, contract, entity)
      →  RUN CHECKS  (progress: "Checking 7 of 23 evidence items…")
      →  THE TOOL OPENS — one assessment, five tabs:
             [ Result ]  plain verdict + summary + "what we couldn't verify"
             [ Red flags ]  blockers, severity-ordered  (tab 2 auto-opens if any)
             [ Pillars ]  radar + per-pillar evidence state + coverage
             [ Evidence ]  every check, with "what this means for you" + source
             [ What would change this ]  disconfirming questions
      →  EXPORT / SHARE (from any tab)
```

### 3.5 Two alternative entry points

- **From an exchange listing:** URL param `?contract=0x…&chain=ethereum` prefills intake.
- **From a whitepaper:** `?project=<slug>` matches a known project and prefills contracts + sources.

---

## 4. Design principles

1. **Show your work, always.** Every finding links to an artifact and a re-runnable check.
2. **The tool must be able to say "I don't know."** `Not enough information` is a valid, prominent outcome.
3. **Serious problems can't be averaged away.** Blockers cap the verdict; they never offset against strengths.
4. **Design for the adversarial reader.** Assume a scammer is reading this spec and optimising against it.
5. **Lay the trap openly.** Copy must make clear that a good score requires *verifiable* evidence, so gaming is self-defeating.
6. **The radar is a summary, not a verdict.** Always captioned as such.
7. **Reuse the reference site's proven interaction patterns**, but never its self-report epistemology.

---

# Part II — How it works

## 5. Methodology: the evidence pipeline

```
┌──────────────────────────────────────────────────────────────┐
│ 1. INTAKE          project identity, chains, contracts, entity │
└───────────────────────────┬──────────────────────────────────┘
                            ▼
┌──────────────────────────────────────────────────────────────┐
│ 2. COLLECT         chain · verified source · registries ·     │
│                    audits · wallets · metrics · documents      │
│                    → each item carries: artifact, source,      │
│                      producer, date, re-runnable check ID       │
└───────────────────────────┬──────────────────────────────────┘
                            ▼
┌──────────────────────────────────────────────────────────────┐
│ 3. RESOLVE STATE   VERIFIED │ CLAIMED │ CONTRADICTED │         │
│                    UNKNOWN / NOT-APPLICABLE                    │
└───────────────────────────┬──────────────────────────────────┘
                            ▼
┌──────────────────────────────────────────────────────────────┐
│ 4. GATES           pre-registered, severity-ordered.          │
│                    Fired FIRST. Cannot be offset.             │
│                    Override requires stronger evidence.       │
└──────────┬───────────────────────────────────┬───────────────┘
   none fired                                ≥1 fired
           │                                   │
           ▼                                   ▼
┌──────────────────────────────┐   ┌──────────────────────────┐
│ 5a. PILLAR ASSESSMENT       │   │ 5b. BLOCKED VERDICT      │
│  per-pillar evidence state   │   │  verdict = blocked band   │
│  + earned percentage         │   │  cap applied              │
│  radar = visual summary      │   │  no overall percentage    │
└──────────┬───────────────────┘   └──────────┬───────────────┘
           └───────────────┬──────────────────┘
                           ▼
┌──────────────────────────────────────────────────────────────┐
│ 6. OUTPUT  verdict phrase · blockers · per-item evidence ·    │
│            "what would change this" · "what we couldn't      │
│            verify" · base rates · radar · exports             │
└──────────────────────────────────────────────────────────────┘
```

---

## 6. Scoring engine: gates, evidence states, no composite score

### 6.1 Evidence states — the foundation

Every check resolves to exactly one of four states. **This is the core abstraction.**

| State | Meaning | Credit | UI label | Icon |
|---|---|---|---|---|
| `VERIFIED` | We independently confirmed it from an artifact | full | **Verified** | ✔ |
| `CLAIMED` | The project asserts it; we found no independent confirmation | partial, flagged | **Claimed, not confirmed** | ◐ |
| `CONTRADICTED` | Artifact evidence contradicts the claim | zero + may raise blocker | **Contradicted** | ✖ |
| `UNKNOWN` | Not checkable with available sources | **zero and excluded from denominator** | **Not enough information** | ? |

> **`UNKNOWN ≠ 0`.** Unknown is excluded from the pillar denominator, and the pillar displays its coverage (`"based on 9 of 14 checks"`). This is the primary control on false positives for young projects ([report §3.3](../01_crypto_legitimacy_research_report.md)).

### 6.2 Gate evaluation — the veto layer

Gates run **before** any aggregation and **cannot be offset** by strong pillars.

**The mathematics that forces this design:** with dimensions normalised to [0,1] and score `S = Σwᵢxᵢ`, if a fatal dimension scores 0 then `S_max = 1 − w_F`. Failure is possible only when `w_F > 1 − τ`. At a 0.7 pass threshold, **no weight ≤ 0.30 can force a failure**. Therefore fatal findings must be implemented as gates, never as weights. *(Full derivation: [report §9.1](../01_crypto_legitimacy_research_report.md).)*

### 6.3 Gate rules

| Rule | Requirement |
|---|---|
| **G-R1** | Every gate fires only from an **observed fact class**, never from a questionnaire answer |
| **G-R2** | Every gate fire must record the **artifact** and the **check ID** that triggered it |
| **G-R3** | Gates are **pre-registered and versioned**. The register is published before scoring; changing it requires a new `toolId` version |
| **G-R4** | An **override requires stronger evidence than default** (DOJ/FTC graduated-rebuttal analogue) and is logged with its justification |
| **G-R5** | A gate can be **overridden**, never silently ignored |
| **G-R6** | Gates are ordered by **severity**, not by frequency |
| **G-R7** | Probing that fails to determine a value yields `UNKNOWN`, **never `absent`** |

> **G-R7 is a hard requirement, not a nicety.** Generic ABI probing against a custom access-control contract reverts, which looks identical to absence. Real case: a major lending protocol's admin role resolves to a bespoke executor that no generic probe recognises. Collapsing "unprobeable" into "absent" would produce a false serious-red-flag. The check result enum is `FOUND | NOT_FOUND | UNPROBEABLE`.

### 6.4 Pillar computation

```
pillar_earned    = Σ (weight_i × credit_i)   for i where state ≠ UNKNOWN
pillar_possible  = Σ weight_i               for i where state ≠ UNKNOWN
pillar_pct       = round(100 × earned / possible)     // only if possible ≥ MIN_WEIGHT
pillar_coverage  = count(state ≠ UNKNOWN) / count(all checks in pillar)
```

- If `pillar_possible < MIN_WEIGHT`, emit `NOT_ENOUGH_EVIDENCE` — do **not** display a percentage.
- `CLAIMED` earns `CLAIM_CREDIT = 0.5 × weight`.
- Never aggregate pillars into a single scalar. **See 6.5.**

### 6.5 Why we do NOT ship a composite score

This is a deliberate, evidence-driven rejection of the obvious design.

| Evidence | Consequence |
|---|---|
| The only survival model in the literature scores **0.98 in-sample and degrades to 0.59–0.65 out of sample**, and omits all off-chain features | A composite weight vector tuned to known cases will not generalise |
| **No published trust/legitimacy score has ever been validated against outcomes** | Precedent offers no safety net |
| CoinGecko's Trust Score is **rank-relative** (curve-graded across the population) and allocates **50% of its weight to liquidity** | A rank inside a bad cohort is not a verdict on quality |
| Wash trading averaged **>70% of reported volume on unregulated exchanges**, and fabricated volumes specifically **improve published rankings** | Volume/TVL inputs to a composite are partly fabricable, and gaming them pays |

> **We therefore output: gates (blockers) + per-pillar evidence panels + a visual summary — and no overall numeric score.**
> A single headline number would be the most quotable and least honest thing we could build.

### 6.6 Two metrics are inverted from reward to penalty

Evidence, not intuition, drove this:

| Metric | Naive treatment | **Correct treatment** | Why |
|---|---|---|---|
| **Trading volume** | Reward high volume | **Penalty** for wash-volume exposure | Most thoroughly falsified metric in the research; fabrication improves rankings |
| **Audit count** | Reward many audits | **Penalty** for unaudited-and-unverified deploys | Key compromise, phishing and social engineering are ~49.6% of losses yet a negligible share of audit findings; <2% of flagged contracts are ever exploited |

**Implementation:** these are *not* positive contributors. High volume with failed wash screening reduces confidence in other claims; absence of any traceable audit raises a penalty. Render them in the evidence panel as **risk** items, never as achievements.

### 6.7 Base rates must be shown

Of 1,244 protocols in a research census, ~400 clear $1M in annual fees but only **~20 (≈1.6%)** pass $10M through to token holders. Therefore:

- **Absence of a holder-accrual mechanism is normal**, so its absence is a *weak* negative for a young project and a *strong* negative for a mature one.
- Any high score is weak evidence in a population where ~98% of projects do not accrue to holders.
- The UI must state this context where a verdict depends on it.

### 6.8 Verdict bands

| Band | Phrase shown to user | Meaning |
|---|---|---|
| `BLOCKED` | **"Serious red flags found"** | ≥1 gate fired |
| `STRONG` | **"Good evidence of a real, usable product"** | No blockers; ≥4 pillars `SUPPORTED` |
| `MIXED` | **"Some real signs, but important gaps"** | No blockers; 2–3 pillars supported |
| `WEAK` | **"Little evidence of real use"** | No blockers; ≤1 pillar supported |
| `INSUFFICIENT` | **"Not enough information to judge"** | Coverage below threshold across pillars |

---

## 7. Pillar definitions

Five pillars. Names are plain English; the technical scope sits behind disclosure.

### P1 — Security & Technical Integrity

**User-facing name:** "Can the code be trusted?"
**What we check:** whether contracts are verified and match published source; whether audits exist, are recent, and are tied to a specific code version; whether an owner or admin can change or pause the system and who holds that power; multisig configuration; known compiler/library vulnerabilities; oracle and price-manipulation exposure; bridge trust model; whether security claims are consistent with the code.

**Key non-obvious checks (must implement):**
- Detect Safe-style proxies **without** relying on the ERC-1967 slot. `GnosisSafeProxy` stores its singleton at **storage slot 0**, and its ERC-1967 slot reads zero. *(Verified on mainnet.)*
- Read `guard` and `modules`; a non-zero guard or any module is a **bypass path**.
- Compute **keys-to-lose-control = (owners − threshold) + 1** *and* **keys-to-act-alone = threshold**.
- **Key-management composite (`KEY-COMPOSITE`):** (a) maximum keys held by one operator vs the signing threshold; (b) non-revoked temporary access, allowlists and gas-free RPC endpoints; (c) single-key ownership of a nominal multisig. *(Ronin: one operator held 4 of 9 against a 5-of-9 threshold, and a gas-free RPC added in Nov 2021 was never revoked — the fifth signature was reachable.)*
- **Governance-body-as-counterparty (`GOV-EXTORTION`):** flag any governance or multisig body that has been the target of an exploiter's negotiation demand. Treat as a governance-integrity red flag, because it evidences that the body is reachable and disposable. *(KyberSwap: the exploiter messaged the KyberDAO multisig demanding protocol and DAO control in exchange for returning 50%.)*
- Determine whether a control multisig controls anything material — **do not** infer importance from token balance. (Verified case: a Safe holding zero tokens holds irreversible pause authority.)

### P2 — Governance & Treasury Integrity

**User-facing name:** "Who really controls it?"
**What we check:** whether voting control is concentrated; whether delegated voting power amplifies or dilutes concentration; whether the treasury is transparent and whether it holds mostly its own token (a circular balance sheet); whether emergency powers are scoped narrowly; whether vesting promises match on-chain reality; whether a legal entity and jurisdiction are identifiable.

**Key non-obvious checks:**
- Nakamoto-style measure: fewest addresses holding >50% of voting power.
- **Delegation amplification ratio = voting-concentration ÷ holding-concentration.** >1 means delegation is *concentrating* control.
- Distinguish **pause-only** emergency roles from **chain-owner** roles.
- Score vesting by **recipient**, not merely by schedule.

### P3 — Token Economics & Value Capture

**User-facing name:** "Does using it pay the token's holders?"
**What we check:** whether fees exist independent of subsidies; whether any of those fees reach token holders (buyback, burn, direct distribution); emission-driven yield; circulating vs total supply; insider/team allocation actually released; concentration of realised profit; buyback announcements versus on-chain execution.

**Anchor check:** does a verifiable holder-accrual mechanism exist, and when did it switch on? A dated "off before DATE" is strong evidence that past valuation was not fundamental.

**Pre-loss observable checks (implement as P0 — these were computable *before* each failure):**
- **`PEG-REDEMPTION` — is the stated backing redeemable by the holder?** Test for a live holder-facing redemption path against the claimed reserve. *(Terra / UST: LFG held ~80,000 BTC but no redemption module ever shipped. A large reserve with no path to it is not backing.)*
- **`STAB-ARB-ASYMMETRY` — simulate the arbitrage that defends the peg under *falling* collateral.** If the stabiliser's pricing source lags the market, the arbitrage that rescues a rising peg becomes unprofitable as it falls. *(Iron Finance: a 60-minute TWAP priced against a real-time AMM.)*
- **`YIELD-VS-CARRY` — is the advertised yield actually paid out of fees earned?** Compare reward payouts against non-emission fee income.

**Presentation note:** explain in plain terms that *protocol revenue* (money the system earns) is not the same as *holder revenue* (money that reaches token owners). Most projects have the first and not the second. Also state that **a fee-to-valuation ratio cannot discriminate**: EigenLayer and Chainlink have near-identical ratios and opposite verdicts, so never threshold on it.

### P4 — Regulatory & Legal Status

**User-facing name:** "Is it legal and properly registered?"
**What we check:** whether a named legal entity exists in an official registry and is active; whether it holds the *specific* authorisation it claims, in the *matching* regime; token classification; sanctions exposure of the operator and counterparties; AML/sanctions programme evidence; custody arrangement and who bears liability; whether compliance claims are register-backed, self-attested, or marketing only.

**Binding presentation rules:**
- Where the law is unsettled, render **`CONTESTED`** with both positions. **Never** resolve it into a penalty.
- Distinguish four states explicitly: *unlicensed but legal · unlicensed and illegal · licensed · sanctioned*.
- Never say a white paper is "approved" because it appears in a register. Registers list filings, not approvals.

### P5 — Real Use-Case Evidence

**User-facing name:** "Is it actually being used?"
**What we check:** organic fee generation net of incentives; repeat/returning users; external integrations; usage sustained through a full market cycle; subsidy dependence; whether users are transacting for a reason other than the next emission.

**Presentation note:** this pillar must distinguish *speculative volume* from *usage*. Present the comparison explicitly: "usage that continued when rewards stopped" is the signal.

---

## 8. Gate register

Version `gates-v1`. Pre-registered. Ordered by severity.

| Gate ID | Condition | Cap | Plain-language blocker headline |
|---|---|---|---|
| `G-SANCTION` | Named operator or controlling counterparty matches a sanctions list entry | BLOCKED | "This project is connected to a sanctioned party" |
| `G-HONEYPOT` | Contract permits insider sale but blocks user sale, or transfer restrictions contradict marketing | BLOCKED | "The token can be sold by insiders but not by you" |
| `G-REG-UNLICENSED` | Operates an activity requiring authorisation under a regime it is demonstrably inside, with no matching authorisation on the relevant register | BLOCKED | "It appears to offer a regulated service without the required licence" |
| `G-NOVERIFY` | No verifiable source for code that holds or can move user funds, and no audit traceable to a specific code version | CAPPED | "The code that would handle your money cannot be independently checked" |
| `G-ADMIN-SINGLEKEY` | Terminal admin resolves to a single externally-owned address, **or** a multisig whose keys-to-lose-control ≤ 1 with non-empty guard/modules | CAPPED | "One person or key appears able to take control" |
| `G-NOACCRUAL` | Verified absence of any holder-accrual mechanism **while the project markets token utility to users** | CAPPED | "The token does not earn anything for its holders" |
| `G-REFLEXIVE` | Realised-profit distribution consistent with a reflexive loop, with no holder-accrual mechanism | CAPPED | "Profitable traders appear to be mostly the project and its early insiders" |

**Override requirements:** overriding `G-SANCTION` or `G-HONEYPOT` requires documented legal/technical evidence of the false positive. Overriding any cap requires evidence **stronger** than the default (G-R4). Every override is exported with its justification.

**Version control:** changing this register requires a new `toolId` (e.g. `CryptoLegitimacy_v2`) and a changelog entry visible in the UI.

---

# Part III — Build it

## 9. Information architecture and navigation

### 9.1 Design authority

**Primary visual and interaction reference: the `Islamic Startup Readiness Suite`**
(`https://www.msawox.com/en/tools/islamic-startup-readiness`) — captured in
[`../reference/screens/islamic_suite.png`](../reference/screens/islamic_suite.png).

**Match its layout, tab component, sidebar, question/evidence card, and live-score panel exactly.** Do not invent a different information architecture. The only permitted departures are the ones in [§9.6](#96-permitted-departures-from-the-reference), each of which exists to fix a correctness or fairness defect.

### 9.2 Page anatomy (top to bottom)

Reproduce this order exactly.

```
┌───────────────────────────────────────────────────────────────────────┐
│ HEADER  logo · nav · [Free Tools] [GET] · Back to home                │
│         "Time on this tool: 00:19"        [Share]                    │
│         disclaimer line (always visible)                               │
├───────────────────────────────────────────────────────────────────────┤
│ EYEBROW   FREE STRATEGIC TOOLS                                        │
│ H1        Crypto Legitimacy Check                                     │
│ SUB       <one line: what it assesses · N checks · client-side>       │
├───────────────────────────────────────────────────────────────────────┤
│ EYEBROW   CRYPTO LEGITIMACY CHECK                                     │
│ TRUST     100% CLIENT-SIDE · ZERO SIGNUP · NO FINGERPRINTING         │
│ ┌──────────────┬──────────────┬──────────────┬──────────────┐        │
│ │  SECURITY    │ GOVERNANCE   │ TOKEN        │  LEGAL &     │        │
│ │              │              │ ECONOMICS    │  REGULATORY  │        │
│ ├──────────────┴──────────────┼──────────────┴──────────────┤        │
│ │                            │  UNIFIED VERDICT             │        │
│ └─────────────────────────────┴─────────────────────────────┘        │
│ // <active-track one-line description>                                │
│ <track meta: N checks · Gate 0–1 · 100% client-side>                 │
├──────────────────────────────────────┬───────────────────────────────┤
│  MAIN  (minmax(0,1fr))               │  SIDEBAR  280px               │
│                                      │  PILLARS & PROGRESS           │
│  ┌────────────────────────────────┐  │   ● Security        62%       │
│  │ SECTION LABEL ·                │  │   ● Governance      41%       │
│  │ Check 04 / 23 · [RED LINE] 5pt │  │   ● Token Economics  0%       │
│  │ Finding title                  │  │   ● Legal           0%       │
│  │ Finding prompt                 │  │   ● Real Use        0%       │
│  │ ── HOW WE CHECKED ── ▼         │  │                               │
│  │ [VERIFIED      ] evidence …    │  │  PROGRESS  4 of 23            │
│  │ [CLAIMED       ] …             │  │                               │
│  │ [CONTRADICTED  ] …             │  │  LIVE SCORE                   │
│  │ [NOT ENOUGH INFO] …            │  │   62 / 100   MIXED            │
│  │ ── gate note (if gated) ──     │  │   Security Governance …       │
│  │ Back   Next   ↑ Jump to first  │  │                               │
│  │ unresolved — C04 · Start over  │  │                               │
│  └────────────────────────────────┘  │                               │
└──────────────────────────────────────┴───────────────────────────────┘
```

Column grid is `grid-cols-[minmax(0,1fr)_280px]`. Outer metrics grid is `grid-cols-4` (`grid-cols-2` below `md`).

### 9.3 The tab group — copy the reference exactly

Five pill tabs in a row. This is the single most important visual element to match.

**Container:** eyebrow label above (`CRYPTO LEGITIMACY CHECK`), trust line below
(`100% CLIENT-SIDE · ZERO SIGNUP · NO FINGERPRINTING`).

**Tab button base class (verbatim from the reference):**

```html
class="flex min-w-0 w-full items-center justify-center gap-2 rounded-xl border px-3 py-2
       text-center text-xs font-mono uppercase leading-snug tracking-[0.12em]
       transition-all sm:w-auto"
```

**Active state (verbatim):**
```html
class="… border-secondary/60 bg-secondary/15 text-secondary shadow-[0_0_15px_rgba(secondary,0.18)]"
```

**Inactive state (verbatim):**
```html
class="… border-border/60 bg-card/40 text-muted-foreground hover:border-secondary/40 hover:text-foreground"
```

**Tab set — our five tracks mapped onto the reference's four-plus-Unified-View pattern:**

| Tab | Purpose |
|---|---|
| `SECURITY` | Contract, admin control, audits, oracle/bridge exposure |
| `GOVERNANCE` | Control concentration, treasury, vesting reality |
| `TOKEN ECONOMICS` | Fees, holder accrual, supply, concentration |
| `LEGAL & REGULATORY` | Entity, registers, token status, sanctions |
| `UNIFIED VERDICT` | The reference's *"Unified View"* — verdict, blockers, radar, all evidence |

**Tab rules:**
- `UNIFIED VERDICT` is the default landing tab after checks complete, matching the reference's Unified View.
- Tabs are **views of one assessment**, never separate tools or separate scores. One verdict exists and is identical everywhere.
- `sm:w-auto` ⇒ full-width stacked on mobile, auto-width row from `sm` up.
- Tab switches are instant, client-side, and never re-run checks.
- Deep link: `?project=X&tab=governance`.

### 9.4 Sidebar — `PILLARS & PROGRESS` (verbatim pattern)

**Pillar row = rounded-full chip button (verbatim from the reference's track chips):**

```html
<!-- active -->
class="inline-flex items-center gap-1.5 rounded-full border px-3 py-1 font-mono text-[11px]
       uppercase tracking-[0.12em] transition-colors
       border-secondary/60 bg-secondary/10 text-secondary font-semibold"

<!-- inactive -->
class="inline-flex items-center gap-1.5 rounded-full border px-3 py-1 font-mono text-[11px]
       uppercase tracking-[0.12em] transition-colors
       border-border/60 text-muted-foreground hover:border-secondary/40 hover:text-foreground"
```

Rendered as: `● A ACTIVITY 0%` in the reference → `● SECURITY 62%` for us. Clicking a chip switches to that pillar's tab.

Below the chips: `PROGRESS   4 of 23` in mono. Below that, the **live panel**:

```
LIVE SCORE
 62
/ 100
MIXED
Security Governance Token Economics …
```

Live-updating as checks resolve. If our no-composite-score rule ([§6.5](#65-why-we-do-not-ship-a-composite-score)) is adopted, this panel shows the **band phrase** and the count of resolved checks instead of a numerator — see [§9.6](#96-permitted-departures-from-the-reference) D1.

### 9.5 Evidence card (the reference's question card, adapted)

The reference shows a question with annotated point values. We show a **check** with annotated evidence states.

```
<TRACK SECTION LABEL>                              ·
Check 04 / 23              [ RED LINE ]   5 pts
<finding title>
<finding prompt>
────── HOW WE CHECKED ──────                          ▼
[ ✔ VERIFIED          ]  <observed value · method · artifact · timestamp>
[ ◐ CLAIMED           ]  <claimed by the project, not confirmed by us>
[ ✖ CONTRADICTED      ]  <claim vs contradicting artifact, both shown>
[ ? NOT ENOUGH INFO   ]  <why it could not be determined>
[ —  NOT APPLICABLE   ]  <why it does not apply to this project>

This check is a gate. <what a definitive answer requires>
Back        Next        ↑ Jump to first unresolved — C04        Start over
```

**Copy these reference behaviours verbatim:**
- `← Back` / `Next` navigation, plus `↑ Jump to first unresolved — C04` (`self-start text-xs font-mono text-secondary hover:underline`)
- `Start over`
- `View assessment results` appears once all checks are resolved
- The `▼` expander on the citation/evidence strip
- `5 pts` weight display beside the gate badge
- The gate note line under the options

**Directly ported from the reference's answer set** — it already anticipates our `UNKNOWN` problem with a `[—] Not applicable yet` option, and its red-line gate note says *"This red-line question requires a definitive stance."* Keep both mechanisms.

### 9.6 Permitted departures from the reference

Everything else must match. These six are deliberate, each for a stated reason:

| # | Departure | Reason |
|---|---|---|
| **D1** | Live panel shows **band phrase + `n of m` checks** instead of `n / 100` | No validated composite score exists; a headline number would be false precision ([§6.5](#65-why-we-do-not-ship-a-composite-score)) |
| **D2** | `NOT ENOUGH INFO` and `NOT APPLICABLE` are distinct states, and a check in either state is **excluded from the denominator** | Prevents young projects being scored as failures |
| **D3** | Radar's low-coverage axis renders **dashed with `?`**, never at zero | Unknown ≠ bad |
| **D4** | Gate badge text is plain English (`RED FLAG`, `BLOCKER`) rather than internal ids | Non-expert audience |
| **D5** | **No visitor IP, geolocation, or fingerprinting** | The reference displays these; a fraud-investigation tool must not |
| **D6** | Radars and pillar cards carry a visible *"summary, not a verdict"* caption | Prevents the chart being read as the conclusion |

### 9.7 Supporting pages

`/method` (methodology + full gate register + limitations) · `/glossary` · `/privacy` · `/changelog` (engine + gate-register versions) · `/legal`.

These support the one tool; they are not tools themselves. No account anywhere.

## 10. User stories and acceptance criteria

**Format:** `US-<EPIC>-<n>`. Every AC is Given/When/Then and testable. **P0** = must ship, **P1** = should, **P2** = nice to have.

> **These EPICs are workstreams inside the single tool.** They are not separate tools or separate scores. EPIC 5 delivers the *Pillars tab*; EPIC 3 delivers *Red flags*; EPIC 6 delivers the *Evidence tab*. All of them read from one run and one verdict.

---

### EPIC 0 — Foundations and design system

**US-0-1 (P0)** — As a developer, I want a single design-token file so that all UI is consistent.
- **AC-0-1.1** Given the token file, when any component renders, then colour, spacing, radius and type scale come only from tokens — no hard-coded hex or px values in components.
- **AC-0-1.2** Given a light and dark scheme, when the user switches, then all four evidence-state colours meet **WCAG AA (4.5:1)** contrast in both, and are **distinguishable by icon and text label without colour** (rule R4).
- **AC-0-1.3** Given `prefers-reduced-motion: reduce`, when any component animates, then transitions are suppressed or ≤1ms.
- **AC-0-1.4** Given a keyboard-only user, when tabbing through `/assess`, then all interactive elements are reachable in logical order with a visible focus ring.

**US-0-2 (P0)** — As a developer, I want a glossary term component so jargon is never unexplained.
- **AC-0-2.1** Given any technical term in visible copy, when rendered, then it links to its `/glossary#id` entry.
- **AC-0-2.2** Given a term with no glossary entry yet, when CI runs, then the build **fails** listing the unmapped terms. *(Enforces rule R1.)*
- **AC-0-2.3** Given a glossary link, when activated by keyboard or click, then the definition is reachable **without leaving the page** (inline expand or focus-trapped dialog).

**US-0-3 (P0)** — As a user, I want to know this tool's limits before I trust it.
- **AC-0-3.1** Given a first-time visitor on `/`, when the page loads, then an explainer states: what we check, what we cannot check, and that missing evidence is not evidence of wrongdoing.
- **AC-0-3.2** Given the explainer, when the user dismisses it, then the choice persists for the session and the user lands on project search.

---

### EPIC 1 — Project intake

**US-1-1 (P0)** — As a non-expert user, I want to find a project by typing its name, so I don't need addresses.
- **AC-1-1.1** Given a query ≥2 characters, when typing, then suggestions appear within 300ms from a name/symbol/ticker index.
- **AC-1-1.2** Given a project with multiple chains, when selected, then the user is shown plain chain names ("Ethereum mainnet") with a one-line purpose, not chain IDs alone.
- **AC-1-1.3** Given a search that matches nothing, when submitted, then the UI says *"We couldn't find that project — try the token's website address or contract address"* and offers a manual-entry form. It must **not** suggest the project is illegitimate.

**US-1-2 (P0)** — As a user, I want to confirm I've picked the right project, so I don't get a result for a different token.
- **AC-1-2.1** Given a selection, when the intake screen renders, then it shows name, ticker, current price, market cap and the contract address, each with a "**Is this right?**" confirm control.
- **AC-1-2.2** Given a contract address pasted from an exchange listing, when entered, then the tool resolves the project and pre-fills intake, and states *"Found: <name> (<ticker>) — confirm this is the token you mean."*

**US-1-3 (P1)** — As a user, I want the tool to ask me to confirm identity facts it cannot verify, so my report is about the right entity.
- **AC-1-3.1** Given an unverified legal entity, when intake runs, then the user is asked for the entity's registered name and country, and the answer is stored as `CLAIMED` — never as `VERIFIED`.
- **AC-1-3.2** Given the user skips all optional intake, when checks run, then the assessment completes and the report states which checks were skipped because of it.

---

### EPIC 2 — Evidence collection

**US-2-1 (P0)** — As a user, I want to see checks running, so I know it's working and how far along.
- **AC-2-1.1** Given a run in progress, when the page renders, then a live progress indicator shows `Checking N of M evidence items` with the **current check named in plain English** ("Now checking: who can change the smart contracts").
- **AC-2-1.2** Given a slow check exceeding 10s, when it resolves, then the UI does not block and shows which check is slow.
- **AC-2-1.3** Given a check that fails to determine a value, when it completes, then it records `UNKNOWN` and the run continues. **The run must never abort on a single check failure.**

**US-2-2 (P0)** — As a developer, I want every check to record its provenance, so results are auditable.
- **AC-2-2.1** Given any resolved check, when stored, then it records: `checkId`, `state`, `artifact` (URL or address), `producer`, `checkedAt`, `method` (the check's logic version).
- **AC-2-2.2** Given a `VERIFIED` state, when a user opens the evidence detail, then a "**Check this yourself**" link re-runs or reproduces the verification instructions.
- **AC-2-2.3** Given a `CLAIMED` state, when displayed, then the UI shows **"Claimed, not confirmed"** — never a checkmark.

**US-2-3 (P0)** — As a security-minded user, I want the admin-control check to be correct, so I don't trust a false claim.
- **AC-2-3.1** Given a Safe-style proxy, when the implementation is resolved, then the tool checks **storage slot 0 and the `singleton()` selector in addition to** the ERC-1967 slot, and records which method succeeded.
- **AC-2-3.2** Given an empty ERC-1967 slot but a populated slot 0, when the check completes, then the contract is classified as a proxy with a **flagged detection method** shown in evidence detail.
- **AC-2-3.3** Given a contract whose admin role cannot be determined by probing, when the check completes, then the result is `UNKNOWN` with the note *"could not be determined"*, and **no blocker fires**.
- **AC-2-3.4** Given a multisig, when displayed, then the UI shows signers, threshold, **keys needed to take control**, keys needed to act alone, guard, and modules — with *"None"* shown explicitly rather than omitted when empty.

---

### EPIC 3 — Gates and blockers

**US-3-1 (P0)** — As a user, I want serious problems to stop a good overall picture, so a red flag is never buried.
- **AC-3-1.1** Given ≥1 gate fired, when the verdict renders, then the verdict band is `BLOCKED` and **no overall percentage is displayed anywhere**.
- **AC-3-1.2** Given a fired gate, when the verdict renders, then blockers appear **before** any positive findings in document order.
- **AC-3-1.3** Given a fired gate, when the user views it, then it shows: plain-language headline, one-sentence explanation, the artifact that triggered it, and a link to the underlying evidence item.
- **AC-3-1.4** Given multiple gates, when displayed, then they are ordered by severity descending.

**US-3-2 (P0)** — As a user, I want to see why a gate *didn't* fire as a checkable statement, so I can verify the tool isn't just quiet.
- **AC-3-2.1** Given a gate that evaluated to pass, when the user opens "All checks", then the gate's evaluated condition is shown with its actual observed value.

**US-3-3 (P1)** — As a user who believes a blocker is wrong, I want to see the override policy, so I can judge the tool's reliability.
- **AC-3-3.1** Given any blocker, when displayed, then the page states what evidence would be required to override it and links to the engine version that decided it.

---

### EPIC 4 — The verdict (the most important screen)

**US-4-1 (P0)** — As a non-expert, I want a clear plain-English verdict immediately, so I know where I stand.
- **AC-4-1.1** Given a completed run, when the results page renders above the fold, then it shows: **verdict phrase**, verdict colour/icon, a 1–3 sentence plain summary, and (if applicable) blocker count.
- **AC-4-1.2** The verdict phrase must be one of exactly five strings defined in [§6.8](#68-verdict-bands). No numeric composite is shown as the headline.
- **AC-4-1.3** Given the verdict, when displayed, then the words *"based on evidence, not price"* appear within the same viewport.

**US-4-2 (P0)** — As a non-expert, I want to know what each finding means for me, so I can act on it.
- **AC-4-3 (P0)** Given any finding, when rendered, then it includes a **"What this means for you"** sentence — mandatory, not optional, one sentence, plain English.
- **AC-4-2.4** Given a technical finding, when collapsed, then the collapsed row shows only: plain headline + evidence-state chip. Technical detail is behind an expand.
- **AC-4-2.5** Given a finding whose state is `CONTRADICTED`, when rendered, then the chip uses the ✖ icon **and** the words "Contradicted" **and** a distinct border — never colour alone (R4).

**US-4-3 (P0)** — As a non-expert, I want to know what the tool *couldn't* check, so I don't over-trust it.
- **AC-4-3.1** Given any `UNKNOWN` items, when the results render, then a dedicated section *"What we couldn't verify"* is always present — even when empty, in which case it says so explicitly.
- **AC-4-3.2** Given `INSUFFICIENT` verdict, when displayed, then the summary explicitly states that the result is a **lack of information, not a negative judgement**, and lists what the user could supply to improve it.
- **AC-4-3.3** Given any pillar percentage, when displayed, then it is accompanied by coverage text: *"based on 9 of 14 checks"*.

**US-4-4 (P1)** — As a user, I want to understand what would make me more confident, so I can do my own next step.
- **AC-4-4.1** Given a completed run, when rendered, then a *"What would change this result"* section lists 2–5 concrete, checkable items (e.g. *"If the team published the addresses of the wallets holding the treasury, we could check whether they can move it alone."*).
- **AC-4-4.2** Each item links to the underlying check so the user understands what is missing.

**US-4-5 (P0)** — As a user, I want base-rate context, so a good score isn't over-read.
- **AC-4-5.1** Given a verdict that depends on holder-accrual evidence, when rendered, then the base rate is shown: *"About 1.6% of the 1,244 protocols studied had a mechanism passing $10M+ a year to token holders."* with a source link.

---

### EPIC 5 — Radar and pillars

**US-5-1 (P0)** — As a user, I want an at-a-glance visual summary, so I can see the shape of a project.
- **AC-5-1.1** Given 5 pillars with computable percentages, when the radar renders, then it shows one axis per pillar using the **plain-English pillar names** from [§7](#7-pillar-definitions).
- **AC-5-1.2** Given a pillar with insufficient coverage, when the radar renders, then that axis is drawn as a **dashed outline at 0 with a "?" marker**, not as a zero value. *(Critical: prevents unknown from reading as bad.)*
- **AC-5-1.3** Given the radar, when displayed anywhere, then a caption is visible in the same viewport: *"This shape summarises the checks. It is not a verdict."*
- **AC-5-1.4** Given a `BLOCKED` verdict, when the radar renders, then it is **visually subordinate** to the blocker list and carries a *"not a verdict"* treatment.
- **AC-5-1.5** Given a screen reader or a user who prefers a list, when the page loads, then an equivalent accessible bar-list of all five pillar values is present in the DOM (not canvas-only).

**US-5-2 (P0)** — As a user, I want the radar to not mislead me, so I read it correctly.
- **AC-5-2.1** The radar must be a **polygon/spider chart**. 3D, doughnut, and area-with-volume encodings are prohibited.
- **AC-5-2.2** Given the radar renders, when a user hovers or focuses an axis, then a tooltip shows: pillar name, percentage, coverage, evidence state, and a link to the underlying findings.
- **AC-5-2.3** Given the five axes, when the polygon is drawn, then the value for a pillar is the **percentage of check-weight earned** within that pillar only. It is **never** an average across pillars.
- **AC-5-2.4** Given a print or export, when the radar renders, then axis labels remain legible in greyscale and the caption is included.

**US-5-3 (P1)** — As a user, I want to compare projects, so I can choose between options.
- **AC-5-3.1** Given two or more completed runs, when compared, then a table shows per-pillar state side by side and **no composite total column**.

---

### EPIC 6 — Evidence detail

**US-6-1 (P0)** — As a technical user, I want to see the raw evidence, so I can disagree with the conclusion.
- **AC-6-1.1** Given a finding, when expanded, then it shows: what was checked, the observed value, the method used, the source artifact with timestamp, and the check ID.
- **AC-6-1.2** Given a `CONTRADICTED` finding, when expanded, then **both** the claim and the contradicting artifact are shown.
- **AC-6-1.3** Given a source, when the user opens it, then the tier (A–D) and access date are visible, so the user can judge source quality.

**US-6-2 (P0)** — As a user, I want to distinguish claim from proof, so I'm not misled by marketing.
- **AC-6-2.1** Every finding displays its evidence state as a chip with icon + word.
- **AC-6-2.2** Given a check sourced from the project's own website, when displayed, then it is capped at `CLAIMED` regardless of confidence in the statement.
- **AC-6-2.3** Given a questionnaire answer, when displayed, then it shows *"Your answer — we checked this ourselves and found…"* and never presents the answer itself as evidence.

**US-6-3 (P1)** — As a user, I want to re-run a single check, so I can test the tool's honesty.
- **AC-6-3.1** Given any check, when the user clicks "Re-check now", then it re-executes and shows the new state with a timestamp delta.

---

### EPIC 7 — The questionnaire (claim generation only)

**US-7-1 (P0)** — As a user, I want to be asked only for things the tool can't check itself, so my time isn't wasted.
- **AC-7-1.1** Given the intake flow, when questions render, then **no question asks for a fact the tool can verify independently**. Every question must have an associated check ID.
- **AC-7-1.2** Given a question, when rendered, then it states its purpose: *"We can't see this from public data — if you have it, it improves the check."*
- **AC-7-1.3** Given a user answering everything "yes", when the verdict renders, then the answers must not raise the verdict above what independently-verified evidence supports. *(Answers carry weight 0.)*

**US-7-2 (P0)** — As a user, I want answers to be treated honestly, so the tool isn't a rubber stamp.
- **AC-7-2.1** Given a `CLAIMED` state that the user asserted, when displayed, then it shows *"Claimed by the project — not confirmed by us."*
- **AC-7-2.2** Given an unverifiable answer, when displayed, then it is excluded from scoring and listed under "What we couldn't verify."

**US-7-3 (P2)** — As a project representative, I want to submit evidence for review, so I can correct a wrong result.
- **AC-7-3.1** Given a result the operator disputes, when they submit evidence, then it is queued as `CLAIMED` pending independent confirmation — never auto-promoted to `VERIFIED`.

---

### EPIC 8 — Export and share

**US-8-1 (P0)** — As a user, I want to export the report, so I can use it elsewhere.
- **AC-8-1.1** Given a completed run, when the user exports, then four options exist, mirroring the reference site: **Copy Markdown**, **Download JSON**, **Export HTML**, **Print / Save as PDF**.
- **AC-8-1.2** Given a Markdown export, when generated, then it contains the verdict, blockers, per-pillar states, the full evidence table with source URLs, the base-rate note, and the disclaimer.
- **AC-8-1.3** Given a JSON export, when generated, then it conforms to [§15.2](#152-json-export-schema) including `schemaVersion`, `gates[]` with artifacts, `dimensions[]` with `signals[]` and `evidenceState`, `contested[]`, `unsourced[]`, and `whatWouldChangeThisVerdict[]`.
- **AC-8-1.4** Given any export, when rendered, then every claim retains its source URL and access date.

**US-8-2 (P1)** — As a user, I want a shareable link, so I can get a second opinion.
- **AC-8-2.1** Given a completed run, when the user copies a share link, then it encodes only the `runId` — **no personal data, no IP, no fingerprint**.

---

### EPIC 9 — Trust and transparency

**US-9-1 (P0)** — As a user, I want to know how the tool decides, so I can audit the logic.
- **AC-9-1.1** Given `/method`, when rendered, then it documents the pipeline, all 5 pillars, the **complete gate register**, the evidence-state definitions, and the base rates.
- **AC-9-1.2** Given `/method`, when rendered, then it states explicitly: **no overall numeric score is produced, and why** ([§6.5](#65-why-we-do-not-ship-a-composite-score)).
- **AC-9-1.3** Given `/sources`, when rendered, then every source is listed with tier, title, URL, and access date, and Tier E (blogs/SEO) sources are marked **"not used as evidence"** if present at all.

**US-9-2 (P0)** — As a user, I want to know what the tool cannot determine, so I don't treat silence as safety.
- **AC-9-2.1** Given `/method`, when rendered, then a "Known limitations" section states: no published trust score has been validated against outcomes; most metrics are gameable to some degree; on-chain address attribution for sanctions requires paid data; early projects cannot be scored on track record.
- **AC-9-2.2** Given any run, when rendered, then limitations relevant to *that* project are surfaced in addition to the global list.

**US-9-3 (P1)** — As a user, I want to know the engine version, so results are reproducible.
- **AC-9-3.1** Given any result, when displayed, then the engine version and gate-register version are shown.
- **AC-9-3.2** Given `/changelog`, when rendered, then every gate and scoring change between versions is listed with its date and rationale.

---

### EPIC 10 — Privacy and anti-fingerprinting

**US-10-1 (P0)** — As a user, I want my data not to be collected, so I can investigate a suspected fraud safely.
- **AC-10-1.1** The tool must **not** record visitor IP addresses, geolocate users, fingerprint devices, or store session identifiers beyond the assessment run.
- **AC-10-1.2** The tool must **not** display the visitor's IP or country. *(Explicit inversion of the reference site.)*
- **AC-10-1.3** Given no account, when a user runs an assessment, then no personal data is required and none is collected.
- **AC-10-1.4** Given the privacy policy, when rendered, then it states what is collected (assessment input and public on-chain reads), what is not (identity, IP, device), and the retention period for run data.
- **AC-10-1.5** Given a user requests deletion, when submitted, then the run and its stored results are deleted within a stated window.

**US-10-2 (P0)** — As an operator, I want to avoid being misled by a prompt-injected project description.
- **AC-10-2.1** Given a project's own documentation or website content ingested during checks, when processed, then it is treated as **untrusted data**, never as instructions.
- **AC-10-2.2** Given fetched third-party content, when parsed, then the parser must not execute embedded scripts and must strip active content.
- **AC-10-2.3** Given an automated check, when it calls an LLM, then the model's output may only set the **evidence state and a short quoted rationale**; it may never set the verdict, fire a gate, or write to the gate register.

---

### EPIC 11 — Admin and operations

**US-11-1 (P1)** — As an operator, I want gates version-controlled, so results are reproducible.
- **AC-11-1.1** Given the gate register, when deployed, then it is immutable per `toolId`; changes require a version bump and a changelog entry.
- **AC-11-1.2** Given a run, when inspected, then the exact gate definitions and check-method versions used are recorded.

**US-11-2 (P1)** — As an operator, I want check failures to be observable, so I can fix the tool.
- **AC-11-2.1** Given any check error, when it occurs, then it is recorded with check ID, error class, and a user-safe message.
- **AC-11-2.2** Given a check failing system-wide, when detected, then the affected pillar shows a degraded notice rather than silently dropping checks — so coverage denominators stay honest.

---

## 11. Visual design system

### 11.1 Direction

**Match the reference site's proven look** — dark, technical, high-contrast, monospace accents, generous spacing, understated motion, a `--secondary` accent used for all active/selected state. Do not restyle. Our only differentiators are **plain language** and **warmth of explanation**, because this audience needs to feel guided rather than examined.

Adopt the reference's semantic token names so the design reads identically: `bg-background`, `bg-card`, `bg-secondary`, `border-border`, `text-foreground`, `text-muted-foreground`, `text-secondary`, `ring-ring`.

### 11.2 Tokens

```css
/* Surfaces — mapped to the reference's token names */
--background:     #0B0F14   /* page  (bg-background)      */
--card:           #121821   /* cards, tabs (bg-card/40)   */
--bg-inset:       #0E141B   /* code, evidence panels      */
--border:         #1E2733   /* border-border/60           */
--border-strong:  #2C3A4B

/* Text */
--text-primary:   #E8EDF2
--text-secondary: #9BAAB9
--text-muted:     #64748B

/* Accent — the reference's "secondary" token carries ALL selected/active state */
--secondary:      #38BDF8   /* active tabs, active chips, gate badges, focus ring */
--secondary-soft: rgba(56,189,248,0.15)   /* active tab bg  (bg-secondary/15) */
--secondary-dim:  rgba(56,189,248,0.10)   /* active chip bg (bg-secondary/10) */
--accent-hover:   #7DD3FC

/* Evidence states — each paired with a distinct icon + border style */
--state-verified:      #34D399;  icon ✔;  border solid
--state-claimed:       #FBBF24;  icon ◐;  border dashed
--state-contradicted:  #F87171;  icon ✖;  border solid 2px
--state-unknown:       #94A3B8;  icon ?;  border dotted

/* Verdict bands */
--verdict-blocked:     #F87171
--verdict-strong:      #34D399
--verdict-mixed:       #FBBF24
--verdict-weak:        #FB923C
--verdict-insufficient:#94A3B8

/* Type */
--font-display: 'Space Grotesk', system-ui, sans-serif;
--font-body:    'Inter', system-ui, sans-serif;
--font-mono:    'JetBrains Mono', ui-monospace, monospace;

/* Spacing scale (4px base) */
--sp-1:4px  --sp-2:8px  --sp-3:12px --sp-4:16px
--sp-5:24px --sp-6:32px --sp-7:48px --sp-8:64px

--radius-sm:6px  --radius-md:10px  --radius-lg:16px
```

### 11.3 Layout

| Region | Spec |
|---|---|
| Header | 64px, sticky, blurred backdrop, max-width 1280px content |
| Sidebar (results) | 280px, per-pillar progress + `N of M checks` counter |
| Main column | max 720px for prose; evidence tables may run to 960px |
| Radar | Square, ~420px, centred, caption directly beneath (same viewport — AC-5-1.3) |
| Status panel | Sticky bottom on mobile, sticky right on desktop |
| Footer | Disclaimer, sources link, engine version |

### 11.4 Component inventory

Components taken **directly from the reference** (name them the same so parity is obvious):

`Header` · `TrustLine` · `Eyebrow` · `TrackTabs` (pill group) · `TrackTab` · `Sidebar` · `PillarsAndProgress` · `PillarChip` (rounded-full) · `ProgressCounter` · `LiveScorePanel` · `CheckCard` · `GateBadge` · `CitationStrip` (▼ expander) · `AnnotatedOptionRow` · `GateNote` · `CardNav` (Back/Next) · `JumpToUnresolved` · `StartOver` · `ViewResults` · `DisclaimerLine` · `Timer`

Components we add:

`SearchCombobox` · `IntakeForm` · `VerdictCard` · `EvidenceStateChip` · `BlockerCard` · `PillarCard` · `RadarChart` · `AccessiblePillarList` · `GlossaryTerm` · `SourceLink` · `BaseRateNote` · `CoverageMeter` · `WhatWouldChangeList` · `UnverifiableList` · `ExportRow`

### 11.5 Reference-parity checklist

Before sign-off, confirm each of these matches the captured reference:

- [ ] Tab pills are `rounded-xl`, `text-xs`, `font-mono`, `uppercase`, `tracking-[0.12em]`, `sm:w-auto`
- [ ] Active tab = `border-secondary/60 bg-secondary/15 text-secondary` + soft glow
- [ ] Inactive tab = `border-border/60 bg-card/40 text-muted-foreground`
- [ ] Pillar chips are `rounded-full px-3 py-1 text-[11px] font-mono uppercase tracking-[0.12em]`
- [ ] Main/sidebar grid is `grid-cols-[minmax(0,1fr)_280px]`, collapsing to one column on small screens
- [ ] `Time on this tool: mm:ss` timer and the disclaimer line are present from first paint
- [ ] `← Back` / `Next` / `↑ Jump to first unresolved — Cnn` / `Start over` all present
- [ ] Citation/evidence strip collapses behind a `▼`
- [ ] Gate badge sits inline with the weight (`5 pts`), as `RED LINE` does in the reference
- [ ] Timer, `Share`, `Free Tools`, `Back to home` chrome all present

### 11.6 Tone of voice

| Do | Don't |
|---|---|
| "We couldn't verify who controls the treasury wallet." | "Treasury control transparency is inadequate." |
| "One person may be able to change how the system works." | "Admin centralization risk: HIGH." |
| "About 1.6% of the projects we studied had this." | "Holders revenue is in the bottom 98%." |
| "We found no evidence either way." | "No risk detected." |

---

## 12. Radar chart specification

### 12.1 Geometry

- **Type:** radar / spider polygon. 5 axes at `72°` intervals, starting at `-90°` (12 o'clock), clockwise.
- **Scale:** 0–100, rings at 20/40/60/80/100.
- **Axis order (clockwise from top):** Security & Technical Integrity · Governance & Treasury Integrity · Token Economics & Value Capture · Regulatory & Legal Status · Real Use-Case Evidence.
- **Value per axis:** `pillar_pct` ([§6.4](#64-pillar-computation)) — percentage of check-weight **earned within that pillar only**.
- **Vertices:** one per axis at `r = (value/100) × R_max`.

### 12.2 Rendering rules

| # | Rule | Acceptance |
|---|---|---|
| R1 | Axis labels use the **plain-English pillar names**, rendered outside the polygon, allowed to wrap to 2 lines | Labels readable at 320px viewport |
| R2 | **Insufficient-coverage axis:** dashed outline, no fill, `?` marker at axis end, grey. **Never drawn at 0.** | Unknown ≠ bad (AC-5-1.2) |
| R3 | Fill: `--accent` at **8% opacity**, stroke `--accent` at full, 2px | Fill must not imply volume |
| R4 | **Prohibited:** 3D, doughnut, extruded areas, gradient fills, drop shadows on the polygon | Prevents misreading area as magnitude |
| R5 | Grid + rings at 12% opacity, 1px, `--border-subtle` | Rings must not compete with data |
| R6 | Caption **directly beneath, same viewport, always**: *"This shape summarises the checks. It is not a verdict."* | AC-5-1.3 |
| R7 | `BLOCKED` verdict → radar rendered at 40% opacity, below blockers in DOM order, with the same caption | AC-5-1.4 |
| R8 | Tooltip on hover **and** keyboard focus: pillar name, %, coverage `n of m`, evidence state, "See findings" link | AC-5-2.2 |
| R9 | Every axis has a tick label showing its value **numerically** (`"72%"`) | Non-visual read of the shape |
| R10 | `prefers-reduced-motion` → no draw-in animation | Renders fully formed |
| R11 | **Mirror accessible DOM:** a `<ul>` bar-list of all 5 pillars with values, present for screen readers and for users who prefer a list | AC-5-1.5 |

### 12.3 Reference-site fidelity

**Measured from the reference, not assumed.** It plots *"% of weight earned per pillar"*, draws the axis labels in plain text beneath/beside the polygon, shows a live `PILLARS & PROGRESS` sidebar with a percentage per pillar, and pairs this with the `LIVE SCORE n / 100` panel in the 280px column. It renders a **polygon/spider chart with 5–7 axes**, no 3D, no doughnut.

**We match** the metric definition, the polygon type, the axis-label treatment, the sidebar percentages and the two-column layout.

**We add, deliberately:**
- **R2 — dashed `?` axis for insufficient coverage** (the reference has no such state; it would draw a zero)
- **R6 — "summary, not a verdict" caption** in the same viewport
- **R7 — radar subordinated at 40% opacity when blocked**, and placed below blockers in DOM order
- **R11 — accessible `<ul>` mirror** of the radar for screen readers and list-preferring users
- Axis labels use our **plain-English pillar names**, per rule R1

### 12.4 Pseudocode

```js
function renderRadar(pillars, verdict) {
  const R = 190, cx = 210, cy = 210;
  const pts = pillars.map((p, i) => {
    const ang = (-90 + i * 72) * Math.PI / 180;
    if (!p.computable) return null;                 // R2: unknown, not zero
    const r = (p.pct / 100) * R;
    return [cx + r * Math.cos(ang), cy + r * Math.sin(ang)];
  });

  // R3 polygon (skip nulls; if <2 computable, render rings only)
  if (pts.filter(Boolean).length >= 2) drawPolygon(pts, {
    fill: 'var(--accent)', fillOpacity: 0.08,
    stroke: 'var(--accent)', strokeWidth: 2,
    opacity: verdict === 'BLOCKED' ? 0.4 : 1,       // R7
  });

  // R2 unknown axes: dashed radial line + "?" label
  pts.forEach((p, i) => { if (p === null) drawUnknownAxis(i); });

  // R1 labels, R9 tick values, R8 tooltips
  pillars.forEach((p, i) => drawAxisLabel(i, p, { tooltip: true }));

  // R6 caption — rendered in DOM, not canvas, so it is always visible + selectable
  // R11 accessible mirror
  renderAccessibleList(pillars);
}
```

---

## 13. Plain-language glossary

Every term below must be implemented as a `/glossary#id` entry and used as an inline `<GlossaryTerm>` in visible copy. **CI must fail on any visible technical term lacking an entry** (AC-0-2.2).

| id | Term | Plain-English definition |
|---|---|---|
| `token` | Token | A digital unit a project issues. Owning it does not automatically mean you own anything in the company. |
| `contract` | Smart contract | A program that holds and moves funds automatically. Once deployed it usually cannot be edited, but the people who deployed it often keep special rights over it. |
| `verified-source` | Verified source code | The published code that matches what is actually deployed. Without it, nobody outside the team can check what the program really does. |
| `audit` | Audit | An outside security firm's review of the code for known flaws. It covers specific code at a specific time — it is not a guarantee, and it does not review how keys are stored. |
| `multisig` | Multisig wallet | A wallet that needs several people's approval before it can move money. On its own it proves nothing — it matters how many people, how they are chosen, and what they can actually do. |
| `threshold` | Threshold | How many of the required people must agree. 5-of-9 means five of nine must sign. |
| `admin-key` | Admin key | A secret that lets one address change or pause the system. A project claiming "no admin keys" is making a strong claim — we check it. |
| `timelock` | Timelock | A waiting period before changes take effect, so people have time to react. A timelock that governs nothing is decorative. |
| `tvl` | Total value locked | The total value deposited in a protocol. It can look large simply because token prices rose. |
| `fees` | Fees | Money users pay to use a protocol. |
| `protocol-revenue` | Protocol revenue | What the protocol keeps after paying out to suppliers and liquidity providers. |
| `holder-revenue` | Revenue for token holders | The part of protocol revenue that reaches people **owning the token**, via buybacks, burns or direct payments. Many projects have revenue but none of it reaches holders. |
| `emissions` | Token emissions | New tokens issued as a reward. Rewards paid in newly printed tokens are not income — they are dilution. |
| `apy` | APY | The advertised annual percentage yield. Check what pays it: real fees, or newly printed tokens? |
| `market-cap` | Market capitalisation | Total value of a token if you multiplied its current price by all tokens in existence. |
| `fdv` | Fully diluted value | That same figure using **every** token that will ever exist — including ones not yet released. A large gap between the two means a lot of supply is still to come. |
| `unlock` | Token unlock | The date scheduled tokens become transferable. Large unlocks often cause price pressure. |
| `holder-concentration` | Holder concentration | How unevenly tokens are held. If a few wallets hold most tokens, those wallets can move the price or the project on their own. |
| `voting-power` | Voting power | Influence over decisions. It can be much more concentrated than ownership, especially when people delegate votes to others. |
| `entity` | Legal entity | The registered company or organisation actually behind the project. |
| `licence` | Licence / authorisation | Official permission from a regulator to offer a regulated service. |
| `sanctions` | Sanctions | Being on an official restricted-party list, which can freeze assets and create legal liability. |
| `custodial` | Custodial | Someone else holds the keys to your money — like a bank. Non-custodial means you alone hold them. |
| `oracle` | Price oracle | A component that feeds prices into a protocol. If the price source can be manipulated, the protocol can be drained. |
| `bridge` | Bridge | A system that moves assets between blockchains. It is usually the single largest security risk in a crypto system. |
| `sybil` | Sybil attack | One actor pretending to be many users, to grab disproportionate rewards. |
| `wash-trading` | Wash trading | Trading a token with yourself to inflate the apparent volume. |
| `mercenary-capital` | Short-term capital | Money that arrives to collect a one-off reward and leaves when it ends. |
| `decentralised` | Decentralised | Control spread across many independent parties instead of one. The label is easy to claim and hard to verify — so we check it. |

---

## 14. Data sources and API integration

Full inventory: [`../research/F_data_and_scoring.md`](../research/F_data_and_scoring.md). Constraints are real and must be designed around.

### 14.1 Tiers

| Tier | Use | Examples |
|---|---|---|
| **A** | Primary regulatory | SEC, CFTC, DOJ, FCA, ASIC, MAS, ESMA, OFAC, MiCA text, official licence registers |
| **B** | Primary technical | Verified source, Safe/Proxy on-chain state, audit PDFs, protocol specs, EIPs |
| **C** | Primary measurement | DeFiLlama, CertiK, Chainalysis, Dune, Nansen, Electric Capital |
| **D** | Established research | Peer-reviewed, named-journalist investigative, think-tank reports |
| **E** | **BANNED as evidence** | Blogs, SEO, listing sites, price predictions. May only *locate* a Tier A–D source. |

**AC:** Given any resolved check, when the source is Tier E, then the engine must reject the state assignment. *(Build-time or runtime assertion; runtime preferred with an alert.)*

### 14.2 Providers, limits and cost

| Source | Provides | Auth / limit | Notes |
|---|---|---|---|
| Etherscan / Blockscout | Code, verified source, holders (top-N), labels | Etherscan free: 3 req/s, 100k/day | `tokenholderlist` is **top-N only** — full holder sets need paid access or self-authored Dune SQL |
| Public JSON-RPC | `eth_call`, `eth_getStorageAt` | Varies; several public endpoints return 403 | Proxy detection requires **both** ERC-1967 slot **and** storage slot 0 + `singleton()` |
| Safe Transaction Service | Multisig owners, threshold, guard, modules, nonce | Free | Confirmed working; used for our verification |
| DeFiLlama | Fees, revenue, **holders revenue**, TVL, incentive data | Free API | **Holders Revenue presence is the P3 anchor check.** TVL is price-confounded by the provider's own documentation |
| CoinGecko | Price, market cap, listing data | Demo: 100 calls/min, 10k/month | Trust Score is **rank-relative** — do not reuse as a verdict |
| OFAC SDN | Sanctions names | Free, **name-only** | Not address-level. Address screening needs a paid provider — **known gap, must be disclosed in the UI** |
| GitHub | Developer activity | 60/hr anon · 5,000/hr auth | Must dedupe by commit fingerprint |
| Electric Capital Open Dev Data | Ecosystem-level dev metrics | Free | **No project-level survival statistic published** — do not fabricate one |
| Dune | Custom on-chain queries | Free tier: 20 credits/MB | For HHI, PnL concentration, clustering |

### 14.3 Known gaps to disclose in-product

1. Address-level sanctions attribution (needs paid data)
2. Treasury balances — no free API
3. Full holder sets on free tiers
4. Artemis REST API is enterprise-gated
5. Organic-vs-incentivised volume share is **defined but not published** by DeFiLlama

Each gap must appear in `/method` and, when it affects a specific run, in that run's *"What we couldn't verify"*.

### 14.4 Cache and reproducibility

- Cache every source response with `fetchedAt`; re-runs must show deltas.
- Store the **check-method version** with each result so historical runs remain interpretable after the engine changes.

---

## 15. Export contracts

### 15.1 Markdown export — required sections

```
# <Project> — <Ticker> — <Verdict phrase>
Engine: <toolId>  ·  Assessed: <timestamp>  ·  Base rates applied: <yes/no>

## Serious red flags            (if any)
## What we checked, and what we found
## What we could NOT verify
## What would change this result
## About the base rates
## Every source we used
## Disclaimer
```

### 15.2 JSON export schema

```jsonc
{
  "schemaVersion": "CryptoLegitimacy_v1",
  "toolId": "CryptoLegitimacy_v1",
  "gateRegisterVersion": "gates-v1",
  "checkedMethodVersions": { "SEC-02": "1.2.0" },
  "timestamp": "2026-10-06T00:00:00Z",
  "subject": {
    "name": "", "symbol": "", "chains": [], "contracts": [],
    "entity": { "claimedName": "", "jurisdiction": "", "registryState": "FOUND|NOT_FOUND|UNPROBEABLE" }
  },

  "verdict": {
    "band": "BLOCKED|STRONG|MIXED|WEAK|INSUFFICIENT",
    "phrase": "",
    "compositeScore": null,
    "compositeScoreOmittedBecause": "No composite score is produced: composite weighting degrades out-of-sample (0.98 in-sample -> 0.59-0.65)."
  },

  "gates": [
    { "gateId": "G-ADMIN-SINGLEKEY", "fired": true, "capOverride": null,
      "blockerSeverity": "CRITICAL", "headline": "",
      "artifact": "0x…", "checkId": "SEC-02", "checkedAt": "2026-10-06",
      "override": null }
  ],

  "dimensions": [
    { "id": "SEC", "name": "Security & Technical Integrity",
      "userFacingName": "Can the code be trusted?",
      "earned": 41.5, "possible": 55, "pct": 75,
      "computable": true,
      "coverage": { "checked": 14, "total": 14 },
      "signals": [
        { "id": "SEC-20", "state": "VERIFIED", "credit": 1.0,
          "observed": "…", "method": "recursive owner() walk",
          "artifact": "https://…", "producer": "chain-rpc",
          "checkedAt": "2026-10-06", "tier": "B" }
      ] }
  ],

  "contested":      [ { "claim": "", "positions": [], "note": "" } ],
  "unsourced":      [ { "claim": "", "why": "" } ],
  "unknownChecks":  [ { "checkId": "", "why": "" } ],
  "whatWouldChangeThisVerdict": [ { "if": "", "then": "", "checkId": "" } ],
  "baseRatesApplied": [ { "stat": "", "value": "", "source": "" } ],
  "disclaimer": "…"
}
```

---

## 16. Privacy, security and non-functional requirements

### 16.1 Privacy (binding — see US-10-1)

| # | Requirement |
|---|---|
| P-1 | **No IP storage, no geolocation, no device fingerprinting, no session tracking** beyond the assessment run |
| P-2 | No account required; no personal data collected |
| P-3 | No analytics on `/assess` results without explicit opt-in |
| P-4 | Run data retention: **30 days**, then deleted; user can delete immediately |
| P-5 | The UI must **never display the user's IP or country** |

### 16.2 Security

| # | Requirement |
|---|---|
| S-1 | **Third-party content is untrusted data.** Project websites, docs and descriptions are never treated as instructions |
| S-2 | HTML parsing must strip scripts and active content |
| S-3 | LLM-assisted steps may set **evidence state + quoted rationale only** — never the verdict, never a gate, never the register |
| S-4 | All gate decisions are pure functions of stored evidence — no LLM in the decision path |
| S-5 | Every check result is reproducible from its recorded artifact + method version |
| S-6 | Depend on pinned versions; audit the chain/registry clients |

### 16.3 Non-functional

| # | Requirement | Target |
|---|---|---|
| N-1 | Results page interactive | < 2.5s |
| N-2 | Full assessment (23+ checks) | < 90s, streamed with live progress |
| N-3 | Lighthouse accessibility | ≥ 95 |
| N-4 | Keyboard operability | 100% of flows |
| N-5 | Viewports | 320px → 1920px |
| N-6 | Uptime | 99.5% |
| N-7 | Graceful degradation | Provider outage ⇒ `UNKNOWN` + coverage notice, never a wrong `VERIFIED` |
| N-8 | i18n-ready | All copy in one file, no hard-coded strings in components |

---

## 17. Traceability matrix

| Story group | Research basis | Report section |
|---|---|---|
| EPIC 0 (foundations) | Reference teardown | [Appendix A](../reference/01_reference_site_teardown.md) |
| EPIC 1 (intake) | Reference teardown; identity resolution | [§10.1](../01_crypto_legitimacy_research_report.md) |
| EPIC 2 (evidence) | Slices A, B, C, D, F | [§4](../01_crypto_legitimacy_research_report.md) §5 §6 §7 §9 |
| EPIC 3 (gates) | Averaging failure; gate register | [§9.1–9.3](../01_crypto_legitimacy_research_report.md) |
| EPIC 4 (verdict) | Non-expert constraint; base rates; limitations | [§3.3](../01_crypto_legitimacy_research_report.md) [§12](../01_crypto_legitimacy_research_report.md) |
| EPIC 5 (radar) | Radar critique; reference radar | [§9.6](../01_crypto_legitimacy_research_report.md) [Appendix A](../reference/01_reference_site_teardown.md) |
| EPIC 6 (evidence detail) | Tiering; CLAIMED handling | [§2.3](../01_crypto_legitimacy_research_report.md) |
| EPIC 7 (questionnaire) | Claim-generation principle | [§9.5](../01_crypto_legitimacy_research_report.md) |
| EPIC 8 (export) | Reference export contract | [Appendix A](../reference/01_reference_site_teardown.md) |
| EPIC 9 (transparency) | No validated score; limitations | [§6.5](#65-why-we-do-not-ship-a-composite-score) [§12](../01_crypto_legitimacy_research_report.md) |
| EPIC 10 (privacy) | Fingerprinting inversion | [§7.6](../01_crypto_legitimacy_research_report.md) |
| EPIC 11 (admin) | Gate versioning; G-R3 | [§10.3](../01_crypto_legitimacy_research_report.md) |

**Signal coverage:** 145 signals in [`../SIGNAL_REGISTER.md`](../SIGNAL_REGISTER.md) map to the five pillars in [§7](#7-pillar-definitions). Implementation should consume the register as the checklist, prioritising signals marked high-confidence.

---

## 18. Build order and definition of done

### 18.1 Suggested sequence

| Phase | Deliverable | Stories |
|---|---|---|
| **1** | Design system, glossary, app shell, routing | US-0-1…3, US-9-1 skeleton |
| **2** | Evidence model + check runner + state resolver | US-2-1…3 |
| **3** | Gate engine + blocker rendering | US-3-1…3 |
| **4** | Verdicts, findings, "what we couldn't verify" | US-4-1…5 |
| **5** | Pillar cards + radar + accessible mirror | US-5-1…2, US-7-1 |
| **6** | Evidence detail, exports, sources page | US-6-1…3, US-8-1, US-9-1…3 |
| **7** | Intake, search, share links | US-1-1…3, US-8-2 |
| **8** | Privacy/security hardening, admin, changelog | US-10-*, US-11-* |

**Phase 2 and 3 are the critical path.** Do not build UI polish before the check runner and gate engine work — the verdict's credibility depends entirely on them.

### 18.2 Definition of done

A story ships when:

1. Every AC passes as an automated test where testable.
2. Accessibility: keyboard-complete, screen-reader-labelled, WCAG AA, Lighthouse ≥95.
3. No unexplained technical term in visible copy (AC-0-2.2 enforced by CI).
4. Every `VERIFIED` finding has an artifact, producer, timestamp and re-runnable check.
5. `UNKNOWN` renders distinctly and is excluded from denominators.
6. Export round-trips and validates against [§15.2](#152-json-export-schema).
7. No PII, IP or fingerprint in any export, log or view.
8. Code review confirms no LLM sits in the verdict or gate decision path (S-4).

### 18.3 Content gate before launch

- [ ] Gate register reviewed and frozen as `gates-v1`
- [ ] Glossary covers every visible technical term
- [ ] Base-rate statistics verified against cited sources
- [ ] Known gaps in §14.3 disclosed in-product
- [ ] All five verdict phrases written in the user's language and reviewed by a non-expert reader
- [ ] Disclaimer reviewed by counsel

---

## 19. Anti-requirements

Things a well-meaning developer might add that would **damage** this tool:

| Do NOT | Why |
|---|---|
| Add an overall 0–100 composite score | Out-of-sample degradation to 0.59–0.65; no validated precedent ([§6.5](#65-why-we-do-not-ship-a-composite-score)) |
| Make the radar the primary verdict | Rewards balanced-looking mediocre profiles; misreads unknown as bad |
| Let questionnaire answers contribute to the score | Self-report is the attack surface; weights answers at 0 |
| Use price, follower count, or raw volume as positive inputs | Volume is the most thoroughly falsified metric; ~49.6% of losses are non-code |
| Render a zero for an unknown check | Destroys credibility on young projects; wrong on the facts |
| Show the user's IP or country, as the reference site does | A fraud-investigation tool must not fingerprint its users |
| Auto-promote operator-submitted evidence to `VERIFIED` | Reintroduces the self-report vulnerability |
| Add an LLM to the verdict or gate path | Non-reproducible and unauditable (S-4) |
| Use the words "safe", "secure", "scam", or "fraud" as verdicts | Unsupported legal/financial claims (R10) |
| Add a price-prediction or "next 10×" panel | Out of scope; would destroy the tool's credibility |

---

## Document control

| Version | Date | Change |
|---|---|---|
| 1.0 | 2026-10-06 | Initial development specification |

**Sources of authority:** [`../01_crypto_legitimacy_research_report.md`](../01_crypto_legitimacy_research_report.md) · [`../SIGNAL_REGISTER.md`](../SIGNAL_REGISTER.md) · [`../research/LEDGER.md`](../research/LEDGER.md) · [`../reference/01_reference_site_teardown.md`](../reference/01_reference_site_teardown.md)

**Design reference:** <https://www.msawox.com/en/tools>