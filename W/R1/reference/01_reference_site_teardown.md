# Reference Site Teardown — msawox.com/en/tools

**Purpose:** reverse-engineer the reference tool suite's questionnaire → score → radar mechanism so the Crypto Legitimacy tool can match its look-and-feel while fixing its epistemics.
**Method:** Playwright (Chromium headless shell 153, Playwright 1.63.0) driving the live site + static extraction of the Next.js client bundles.
**Date:** 2026-10-06
**Artifacts:** `reference/msawox_questionnaires.json`, `reference/msawox_scoring_engine.json`, `reference/screens/*.png`

---

## 1. Suite Inventory — 17 tools, 10 distinct scoring engines

All 17 routes under `/en/tools/`:

| Tool slug | Title shown | Engine `toolId` |
|---|---|---|
| `ethical-factor-score` | Ethical Startup & Ethical AI Assessment Suite | `EthicalStartup_Assessment_v1` + `EthicalAI_Assessment_v1` |
| `ai-act-risk-tier` | What's your EU AI Act risk tier? | *(shared risk-tier engine)* |
| `ai-readiness-score` | How ready is your company for AI? | *(shared)* |
| `nist-ai-rmf-profile` | How strong is your AI risk management profile? | `AIRMFRMF_Profile_v1` |
| `owasp-llm-security-checklist` | Is your LLM app secure against the OWASP Top 10? | `LLMSec_Top10_Checklist_v1` |
| `dora-ict-resilience` | Is your ICT resilience DORA-ready? | `DORA_Resilience_Readiness_v1` |
| `nis2-gap-assessment` | Is your company in scope for NIS2? | *(NIS2 engine, `overrideReasons`)* |
| `cra-conformity-readiness` | Is your product CRA-ready? | `CRA_Conformity_Readiness_v1` |
| `gdpr-dpia-screener` | Is your data processing GDPR-ready? | `GDPR_DPIA_Screener_v1` |
| `iso27001-gap-assessment` | Is your ISMS ISO/IEC 27001:2022 ready? | `ISO27001_Gap_Assessment_v1` |
| `soc2-audit-readiness` | Is your company SOC 2 audit-ready? | `SOC2_Audit_Readiness_v1` |
| `due-diligence-checklist` | Will your startup pass technical due diligence? | *(checklist)* |
| `technical-risk-score` | How risky is your technical stack? | *(risk score)* |
| `founder-market-fit` | Are you the right founder for this problem? | *(simple 6-q fit)* |
| `build-vs-buy` | Should you build, buy, or partner? | *(decision tree)* |
| `role-decision-tree` | Which technical help do you actually need? | *(decision tree)* |
| `islamic-startup-readiness` | Islamic Startup Readiness Suite | `IslamicStartupReadinessSuite_v1` |

Note the two non-scoring tools (`build-vs-buy`, `role-decision-tree`) are **decision trees**, not scores — the suite mixes three interaction archetypes: *weighted questionnaire*, *checklist*, *decision tree*.

---

## 2. The Scoring Engine (extracted from client bundles)

### 2.1 Question object schema

Two variants coexist. The sophisticated form:

```js
{
  id: "Q02", n: 2, category: "C1",
  weight: 5,
  scale: "scale_0_2",              // typed answer scale
  gateId: "GATE_EST_01",           // optional hard-gate linkage
  title: "Related-Party Dealings & Conflict of Interest Transparency",
  citation: "Ethisphere 2026 ...; Canada Treasury Board Maturity Framework 2026",
  prompt: "Is there a formal, documented register of related-party transactions ...",
  guidance: "Undisclosed related-party ..."
}
```

The standards-anchored form adds per-answer typing and crosswalk keys:

```js
{ id:"Q01", n:1, category:"A", weight:5, kind:"scale_0_2",
  tscAnchor:"CC1.2, CC1.3", options:["0","1","2"] }              // NIST AI RMF (4 functions)
{ id:"Q02", n:2, category:"C", weight:4, kind:"yes_no",
  citation:"MANAGE 3.1, 3.2", options:["No","Yes"] }             // ISO 27001
{ id:"Q05", n:5, category:"3", weight:6, legalCitation:"...", options:[...] }  // CRA/DORA
```

Answers are stored as a flat map keyed by question id: `{Q01, Q02, ...}`.

