# Slice C — Token economics, value capture, and "casino mechanics" vs real utility

Research date: 2026-10-06. Scope: economics only. No smart-contract audit, no governance-design critique, no regulatory analysis (other slices own those).

**Central claim of this slice.** A token's price is almost never a function of product usage directly. It is a function of (a) how much float exists, (b) how concentrated that float is, (c) how much attention is being manufactured, and (d) how much of any real cash flow is contractually routed to holders versus to the team, LPs, and validators. "Casino mechanics" is the name for token designs where (c) is the primary economic engine and (d) is approximately zero. The measurable consequence is that the project's headline metrics (TVL, volume, holders, APY, market cap) can be entirely decoupled from whether anyone is actually using the product.

---

## C1. Defining and taxonomizing "casino mechanics"

### Definition

A token exhibits **casino mechanics** when its dominant monetary flow is *recruitment of players* — converting attention, capital, or social-graph position into token demand — rather than *provision of a service* that users pay for because they want the service. Three necessary conditions:

1. **No or de minimis contractual link** between product success and token-holder cash flow (see C2).
2. **The mechanism that grows the metric is itself the monetization**: the airdrop, the points season, the listing, the TGE float scarcity — these events *are* the revenue, paid for by the new buyer's order.
3. **The attracting population is transient by construction.** The economically rational actor participates for one cycle and exits, so the metric necessarily decays. A real product does not have this property.

The CFTC's own framing of pump-and-dump is the canonical legal description: defendants "strategically selected digital assets suitable for their scheme," "secretly accumulated a position… in anticipation of price spikes following" promotion, "pumped… by touting the asset in order to increase demand," then "dumped… by selling it into the inflated demand" [C1]. Note what this describes: not a bug, not a rug, but an *economic design* where the asset was selected *because* it was thin enough to move.

The CFTC customer advisory gives the operational texture: organized chat rooms, a countdown buy signal, an announced coin, a named exchange, "buy and sell cycle… over in less than eight minutes," with fake news about a famous investor planning to pour millions into a small coin or a major bank announcing a partnership [C2]. That last item is directly relevant to slice C7: *fake partnership announcements* are a named, regulator-described pump tool.

### The taxonomy

I built nine mechanics (M1–M9), each with mechanism, the metric it inflates, and the observable tell. The table at the end of this document is the machine-readable version. The load-bearing ones:

**M1 — Reflexive / ponzi loop.** New buyers fund returns to earlier buyers; the token has no cash-flow anchor. Inflates: holder count, market cap, social proof. Tell: realized-PnL distribution where a tiny address cohort captures most profit while the median holder is net-negative. On Polymarket, >70% of ~1.7M addresses realized losses while **<0.04% captured over 70% of $3.7B in realized profits**; 668 addresses with >$1M profit accounted for 71% of gains [C48]. This is the *behavioral* signature of a reflexive asset and is directly measurable on any chain.

**M2 — Emission-funded APY presented as yield.** The "yield" a user earns is newly minted tokens, not fees paid by other users. Inflation at zero is *transfers from non-stakers to stakers plus dilution of everyone else*, not revenue. The distinction matters and is mechanical: fee-funded rewards are users paying for a service (external to the holder base); issuance-funded rewards create nothing and only reallocate claim [C45]. This is the single most common way on-chain "APY" is made to look like a business when it is a marketing budget. The SEC's Saitama complaint is a pure version of this: promoters claimed Saitama "would create wealth for investors through **passive income**" while dumping, and the "applications and platforms they were developing" did not exist [C5].

**M3 — Mercenary-capital incentives (points / liquidity mining / high-FAR TVL subsidies).** Capital is rented at a subsidy rate that exceeds organic yield, participates for exactly as long as the subsidy lasts, and exits on the announcement of the token distribution. Inflates: TVL, "users," volume. Tells: (a) TVL and USD inflows decouple; (b) sharp TVL decline within one or two quarters of the distribution event; (c) APY decomposition showing reward-share dominating base APY.

The EigenLayer case is the best-documented instance: from the March 15 snapshot, staked ETH grew from ~3.14M to ~4.86M in six weeks, growth "largely attributed to the implicit promise of an airdrop scaled to its points program" [C34]; TVL peaked at $20B on June 6 and was down 20% by mid-July as farmers "sought greener pastures," with an analyst attributing outflows to incentives being cut rather than to product failure [C35]. The distributional tell is sharper than the TVL tell: **the top 2,227 depositors — ~2% of depositors — accumulated almost 90% of the points supply**, earmarked against a stakedrop of 15% of total supply [C34]. Points programs that "look" broad are usually capital-weighted.

**M4 — Exchange-listing / TGE-liquidity events as the primary monetization.** In a token whose only mechanism is attention, the listing and the float decision are the product. Tells: valuation multiple on *future* unlocks rather than on current usage (Binance Research found 2024 launches had MC/FDV of **12.3%**, with circulating supply "as low as 6% and none exceeding 20%," and roughly **$155B of tokens scheduled to unlock 2024–2030** — with ~$80B of buy-side liquidity needed just to hold prices flat [C19]); correlated volume spikes on listing dates with no change in on-chain user counts.

**M5 — Paid wash volume / market-maker-as-a-service.** The SEC's October 2024 crackdown is the definitive primary description. Promoters "hired so-called market makers ZM Quant and Gotbit to provide **market-manipulation-as-a-service**, which included generating artificial trading volume"; the bots "at times generated **quadrillions of transactions and billions of dollars of artificial trading volume each day**," and in the Robo Inu matter alone generated "more than $1 million dollars of artificial trading volume each day" [C3][C4]. Kaiko's independent measurement found **30% of all wallets in one Uniswap pool** engaged in wash trading, "later confirmed by the FBI," and flagged volume-to-1%-depth ratios above 100x concentrated on meme coins and low-cap alts [C14]. The pump.fun litigation adds the operational variant: insiders took positions *before* public availability, spread purchases across "bundled" transactions, then a KOL promotion drove retail demand [C7].

**M6 — Thin-liquidity price manipulation as an extractable mechanic.** Where a token is both the collateral and the oracle input, price is a lever rather than a measurement. CFTC v. Eisenberg: MNGO "jumped over 13-fold during a 30-minute span" after purchases on the three exchanges feeding Mango's oracle, and the inflated value was used as collateral to withdraw >$110M [C6]. This matters for scoring because it means *on-chain price-based metrics (TVL in token terms, market cap, P/E on price)* can be attacker-manipulated, and thin book depth is itself the tell.

**M7 — Referral / affiliate / MLM-style economics.** Compensation for recruitment rather than for value delivered. Tells: referral payouts structurally excluded from the platform's own revenue line — DefiLlama's methodology explicitly excludes referral payouts from Fees and caps them at Supply-Side Revenue [C10], so any project reporting "revenue" that nets out referral commissions is reporting a smaller number than it appears; a compensation plan paying in the project's own token converts recruitment into emission.

**M8 — Points-to-airdrop schemes that reward capital, not usage.** The distinguishing feature vs M3 is the *scoring function*: capital-at-rest or cumulative volume rather than distinct actions, distinct counterparties, and duration. Coindesk documented the failure mode directly — points programs became "real money" and were themselves traded and levered, while issuers "rarely confirmed that they were even tied to airdrops" [C36]. Academic measurement of nine major airdrops (1inch, Arbitrum, Arkham, dYdX, ENS, Lido, Optimism, Tornado Cash, Uniswap) found **up to 66% of distributed tokens sold in recipients' first post-claim transfer** — Lido 65.75%, 1inch 58.67%, Optimism 48.21% — with **median transfers per recipient of 1** and **median time between first and last transfer of 0 days** [C16]. If an airdrop's first act is liquidation, the distribution did not create users; it created a one-day selling event.

**M9 — PnL-token / vault schemes where emissions are the only yield source.** A vault or synthetic that only pays because the issuer prints its own token is M2 wearing a product-shaped costume. The tell is definitional: does the vault have any APY component *not* paid in the protocol's own token? DefiLlama tracks this distinction explicitly by separating base APY from reward APY and classifying pools where reward share is material as incentivized rather than organic [C40][C10].

