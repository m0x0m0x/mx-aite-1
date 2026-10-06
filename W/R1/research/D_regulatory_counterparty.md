# Slice D — Regulatory Status, Licensing, Counterparty & Legal-Entity Legitimacy

Research date: 2026-10-06. Scope: US / EU / UK / AU / SG regulatory posture; licensing; lawful-operation status; legal-entity identity; custody; sanctions & AML; privacy and operational compliance. **Out of scope:** tokenomics, code security, governance design, case studies.

Tier discipline: Tier A = regulator/statute/primary text. Tier B = official registers and verification tools. Tier C = primary measurement (Chainalysis). Tier D = media/law-firm/academic secondary. Tier E = blogs/aggregators — discovery only, never cited.

---

## D1 The securities question: what is legally settled, what is not

**Current US test — the Howey overlay, now with a token taxonomy.**

The SEC's own 2026 interpretation states that for more than a decade "the Commission generally looked to the test developed by the U.S. Supreme Court in SEC v. W.J. Howey Co. (known as the 'Howey test')" [T-A1]. On 17 March 2026 the Commission issued Commission Interpretation **Release No. 33-11412**, "Application of the Federal Securities Laws to Certain Types of Crypto Assets and Certain Transactions Involving Crypto Assets" [T-A1, T-A2]. The CFTC *joined* the interpretation, stating that "non-security crypto assets" could meet the definition of "commodity" under the CEA and that CFTC staff will administer the CEA consistently with it [T-A1].

The taxonomy is five-fold [T-A1]:

| category | securities status | rationale (SEC's own words) |
|---|---|---|
| Digital Commodities | **not** securities | value derives from "programmatic operation of a crypto system" and supply/demand, not from expectation of profits from others' essential managerial efforts |
| Digital Collectibles | **not** securities | art, music, video, in-game items, memes, etc. |
| Digital Tools | **not** securities | "membership, ticket, credential, title instrument, or identity badge" |
| GENIUS Act stablecoins | **not** securities | only if a "payment stablecoin issued by a permitted payment stablecoin issuer" |
| Digital Securities ("tokenized securities") | **are** securities | enumerated instruments where the record of ownership is on a crypto network |

The critical mechanic for scoring is that the *token* category is not the only question. The SEC "explained how a non-security crypto asset becomes subject to an investment contract when an issuer offers it by inducing an investment of money in a common enterprise with representations or promises to undertake essential managerial efforts from which a purchaser would reasonably expect to derive profits" — and "how a non-security crypto asset ceases to be subject to an investment contract when the investment contract terminates because either the issuer has fulfilled its representations or promises or the issuer has failed to satisfy them" [T-A1]. **CONTESTED / legally unsettled:** the fact sheet does not specify the bright line between "purchaser would reasonably expect to derive profits" and "functional" utility; this is exactly the line the 2026 proposed rule tries to draw.

**Statutory framework actually in force.** Congress passed exactly one crypto statute in 2025: the **GENIUS Act**, signed 18 July 2025, Pub. L. No. 119-27, 139 Stat. 419 (2025), codified in part at 12 U.S.C. 5901 et seq. [T-A3, T-A4]. It creates a federal regime for *payment stablecoins* only. It is **not** a market-structure law.

The market-structure bill — the **Digital Asset Market Clarity Act of 2025 (CLARITY Act), H.R. 3633** — was reported favourably by the House Financial Services and Agriculture Committees on 23 June 2025 (H. Rept. 119-168) [T-A5]. As of the SEC's own 18 August 2026 statement, it was **still not law**: the Chairman "will continue to support Congress in delivering the CLARITY Act to President Trump's desk" [T-A6]. Any project marketing itself as operating under a "CLARITY Act exemption" in 2026 is asserting a legal status that does not exist.

**Proposed, not final.** On 18 August 2026 the SEC proposed **Regulation Crypto Assets**, Release No. 33-11434 [T-A6, T-A7]: a one-time $5m exemption over four years; a $75m-per-12-month exemption (with financial statements and ongoing reporting); and a **conditional safe harbor from the term "investment contract"**, under which a crypto asset "would be deemed not to be subject to an investment contract" once the issuer has completed or permanently ceased all essential managerial efforts it represented it would take. It also proposes state-law preemption for securities issued under those exemptions. Comment period: 60 days from Federal Register publication. **CONTESTED:** a Commission-created safe harbor from a statutory term, plus asserted preemption of state securities registration, is a legally vulnerable construct. Treat "Regulation Crypto Assets safe harbour" as a *pending* claim.

**Enforcement posture has flipped, which matters for scoring.** The SEC's 2025 fact sheet says the prior Commission "failed to develop a tailored regulatory framework … and instead focused its resources on bringing enforcement actions, thereby 'regulating by enforcement'" [T-A1]. The record of dismissals:
- **Coinbase** — joint stipulation of dismissal, press release 2025-47, 27 Feb 2025 [T-A8, T-A9].
- **Kraken (Payward Inc. / Payward Ventures Inc.)** — dismissed with prejudice, Litigation Release 26278, 27 Mar 2025 [T-A10].
- **Binance** — complaint dismissed 29 May 2025 per SEC-filed stipulation [T-A11], after the June 2023 settlement.
- **Uniswap Labs and OpenSea** — investigations closed without action, announced February 2025 [T-A12, T-B1].

A registrant filing on EDGAR states "the SEC dropped or froze approximately 89 high-profile cryptocurrency enforcement cases" in 2025 [T-B1]. **Interpretation for scoring:** *absence of a US securities charge against a protocol's operator is weak evidence of legality.* It is evidence of the opposite — that the question is now governed by an interpretation and a proposed rule rather than by case law.

**SEC v. Winding Tree LLC (D.C. Cir. 2025) — UNSOURCED (primary not retrieved).** Despite multiple search strategies (court-issued site, CourtListener, gov-press indexes, targeted queries) I could not retrieve the opinion text. I therefore do **not** assert its holding. Its commonly reported effect (a narrower reading of the broker-dealer exclusion than the Commission had applied, and attention to decentralised intermediaries) is **CONTESTED** here and must be re-sourced before use. Same treatment for the **Grayscale** (D.C. Cir. 2024) and **CoinShares** (3d Cir. 2023) decisions: widely reported, primary texts not retrieved in this pass. The *functional* effect of Grayscale (spot bitcoin ETFs became registrable) is corroborated by GENIUS Act § 2(6) cross-reference to the "Digital Asset" definition in the SEC's own interpretation PDF [T-A2], but the rulings themselves are **UNSOURCED**.

**DeFi / DePIN / DAO analysis.**
- *DePIN / protocol-mining / staking projects*: 33-11412 states that "protocol mining," "protocol staking," and "wrapping" of a non-security crypto asset "do not involve the offer and sale of a security," and that "certain" airdrops involve no "investment of money" [T-A1]. This is the strongest current support for treating DePIN hardware/incentive tokens as non-security *at the protocol layer*. **CONTESTED:** "certain" is doing work; and the investment-contract overlay remains available to the issuer.
- *DAOs*: the CFTC held the **Ooki DAO** "could be sued and served as an unincorporated association and is a 'person' under the CEA and thus can be held liable for violations of the law" [T-A14]. **Lesson:** ring-fencing in a DAO does not defeat entity liability; the aggregator front end was treated as the DAO's public face. (I did not retrieve the penalty amount — **UNSOURCED**.)

**Verifiable checks**
- **[D1]** Locate the exact token category the project asserts, quoted from its own marketing/ToS, and test it against the five categories in 33-11412. Absence of an asserted category is itself a finding.
- **[D2]** Determine whether a non-security asset is offered under "essential managerial efforts" promises. Extract the 3–5 concrete promises (team commitments, roadmap delivery, treasury deployment) and test whether each is "complete or not."
- **[D3]** Search the project's own legal page for the words "CLARITY Act," "GENIUS Act," "Regulation Crypto Assets," "safe harbor." If it relies on the CLARITY Act or on Regulation Crypto Assets, flag as **asserting an unenacted/proposed legal status**.
- **[D4]** Check whether the project's US-facing activity includes token *sales* (not just secondary trading). Secondary trading of a non-security digital commodity is materially different from primary issuance.
- **[D5]** For protocol-mining/staking/DePIN: confirm the project's value accrual derives from "programmatic operation of a crypto system" rather than from managerial-effort promises. Log which of the two the founder's own copy implies.

---

## D2 The unlawful-activity dimension: four distinct states

Distinguishing these four is the core of this slice. Conflating them is the single largest error a scoring tool can make.

**(a) Unlicensed but legal.** The activity requires no licence. Examples: offering a digital commodity that is not a security and is not a stablecoin; operating a *fully decentralised* service with no intermediary — ESMA's Q&A position is that "crypto-asset services provided in a fully decentralised manner without any intermediary are not in scope of the MiCA Regulation" [T-A16, T-A17]. Also: purely permissionless code with no operator. Absence of a licence here is not a red flag.

**(b) Unlicensed and illegal.** The activity requires a licence or authorisation and the entity lacks it. Enforcement bites in four distinct ways:
- *Securities*: unregistered broker/dealer activity. The SEC's 2025 settled orders on "finder"-type intermediaries held that "receipt of transaction-based compensation in connection with [finders'] activities is a hallmark of broker-dealer activity," because it creates a "salesman's stake" [T-D1]. Reciprocally, courts have applied a "finder's exception" for pure intermediation [T-D2]. **CONTESTED:** where a self-custody wallet platform sits between a user and a DEX is fact-specific and not resolved.
- *Commodities*: CFTC brought 47 digital-asset cases in FY2023 (>49% of all actions), including a "first-of-its-kind litigation victory against a decentralized autonomous organization" [T-A14].
- *Money transmission*: DOJ prosecutes operating an unlicensed money-transmitting business. Roman Storm was charged 23 Aug 2023 with laundering "more than $1 billion" [T-A18] and was convicted 6 Aug 2025 of conspiring to operate an **unlicensed money transmitting business** [T-A19]. (The 6 Aug 2025 DOJ page body was bot-protected; title, court, judge and verdict date are confirmed via the DOJ search index — **partial Tier A retrieval**.)
- *National licensing*: ASIC's Block Earner appeal culminated in a unanimous **7-0 High Court** holding that a fixed-yield digital-asset product *was* a financial product requiring an AFSL, because "it was sufficient that investors' funds were used or intended to be used to generate a return for both the investor and the issuer," and any contrary contention "would ignore the commercial reality of any such financial investment" [T-A22, T-A23].