**Key fields:** `n` = display order · `category` = pillar id · `weight` = max points · `scale`/`kind` = answer type · `gateId` = hard-gate linkage · `citation`/`legalCitation`/`tscAnchor` = external authority for that question.

### 2.2 Weight distribution (measured)

**Ethical Startup Assessment** — 22 questions, weights sum to **exactly 100**:

```
[5,5,4,4,4,4,4,4,6,5,5,4,4,4,4,5,5,4,4,6,5,5]  → 100
pillars C1..C6 · gates GATE_EST_01..03
```

**Ethical AI Assessment** — 22 questions, 7 pillars, 4 gates:

```
[5,5,4,5,5,4,4,5,4,5,4,3,4,3,5,5,4,5,5,4,6,6]  → 100
```

Weights are near-uniform 3–6 points per question. `weight: 0` exists in the schema for **non-scoring questions** (informational or gate-only probes).

Per-pillar weights are separately declared, e.g. DORA `{1:16, 2:20, 3:20, 4:16, 5:11, 6:17}`, CRA `{1:9, 2:24, 3:24, 4:15, 5:20, 6:8}`.

### 2.3 Answer scales

`scale_0_2` = three options rendered as a Likert row, with **explicit point values printed in the UI**:

```
[0 / 5 pts]  Founder-Only Bottleneck — All employee concerns must be reported
            directly to founders; no external or independent avenue exists.
[2.5 / 5 pts] Designated Internal Peer — A peer co-founder or junior HR person
            handles complaints, but lacks true independence or board escalation.
[5 / 5 pts]  Independent Escalation Pathway — Documented escalation route directly
            to an independent board director, external legal counsel, or retained
            advisory ombudsperson.
```

So each answer is worth `weight × (index / 2)`. The mid option grants **half credit** — the engine rewards partial maturity rather than demanding binary compliance. `yes_no` scales use `["No","Yes"]`.

### 2.4 Score, bands, statuses

```js
rawPoints  = Math.round(10 * ratio) / 10          // one-decimal precision
percentage = rawPoints / maxPoints
status     = s >= 80 ? "satisfactory"
           : s >= 60 ? "moderate"
           : s >= 40 ? "high"                    // else "critical"
riskBand   = d >= 80 ? "ready"
           : d >= 60 ? "moderate"
           : d >= 40 ? "high"
           :               "critical"
```

Overall score = `Σ(question rawPoints)`, i.e. **weighted sum over questions whose weights total 100**, rendered `/100`.

### 2.5 Hard gates and score capping — the most important mechanism

Seven gates exist: `GATE_EST_01..03` (Ethical Startup) and `GATE_EAI_01..04` (Ethical AI). A gate is
attached to a question; failing it **caps the overall score**:

```js
{
  gateId: "GATE_EST_01",
  questionId: "Q02",
  gateName: "Fiduciary Separation & Related-Party Gate",
  passed: Number(answers.Q02 ?? "0") > 0,
  score: 0,
  capOverride: 49,                    // hard ceiling
  blockerSeverity: "CRITICAL",
  warningHeadline: "Unmonitored Related-Party Dealings",
  warningDetails: "Undisclosed related-party transactions represent a breach of
                   fiduciary duty blocking institutional venture capital diligence."
}
// and:
score = Math.min(score, 49)
```

Three cap levels are used: **`39`**, **`49`**, **`59`**. This is a deliberate **anti-averaging
design**: a perfect 100 with one critical gate breach is reported as 49, not 100. The UI surfaces it:

> **Critical Diligence & Regulatory Blocker Active** — "Your raw score (100/100) has been capped at
> 49/100 due to a critical audit gate breach."

The design rationale is visible in the strings: a critical flaw must not be *averaged away* by a
dozen healthy dimensions. **This is the single most important idea to carry into the crypto tool.**

### 2.6 Gap table, radar, and action plan

- **Radar**: one axis per pillar, plotting *the % of weight earned within that pillar* (`radarNote: "Six-axis radar of the % of weight earned per DORA pillar."`). Vertically stacked axis labels.
- **Category breakdown table**: `Pillar | Earned | Max | % | Status`.
- **Gap severity** drives the remediation plan: `actionPlanSub: "Gaps ranked algorithmically by Weight × Gap Severity and sequenced into three agile sprint backlogs."`
- **Action items have stable ids**: `ACT-DORA-01…09`, `ACT-GDPR-01…06`, `ACT-NIS2-05…06`, bucketed into `short_term` / `medium_term` / `long_term` sprint backlogs.
- **Weakest questions** surfaced explicitly to focus attention.