**Verifiable checks (C1)**
- Compute realized PnL per address for the token's whole trading history; report the share of total *positive* realized PnL captured by the top 0.01%/0.1%/1% of addresses. Polymarket reference: <0.04% captured >70% of profits while 70% of addresses lost money [C48].
- Decompose headline APY into base-APY (fees) and reward-APY (emissions). Reject any pool where reward-share > 50%.
- Check the token's price-elasticity of volume: if volume scales with attention events and not with organic usage, M5/M4 are live.
- Compare TVL before/after the distribution event on a per-week basis; a >15% drop within 90 days of a listing/airdrop is the M3 signature (EigenLayer: -20% [C35]).
- For any "passive income" or "yield" claim in marketing, locate the paying counterparty. If the payer is the issuer's treasury or the emissions schedule, classify as M2.
- Check order-book depth: volume/1%-depth above ~100x is a Kaiko-flagged wash-trading pattern, concentrated in memes and low caps [C14].
- Cluster addresses by funding source; measure whether the top 2% of stakers hold ~90% of points/airdrop-eligible balance [C34 pattern].

Sources: [C1][C2][C3][C4][C5][C6][C7][C14][C16][C34][C35][C36][C40][C45][C48]

---

## C2. Value capture — does product success increase token value?

### The measurement problem is a measurement *design* problem

Three Tier C houses now define revenue differently, and the differences are the whole game.

- **DefiLlama**: `Revenue = Fees − Supply-Side Revenue`, then attributed between **Protocol Revenue** and **Holders Revenue**. Fees = "total amount paid by users," including what flows to LPs. Holders Revenue is "the part of protocol revenue that is returned to tokenholders through staking rewards, fee burns, or direct payouts" — the direct analogue of dividends and buybacks. Explicit rules: block rewards and token emissions are *incentives, not fees*; token taxes and referral payouts are not fees; only one listing per flow [C10].
- **Artemis** (Sept 2025, with 10+ protocols/funds): revenue = "value accrued to tokenholders via fees accrued to a treasury, burns, buybacks and burns, or direct fee distributions… derived from the protocol's core operations on a recurring basis," explicitly *prior to* opex and emissions, and explicitly excluding value captured by the team or Foundation. They split `ACTIVE_REVENUE` (claimable by a staker) vs `PASSIVE_REVENUE` (claimable by a plain holder) and restated history — meaning past published revenue numbers for ETH, SOL, CRV, AERO changed [C12].
- **Token Terminal**: revenue is fees *retained by the protocol* (treasury, team, or holders); "earnings" is net value retained after costs [C11].

The operative distinction for a scoring tool is **protocol revenue ≠ token-holder revenue**, and neither equals what a holder earns if the token is only used as a governance/collateral/attention object.

### Why many major tokens capture nothing — the mechanism

CoinGecko's research is the sharpest published statement of the failure mode: "There is no direct value accrual to governance tokens." The structural reason is stated precisely: rewards "are usually distributed to liquidity providers (which is often temporary and unsustainable), or in a few cases governance staking programs" [C37]. And the consequence follows logically — "since the growth of the protocol and its revenue does not usually lead to dividends or rewards, the likelihood of creating a community of short-sighted opportunists is high. Token holders are incentivized to pump the price of the token over everything else, e.g. staking programs that come with rewards but don't actually do anything, aggressive buyback and burn programs, and airdrops."

That is the casino-mechanics argument stated from the demand side: **when there is no value-capture link, the token holder's rational strategy is attention-generation, because attention is the only variable that moves the price.** Value capture is not a "nice to have" feature of a token design; it is the mechanism that determines whether holders behave as users or as promoters.

### Quantified examples of value capture, and of its absence

**Capture is now real but concentrated in a small set of tokens.** DefiLlama's Holders Revenue leaderboard is the cleanest census: Pump.fun ~$24.9M/30d from PUMP buybacks sourced from on-chain burns; Hyperliquid ~$54.3M/30d with "99% of fees go to Assistance Fund for buying HYPE… excluding builders fees"; Uniswap ~$14.5M/30d, and — importantly — the leaderboard records the *dates* on which fee capture switched on: "V1: No revenue for UNI holders. V2: From 28 Dec 2025, 17% (0% before) fees… shared to buy back and burn UNI"; PancakeSwap routes 0.0575% of AMM fees and 40% of StableSwap admin fees to buyback/burn; Jupiter buys back from 50% of platform revenue since 2025-02-17 [C13].

Two things follow. First, **Holders Revenue is a small, enumerable set** — a token absent from this leaderboard has, by definition, no verifiable fee-accrual route to holders. Second, the "0% before" annotations are the cleanest possible retrospective proof that a token's prior valuation rested on something other than its product: Uniswap's fee switch was off, and the token had a multi-billion-dollar valuation anyway.

**Primary-spec corroboration.** Uniswap's UNIfication governance proposal is a Tier B document turning on the fee switch and burning UNI, including burning 100M UNI from treasury [C28]; a subsequent fee-activation proposal records "Last month, the protocol set a record burning 186,000 UNI in one day," and routes v4 fees to a TokenJar with UNI "burned on L2s and alt-L1s… bridged back to Ethereum mainnet and sent to `0xdead`" [C29]. Aave's Aavenomics proposal mandates a "$1M/week AAVE acquisition for the first 6 months," sized later to match protocol spend [C30]. Hyperliquid's own architecture docs state the Assistance Fund "Receives approximately 93% of platform fees" and as an asset strategy "Constantly purchases HYPE tokens" [C31].

**A cautionary definitional case.** Hyperliquid's numbers differ by source: the protocol's own docs say ~93% of platform fees [C31]; DefiLlama's per-venue definition says 99% of perp fees excluding builder fees, and 97%→99% of spot fees after 2025-08-30 excluding "unit protocol fees" [C13]. Both can be true — the difference is the carve-outs. This is the general hazard: **a headline "X% of fees accrue to holders" is meaningless without the carve-out list**, because builder/referrer/MEV take-outs sit *ahead* of the holder in the waterfall.

**The most cautionary case of all: a corporate vehicle as the value-capture mechanism.** Hyperliquid Strategies' S-1/A discloses that at closing it held "approximately $580 million in HYPE tokens (based on an agreed spot price of HYPE of $46.372)" plus ~$310M cash, and that under a Chardan equity facility of up to $1.0B, net proceeds "were primarily used to purchase HYPE tokens" [C32]. That is not value accrual from product usage; it is a listed vehicle providing a bid. A scoring tool must be able to distinguish *protocol fee accrual* from *third-party treasury accumulation*, and both are frequently conflated in project marketing.