**(c) Licensed.** Registers that establish this exist and are public: ESMA's interim MiCA register (five CSVs, incl. authorised CASPs) [T-A15], the FCA Register (which "tells you if the firm is registered under the Money Laundering Regulations" for cryptoasset firms) [T-A20], MAS Financial Institutions Directory filtered to Digital Payment Token Service [T-B2], FinCEN MSB Registrant Search [T-B3], SEC IAPD/Form ADV [T-B4].

**(d) Sanctioned.** Distinct from unlicensed — a sanctioned party may hold or have held licences. OFAC designated **Tornado Cash** on 8 Aug 2022 under E.O. 13694 as amended, after the mixer had laundered "more than $7 billion" since 2019, including >$455m from the Lazarus Group, >$96m from the Harmony Bridge heist and ≥$7.8m from the Nomad heist [T-A13]. Treasury's stated basis is that mixers "repeatedly failed to impose effective controls designed to stop it from laundering funds for malicious cyber actors" and that they "should in general be considered as high-risk by virtual currency firms" [T-A13]. OFAC re-designated **Garantex Europe OU** and designated successor **Grinex** plus three executives and six companies on 14 Aug 2025 under E.O. 13694 as further amended by E.O. 14144 and 14306 [T-A21]. Garantex is the cleanest model of (b)→(d): it "lost its Estonian license to provide digital asset services" in February 2022 after Estonia's FIU found "critical anti-money laundering and countering the financing of terrorism (AML/CFT) deficiencies" [T-A21]. Also designated: SUEX, Chatex, Bitpapa, NetEx24, AWEX, Cryptex; FinCEN identified PM2BTC [T-A21].

**Registration-by-law is the quiet floor.** FinCEN's 2019 CVC guidance holds that "persons accepting and transmitting CVC are required, like any money transmitter, to register with FinCEN as MSBs" and that registration is due "within 180 days of starting to engage in money transmission" [T-A24]. So a project that takes custody of user funds is an MSB registrant *by rule* — non-registration is a hard fail, not a grey zone.

**Verifiable checks**
- **[D6]** Enumerate every front-end service the project actually offers (trading, fiat on-ramp, fiat off-ramp, custody, staking-as-a-service, lending, order book, OTC desk). For each, name the licence it would require in the project's operating jurisdiction.
- **[D7]** Query the jurisdiction's official register for the named operating entity. Absence is recorded as "entity absent", not as "no violation" — and is scored as its own signal.
- **[D8]** Check whether the project's jurisdiction has a *specific* crypto licensing regime (AU: AFSL+DCE; SG: PSA Major Payment Institution with DPT service; UK: FSMA cryptoasset permissions; EU: CASP authorisation) versus a generic financial-products regime. Generic regimes produce *controversy* (Block Earner) where specific regimes produce *clearances*.
- **[D9]** If the project takes custody or transfers value for users, test the MSB-registration hypothesis directly against the national register.

---

## D3 Verifying "we are compliant"

This is where marketing and reality separate most cleanly, because the claims are each independently checkable against a public artefact.

**Claim taxonomy — four levels, descending assurance:**
1. **Marketing claim** — "MiCA-compliant", "fully regulated", "bank-grade", "SOC 2 certified". Zero evidentiary content.
2. **Self-attestation** — a policy page, an audit-request button, a "Regulatory Compliance" doc. Checkable only as to existence and date.
3. **Third-party attestation** — an auditor's opinion on a *defined scope* with a named auditor, scope statement, period and opinion type. SOC 2 Type I vs II, and ISO/IEC 27001 certificates, are verifiable via the IAF CertSearch tool [T-B5] (which checks that the certification body was accredited by an IAF-recognised accreditation body) — a claim failing that check is a fabricated-certificate signal.
4. **Register entry** — the entity appears in the official register with the *specific permission* claimed. This is the only level that establishes regulatory status.
5. **On-chain proof** — limited to reserves/custody, not to regulatory status. (See below.)

**The decisive MiCA caveat, from ESMA itself:** "The crypto-asset white papers listed in ESMA's register have not been reviewed or approved by any competent authority in any Member State of the European Union. The offeror and/or issuer of the crypto-asset is solely responsible for the content of each crypto-asset white paper" [T-A15]. Therefore: **presence in ESMA's white-paper register is not regulatory approval.** A tool that treats register presence as approval is wrong. The register does contain a separate file of **"Non-compliant entities providing crypto-asset services"** [T-A15] — a direct negative-check resource.

ESMA also warns that the register is republished on **weekly** intervals and that "information that may be reported to ESMA by competent authorities … will not be immediately displayed" [T-A15] — i.e. register absence is a *laggy* negative signal and cannot by itself establish non-compliance. This is a genuine methodological constraint on any scoring tool.

**Register set to query mechanically** [T-B2, T-B3, T-B4, T-A20, T-A15]:
| claim | register / tool | authority |
|---|---|---|
| MiCA CASP authorised | ESMA interim MiCA register, "Crypto-asset service providers" CSV | ESMA (NCAs/EBA supply data) |
| MiCA white paper notified | ESMA interim MiCA register, Title II/III/IV CSVs | ESMA |
| Explicitly non-compliant | ESMA interim MiCA register, "Non-compliant entities" CSV | ESMA |
| UK cryptoasset firm | FCA Register (cryptoasset firms registered under MLR) | FCA |
| SG payment licence (DPT) | MAS Financial Institutions Directory, filter = Digital Payment Token Service; plus MAS list of entities that notified MAS under the PS Act | MAS |
| US money transmitter | FinCEN MSB Registrant Search (BSA ID lookup) | FinCEN |
| US adviser / broker | SEC IAPD (Form ADV); IAPD also queries FINRA BrokerCheck | SEC / FINRA |
| AU financial licence | ASIC licence registers; AUSTRAC DCE register | ASIC / AUSTRAC |
| Entity existence | State/territory Secretary of State corporate registries | state SOS |
| Certificate validity | IAF CertSearch (ISO 27001/SOC-adjacent) | IAF |
| Sanctions status | OFAC SDN list + consolidated sanctions list | OFAC |