### 2.7 Export contract (machine-readable JSON)

Every tool emits a versioned JSON document via `Download JSON`:

```json
{
  "schemaVersion": "…",
  "toolId": "DORA_Resilience_Readiness_v1",
  "timestamp": "2026-10-06T…Z",
  "rawScore": 71.4,
  "overallScore": 59,          // post-cap
  "isCapped": true,
  "isCappedByQ06": true,
  "capReasons": ["…"],
  "overrideReasons": ["…"],
  "riskBand": "high",
  "posture": "…",
  "categories": [ { "category":"C1", "name":"…", "rawPoints":8, "maxPoints":16,
                    "percentage":50, "status":"high" } ],
  "gaps": [ … ], "blockers": [ … ], "fixPlan": [ … ],
  "actionPlan": [ { "id":"ACT-DORA-01", "sprint":"short_term", … } ],
  "policyMappings": [ … ], "owaspEntries": [ … ], "calendar": [ … ],
  "evidence": [ … ], "isAgenticEnabled": true
}
```

Exports also offered: **Copy Markdown** (a full report written straight to clipboard), **Export HTML**, **Export PDF / print**.

### 2.8 UX shell (what "look and feel" means here)

Header → title + one-line subtitle (`22 questions · ~15 min · zero signup · 100% client-side privacy`) → **live sidebar** with per-pillar progress percentages and `DIAGNOSTIC COMPLETION 0 / 22` → question card (`Question 16 / 22`, `5 pts max`, question title, prompt, expandable citation strip `▼`, annotated options) → `Previous` / `Next` / `↑ Jump to first unanswered (Q01)` / `Start over` → **sticky LIVE READINESS SCORE 0 / 100 + band label** → radar chart → category table → blockers → action plan → share / export row.