**A worked severity ordering.** From strongest to weakest: (1) fee burned from core operations (ETH via EIP-1559, where "the base fee is burned" [C27], and ethereum.org's threshold that average gas ≥ ~16 gwei offsets ~1,700 ETH/day of issuance [C26]); (2) fees routed to a buyback program with on-chain evidence (Uniswap's `0xdead`, Pump.fun's burn-sourced PUMP buyback); (3) fees distributed to stakers (Hyperliquid AF, PancakeSwap); (4) treasury-directed buybacks sized to protocol spend (Aave); (5) no route at all — governance-only. Category 5 is the modal state and should score near zero on value capture regardless of product quality.

**Verifiable checks (C2)**
- Pull DefiLlama **Holders Revenue** for the token/protocol. If it is absent or $0, there is no fee-accrual route to holders. Confirm on the project's own page that the accrual is *dated* — an undated claim is unverifiable.
- Read the accrual spec and enumerate **carve-outs** (builder fees, referrers, MEV, LP share, validator commission). Compute the true post-carve-out holder share.
- Require on-chain execution evidence: a burn address balance that monotonically increases (`0xdead`), or an identifiable buyback contract with inflows traceable to fee revenue. Marketing announcements without this fail.
- Compute protocol revenue vs holders revenue vs token-incentive spend. If token incentives exceed holders revenue by more than ~2x, emissions are funding the reward, not usage funding it [C45].
- Exclude third-party treasury accumulation (corporate vehicles, "strategic holders") from value-capture credit [C32].
- Test the fee-switch dependency: was there any period where the token had a large valuation and the accrual mechanism was off? Uniswap's "0% before 28 Dec 2025" is the reference pattern [C13].
- For any burn/buyback claim, verify against the token contract's own transfer/burn logs, not the project's dashboard.

Sources: [C10][C11][C12][C13][C26][C27][C28][C29][C30][C31][C32][C37]

---

## C3. Supply and unlock mechanics

### Emission schedules and inflation vs deflation

The correct primitive is **net issuance**, not the nominal reward rate. New issuance dilutes every non-recipient and is paid by no one; priority fees and MEV are paid by users and are not dilutive; only the first is "reward." A chain whose burn exceeds issuance has negative supply growth and its real staking yield depends on usage rather than on the schedule — which also makes it a backward-looking number whose value depends on the chosen window [C45]. Ethereum is the reference implementation: post-Merge issuance fell ~88.7% (4.61% → 0.52% annualized at 14M ETH staked) and burn from EIP-1559's base fee can offset it entirely at ~16 gwei average [C26][C27].

Tokenomist's methodology gives the mechanical definitions a scorer should adopt: **Cliff Unlock** = discrete release at intervals >1 day; **Linear Unlock** = continuous daily release; **Net Emission = Inflation − Deflation**; and the explicit caveat that projections *exclude future burns* because burns are "variable and unpredictable" [C24]. That last caveat is a red flag in the other direction — a supply model with no burn assumption systematically overstates future dilution.

### Circulating vs released vs max supply — the ambiguity that flatters every token

Tokenomist's supply taxonomy is the right frame [C25]:
- **Released Supply** — unlocked *and claimable*, including tokens still sitting in team/investor wallets. Explicitly "not the same as Circulating Supply."
- **Circulating Supply** — only the portion of unlocked tokens that have *left stakeholder wallets*.
- **Locked Supply** — restricted, with a specified time-based release type.
- **TBD Locked** — restricted with no schedule.
- **Available Supply = Released Supply + TBD Locked** — "maximum potential near-term supply."
- **Unclaimed overhang** — the gap between Released and Circulating.

This gives a hard, mechanically checkable red flag: **Released/Circulating ratio.** A large gap means insiders hold claimable tokens that can hit the market at any time. A token reporting only "circulating supply" hides this entirely.

### Benchmark ranges for allocations and TGE floats

Sources are Tier D research (Stephanian's two editions of the offchain data study, Liquifi, Pulley) and Tier C market measurement (Binance Research, Tokenomist). Treat as orientation ranges, not rules.

- **Team**: 17.5% typical in Stephanian's earlier 60-project dataset, spread over 20–40 people [C21]; **24% average in 2023** across 150+ projects [C20]. Liquifi-derived ranges 17.5–18.6% [C22]; Redwood's cited band 15–25% with ~20% as common practice. Practical band: **15–25%**.
- **Private investors**: 25% (2013) → ~15% (2021) [C21]; **20% average in 2023** [C20]; ~11% across all projects but **19% when the project actually raised private capital** [C22]. Band: **11–20%**, and the conditional figure is the honest one.
- **Vesting**: 3–4 year vest, 0–12 month cliff, most common **4-year vest with 1-year cliff** [C22]; ~**85% of companies** using 4yr/1yr in a separate sample, 92% pairing investor grants with lockups [C23]. Some 31% use no cliff at all [C22].
- **TGE float**: modern L1s launched with **0% of insider tokens liquid at TGE**, with floats of roughly 13–20% (Aptos ~13% of 1B initial supply). Team TGE unlock above 5% is the point at which institutional framing treats it as a systemic sell-pressure red flag [C46-tier-E corroboration — see caveat below].
- **Real-world float distribution**: Binance Research found 2024 launches at **MC/FDV of 12.3%**, circulating supply "as low as 6% and none exceeding 20%" [C19].

**Important caveat on the 0%-team-unlock-at-TGE norm.** I could not source this from a Tier B/C primary document. It reaches me only through a Tier E benchmark aggregator that cites Liquifi (whose original report was taken offline after Coinbase's acquisition) and unnamed consultancies [C46]. I am recording it as **UNSOURCED-at-primary-tier** and not treating it as a scoring threshold. The defensible sourced claims are the directional ones from Stephanian [C20][C21], Liquifi [C22], Pulley [C23], and Binance Research [C19].

### How large near-term unlocks create measurable sell pressure

Binance Research quantifies the mechanism directly: "a total of US$155B will be unlocked in the next few years," and for 2024 launches, "for these tokens to maintain their current prices over the next couple of years, approximately US$80B in demand-side liquidity would need to flow into these tokens to match the increase in supply" [C19]. Liquifi's rationale for investor lockups is likewise explicit — they exist "to mitigate selling pressure and significant drops in price" [C22].

The mechanically checkable version: **monthly unlock USD ÷ trailing-90-day average daily volume × 30**. If unlock supply exceeds roughly one to two months of *organic* (non-wash) turnover, distribution is not absorbable without a price concession. DefiLlama makes the same point for TVL: "just looking at the TVL chart is not the best way to see if a protocol is receiving deposits or money is exiting, as that info gets mixed with price movements" — hence USD Inflows as a separate metric [C10].

**Verifiable checks (C3)**
- Compute Released/Circulating. Flag >1.2x.
- Compute FDV/MC. Flag <20%; note Binance's 2024 cohort mean of 12.3% [C19].
- Enumerate all unlock events ≥1% of max supply in the next 12 months, with dates and USD value.
- Compute unlock-days-of-volume using wash-adjusted turnover.
- Sum uncapped emission: if annual issuance / circulating supply is material, compute net emission *after* verified burns; do not accept nominal APY.
- Verify whether the supply model includes a burn assumption; if not, treat the dilution forecast as an upper bound [C24].
- Check team/investor allocation against the 15–25% / 11–20% bands [C20][C21][C22]; check cliff against the 4yr/1yr norm [C22][C23].
- Confirm whether treasury or ecosystem unlocks are excluded from "circulating" while being unlocked and unstaked.

Sources: [C19][C20][C21][C22][C23][C24][C25][C26][C27][C45][C46]

---

## C4. Holder distribution and concentration

### Why concentration is the variable that decides whether a token is a pump

Concentration is the single most predictive structural variable, because it converts "how many holders" from a vanity metric into a supply-risk metric. Three distinct clusters matter and they must be separated:

1. **Team/founder/VC/treasury wallets** — insider supply. These are known in advance from tokenomics and are measurable as unlock risk (C3).
2. **Deployer-linked / launchpad / bundler clusters** — supply acquired *before* public availability. The pump.fun opinion describes the mechanism in primary judicial language: insiders "obtained positions in selected tokens before those tokens were made available to the public," with purchases "spread among different wallets or 'bundled'" [C7]. A scorer must treat a cluster funded by one address at minute zero as insider supply even if each address is tiny.
3. **Exchange, staking, and bridge contracts** — *not* holders at all, but they inflate naive top-10 tables. Any concentration number must be computed net of labeled contracts.

The reference measurements: on the NFT market, "the top 0.1% of NFT traders (i.e., whales) drive" volume, and holding-value leaders "perform 69% of wash trading" [C18]. On Polymarket, <0.04% of addresses took >70% of profits [C48]. On EigenLayer, ~2% of depositors took ~90% of points [C34]. The consistent finding across three independent datasets: **the top ~0.1–2% of addresses is where the economics live, and that is true in both liquid and points-based systems.**

For a pump, concentration rises and holders churn; for a project with real usage, concentration in insiders is normal but *redeemable usage* — recurring fee-paying addresses — builds a persistent mid-tail. The observable difference is not "how concentrated is the top 10" but **whether the address set with repeat fee-paying behavior persists across quarters.**

### Method caveats I have to flag

- Nansen's public API exposes top holders with entity aggregation and labels (whale, smart_money, exchange, public_figure) and orders by ownership percentage or USD value [C39] — but the *thresholds* for "high" and "low" concentration that practitioners quote are not published by Nansen. Any specific cutoff (e.g. "top-10 > X% = red flag") circulating in the wild is **UNSOURCED**. Scoring tools should compute the raw number and apply their own calibrated band rather than inherit an unattributed threshold.
- The commonly cited per-coin top-10 figures (e.g. SHIB ~61%, LINK ~33%) come from Santiment via secondary aggregation [C50]; they are Tier D/E quality and, critically, **unadjusted for exchange and burn wallets**, which makes them near-meaningless as published. I record them only to show that the underlying quantity is tracked by data vendors, not as benchmarks.
- Symbilled airdrop recipients mechanically inflate holder counts and mechanically concentrate the tradable float. Correct approach: cluster by funding source and by claim-window behavior, then measure the *post-distribution* realized-PnL distribution [C16].

**Verifiable checks (C4)**
- Compute top-10 / top-100 / top-1% share of circulating supply, **excluding** labeled exchange, bridge, staking, burn and team-treasury addresses.
- Run funding-source clustering; report the largest single-entity cluster and its share of supply.
- Compare holder count 90 days post-distribution vs pre-distribution; count the addresses that sold within 30 days of claim [C16].
- Compute the realized-PnL concentration ratio (top 0.1% profit share ÷ median address PnL). Compare against the Polymarket reference shape [C48] and the NFT reference shape [C18].
- Count addresses with ≥2 distinct fee-paying interactions per month across ≥3 consecutive quarters — the sticky-user proxy.
- Flag any token where top-10 ex-contract share >35% AND holders are declining.

Sources: [C7][C16][C18][C34][C39][C48][C50]

---

## C5. Quantifying speculative dependence

Four measurable quantities, in increasing order of difficulty.

**(1) Exchange inflow share of volume.** Nearly all on-chain trading is executed through CEX venues, so for most tokens the "exchange" is not optional infrastructure — the DEX is the minority venue and any on-chain volume metric is a small, manipulable sample. The defensible use of exchange-flow data is as a *ratio*: net exchange inflow ÷ total token supply change, and inflow persistence during price declines. CryptoQuant publishes the methodology (Exchange Supply Ratio = exchange reserves ÷ total supply) [C43], but the *thresholds* implying manipulation are not published — **UNSOURCED**, must be self-calibrated.

**(2) Share of volume that is wash or incentivized.** This is the most heavily quantified quantity in the literature and the numbers are sobering:

- **NBER working paper (systematic tests on 29 exchanges)**: wash trading "averaged over 70% of the reported volume" on unregulated exchanges; a specific estimate puts it at **77.5% of total reported volume**, median 79.1%; **>53.4% on Tier-1 and 81.7% on Tier-2** venues; and "over 4.5 trillion USD in spot markets and over 1.5 Trillion USD in derivatives markets in the first quarter of 2020 alone." Fabricated volumes improve exchange rankings and "temporarily distort prices" [C15].
- **SEC**: bots generating "quadrillions of transactions and billions of dollars of artificial trading volume each day"; Gotbit >$1M/day on a single token [C3][C4].
- **Kaiko**: 30% of wallets in a Uniswap pool wash trading, FBI-confirmed; >100x volume-to-depth ratios cluster on meme coins and low-cap alts; 4,200 uniform 1M-PEPE buy orders in 24 hours on HTX vs organic patterns on Kraken [C14].
- **Prediction markets**: wash-trading patterns on Polymarket "peaking at nearly 60 percent of volume in December 2024," persisting through April 2025, and ~25% of Polymarket's historical volume overall [C17].
- **NFTs**: holding-value leaders perform 69% of wash trading [C18].

The correct conclusion for a scoring tool is not "X% of volume is fake" but: **exchange-reported and DEX-reported volume for low-cap tokens must be treated as an upper bound, and volume-based metrics must be discounted heavily or replaced with fee-revenue-per-unique-address.**

**(3) Holder PnL distribution.** Covered in C1/M1 and C4. This is the single highest-value quantitative signal because it is derived from the chain itself, needs no external vendor, and directly answers "is this a wealth-transfer vehicle."

**(4) Retention / returning users.** The academic airdrop study gives the shape: median transfers per recipient = 1, median first-to-last span = 0 days [C16]. The Blockworks/DL News reporting gives the macro version for points-driven TVL [C34][C35]. Neither is a clean "retention rate," so a scorer must construct it: fraction of addresses active in month *M* also active in *M+3*, with the address set restricted to addresses that generated fees rather than incentives.

### Why "TVL up + token down" is the diagnostic for subsidy-driven mercenary capital

This is the single most informative *divergence* pattern available, and it is structural rather than anecdotal. The logic:

1. If TVL rises because a protocol subsidizes deposits with token rewards, the marginal capital is **borrowed from the emissions budget**, not earned from organic yield. It arrives, is counted, and inflates TVL. [C40] measures exactly this decomposition.
2. Token price simultaneously falls when that subsidy is *not* large enough to offset (a) dilution from emissions and (b) unlocks [C19].
3. TVL denominated in the token therefore falls mechanically even with zero net withdrawals — which is precisely why DefiLlama built USD Inflows as a separate metric and states that TVL alone "is not the best way to see if a protocol is receiving deposits or money is exiting" [C10].
4. Therefore: **TVL up (in USD) + token price down + token's own emissions as the top reward source** = the protocol is paying for its own metrics. It is not evidence of usage.

DefiLlama's organic-vs-incentivized TVL research quantifies how much of the sector is affected and how the ratio moves: **Lending improved from 58.5% to 90.3% organic share** and is now the largest classified sector at $45.64B TVL; **Yield Aggregators inverted from 96.2% incentivized (Aug 2022) to 86.0% organic (Apr 2026)**; aggregate organic share peaked at 96.95% (Nov 2024) and had retreated to 94.34% by Apr 2026 as new incentive programs launched. It also names protocols whose classification is genuinely ambiguous — Pendle (fixed-income characteristics), Ethena (delta-neutral synthetic with perp funding exposure), Convex (Curve LP emission booster) [C40]. That ambiguity list is itself a useful signal: a protocol a top-tier data house cannot classify as organic is not a protocol to score as organic.

**Verifiable checks (C5)**
- Wash-trading screen: volume ÷ 1%-depth; cross-exchange volume correlation (Kaiko's method: "Consistent, monotonic volume, periods of zero volume, or discrepancies between different exchanges can signal irregular trading activity") [C14]. Treat volume as an upper bound under any ratio >100x.
- Round-number ratio (unrounded vs rounded trade sizes) as a cross-check on the NBER method [C15].
- Realized-PnL concentration (see C4).
- Monthly retention: M→M+3 fee-paying address overlap, and the same for *incentive-earning* addresses. Divergence between the two is the points-farming signature.
- Organic-vs-incentivized TVL share for the protocol, at three reward-share thresholds (0.1/0.5). If the protocol appears in DefiLlama's "ambiguous" mapping, downgrade [C40].
- The TVL-up/token-down divergence test: 90-day correlation of USD inflows against token price, cross-referenced with the emissions/revenue ratio.
- Count unique fee-paying addresses vs total active addresses. Ratio <0.3 indicates most activity is reward-driven.

Sources: [C3][C4][C10][C14][C15][C16][C17][C18][C19][C34][C35][C40][C43][C48]

---

## C6. Real-use-case marker set

### What actually demonstrates usage independent of speculation

Ranked by how hard they are to fake:

**Tier 1 — Cash the user must spend (hardest to fake).**
- **Organic fee revenue**, defined as `Fees − Supply-Side Revenue`, measured on-chain, not self-reported. DefiLlama refuses to track self-reported figures that cannot be checked against trade-level or on-chain data [C10] — that refusal is the methodological standard a scorer should adopt.
- **Fees paid per unique address**, and the *count* of addresses paying fees. A protocol where fee revenue is concentrated in a handful of whale addresses is not retail-used.
- **Fee revenue in a token other than its own**, and **net of emissions spend**. If incentives > fees, the protocol is subsidizing itself.

**Tier 2 — Behavior that requires cost or time.**
- **Sticky/returning users**: address retention across quarters measured on fee-paying (not reward-earning) behavior. Academic baseline for what failure looks like: median 1 transfer, 0-day engagement span post-distribution [C16].
- **External developers / integrations committed without incentives**: Electric Capital's 2024 report found established developers (2+ years in crypto) at all-time highs, +27% YoY, committing 70% of code, while total developers *fell* 7% — i.e. the durable cohort grew as the speculative cohort shrank [C33]. That divergence (total devs −7%, established devs +27%) is a usable real-usage tell at ecosystem level.
- **Third-party capital at market terms**: external protocols/integrations whose exposure survives after incentives end.

**Tier 3 — Treasury and durability.**
- **Real treasury revenue** covering operations. Protocols with fee-funded treasuries survive drawdowns; those dependent on token sales do not. Binance Research's framing: "Projects that have the ability to generate profits and real cash flow are self-sustaining and are less reliant on external sources of funding" [C42].
- **Usage sustained through market cycles.** Messari's Q4 2023 data shows the pattern to look for: Bitcoin daily transactions +2.0% QoQ while **daily active addresses declined 4.7% QoQ** — the report's own read is that activity became concentrated in a "relatively small group of 'super users'" [C41]. Sustained usage should therefore be measured per-address (activity *density*), not in aggregate, because aggregate activity can be manufactured by a handful of sybil clusters while genuine usage decays.

**The published answer to "which metrics persist through bear markets"** is, in short: fee revenue per address, treasury revenue, and long-tenure developer counts. What does not persist: TVL, aggregate volume, holder count, and reward-inflated APY. Electric Capital's developer cohort data is the cleanest demonstration that a *retention-based* measure moved counter-cyclically while a headline measure (total developers) fell [C33]. Messari's fee-vs-issuance framing gives the corresponding supply-side version: in Q4 2023, 83.8% of miner revenue came from issuance, and 92.4% across 2023 vs 98.4% in 2022 — the fee-funded *share* is the durable quantity [C41].

**Verifiable checks (C6)**
- Recompute `Revenue = Fees − Supply-Side Revenue` for the protocol; require on-chain verification.
- Compute fees per unique fee-paying address, and the HHI of fee concentration across addresses.
- Compute incentive spend ÷ revenue. Flag >2x.
- Build M → M+3 and M → M+12 retention for fee-paying addresses.
- Count external repositories/integrations referencing the protocol with no financial relationship.
- Verify treasury revenue (DefiLlama Treasury metric, "Total Assets" analogue) against annualized operating cost.
- Measure activity density (tx per active address) rather than absolute activity; a rising density with falling active addresses is the super-user tell [C41].
- Require the product to have retained meaningful usage in at least one full drawdown period.

Sources: [C10][C16][C33][C41][C42]

---

## C7. Marketing and claim red flags as economic tells

Red flags here are not stylistic complaints. Each one is evidence about the *economic mechanism*, which is why they belong in a legitimacy scorer at all.

**The label-substitutes-for-product pattern.** CoinGecko identifies the *purpose* of the label: governance tokens were introduced "as a way to distribute tokens and 'decentralize' the protocol," but "the second, and perhaps more important aim, is for these tokens to serve as **liquidity mining incentives** in order to bootstrap liquidity" [C37]. So "DeFi," "AI," "RWA," or "GameFi" in a token's name is not a claim about product scope — it is frequently a claim about the *reward schedule*. The mechanical test: does the protocol's fee-revenue line exist, and is it non-trivial relative to emissions? If not, the label is the only substantive asset. The SEC's 2025 action on fake crypto platforms selling non-existent "Security Token Offerings" is the terminal case: "no trading took place on the trading platforms, which were fake, and the Security Token Offerings and their purported issuing companies did not exist" [C9].

**Buyback announcements with no verifiable on-chain execution.** This is the most common verifiable-but-unauditable claim. The standard I propose: a buyback claim is *only* counted when the on-chain path exists — fee revenue → identifiable contract/address → market purchase → burn or treasury hold. Uniswap's `0xdead` routing is the reference implementation [C29]; DefiLlama's PUMP line is explicitly "sourced from onchain burns" [C13]. Conversely, Hyperliquid Strategies' disclosure that equity proceeds were "primarily used to purchase HYPE tokens" [C32] is a bid, not accrual — and is disclosed *because it is a listed issuer*, which is exactly the disclosure standard a private project lacks.

**Fake partnership announcements are a regulator-named pump tool.** The CFTC advisory describes pump groups using "false news reports" about "a famous high-tech business leader or investor who plans to pour millions of dollars into a small, lesser known virtual currency," and "major retailers, banks, or credit card companies, announcing plans to partner with one virtual currency or another," with links "accompanied by posts that create false urgency and tell readers to buy now" [C2]. Verification rule: a partnership counts only when the counterparty's own channels confirm it. Everything else is a promotional event, not evidence.

**Influencer/paid-promo-funded launches.** The SEC charged eight influencers in a $100M scheme in which they "cultivated hundreds of thousands of followers," "encouraged their substantial social media following to buy" promoted assets, then "regularly sold their shares without ever having disclosed their plans to dump" [C8]. The pump.fun litigation names the mechanism in the modern token context — 25 unidentified "Lead KOL Doe Defendants" promoting select tokens to retail, plus insiders "pre-positioning" before promotion [C7]. Verification rule: promotion without disclosed compensation is not evidence of demand; promotion *with* undisclosed compensation is affirmative evidence of manipulation.

**The passivity claim.** "Passive income," "yield," and "APY" in project marketing should be traced to a payer. In the SEC's Saitama complaint the "passive income" claim coexisted with dumping and with "applications and platforms they were developing" that were never delivered [C5]. M2 in the taxonomy is precisely this: a reward paid by the issuer is a marketing expense wearing a yield-product costume.

**Ticker as value capture.** Permissionless deployment means the same ticker exists many times over with unrelated contracts and supplies. Any scoring system that keys on symbol identity rather than contract address is measuring an attention artifact. Related and adjacent: rebrands and rebranded narratives. I found no Tier A–D source establishing specific rebrand statistics — **UNSOURCED**; the general point is that ticker identity carries no evidentiary weight and must be resolved to a contract address before scoring.

**What I could not source:** published quantified incidence rates for each red flag (e.g. "X% of buyback announcements are unexecuted"). No authoritative dataset exists. All of C7's rules are therefore *structural* (must-have evidence) rather than *probabilistic*, deliberately.

**Verifiable checks (C7)**
- Resolve ticker → contract address → deployer address. Reject any scoring keyed on symbol.
- For every partnership claim, require confirmation from the counterparty's official channel; log the date of first appearance on each side.
- For every buyback/burn claim, require on-chain evidence (balance of a burn address, or a buyback contract's inflows traceable to fee revenue) [C29][C13].
- For every yield/APY claim, name the payer and whether the payer is the protocol's emissions schedule.
- Flag any project whose announced "users," "partners," or "revenue" cannot be reproduced from a primary on-chain source.
- Check whether the team/founders have prior enforcement or undisclosed-wallet selling.
- Reject marketing claims sourced only from press releases with no independent or on-chain corroboration.

Sources: [C2][C5][C7][C8][C9][C13][C29][C32][C37]

---

## Sources

| Ref | Tier | Title | URL | Date accessed |
|---|---|---|---|---|
| C1 | A | CFTC Charges Two Individuals with Multi-Million Dollar Digital Asset Pump-and-Dump Scheme (Release 8366-21) | https://www.cftc.gov/PressRoom/PressReleases/8366-21 | 2026-10-06 |
| C2 | A | CFTC Customer Advisory: Beware Virtual Currency Pump-and-Dump Schemes | https://www.cftc.gov/LearnAndProtect/AdvisoriesAndArticles/beware_virtual_currency_pump_dump.html | 2026-10-06 |
| C3 | A | SEC Press Release 2024-166: SEC Charges Three So-Called Market Makers and Nine Individuals in Crackdown on Manipulation of Crypto Assets | https://www.sec.gov/newsroom/press-releases/2024-166 | 2026-10-06 |
| C4 | A | SEC Litigation Release LR-26156: Gotbit Consulting LLC and Fedor Kedrov (Robo Inu) | https://www.sec.gov/enforcement-litigation/litigation-releases/lr-26156 | 2026-10-06 |
| C5 | A | SEC v. Armand et al. Complaint (Saitama Inu / SaitaRealty), D. Mass. | https://www.sec.gov/files/litigation/complaints/2024/comp-pr2024-166-saitama.pdf | 2026-10-06 |
| C6 | A | CFTC Charges Avraham Eisenberg with Manipulative and Deceptive Scheme (Mango Markets oracle manipulation), Release 8647-23 | https://www.cftc.gov/PressRoom/PressReleases/8647-23 | 2026-10-06 |
| C7 | A | Aguilar v. Baton Corporation Ltd. d/b/a Pump.fun et al., Opinion and Order on Motions to Dismiss, S.D.N.Y. 25-cv-880 | https://www.wolfpopper.com/siteFiles/News/Pump.funOpinionandOrder.pdf | 2026-10-06 |
| C8 | A | SEC Press Release 2022-221: SEC Charges Eight Social Media Influencers in $100 Million Stock Manipulation Scheme | https://www.sec.gov/newsroom/press-releases/2022-221 | 2026-10-06 |
| C9 | A | SEC Press Release 2025-144: SEC Charges Three Purported Crypto Asset Trading Platforms and Four Investment Clubs | https://www.sec.gov/newsroom/press-releases/2025-144-sec-charges-three-purported-crypto-asset-trading-platforms-four-investment-clubs-scheme-targeted | 2026-10-06 |
| C10 | C | DefiLlama Data Definitions (fees, revenue, holders revenue, TVL, USD inflows, organic/incentivized) | https://docs.llama.fi/analysts/data-definitions | 2026-10-06 |
| C11 | C | Token Terminal Docs — methodology / earnings definitions | https://tokenterminal.com/docs | 2026-10-06 |
| C12 | C | Artemis, "Crypto Revenue: a standard and consistent definition for value accrual to token holders" | https://research.artemis.ai/p/crypto-revenue-a-standard-and-consistent | 2026-10-06 |
| C13 | C | DefiLlama Holders Revenue Rankings (per-protocol dated accrual definitions) | https://defillama.com/holders-revenue | 2026-10-06 |
| C14 | C | Kaiko, "Market Abuse in Crypto Markets: A practical guide to countering market abuse" | https://resources.kaiko.com/hubfs/Market%20Abuse%20in%20Crypto%20Markets%20by%20Kaiko.pdf | 2026-10-06 |
| C15 | C | NBER Working Paper 30783, systematic detection of wash trading across 29 crypto exchanges | https://www.nber.org/system/files/working_papers/w30783/w30783.pdf | 2026-10-06 |
| C16 | C | "Airdrops: Giving Money Away Is Harder Than It Seems" (arXiv 2312.02752) — empirical study of nine major airdrops | https://arxiv.org/html/2312.02752 | 2026-10-06 |
| C17 | C | "Network-Based Detection of Wash Trading" (Polymarket study) | https://gamblingharm.org/wp-content/uploads/2025/11/Polymarket-Wash-Trading-Study.pdf | 2026-10-06 |
| C18 | C | "A Deep Dive into NFT Whales: A Longitudinal Study of the NFT Trading Ecosystem" (arXiv 2303.09393) | https://export.arxiv.org/pdf/2303.09393v1.pdf | 2026-10-06 |
| C19 | D | Binance Research, "Low Float & High FDV: How Did We Get Here?" | https://public.bnbstatic.com/static/files/research/low-float-and-high-fdv-how-did-we-get-here.pdf | 2026-10-06 |
| C20 | D | Stephanian, "Optimizing Your Token Distribution in 2024" (150+ projects) | https://offchaindata.substack.com/p/optimizing-your-token-distribution-29d | 2026-10-06 |
| C21 | D | Stephanian, "Optimizing Your Token Distribution" (60 projects) | https://offchaindata.substack.com/p/optimizing-your-token-distribution | 2026-10-06 |
| C22 | D | LiquiFi, "Token Vesting and Allocations Industry Benchmarks" (Robin Ji) | https://liquifi.finance/post/token-vesting-and-allocation-benchmarks | 2026-10-06 |
| C23 | D | Pulley, "Token Compensation Insights: Best Practices and Benchmarks 2023" | https://site.pulley.com/guides/token-compensation-insights-report | 2026-10-06 |
| C24 | C | Tokenomist Methodology — Emission: Cliff & Linear | https://docs.tokenomist.ai/methodology/cliff-and-linear-emission | 2026-10-06 |
| C25 | C | Tokenomist Methodology — Supply Metrics (released vs circulating vs locked vs FDV) | https://docs.tokenomist.ai/methodology/supply-metrics | 2026-10-06 |
| C26 | B | ethereum.org, "How The Merge impacted ETH supply" (issuance/burn threshold) | https://ethereum.org/roadmap/merge/issuance/ | 2026-10-06 |
| C27 | B | EIP-1559: Fee market change for ETH 1.0 chain | https://eips.ethereum.org/EIPS/eip-1559 | 2026-10-06 |
| C28 | B | Uniswap Foundation, UNIfication proposal (protocol fees on, UNI burn, 100M UNI treasury burn) | https://gov.uniswap.org/t/unification-proposal/25881 | 2026-10-06 |
| C29 | B | Uniswap Governance, "Activate v4 Protocol Fees (Part 1/2)" (record 186,000 UNI burned in one day; TokenJar → 0xdead) | https://vote.uniswapfoundation.org/proposals/100 | 2026-10-06 |
| C30 | B | Aave Governance, "[ARFC] Aavenomics implementation: Part one" ($1M/week AAVE buyback mandate) | https://governance.aave.com/t/arfc-aavenomics-implementation-part-one/21248 | 2026-10-06 |
| C31 | B | Hyperliquid Architecture Docs — Protocol Vaults: Assistance Fund (~93% of platform fees; buys HYPE) | https://hyperliquid-co.gitbook.io/wiki/architecture/hypercore/vault | 2026-10-06 |
| C32 | A | Hyperliquid Strategies Inc. Form S-1/A (SEC EDGAR) — ~$580M HYPE held; equity facility proceeds used to purchase HYPE | https://www.sec.gov/Archives/edgar/data/2078856/000119312526309340/d50758ds1a.htm | 2026-10-06 |
| C33 | D | Electric Capital, 2024 Crypto Developer Report | https://electriccapital.substack.com/p/2024-crypto-developer-report | 2026-10-06 |
| C34 | D | Blockworks Empire Newsletter, "What's wrong with EigenLayer's airdrop" (top 2% depositors ~90% of points; ETH staked 3.14M→4.86M) | https://blockworks.co/news/empire-newsletter-eigenlayer-airdrop-criticisms | 2026-10-06 |
| C35 | D | DL News, "Why EigenLayer and other restaking projects are cooling off" (TVL −20% post-airdrop; incentive-driven outflows) | https://www.dlnews.com/articles/defi/why-eigenlayer-and-other-restaking-projects-are-cooling-off/ | 2026-10-06 |
| C36 | D | CoinDesk, "As Crypto 'Points' Farming Grows, So Does Risk of Vague Promises" | https://www.coindesk.com/tech/2024/02/21/as-crypto-points-farming-grows-so-does-risk-of-vague-promises | 2026-10-06 |
| C37 | D | CoinGecko Research, "Valueless Governance Tokens: Are they a meme or not?" | https://www.coingecko.com/research/publications/valueless-governance-tokens | 2026-10-06 |
| C38 | C | CoinGecko Research, "State of Memecoins Report 2025" | https://www.coingecko.com/research/publications/state-of-memecoins-2025 | 2026-10-06 |
| C39 | C | Nansen API docs — Token God Mode Holders endpoint (labels, entity aggregation) | https://docs.nansen.ai/api/token-god-mode/holders | 2026-10-06 |
| C40 | C | DefiLlama Research, "DeFi TVL: Organic vs. Incentivized — v2" | https://artifacts.llama.fi/pdf-exports/e7eacbe2-5a51-4ae2-b3ec-49f306129b06.pdf | 2026-10-06 |
| C41 | D | Messari, "State of Bitcoin Q4 2023" (active addresses −4.7% QoQ vs tx +2.0%; fee vs issuance revenue share) | https://messari.io/report/state-of-bitcoin-q4-2023 | 2026-10-06 |
| C42 | D | Binance Research, "A Guide to Fundamental Analysis in Crypto" | https://research.binance.com/static/pdf/A-Guide-to-Fundamental-Analysis-in-Crypto.pdf | 2026-10-06 |
| C43 | C | CryptoQuant API docs — Stablecoin Flow Indicator, Exchange Supply Ratio definition | https://userguide.cryptoquant.com/api/stablecoin-flow-indicator | 2026-10-06 |
| C44 | D | Messari/CoinGecko sector data cross-check: CEX.IO "Memecoins: Too Big to Ignore" (memecoin volume/MCap ratio) | https://blog.cex.io/ecosystem/memecoins-too-big-to-ignore-34839 | 2026-10-06 |
| C45 | D | Digital Asset Database, "Inflation versus dilution, and what real staking yield means" | https://digitalassetdatabase.com/learn/inflation-dilution-and-real-staking-yield | 2026-10-06 |
| C46 | E | 8Blocks, "Token Vesting & Allocation Benchmarks (2026)" — **NOT CITED**; used only to locate Stephanian/Liquifi/Binance/Tokenomist primaries | https://8blocks.io/learn/token-vesting-benchmarks | 2026-10-06 |
| C47 | E | QuantAbundancia HYPE tokenomics analysis — **NOT CITED**; superseded by [C31][C13] | https://quantabundancia.com/articles/hype-tokenomics-assistance-fund-buyback | 2026-10-06 |
| C48 | D | CryptoNews reporting on DeFi Oasis realized-PnL analysis of Polymarket (<0.04% of addresses captured >70% of $3.7B profits) | https://cryptonews.com/news/70-of-polymarket-traders-lost-money-as-top-0-04-captured-most-profits-research/ | 2026-10-06 |
| C49 | E | FindAS blog, "Unlocking Engagement: Cryptocurrency Points Systems" — **NOT CITED**; Hyperliquid 31%/94k-users figure left UNSOURCED | https://www.findas.org/blogs/points-systems | 2026-10-06 |
| C50 | D | Blockchain.news aggregation of Santiment top-10 holder concentration data (unadjusted for contract wallets) | https://blockchain.news/flashnews/analysis-of-top-wallet-holdings-in-major-altcoin-markets | 2026-10-06 |

### Explicitly UNSOURCED claims

1. **"Team TGE unlock must be 0%; >5% is a red flag."** Reaches me only via Tier E aggregator [C46] citing an offline Liquifi report. Not used as a threshold.
2. **Published concentration thresholds** (e.g. "top-10 >X% = red flag"). No Tier A–D source publishes a validated cutoff. Compute raw numbers; calibrate in-house.
3. **Manipulation thresholds for CryptoQuant exchange-flow indicators.** Methodology is published [C43]; thresholds are not.
4. **Incidence rates for each C7 red flag** (e.g. share of announced buybacks never executed). No authoritative dataset exists.
5. **Hyperliquid points→TGE distribution specifics** (310M HYPE / 31% of supply to ~94,000 users). Only in Tier E [C49].
6. **Academic quantification of staking rewards exceeding organic fees across the sector.** The conceptual decomposition is well-sourced [C45]; sector-wide incidence is not.

---

## Casino-mechanics taxonomy

| mechanic_id | mechanism | metric it inflates | observable tell | false-positive risk |
|---|---|---|---|---|
| M1 | Reflexive / ponzi loop; new buyers fund earlier holders' returns; no cash-flow anchor | Holder count, market cap, "community size" | Realized-PnL distribution: <0.1% of addresses take >50% of profits while median address is net-negative [C48] | Low. A legitimate protocol can produce concentrated PnL in a strong up-market; require the loss-side majority to persist across a full cycle |
| M2 | Emission-funded APY presented as yield; reward payer is the protocol's own issuance schedule, not users | APY, TVL, "revenue" | Reward-share >50% of APY; emissions > fees; real yield ≈ nominal minus inflation [C45]; marketing cites "passive income" with no external payer [C5] | Medium. Legit pre-subsidy growth phases look identical for 2–3 quarters. Require the pattern to persist after incentives taper |
| M3 | Mercenary capital rented via points / liquidity mining / high-FAR TVL subsidies | TVL, users, deposits | TVL and USD inflows decouple [C10]; >15% TVL drop within 90 days of the distribution event (EigenLayer −20%) [C35]; top ~2% of stakers hold ~90% of points [C34] | Medium. Genuine incentives also attract opportunists. Distinguish by whether activity persists after the subsidy ends |
| M4 | Exchange listing / TGE float scarcity as the primary monetization event | Market cap, FDV, headline valuation | MC/FDV <20% (2024 cohort mean 12.3%; float as low as 6%) [C19]; volume spike on listing with flat on-chain user count | Low. Thin floats are sometimes an accident of distribution design. Confirm with organic usage metrics over the following quarter |
| M5 | Paid wash volume / market-maker-as-a-service | Exchange volume, DEX volume, ranking | Bots generating "quadrillions of transactions and billions of dollars of artificial trading volume each day" [C3]; 30% of wallets in a Uniswap pool wash trading [C14]; volume/1%-depth >100x [C14]; >70% of reported volume wash on unregulated venues [C15] | Low-moderate. Zero-fee campaigns legitimately inflate volume [C14] — subtract known incentive programs before flagging |
| M6 | Thin-liquidity price manipulation as an extractable mechanic (token is both collateral and oracle input) | Price, TVL in token terms, market cap | Oracle inputs across only a few thin venues; 13x price move in 30 minutes on minimal flow [C6]; order-book depth insufficient to absorb the position | Low. Requires the specific oracle-collateral structure; check before scoring |
| M7 | Referral / affiliate / MLM-style recruitment economics | Signups, "users", social metrics | Referral payouts structurally excluded from Fees and capped at Supply-Side Revenue [C10]; compensation denominated in the project's own token | Medium. Many legitimate growth programs have referral components. Flag when *most* new-user acquisition is attributed to referrals |
| M8 | Points-to-airdrop schemes scoring capital or cumulative volume rather than usage | "Engagement", points leaderboards, pre-TGE expectations | Up to 66% of airdropped tokens sold in the first post-claim transfer (Lido 65.75%, 1inch 58.67%) [C16]; median 1 transfer per recipient; median 0-day engagement span [C16]; points themselves traded/levered [C36] | Low. Points programs vary enormously in scoring design; a duration-weighted, behavior-diverse program is materially better [C49-not-cited, design principles only] |
| M9 | PnL-token / vault schemes where emissions are the only yield source | Vault TVL, "yield", AUM | All APY components paid in the issuer's own token; no non-emission base APY; vault share price depends on secondary token price | Low. Some vaults have a real fee-funded component; decompose APY before flagging |
| M10 | Treasury/insider price support mistaken for value accrual | Token price, reported "revenue" | Corporate or insider vehicles accumulating the token (Hyperliquid Strategies: ~$580M HYPE; equity facility proceeds "primarily used to purchase HYPE") [C32]; protocol treasury buying its own token disclosed as a program, not earnings [C30] | Moderate. Protocol-directed buybacks funded by fees *are* legitimate accrual [C29][C30]. Flag only when the funding source is external capital or unearned revenue |
| M11 | Narrative-label substitution for product (AI/DeFi/RWA/GameFi label with no fee line) | Narrative premium, search interest, launch valuation | No `Revenue = Fees − Supply-Side Revenue` series exists; fee switch historically off while valuation was large (Uniswap "0% before 28 Dec 2025") [C13]; platform/offerings do not exist [C9] | Low. Early-stage projects legitimately have no revenue yet — require *stated* roadmap plus deployed fee-generating contract, not revenue |

---

## Scoring signals extracted

| signal_id | what it measures | how to verify mechanically | evidence tier | failure mode | confidence |
|---|---|---|---|---|---|
| CS01 | Holders Revenue (fee accrual to token holders) | Pull DefiLlama Holders Revenue 24h/7d/30d for the protocol; absent or $0 ⇒ no accrual route | C | Protocol-specific fee definitions differ (Hyperliquid 93% vs 99% depending on carve-outs) [C31][C13] | High |
| CS02 | Post-carve-out effective holder share of fees | Read accrual spec; enumerate builder/referrer/MEV/LP/validator take-outs sitting ahead of holders; recompute | C | Carve-out lists change without notice; some are undocumented off-chain | Medium-High |
| CS03 | Protocol revenue ≠ token-holder revenue | Compute DefiLlama Revenue then subtract team/treasury portion; per Artemis, exclude team and Foundation value | C | Revenue accrues to a treasury controlled by holders — economically still holder value [C12] | High |
| CS04 | Emission-funded vs fee-funded reward share | `emissions_annual_usd ÷ tokenholder_revenue_annual`; flag >2x | C | Emissions valuation depends on volatile token price | High |
| CS05 | Organic-vs-incentivized TVL share | Recompute base vs reward APY split per pool; classify at reward-share thresholds 0.1/0.5 | C | Pools with NULL APY breakdown are unclassified and excluded — coverage gap [C40] | High |
| CS06 | Protocol appears in data house "ambiguous" classification | Check DefiLlama's ambiguous-protocol mapping (Pendle, Ethena, Convex, Aave GHO) | C | Being listed is not proof of being incentive-driven | Medium |
| CS07 | Buyback/burn executed on-chain | Verify burn-address balance monotonically increasing (e.g. `0xdead`) or buyback contract inflows traceable to fee revenue | B | Multi-sig/relay indirection can obscure the source | High |
| CS08 | Fee-switch dependency (was accrual ever off while valuation was high?) | Check dated accrual annotations; "0% before <date>" flags pre-accrual valuation as non-fundamental | C | Some switches predate credible fee generation | High |
| CS09 | Third-party treasury accumulation miscounted as accrual | Detect corporate vehicles/strategic holders disclosing token purchases; exclude from value-capture credit [C32] | A | Non-disclosing treasuries are invisible | Medium-High |
| CS10 | Realized-PnL concentration across addresses | Sum positive realized PnL; report share captured by top 0.01%/0.1%/1%; report share of addresses net-negative. Reference: <0.04% → >70% [C48] | C | Requires full history and robust cost basis; wash trades inflate apparent volume but not PnL | High |
| CS11 | Wash/incentivized volume discount | Volume ÷ 1%-market-depth ratio; cross-exchange volume correlation; round-number (unrounded vs rounded size) ratio [C14][C15] | C | Zero-fee campaigns and legitimate market making mimic the pattern [C14] | Medium-High |
| CS12 | Absolute wash-trading share of reported volume (context prior) | Apply published estimates as a prior: >70% on unregulated venues, 53.4% Tier-1, 81.7% Tier-2 [C15] | C | Estimates are venue-level and dated; decay over time | Medium |
| CS13 | Stablecoin/flow contamination of volume | CryptoQuant Exchange Supply Ratio and net inflow ÷ supply change; volume that reverses within one cycle is flow noise [C43] | C | Published thresholds for manipulation are not available — self-calibrate | Medium |
| CS14 | Released vs circulating overhang | Compute Released/Circulating per Tokenomist definitions; flag >1.2x; also report TBD-locked share [C25] | C | Vendors differ on what counts as "stakeholder wallet" | High |
| CS15 | FDV/MC float thinness | `MC ÷ FDV`; flag <20%. Reference: 2024 launches averaged 12.3%, floats as low as 6% [C19] | C | Legitimate long-vesting designs produce low float by intent — check allocation design before penalizing | High |
| CS16 | Near-term unlock absorption capacity | `monthly_unlock_USD ÷ (trailing-90d wash-adjusted daily volume × 30)`; flag >1–2 months | C | Volume forecasts are unstable in low-liquidity names | Medium-High |
| CS17 | Net emission vs realized burn | `inflation − deflation` using verified burns only; do not accept projections that exclude burns as precise [C24] | C | Burn flows can be one-off or vanity | High |
| CS18 | Team / investor allocation plausibility | Team 15–25%; investors 11–20% (19% when VC-funded); cliff 12mo, vest 3–4yr, 4yr/1yr most common [C20][C21][C22][C23] | D | Allocation buckets are self-reported and inconsistently defined across 150+ projects [C20] | Medium |
| CS19 | Holder concentration, contracts excluded | Top-10/100/1% share net of labeled exchange, bridge, staking, burn and treasury addresses | C | Vendor labels are incomplete; cluster-level aggregation is needed | High |
| CS20 | Deployer-linked / bundler clustering | Cluster addresses by common funding source and by first-funding timestamp near deploy; treat pre-public clusters as insider supply [C7] | A | Legitimate multi-sig and treasury operations look like clusters | Medium-High |
| CS21 | Airdrop liquidation velocity | Share of distributed tokens sold in the first post-claim transfer. Reference: Lido 65.75%, 1inch 58.67%, Optimism 48.21%, up to 66% [C16] | C | Requires token-level transfer tracing with exchange labeling | High |
| CS22 | Post-distribution engagement depth | Median transfers per recipient and median first-to-last span. Failure reference: 1 transfer, 0 days [C16] | C | Long-tail protocols have heavy medians; use medians not means | High |
| CS23 | Points/airdrops capital concentration | Share of points or eligibility-weighted allocation captured by top 1–2% of participants. Reference: ~90% to top 2% [C34] | D | Points weights are unpublished by most programs | Medium |
| CS24 | Fee-paying vs reward-earning user split | Count addresses paying fees ÷ addresses earning rewards; flag <0.3 | C | Reward-earning addresses are easier to count than fee payers on some chains | Medium-High |
| CS25 | Returning-user retention (M→M+3, M→M+12) on fee-paying addresses | Build monthly cohorts on fee-paying behavior only; reward-driven retention is not evidence | C | Address churn from bridges/CEX deposit addresses inflates churn | Medium-High |
| CS26 | TVL-up / token-down divergence | 90-day correlation of USD inflows vs token price, conditioned on emission-funded APY share. Test the "protocol is paying for its own metrics" hypothesis [C10][C40] | C | Legit growth plus market drawdown produces the same divergence; condition on emission share | Medium |
| CS27 | Activity density (tx per active address) | Compute tx/active-address ratio trend; rising density with falling active addresses = super-user concentration [C41] | D | Denominator definitions vary by data vendor | Medium |
| CS28 | Developer durability counter-cyclicality | Established (2+ yr) developer count vs total developer count. Reference: 2024 total −7% while established +27%, 70% of commits [C33] | D | Git-derived developer identity is heuristic and double-counts monorepos | Medium |
| CS29 | Ticker → contract → deployer resolution before any scoring | Resolve symbol to contract address and deployer; reject symbol-keyed scoring | D | Multiple legitimate contracts can share a symbol; needs cross-chain resolution | High |
| CS30 | Claim verifiability gate (partnerships, buybacks, yield payers) | For each claim require: counterparty-side confirmation (partnerships), on-chain flow evidence (buybacks/burns), named external payer (yield/APY). Fake partnership announcements are a named pump tool [C2]; fake platforms sold non-existent offerings [C9]; undisclosed influencer compensation is charged fraud [C8] | A | Legitimate low-profile deals may lack public confirmation — mark "unverifiable" rather than "false" | High |
| CS31 | Promoter/insider alignment pre-event | Check for insider position accumulation and bundled buys preceding promotion events [C7]; check for undisclosed promotional compensation | A | Post-hoc detection only; requires event-level wallet analysis | Medium-High |
| CS32 | Value-capture mechanism class assignment | Classify as (1) fee burn, (2) fee-funded buyback with on-chain proof, (3) fee distribution to stakers, (4) treasury-directed buyback funded by fees, or (5) none. Score (5) near zero regardless of product quality [C27][C29][C30][C31] | B | Mechanisms can be layered and switched on later — evaluate the current, dated state | High |