**Proof of reserves: a four-step ladder, and an explicit limit.** Marketing claim ("audited") → self-attested on-chain snapshot → third-party attestation at a stated block height with a Merkle root of liabilities → user-verifiable inclusion proof. The structural limitation is that a Merkle-root attestation is a **point-in-time, narrow-scope check on assets held vs. liabilities committed**; it is not a financial audit, does not test off-chain debts, internal controls, or related-party balances, and yields no opinion on solvency. Secondary commentary agrees [T-D3]. **Flagged UNSOURCED:** I could not locate a Tier A/B regulator statement on the sufficiency of proof of reserves; a widely-repeated PCAOB warning on proof-of-reserves reports appears only via secondary/Tier-E citation and is **not** relied on here.

**Verifiable checks**
- **[D10]** For every compliance claim, record the *register* it should appear in, then query it. Score register-hit ≫ attestation ≫ policy page ≫ marketing.
- **[D11]** Specifically query the ESMA "Non-compliant entities" file. A hit is a near-terminal finding.
- **[D12]** Distinguish "registered under MLR" from "authorised under FSMA cryptoasset permissions" — these are different regimes and are commonly conflated in marketing [T-A20].
- **[D13]** For any SOC 2 / ISO 27001 claim, require certificate number + issuing body, then validate via IAF CertSearch. No number = no claim.
- **[D14]** For proof-of-reserves, require: named auditor/third party, statement date, block height, scope language, and whether user inclusion proofs are served. Record which ladder rung is claimed vs evidenced.

---

## D4 Identity, custody and jurisdiction: who runs the front end and who holds the keys

This is the legitimacy question the other slices cannot answer, because a protocol can be perfectly engineered and still be operated by nobody.

**MiCA is intermediarity-triggered, and that cuts both ways.** ESMA's Q&A position: services "provided in a fully decentralised manner without any intermediary are not in scope of the MiCA Regulation" [T-A16], a formulation also carried in ESMA's earlier MiCA consultation material [T-A17]. So a bare protocol can be out of scope — **and its front end is in scope.** MiCA Art. 59(1) means "only legal persons or other undertakings that have been authorised as crypto-asset service providers … may provide crypto-asset services" in the Union [T-A25]. MiCA is therefore a machine for identifying a responsible *person*, and non-custodial-by-design is not a shield for the operator.

**EU establishment and the non-EU firm.** MiCA Art. 61 allows a third-country firm to serve an EU client only "where a client established or situated in the Union initiates at its own exclusive initiative" the provision of the service. Paragraph 1 second subparagraph then forecloses the usual workaround: solicitation of EU clients "regardless of the means of communication used for the solicitation, promotion or advertising in the Union, it shall not be deemed to be a service provided on the client's own exclusive initiative," and this applies "**notwithstanding any contractual clause or disclaimer purporting to state otherwise**" [T-A26]. ESMA's Reverse Solicitation Guidelines (Final Report, 17 Dec 2024) confirm the exemption "should be understood as very narrowly framed and as such must be regarded as the exception; and it cannot be assumed, nor exploited to circumvent MiCA" [T-A25]. **Scoring consequence: a Terms-of-Service clause saying "we do not provide services in the EU" is not a jurisdictional fact.** It is explicitly legislated against. Judge behaviour, not the disclaimer.

Since the **1 July 2026** end of the MiCA transitional period, ESMA (23 June 2026) requires unauthorised CASPs to "immediately stop onboarding new EU clients … and cease marketing activities and solicitation," to limit services to orderly exit, and to maintain "customer due diligence measures, transaction monitoring, **screening against restrictive measures and sanctions lists**, suspicious transaction and activity reporting, record-keeping" throughout. It further states that CASPs established outside the EU "cannot provide MiCA services to EU clients or solicit EU clients … **This also applies in a business-to-business context**" — and that "MiCA prohibits CASPs from outsourcing or delegating certain services, notably custody, to entities that are not authorised as CASPs" [T-A27]. ESMA directs clients to "verify whether their provider is authorised under MiCA in the ESMA Register" and to move assets to an authorised CASP or "to a self-hosted wallet" [T-A27].

**Custodial vs non-custodial, and who bears liability.** Legal analysis of self-custody wallet platforms argues that without "temporary possession and control over terms," and where "no more than that a dealer acts at his own risk," a platform "connecting the buyer or seller of a digital asset with a DEX and never holding custody" is closer to a finder than a broker [T-D2]. That is advocacy, not holding — treat as CONTESTED. Against that, three hard constraints:
1. **FinCEN**: custodial operators/exchangers that accept and transmit value are MSBs by definition and must register [T-A24]. Non-custodial marketing does not defeat a factual custody analysis.
2. **ESMA**: delegation of custody to an unauthorised entity is prohibited [T-A27]. Compliance cannot be outsourced into a gap.
3. **CFTC Ooki DAO**: ring-fencing into an unincorporated association did not defeat CEA "person" status [T-A14]. Liability follows the public face, not the code.
4. **ASIC's Block Earner** lesson is about substance over labels: the High Court "focused on the underlying arrangements and contractual substance of Earner, rather than how it was labelled or marketed" [T-A23]. This is the general principle for identity scoring: descriptions of a project in its own marketing are not evidence of its legal structure.