Cross-cutting details: a live session timer (`Time on this tool: 00:20`), a standing disclaimer
("Guidance only — not a legal decision, legal advice, or a substitute for qualified professional
review"), a visitor-IP/country footer, a "related regulation" cross-sell card, and a concurrent-visitor
counter. All computation is **client-side**; no signup.

---

## 3. What Is Good Here — Carry It Forward

1. **Gates + caps.** Critical blockers hard-cap the score. Prevents averaging away a fatal flaw.
2. **Explicit per-answer point values and rationale.** `[2.5 / 5 pts] Designated Internal Peer — …` makes the score fully explainable and non-opaque. Trust comes from auditability.
3. **Every question carries an external citation** (`citation`, `legalCitation`, `tscAnchor`). Grounds the score in authority, not opinion.
4. **Part-credit scale** (`scale_0_2`, mid = half) rather than binary compliance. Matches reality, where maturity is graded.
5. **Versioned machine-readable export** + Markdown/HTML/PDF. Output is a reusable artifact, not a dead-end score.
6. **Weight-0 questions.** Lets a probe inform without inflating or deflating the score.
7. **Weakest-question surfacing and Weight × Gap prioritisation.** Converts a score into a work queue.
8. **Live progress + client-side privacy.** Low friction; honest about data handling.

## 4. What Must Change for a Crypto Legitimacy Tool — Do Not Copy

This is the crux. The reference suite answers *"does this founder self-assess honestly?"* — a
self-report instrument. A crypto-legitimacy instrument must answer *"does verifiable evidence
exist?"* These are different epistemic objects, and the reference design leaks self-report into
every layer.

| # | Reference-site weakness | Why it fails for crypto | Required change |
|---|---|---|---|
| 1 | **Entirely self-report.** No external verification of any answer. | Fatal. A project founder answering "yes, we have a multisig" is worth nothing — self-report is the *attack surface*, not the evidence. Distrust is the premise. | Every substantive signal must resolve to a **mechanically checkable artefact**: a chain, an address, a verified contract, a registry entry, a document. Self-report is admissible only as a *pointer* to where to check, never as the check. |
| 2 | **Near-uniform weights** (3–6 pts) across all questions. | Crypto risks are wildly non-uniform: a live admin key that can drain the treasury is categorically different from an un-updated docs page. | Weight from **evidence of harm severity**, not from layout convenience. Expect a steep power law, not 4–5 everywhere. |
| 3 | **Weights summing to 100 → one headline integer.** | False precision. Implies a project scoring 72 vs 68 is meaningfully different. It is not. | Report a **band + per-dimension evidence**, keep the integer subordinate. Confidence intervals on unverifiable dimensions. |
| 4 | **Gate = one question's answer.** | Gates must be **cross-verified facts**, not answers. | Gate triggers from an observed fact class (e.g. "unverified admin EOA holds upgrade rights"), never from a checkbox. |
| 5 | **Radar chart of 6–7 axes.** | Radar areas imply axis comparability and ordering, and reward *balanced-looking* mediocre profiles. Legitimacy profiles are legitimately lopsided (huge security, thin docs). | Radar is fine as a *visual*, but must be paired with the underlying per-axis **evidence state** (`verified / claimed / contradicted / unknown`) — never a bare percentage. Unknown ≠ zero. |
| 6 | **Sources include soft trade press** (`Ethisphere`, `Japan ICLG ESG`, `Wilbur Labs Startup Failure Report`, unattributed `2026` case notes). | Not adequate for an investment-decision instrument. | Tier A–D sources only (regulator, audit firm, verified code, measurement provider, peer-reviewed). Every claim traceable. |
| 7 | **No counter-evidence surface.** | Biases toward confirmation. A tool that can't say *"we don't know"* will confidently score garbage. | Mandatory `CONTESTED` and `UNSOURCED` states; an explicit "what would change this verdict" section. |
| 8 | **One score for one questionnaire.** | Crypto legitimacy has **hard gates with different consequences** — an unlicensed securities offering is not partially offset by good docs. | Keep gates/caps (the best idea here) but make them **pre-registered and rule-based**, applied before any weighting. |
| 9 | **No distinction between "absent" and "not applicable".** | New projects legitimately lack a track record. Penalising them equally to an established scam is a false positive that destroys trust in the tool. | Four-state answer model: `VERIFIED` / `CLAIMED` / `CONTRADICTED` / `UNKNOWN-or-N-A`, scored asymmetrically. |
| 10 | **Live visitor-IP/country tracking and session recording** with a cookie-style notice. | A tool analysing suspected fraud should not fingerprint its users. | Anonymous by design, or explicitly opt-in telemetry, disclosed. |

---

## 5. Spec Transferred to the Crypto Tool

```
                 ┌──────────────────────────────────────┐
                 │ EVIDENCE COLLECTION (per dimension)  │
                 │  chain · verified contract · registry│
                 │  audit PDF · team wallet · metrics    │
                 └───────────────┬──────────────────────┘
                                 ▼
                 ┌──────────────────────────────────────┐
                 │ STATE RESOLUTION                      │
                 │  VERIFIED │ CLAIMED │ CONTRADICTED │  │
                 │           │           │  UNKNOWN/N-A  │
                 └───────────────┬──────────────────────┘
                                 ▼
                 ┌──────────────────────────────────────┐
                 │ GATE EVALUATION  (pre-registered,    │
                 │  severity-ordered, hard-fail)        │
                 │  e.g. sanctions → admin-key → no code │
                 └───────────────┬──────────────────────┘
                        pass ▼            ▼ fail
                             │     ┌──────────────┐
                             │     │ CAP + BLOCKER │
                             │     └──────┬───────┘
                             ▼            ▼
                 ┌──────────────────────────────────────┐
                 │ WEIGHTED PILLAR SCORES + RADAR       │
                 │  Security · Tokenomics · Governance │
                 │  Regulatory · Real-use-case evidence  │
                 └───────────────┬──────────────────────┘
                                 ▼
                 ┌──────────────────────────────────────┐
                 │ OUTPUT: band + per-dimension evidence │
                 │ + gaps (Weight × Severity) + JSON/MD  │
                 │ + "what would change this verdict"   │
                 └──────────────────────────────────────┘
```

**Inherited verbatim:** 6-axis radar + per-pillar progress sidebar, annotated per-answer point values, explicit gate/cap banner, weakest-dimension surfacing, `Weight × Severity` remediation backlog in sprint buckets, versioned JSON + Markdown export, live score panel, standing disclaimer.

**Added:** four-state evidence resolution, pre-registered hard gates, source tiers on every claim, contested/unknown states, counter-evidence requirement, false-positive controls for new projects.