**Identifying the operating entity mechanically.** The chain is: (a) ToS/Privacy Policy/Impressum named legal person + registered address + company number; (b) domain registrant via **RDAP** (ICANN's Replacement for WHOIS) under the ICANN Registration Data Policy [T-B6] — but note that since GDPR-era redaction, registrant personal data is suppressed by default, so **a redacted WHOIS record is not evidence of anonymity**; (c) corporate-registry verification of the named person at the state/territory SOS [T-B7]; (d) corporate registry of the **actual** jurisdiction (a project named in ToS as "Cayman Islands" or "Wyoming" is offering no jurisdictional substance); (e) payment-processor and banking counterparties, which are the real chokepoint. MiCA reinforces (a): the mandatory white-paper disclosure items include "Name; Legal form; Registered address and head office, where different; Date of the registration; Legal entity identifier or another identifier" [T-A18b].

**Verifiable checks**
- **[D15]** Extract the named legal entity from ToS + privacy policy. If none, record "no named operator."
- **[D16]** Query the jurisdiction's corporate registry for that entity: registered? status active? directors? incorporation date? A registry-confirmed operator is the single strongest legitimacy signal in this slice.
- **[D17]** If the named entity is not in the EU but the frontend geo-accesses or targets EU users: flag. MiCA Art. 61 + the anti-disclaimer rule means ToS language cannot cure this [T-A26].
- **[D18]** Determine custody empirically: are there deposit addresses controlled by the operator? Is there a staking/lending balance sheet? Does the frontend hold keys? Each "yes" pulls the operator into AML/KYC regimes regardless of "non-custodial" branding.
- **[D19]** Check whether the project is an *operated service* or *bare code*. ESMA's own line — fully decentralised, no intermediary = out of scope — makes "no operator" a distinguishable legal state. Flag as "operator unattributable" rather than assuming legitimacy.

---

## D5 Sanctions, AML and geographic red flags

**OFAC exposure is strict liability, so "OFAC-compliant" is a legal conclusion, not a certification.** Treasury's own risk assessments state that "OFAC sanctions compliance works on a strict liability standard" [T-A28]. A project claiming "OFAC-compliant" is asserting it has discharged a strict-liability standard — which requires demonstrable screening. Verifiable ask: named screening provider, screening of depositor addresses vs the SDN and consolidated lists, documented escalation policy, and whether the published *Framework for OFAC Compliance Commitments* (2 May 2019) — which is **voluntary** guidance, not a safe harbour — is actually adopted. Absent that, the claim is unverifiable marketing.

**Counterparty exposure is where projects actually fail.** The pattern in every named case is exposure to a dirty counterparty, not direct bad intent: Garantex "has received millions of dollars in cryptocurrency directly from the proceeds of various Russia-linked ransomware attacks, including those involving the Conti, Black Basta, LockBit, NetWalker, and Phoenix Cryptolocker ransomware variants" [T-A21]; Tornado Cash laundered Lazarus/Harmony/Nomad proceeds [T-A13]. After Estonia's action, Garantex "developed infrastructure intended to prevent financial institutions from attributing cryptocurrency wallet addresses back to the exchange" [T-A21] — i.e. deliberate attribution-blocking. **Scoring signal: attribution-resistance engineering is itself a red flag**, independent of what it was built for.

**Base rates** (Tier C, Chainalysis 2026 Crypto Crime Report) [T-A29, T-A30]:
- Illicit addresses received **≥$154bn in 2025**, +162% YoY, a record — driven by a **+694%** increase in value received by **sanctioned entities ($104bn)**.
- Even excluding sanctioned entities, 2025 would still be a record year for crypto crime.
- Illicit share of all attributed crypto volume **remains below 1%** — important: sector-wide illicit rates are *not* a project-level legitimacy discriminator, and should not be used as one.
- **Stablecoins = 84%** of all illicit transaction volume.
- Nation-state: DPRK stole **>$2bn** in 2025 (Bybit ~$1.5bn); Russia's ruble-backed **A7A5** moved **$93.3bn** in under a year; Grinex ≥$4.76bn and Meer $305m in 2025, both sanctioned partly for A7A5 facilitation; IRGC/proxy networks >$3bn.
- Rising infrastructure layer: "full-stack illicit infrastructure providers" — "domain registrars, bulletless hosting services" — which is directly relevant to domain/provider choices (D4e).

**Tornado Cash delisting (March 2025)** — Chainalysis reports OFAC "formally delisted decentralized, non-custodial mixer Tornado Cash from its SDN List following a court ruling that its autonomous smart contracts could not be treated as property subject to sanctions" [T-A30]. The underlying court ruling was **not retrieved** — mark the *reason* UNSOURCED; the fact of delisting is Tier C corroboration of a Tier A event. Its significance for scoring: designation and delisting can both happen, so a **single SDN-list snapshot is not stable evidence**; time-series the list.

**Geo-blocking / DNS / frontend jurisdiction signals.** Methodology note: I did **not** find a Tier A/B authority that prescribes frontend geofencing as a compliance test. These are therefore **UNSOURCED-as-legal-test / legitimate-as-inference** signals, and must be labelled as such in the tool: (i) which countries the frontend serves vs blocks — a project blocking only US/UK while serving sanctioned jurisdictions is disclosing its own risk model; (ii) DNS hosting / CDN jurisdiction (Chainalysis flags "bulletproof hosting" and rogue registrars as an illicit infrastructure layer [T-A29]); (iii) whether there is a distinct entity or subdomain per regulated market; (iv) whether front-end copy changes by geography (a common sign of a geo-specific regulated entity behind one brand).

**AML floor.** Travel Rule: Regulation (EU) 2023/1113 applies **from 30 December 2024**, requires originator/beneficiary information to accompany crypto-asset transfers, and requires PSPs/CASPs to ensure transmission of personal data complies with GDPR [T-A31]. AMLR = Regulation (EU) 2024/1624 [T-A32]. FinCEN's 311 mixer rulemaking (19 Oct 2023 NPRM) targets CVC mixers [T-A33]. FinCEN's enforcement reach is not theoretical: TD Bank consented to a $60m+ civil penalty in Oct 2024 [T-A34] — no, *that* number is **UNSOURCED**; the consent order itself is a Tier A artefact [T-A34].

**Verifiable checks**
- **[D20]** Query the operating entity and its named principals against the OFAC SDN list and Consolidated Sanctions List; do the same for the top 3 disclosed counterparties/custodians. Record the list snapshot date.
- **[D21]** Require named screening vendor + screening scope + documented policy for any "OFAC-compliant" or "sanctions-screened" claim. Absent → unverified.
- **[D22]** Time-series the SDN list: was the project, its entity, or a named counterparty designated *and subsequently delisted*? (Prevents stale-flag false positives.)
- **[D23]** Check whether the protocol's contracts *can* receive from mixer-contract addresses and whether there is any on-chain screening of inflows. No screening at protocol level = structurally unlimited sanctions exposure for any front end built on it.

---

## D6 Data privacy, KYC/AML and the CEX-likeness gradient

**GDPR is not optional paperwork for a wallet-connecting product.** The EDPB's **Guidelines 02/2025 on processing of personal data through blockchain technologies** (final version, 7 July 2026) [T-A35] states: "encrypted personal data is still personal data and encryption does not remove the need for GDPR compliance"; "even state-of-the-art encryption perfectly implemented will be overtaken by time if the blockchain is retained indefinitely"; "As a general rule, storing personal data on a blockchain should be avoided"; and a **DPIA is required prior to implementing a processing using blockchain technology**. It emphasises "the need for Data Protection by Design and by Default," the storage-limitation principle, and "the effective exercise of data subjects' rights such as the right to rectification and the right to be forgotten" — which are architecturally in tension with an immutable ledger [T-A35]. TFR reinforces the data layer: PSPs/CASPs "shall ensure at all times that the transmission of any personal data on the parties involved in a transfer … is conducted in accordance with Regulation (EU) 2016/679" [T-A36].

Scoring consequence: a project running a wallet-connecting frontend, doing address clustering, or linking addresses to identity is processing personal data and needs a lawful basis, a privacy notice naming a controller, retention limits, and a DPIA. Absence of a named controller, absence of a lawful-basis statement, or a policy promising deletion of on-chain-referenced data are all concrete, checkable deficiencies.

**KYC/AML as a CEX-likeness gradient.** The gradient is real and regulatory triggers accumulate: (i) fiat on-ramp → money transmission / e-money / CASP activity [T-A24]; (ii) fiat off-ramp to bank accounts → same, plus Travel Rule [T-A31]; (iii) card issuance or bank-account access → payments/EIOPA-adjacent perimeter; (iv) pooled staking or lending with promised yield → securities/derivative and managed-investment-scheme perimeter (Block Earner: a fixed-yield product was a financial product because "it was sufficient that investors' funds were used or intended to be used to generate a return for both the investor and the issuer" [T-A23]); (v) order book / matching engine with price-time priority → MTF/exchange perimeter (CFTC's Ooki theory: illegally operating as an FCM [T-A14]); (vi) salary in token → employment/tax perimeter. Each rung raises regulatory salience. A tool can count rungs mechanically.

**AML credibility markers.** Minimum credible set: a published sanctions/AML policy naming the screening list; a travel-rule implementation for on-chain transfers; a named MLRO/compliance officer or outsourced regulated compliance provider; a published suspicious-activity reporting process; and, for CEX-like operations, independent testing. **Absent all of these, AML compliance is not "absent" but *unevidenced*** — and in the 2026 NMLRA Treasury notes that "some digital asset service providers, including purportedly decentralized finance (DeFi) services or P2P platforms, may claim not to be regulated financial institutions" [T-A37], i.e. the *claim of non-regulation* is itself a supervisory risk marker.

**The structural argument (important, and it cuts against both "DeFi is unregulated" and "DeFi inherits compliance"):**
*Compliance does not travel down to the protocol, and does not travel up from the protocol.* Four independent supports:
1. **MiCA scope**: fully decentralised, no-intermediary services are out of scope [T-A16]. The protocol is therefore *not* regulated — that is a fact about the protocol, not an exemption for the operator.
2. **MiCA delegation prohibition**: CASPs may not outsource/delegation custody to unauthorised entities [T-A27]. Regulators explicitly anticipated the "we outsourced it, so it's not ours" move and closed it.
3. **ESMA's non-EU/B2B warning**: even providing services to *other businesses* in the EU counts as solicitation [T-A27]. "Our only customers are protocols/corporates" is not a jurisdictional answer.
4. **Entity liability defeats the ring-fence**: Ooki DAO was held suable as an unincorporated association [T-A14], and the SEC's 2025 "finder" theory attaches broker-dealer status to *transaction-based compensation*, i.e. to the fee-taking layer rather than the code [T-D1].

Therefore a scoring tool must evaluate **the operator** and **the protocol** separately and never let one launder the other. A protocol with excellent smart-contract hygiene and an anonymous, unscreened, KYC-free front end is a high-risk counterparty, not a low-risk one. Conversely, an anonymous codebase with an identifiable, licensed, audited CASP operator is a *different* risk profile from an anonymous codebase with an offshore operator.

**Verifiable checks**
- **[D24]** Check for: privacy policy naming a controller; lawful-basis statement; DPIA reference; retention/deletion policy; and consistency of any deletion promise with on-chain immutability (EDPB conflict [T-A35]).
- **[D25]** Check for Travel Rule / origin-and-destination data implementation on transfers (EU-facing) [T-A31].
- **[D26]** Count CEX-likeness rungs (fiat on-ramp, fiat off-ramp, card, pooled yield, matching engine, token salary). Score each; record the highest rung reached.
- **[D27]** Test for the compliance-delegation fallacy: does the project claim compliance by pointing at a *third party's* licence or at the protocol's decentralisation? If yes, require the delegation chain and verify each node is itself authorised [T-A27].
- **[D28]** Test whether the project markets itself as "not regulated" / "not a financial institution." Per the 2026 NMLRA this is itself a flagged pattern [T-A37].

---

## D7 Scoring implications: falsifiable checks

The design rule for this slice: **every negative finding must be labelled as one of (i) positively verified in an official register, (ii) affirmatively absent from an official register after a documented query, or (iii) not checkable.** Never collapse (ii) and (iii).

**Critical scoring constraints derived from the above:**
- **Register absence ≠ violation.** ESMA republishes weekly and lags NCAs' data [T-A15]; FCA/MAS registers have scope boundaries. Score "entity absent" as its own signal with a lag caveat, never as a proof of illegality.
- **Register presence ≠ approval of substance.** ESMA white papers are "not reviewed or approved by any competent authority" [T-A15]. A white-paper entry proves *notification*, nothing more.
- **Industry illicit rate ≠ project signal.** Chainalysis puts illicit share below 1% of attributed volume [T-A29]. Using a sector rate to penalise a specific project is a category error. Use *project-level* counterparty exposure instead.
- **Sanctions lists are non-static.** Tornado Cash was designated (2022) and delisted (2025) [T-A13, T-A30]. Single-snapshot SDN flags produce false positives; require the snapshot date and check delisting.
- **US non-enforcement is not legality.** ~89 cases dropped/frozen in 2025 [T-B1]; the governing instrument is an interpretation plus a *proposed* rule [T-A1, T-A6].
- **Legal conclusions must be evidence-tiered.** An agency's *pending* proposal, a court decision whose primary text I could not retrieve (Winding Tree, Grayscale, CoinShares), and a secondary law-firm analysis must never be scored at the same weight as a statute or a register entry.

**Concrete falsifiable checks, ordered by signal value:**
1. Named legal operator exists in the relevant corporate registry (strongest single signal).
2. Named legal operator appears in the specific regulatory register for the permission claimed — AND the permission scope matches the activity performed.
3. Named legal operator is absent from the ESMA "Non-compliant entities" file.
4. Named legal operator is absent from the OFAC SDN list as of a recorded snapshot date, and absent from delisted-in-past designations.
5. Top disclosed counterparties/custodians are SDN-clear.
6. The project states a token category and the stated category survives a 33-11412 read-through.
7. Custodial fact-pattern is established or excluded empirically (deposit addresses, key custody, staking balance sheet).
8. Geo-jurisdiction facts (which markets are served) contradict ToS exclusionary clauses.
9. Third-party attestations exist with verifiable identifiers (IAF CertSearch; named auditor; scope; period).
10. Proof of reserves reaches at least rung 3 (third-party, dated, scoped) — never score a marketing word "audited."

**Verifiable checks**
- **[D29]** Every check above must emit a structured record: `{check_id, query_performed, source_register, source_url, snapshot_date, result: hit|miss|not_checkable, evidence_tier}`. A check that cannot be logged with a snapshot date is not a valid signal.
- **[D30]** Any score reduction based on register absence must be capped (e.g. ≤ the weight of "not checkable") until a second, lag-aware re-query confirms it.

---

## Sources

| Ref | Tier | Title | URL | Date accessed |
|---|---|---|---|---|
| T-A1 | A | SEC Fact Sheet, *Application of the Federal Securities Laws to Certain Types of Crypto Assets and Certain Transactions Involving Crypto Assets* (Rel. 33-11412, 17 Mar 2026) | https://www.sec.gov/files/33-11412-fact-sheet.pdf | 2026-10-06 |
| T-A2 | A | SEC Commission Interpretation 33-11412 (full PDF) | https://www.sec.gov/files/rules/interp/2026/33-11412.pdf | 2026-10-06 |
| T-A3 | A | SEC Press Release 2026-30, *SEC Clarifies the Application of Federal Securities Laws to Crypto Assets* (17 Mar 2026) | https://www.sec.gov/newsroom/press-releases/2026-30-sec-clarifies-application-federal-securities-laws-crypto-assets | 2026-10-06 |
| T-A4 | A | SEC Press Release 2026-76 + Proposed Rule 33-11434, *Regulation Crypto Assets* (18 Aug 2026) | https://www.sec.gov/newsroom/press-releases/2026-76-sec-proposes-new-regulation-crypto-assets | 2026-10-06 |
| T-A5 | A | H.R. 3633, *Digital Asset Market Clarity Act of 2025* — House Report 119-168 (23 Jun 2025) | https://www.congress.gov/119/crpt/hrpt168/CRPT-119hrpt168.pdf | 2026-10-06 |
| T-A6 | A | SEC, Statement of Chairman Atkins on Regulation Crypto Assets — "support Congress in delivering the CLARITY Act" (18 Aug 2026) | https://www.sec.gov/newsroom/speeches-statements/atkins-statement-regulation-crypto-assets-081826 | 2026-10-06 |
| T-A7 | A | SEC Proposed Rule 33-11434, *Regulation Crypto Assets* (full PDF) | https://www.sec.gov/files/rules/proposed/2026/33-11434.pdf | 2026-10-06 |
| T-A8 | A | SEC Press Release 2025-47, *SEC Announces Dismissal of Civil Enforcement Action Against Coinbase* (27 Feb 2025) | https://www.sec.gov/newsroom/press-releases/2025-47 | 2026-10-06 |
| T-A9 | A | SEC-filed Joint Stipulation of Dismissal, *SEC v. Coinbase* (2025) | https://www.sec.gov/files/litigation/complaints/2025/stipulation-pr2025-47.pdf | 2026-10-06 |
| T-A10 | A | SEC Litigation Release 26278, *Payward, Inc. and Payward Ventures, Inc. (d/b/a Kraken)* — dismissed with prejudice (27 Mar 2025) | https://www.sec.gov/enforcement-litigation/litigation-releases/lr-26278 | 2026-10-06 |
| T-A11 | A | SEC-filed Stipulation of Dismissal, *SEC v. Binance* (29 May 2025) | https://www.sec.gov/files/litigation/litreleases/2025/stipulation-dismissal-26316.pdf | 2026-10-06 |
| T-A12 | A | SEC Newsroom, SEC Crypto Task Force chairman letter, 13 Mar 2025 (context for CASP/discovery posture) | https://www.sec.gov/files/ctf-input-andreesen-horowitz-2025-03-13.pdf | 2026-10-06 |
| T-A13 | A | US Treasury / OFAC, *U.S. Treasury Sanctions Notorious Virtual Currency Mixer Tornado Cash* (8 Aug 2022) | https://home.treasury.gov/news/press-releases/jy0916 | 2026-10-06 |
| T-A14 | A | CFTC Press Release 8822-23, *CFTC Releases FY 2023 Enforcement Results* (7 Nov 2023) — Ooki DAO "person"/unincorporated association holding; 47 digital-asset actions | https://www.cftc.gov/PressRoom/PressReleases/8822-23 | 2026-10-06 |
| T-A15 | A/B | ESMA, *Markets in Crypto-Assets Regulation (MiCA)* — Interim MiCA Register (5 CSVs incl. Non-compliant entities); last update 30 Sep 2026 | https://www.esma.europa.eu/esmas-activities/digital-finance-and-innovation/markets-crypto-assets-regulation-mica | 2026-10-06 |
| T-A16 | A | ESMA Q&A on MiCA scope (Apr 2026): fully decentralised services without an intermediary out of scope | https://www.esma.europa.eu/print/view/pdf/esma_q_a_search_page/page_2?&page=75 | 2026-10-06 |
| T-A17 | A | ESMA75-453128700-438, *Second Consultation Paper on MiCA* (5 Oct 2023) | https://www.esma.europa.eu/sites/default/files/2023-10/ESMA75-453128700-438_MiCA_Consultation_Paper_2nd_package.pdf | 2026-10-06 |
| T-A18 | A | DOJ SDNY, *Tornado Cash Founders Charged With Money Laundering and Sanctions Violations* (23 Aug 2023) | https://www.justice.gov/usao-sdny/pr/tornado-cash-founders-charged-money-laundering-and-sanctions-violations | 2026-10-06 |
| T-A18b | A/B | ESMA, *Disclosure items for the crypto-asset white paper* (name, legal form, registered address, LEI) | https://www.esma.europa.eu/publications-and-data/interactive-single-rulebook/mica/disclosure-items-crypto-asset-white-paper-e | 2026-10-06 |
| T-A19 | A* | DOJ SDNY, *Founder of Tornado Cash Crypto Mixing Service Convicted of Knowingly Transmitting Criminal Proceeds* (6 Aug 2025) — verdict before Judge Katherine Polk Failla | https://www.justice.gov/usao-sdny/pr/founder-tornado-cash-crypto-mixing-service-convicted-knowingly-transmitting-criminal | 2026-10-06 |
| T-A20 | A/B | FCA, *Cryptoasset firms: Authorisation, supervision and enforcement* (final rules 30 Jun 2026; applies to firms authorised on/after 25 Oct 2027) | https://www.fca.org.uk/firms/new-regime-cryptoasset-regulation/authorisation-supervision-enforcement | 2026-10-06 |
| T-A21 | A | US Treasury / OFAC, *Treasury Sanctions Cryptocurrency Exchange and Network Enabling Sanctions Evasion and Cyber Criminals* — Garantex re-designation, Grinex, executives, six companies (14 Aug 2025) | https://home.treasury.gov/news/press-releases/sb0225 | 2026-10-06 |
| T-A22 | A | ASIC Media Release 25-194MR, *High Court grants ASIC special leave to appeal Block Earner decision* (5 Sep 2025) | https://www.asic.gov.au/about-asic/news-centre/find-a-media-release/2025-releases/25-194mr-high-court-grants-asic-special-leave-to-appeal-block-earner-decision | 2026-10-06 |
| T-A23 | A | ASIC Media Release 26-124MR, *ASIC successful in High Court Block Earner appeal* — unanimous 7-0; Corporations Amendment (Digital Assets Framework) Act 2026 | https://www.asic.gov.au/about-asic/news-centre/find-a-media-release/2026-releases/26-124mr-asic-successful-in-high-court-block-earner-appeal | 2026-10-06 |
| T-A24 | A | FinCEN, *Guidance on Application of FinCEN's Regulations to Certain Business Models Involving Convertible Virtual Currencies* (FIN 2019-G001, 9 May 2019) — MSB registration within 180 days | https://www.fincen.gov/system/files/2019-05/FinCEN%20CVC%20Guidance%20FINAL.pdf | 2026-10-06 |
| T-A25 | A | ESMA35-1872330276-1899, *Final Report on the Guidelines on reverse solicitation under MiCA* (17 Dec 2024) — Art. 59(1) authorisation requirement | https://www.esma.europa.eu/sites/default/files/2024-12/ESMA35-1872330276-1899_-_Final_report_on_GLs_on_reverse_solicitation_under_MiCA.pdf | 2026-10-06 |
| T-A26 | A | Regulation (EU) 2023/1114 (MiCA), Article 61 — provision of services at client's exclusive initiative; anti-disclaimer rule | https://www.esma.europa.eu/publications-and-data/interactive-single-rulebook/mica/article-61-provision-crypto-asset-services | 2026-10-06 |
| T-A27 | A | ESMA Public Statement ESMA75-113276571-1710, *ESMA calls on unauthorised CASPs to wind down… as MiCA transitional period ends* (23 Jun 2026) | https://www.esma.europa.eu/sites/default/files/2026-06/ESMA75-113276571-1710_Public_Statement_MiCA_transitional_period_ends.pdf | 2026-10-06 |
| T-A28 | A | US Treasury, *2024 National Proliferation Financing Risk Assessment* — "OFAC sanctions compliance works on a strict liability standard" | https://home.treasury.gov/system/files/136/2024-National-Proliferation-Financing-Risk-Assessment.pdf | 2026-10-06 |
| T-A29 | C | Chainalysis, *2026 Crypto Crime Report — Introduction* (8 Jan 2026): ≥$154bn illicit 2025, +162%, sanctioned +694%, <1% of attributed volume, stablecoins 84% | https://www.chainalysis.com/blog/2026-crypto-crime-report-introduction/ | 2026-10-06 |
| T-A30 | C | Chainalysis, *Crypto Sanctions: 2026 Crypto Crime Report* (5 Mar 2026): A7A5 $93.3bn; Grinex/Meer; Tornado Cash SDN delisting Mar 2025 | https://www.chainalysis.com/blog/crypto-sanctions-2026/ | 2026-10-06 |
| T-A31 | A | Regulation (EU) 2023/1113 (TFR) — EUR-Lex summary; applies from 30 Dec 2024 | https://eur-lex.europa.eu/EN/legal-content/summary/information-accompanying-transfers-of-funds-and-certain-crypto-assets.html | 2026-10-06 |
| T-A32 | A | Regulation (EU) 2024/1624 (AMLR) | https://eur-lex.europa.eu/eli/reg/2024/1624/oj/eng | 2026-10-06 |
| T-A33 | A | FinCEN, *Requests for Information on Existing Registrant Status Regarding Money Services Business Activities Relating to Mixing* (311 NPRM, 19 Oct 2023) | https://www.fincen.gov/system/files/federal_register_notices/2023-10-19/FinCEN_311MixingNPRM_FINAL.pdf | 2026-10-06 |
| T-A34 | A | FinCEN, *TD Bank Consent Order, Number 2024-02* (10 Oct 2024) | https://www.fincen.gov/system/files/enforcement_action/2024-10-10/FinCEN-TD-Bank-Consent-Order-508FINAL.pdf | 2026-10-06 |
| T-A35 | A | EDPB, *Guidelines 02/2025 on processing of personal data through blockchain technologies* (final version, 7 Jul 2026) | https://www.edpb.europa.eu/documents/guideline/guidelines-022025-on-processing-of-personal-data-through-blockchain_en | 2026-10-06 |
| T-A36 | A | Regulation (EU) 2023/1113, Art. — personal data transmitted in accordance with GDPR | https://eur-lex.europa.eu/eli/reg/2023/1113/oj/eng | 2026-10-06 |
| T-A37 | A | US Treasury, *2026 National Money Laundering Risk Assessment* (2026-NMLRA) — DASPs "may claim not to be regulated financial institutions" | https://home.treasury.gov/system/files/246/2026-NMLRA.pdf | 2026-10-06 |
| T-A38 | A | US Treasury, *Illicit Finance Risk Assessment of Decentralized Finance* (DeFi-Risk-Full-Review) | https://home.treasury.gov/system/files/136/DeFi-Risk-Full-Review.pdf | 2026-10-06 |
| T-A39 | A | US Treasury, *Report to Congress from the Secretary of the Treasury* on GENIUS Act illicit finance (Mar 2026) | https://home.treasury.gov/system/files/246/GENIUS-Act-Illicit-Finance-Innovation-Congressional-Report-March-2026.pdf | 2026-10-06 |
| T-A40 | A | SEC, Statement on President Trump Signing the GENIUS Act into Law (18 Jul 2025) | https://www.sec.gov/newsroom/speeches-statements/atkins-statement-genius-act-071825 | 2026-10-06 |
| T-A41 | A | SEC, *Frequently Asked Questions Relating to Crypto Asset Activities Distributed Ledger Technology* (15 May 2025) — references GENIUS Act, 12 U.S.C. 5901 et seq. | https://www.sec.gov/rules-regulations/staff-guidance/trading-markets-frequently-asked-questions/frequently-asked-questions-relating-crypto-asset-activities-distributed-ledger-technology | 2026-10-06 |
| T-A42 | A/B | Regulation (EU) 2023/1114 (MiCA) full text (EUR-Lex) | https://eur-lex.europa.eu/legal-content/EN/TXT/PDF/?uri=CELEX:32023R1114 | 2026-10-06 |
| T-A43 | A | ASIC Information Sheet 225 (INFO 225), *Digital assets: Financial products and services* | https://www.asic.gov.au/regulatory-resources/digital-transformation/digital-assets-financial-products-and-services | 2026-10-06 |
| T-A44 | A | MAS Media Release, *MAS Strengthens Regulatory Measures for Digital Payment Token Services* (23 Nov 2023) | https://www.mas.gov.sg/news/media-releases/2023/mas-strengthens-regulatory-measures-for-digital-payment-token-services | 2026-10-06 |
| T-A45 | A | CFTC Press Releases 9059-25 / 9060-25, withdrawal of Staff Advisories on virtual currency (28 Mar 2025) | https://www.cftc.gov/PressRoom/PressReleases/9059-25 | 2026-10-06 |
| T-B1 | B | Registrant statements on SEC EDGAR: SEC closed Uniswap Labs and OpenSea investigations (Feb 2025); ~89 crypto enforcement cases dropped/frozen in 2025 | https://www.sec.gov/Archives/edgar/data/2078856/000119312526272886/d50758ds1a.htm | 2026-10-06 |
| T-B2 | B | MAS Financial Institutions Directory (filter: Digital Payment Token Service) | https://eservices.mas.gov.sg/fid/institution?category=Major+Payment+Institution&activity=Digital+Payment+Token+Service | 2026-10-06 |
| T-B3 | B | FinCEN *MSB Registrant Search* (BSA ID / entity lookup; ~39,713 registered MSBs) | https://www.fincen.gov/resources/msb-state-selector | 2026-10-06 |
| T-B4 | B | SEC *Investment Adviser Public Disclosure* (IAPD; Form ADV + FINRA BrokerCheck) | https://adviserinfo.sec.gov/ | 2026-10-06 |
| T-B5 | B | IAF CertSearch — ISO/IEC 27001 certificate validation | https://www.iafcertsearch.org/ | 2026-10-06 |
| T-B6 | B | ICANN *Registration Data Policy* and *RDAP* (registrant-data redaction post-GDPR) | https://www.icann.org/en/contracted-parties/consensus-policies/registration-data-policy | 2026-10-06 |
| T-B7 | B | State Secretary of State corporate registries (entity-existence verification) | https://bizfileonline.sos.ca.gov/search | 2026-10-06 |
| T-B8 | B | FCA Register — cryptoasset firms registered under the Money Laundering Regulations (entry format) | https://register.fca.org.uk/s/firm?id=001b000003O1uMmAAJ | 2026-10-06 |
| T-B9 | B | ESMA *Databases and Registers* (MiCA registers under Arts. 109–110) | https://www.esma.europa.eu/publications-and-data/databases-and-registers | 2026-10-06 |
| T-D1 | D | Wilson Sonsini, *No Commission Without Permission* (3 Mar 2025) — transaction-based compensation as hallmark of broker-dealer activity; citing Rel. 102230, 102174, 102175, 102176 | https://www.wsgr.com/en/insights/no-commission-without-permission-sec-reinforces-focus-on-sales-activities-and-transaction-based-compensation-as-hallmarks-of-broker-dealer-status-in-recent-settlements.html | 2026-10-06 |
| T-D2 | D | *Business & Finance Law Review* note, J. Berkun — self-custody wallet platforms and the "finder's exception" (analytic, not a holding) | https://gwbflr.org/wp-content/uploads/2025/05/J.Berkun-Note_FINAL.pdf | 2026-10-06 |
| T-D3 | D | AGIO Ratings, *Why Proof of Reserves Is Not Enough for Crypto Counterparty Risk* (4 Jul 2026) — PoR limitations | https://www.agioratings.io/insights/why-proof-of-reserves-is-not-enough-for-crypto-counterparty-risk | 2026-10-06 |

**UNSOURCED / CONTESTED inventory**
- **UNSOURCED:** *SEC v. Winding Tree LLC* (D.C. Cir.) — opinion text not retrieved; holding must not be asserted.
- **UNSOURCED:** *SEC v. Grayscale Investments* (D.C. Cir. 2024); *SEC v. CoinShares Capital Markets* (3d Cir. 2023) — primary texts not retrieved.
- **UNSOURCED:** Ooki DAO penalty amount; CFTC default-judgment date (only the FY2023 results release and the outcome were confirmed).
- **UNSOURCED:** DOJ 6 Aug 2025 Storm conviction — page body bot-protected; only index metadata (title/court/judge/date) confirmed.
- **UNSOURCED:** court ruling underlying the March 2025 Tornado Cash SDN delisting (reported only via Tier C).
- **UNSOURCED:** any Tier A/B authority endorsing proof-of-reserves as evidence of solvency; and no Tier A/B source for the widely-cited PCAOB caution.
- **UNSOURCED-as-legal-test:** geo-blocking / DNS / frontend jurisdiction signals have no Tier A/B authority prescribing them; they are inference-grade only.
- **UNSOURCED:** exact GENIUS Act stablecoin eligibility criteria (only the Act's citation and the SEC's paraphrase were retrieved, not § 4 text).

---

## Named enforcement cases

| case | authority | date | outcome | lesson for scoring |
|---|---|---|---|---|
| **SEC v. Coinbase, Inc.** | SEC / SDNY | dismissed 27 Feb 2025 (Press Release 2025-47; joint stipulation filed) | Case dismissed (with prejudice per SEC stipulation) | Non-enforcement by the SEC is a *policy posture*, not a legality signal. Absence of a US securities charge cannot be read as an all-clear. |
| **SEC v. Payward Inc. (Kraken)** | SEC | Litigation Release 26278, 27 Mar 2025 | Dismissed with prejudice | Same; reinforces that enforcement outcomes are reversible within months. |
| **SEC v. Binance Holdings et al.** | SEC | complaint dismissed 29 May 2025 per filed stipulation (following June 2023 resolution) | Dismissed | A settled-and-dismissed case still leaves a disclosed regulatory history; retain the historical record even when the case closes. |
| **SEC investigations of Uniswap Labs and OpenSea** | SEC | closed without action, announced Feb 2025 | No enforcement action | Protocol-developer investigations *were* opened, i.e. protocol developers are reachable. A closed investigation leaves no forward-looking precedent. |
| **CFTC v. Ooki DAO** | CFTC / N.D. Cal. | order 7 Sep 2023; default judgment thereafter | Court held the DAO could be sued and served as an unincorporated association and is a **"person"** under the CEA; found it violated the law as charged | A DAO ring-fence does not defeat entity liability. Score the *aggregator/front-end operator* as the public face of the protocol for liability purposes. |
| **US v. Roman Storm (Tornado Cash)** | DOJ SDNY (Judge Failla) | indicted 23 Aug 2023; **convicted 6 Aug 2025** | Convicted of conspiring to operate an **unlicensed money transmitting business** | The unlicensed-MTB charge is the durable criminal exposure for infrastructure even where sanctions and money-laundering theories fail. |
| **OFAC v. Tornado Cash SDN designation** | US Treasury / OFAC | 8 Aug 2022 (E.O. 13694 as amended) | Sanctioned; ≥$7bn laundered since 2019 incl. Lazarus, Harmony, Nomad proceeds. SDN **delisting reported Mar 2025** | Designation risk attaches to *function* (mixing/obfuscation without controls), and list status is reversible — time-series SDN data, do not snapshot. |
| **OFAC v. Garantex Europe OU / Grinex / 3 executives / 6 companies** | US Treasury / OFAC (with Secret Service, FBI, State) | re-designated 14 Aug 2025 (E.O. 13694 as amended by E.O. 14144, 14306); DOJ indicted executives 7 Mar 2025 | Sanctioned; Garantex had **lost its Estonian digital-asset licence in Feb 2022** after Estonia's FIU found critical AML/CFT deficiencies; built attribution-blocking infrastructure post-designation | Textbook (b)→(d) progression. Counterparty design to prevent attribution is itself a scoring signal. |
| **ASIC v. Block Earner (Web3 Ventures Pty Ltd)** | High Court of Australia (on ASIC appeal from Full Federal Court) | proceedings 23 Nov 2022 → Federal Court 9 Feb 2024 → Full Federal Court 22 Apr 2025 → **High Court heard 12 Mar 2026, unanimous 7-0** | High Court held the "Earner" fixed-yield product **was** a financial product requiring an AFSL; focusing on "contractual substance … rather than how it was labelled or marketed" | Substance beats labelling. A yield feature can pull an entire product into the regulated perimeter regardless of "on-chain"/"DeFi" branding; regulators win even after a first-instance loss. |
| **ASIC v. Block Earner (Federal Court penalty stage)** | Federal Court of Australia | 9 Feb 2024; penalty relief 4 Jun 2024; appeal heard 6 Mar 2025; matter returned to Full Federal Court for ASIC's penalty appeal (post-High Court) | Unlicensed financial services conduct **and** operating an unregistered managed investment scheme found at first instance | The **unregistered managed investment scheme** theory is an independent hook against pooled-yield DeFi products. |
| **FinCEN v. TD Bank N.A.** | FinCEN | consent order 10 Oct 2024 (No. 2024-02) | Civil enforcement; BSA/AML consent order | The BSA reaches conventionally licensed banks too — registration/registration-adjacency is not a proxy for compliance. |
| **EU: unauthorised CASPs after MiCA transition** | ESMA + NCAs | transitional period ended **1 Jul 2026**; ESMA statement 23 Jun 2026 | Unauthorised CASPs must stop onboarding new EU clients and cease marketing; non-EU CASPs cannot provide MiCA services to or solicit EU clients, including B2B; delegation of custody to unauthorised entities prohibited | From 1 Jul 2026 EU-facing operation without authorisation is a dated, documented breach state — a clean, high-value binary check. |

---

## Scoring signals extracted

| signal_id | what it measures | how to verify mechanically | evidence tier | failure mode | confidence |
|---|---|---|---|---|---|
| D-S01 | Named legal operator exists and is active in its home corporate registry | Extract entity name + company number from ToS/privacy page; query the state/territory/ national registry; check status, directors, incorporation date | B | Entity named but registry-dormant, or a nominee/agent address; "Cayman/Wyoming/Delaware LP" with no substance | High |
| D-S02 | Named entity appears in the *specific* regulatory register for the permission claimed | Query ESMA CASP CSV, FCA Register, MAS FID (DPT filter), FinCEN MSB search, SEC IAPD/FINRA, ASIC/AUSTRAC | A/B | Register hit for a *different* permission or scope (MLR-registered ≠ FSMA-authorised) conflated with the claim | High |
| D-S03 | Named entity absent from ESMA "Non-compliant entities providing crypto-asset services" | Download the 5th CSV from the ESMA interim MiCA register; match on legal name and LEI | A | Miss (false negative) if a non-EU entity offers services without an EU NCA filing; ESMA data is NCAs' inputs | Medium-High |
| D-S04 | Named entity clear of the OFAC SDN list | Query SDN + Consolidated Sanctions List by name and principals; record snapshot date | A | Miss on transliteration / alternate spelling / non-US nexus; stale lists produce false positives | High |
| D-S05 | Sanctions exposure via counterparties (custodians, payment processors, market makers) | Query SDN for each disclosed third-party counterparty; check guaranteed-fiat-ofacied/settlement partners | A | Counterparties undisclosed → cannot clear; treat undisclosed as unresolved, not clear | Medium-High |
| D-S06 | Past designation / subsequent delisting of the entity, a principal or a counterparty | Time-series SDN delisting notices back to project inception, not just a live snapshot | A | Single-snapshot flags Tornado Cash as currently sanctioned when it was delisted in Mar 2025 → false positive | High |
| D-S07 | Token classification is stated by the project | Extract the project's own asserted legal characterisation; map to the five 33-11412 categories; record which element is missing | A | Project claims "not a security" with no category; or claims CLARITY Act / Regulation Crypto Assets status that is not law | High |
| D-S08 | Investment-contract overlay: essential managerial efforts promised | Extract 3–5 concrete team promises; test whether each is completed or abandoned per 33-11412's "ceases to be subject" mechanic | A | Subjective line-drawing; thresholds for "reasonably expect to derive profits" are **CONTESTED** — score as range, not boolean | Medium |
| D-S09 | Regulatory status asserted from a pending or unenacted instrument | Search project legal copy for "CLARITY Act", "GENIUS Act", "Regulation Crypto Assets", "safe harbor"; check status against enacted statute + rulemaking docket | A | Safe-harbor claim based on a *proposed* rule (33-11434, comment period open from Aug 2026) | High |
| D-S10 | Custodial fact pattern established or excluded | Detect operator-controlled deposit addresses, per-user staking/ledger balances, key-custody language, withdrawal-pool addresses on-chain | A/B (on-chain facts) | "Non-custodial" branding while operating pooled staking/lending → FinCEN CVC guidance makes operator an MSB by fact | High |
| D-S11 | CEX-likeness rung (fiat on-ramp / fiat off-ramp / card / pooled yield / matching engine / token salary) | Feature-scan frontend and ToS; count rungs; map each to the licence it would require | A (framework) + D (feature inference) | Feature detection misses API-only or invite-gated functionality; evidence tier honestly D | Medium |
| D-S12 | Frontend serves jurisdictions the ToS disclaims | Compare ToS exclusionary clauses with observable geo-serving (language variants, subdomains, marketing copy, geofence behaviour) | A (legal basis: MiCA Art. 61 anti-disclaimer) + D (observation) | Misreading Art. 61 scope (only MiCA CASP services) as a general rule; **CONTESTED** as a general test | Medium |
| D-S13 | Travel Rule / origin-destination data implemented on transfers | Look for originator/beneficiary data fields on withdrawals; EU-facing transfers post-30 Dec 2024 must comply (Reg. 2023/1113) | A | Absence of fields may reflect non-EU scope → scope check required before penalising | Medium-High |
| D-S14 | AML/sanctions programme is evidenced, not asserted | Require named screening provider, screening scope (lists), escalation policy, MLRO/compliance owner, published SAR process, published *Framework for OFAC Compliance Commitments* adoption | A (strict-liability standard) + D (documents) | A policy PDF with no named provider, no list scope, no escalation path = marketing only | High |
| D-S15 | "OFAC-compliant" claim passes verification | Confirm screening vendor + list coverage + documented escalation + strict-liability rationale; test whether the claim covers *on-chain* exposure (protocol-level screening of inflows) | A | Frontend-only screening leaves the protocol unlimited; claim says "OFAC compliant" with no protocol-layer control | Medium-High |
| D-S16 | Protocol-level sanctions screening of inflows | Test whether contract code or an on-chain screening layer can reject/flag mixer-derived inflows; check for documented mixer/blocklist integrations | A (risk assessment: mixers "high-risk") + B (code/config) | No screening = structurally unlimited sanctions exposure for any front end built on it; degrades every downstream user | Medium-High |
| D-S17 | Privacy posture: named controller, lawful basis, DPIA, retention vs immutability | Check privacy policy for controller identity, lawful-basis statement, DPIA reference, retention/deletion policy; flag deletion promises that on-chain data cannot honour (EDPB Guidelines 02/2025) | A | No named controller; promise to erase data that is immutable; no DPIA despite blockchain processing | High |
| D-S18 | Attestation quality ladder reached (marketing → self-attested → third-party → register) | Record rung per claim: require auditor/issuer name, scope statement, period, opinion type; validate ISO 27001 via IAF CertSearch; require register entry for regulatory status | A/B | "Audited" with no auditor named; a certificate number that fails IAF accreditation chain | High |
| D-S19 | Proof of reserves reaches rung ≥3 (third-party, dated, scoped) | Require named third party, statement date, block height, scope language, Merkle root of liabilities, and whether user inclusion proofs are served | C/D (measurement practice) — **no Tier A/B sufficiency authority found (UNSOURCED)** | Treating PoR as a solvency opinion; it is a point-in-time asset-vs-committed-liability check that ignores off-chain debt and internal controls | Medium |
| D-S20 | Compliance-not-delegated: operator, not just protocol, is authorised and accountable | Trace any claimed delegation/outsourcing chain; verify every node is itself authorised (MiCA prohibits delegating custody to unauthorised CASPs); test B2B-only counterparty arguments (ESMA: B2B solicitation counts) | A | "Our only customers are protocols/corporates" or "we outsourced custody" treated as jurisdictional or compliance exemptions | High |
| D-S21 | Project markets itself as unregulated / not a financial institution | Keyword scan marketing + legal pages for "not regulated", "not a financial institution", "purely decentralised so no licence required"; per 2026 NMLRA this is a supervisory risk pattern | A | Genuinely non-intermediary protocols trip this signal falsely → combine with D-S19 evidence of an intermediary operator | Medium-High |
| D-S22 | Jurisdiction substance: named operator jurisdiction matches the markets served | Compare operator's registration jurisdiction, bank/PSP relationships and data-hosting against the user markets; flag "registered in X, serves Y" mismatches | B | Nominee-jurisdiction incorporation (Cayman/Wyoming/Estonia 2019-era pattern) with all operations elsewhere | Medium |
| D-S23 | Enforcement history of the operator, founders and predecessors (any jurisdiction) | Search SEC/DOJ/CFTC/OFAC/FCA/ASIC/MAS enforcement releases and litigation releases by entity and principal names | A | Principal-level history missed because enforcement names a different legal entity than the ToS entity | High |
| D-S24 | Disclaimers vs substance divergence (labels ignored) | Compare ToS self-description against registry data, custody facts and CEX-likeness rungs; ASIC Block Earner standard = underlying contractual substance | A | Sympathetic scoring of well-drafted ToS language that contradicts operational facts | Medium-High |
| D-S25 | Negative findings are correctly typed (hit / miss / not-checkable) with snapshot dates | Emit `{check_id, query, register, url, snapshot_date, result, tier}` per check; cap weight of register-absent until re-queried | A (register lag: ESMA republishes weekly, data lags NCAs) | Treating register absence as proof of illegality, or as equivalent to not-checkable | High |