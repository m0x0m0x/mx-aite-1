# Slice A — Technical & Smart-Contract Security Integrity

**Scope:** canonical security due-diligence artifacts; multisig/timelock verification; upgradeability & hidden admin; oracle/economic attack surface; exploit taxonomy with named losses; audit effectiveness; governance-linked security interfaces.
**Out of scope (other slices):** tokenomics, governance design, regulation, case studies.
**Method note:** every `eth_call` / `eth_getStorageAt` result in this document was executed directly against a public Ethereum or Arbitrum JSON-RPC endpoint on 2026-10-06 by this analyst. Those readings are Tier B primary evidence (state of a verified contract), and are marked **[ON-CHAIN, verified]**. Where a figure or claim could not be retrieved from a retrievable source it is marked **UNSOURCED**.

---

## A1 Canonical security due-diligence artifacts, and what each actually proves

A credible project should publish a *set* of artifacts. Each proves something different, and the set is only meaningful if the artifacts are dated and cross-referenced to the exact deployed bytecode.

**1. Audit scope + methodology.** The minimum viable artifact is a published PDF whose scope section names the exact contract addresses audited, the commit hash, the compiler version, and the audit date. The reference checklist for "what should have been in scope" is the Smart Contract Security Verification Standard (SCSVS v1.2), a 14-part checklist whose categories are V1 Architecture/Design/Threat Modelling, V2 Access Control, V3 Blockchain Data, V4 Communications, V5 Arithmetic, V6 Malicious Input Handling, V7 Gas Usage & Limitations, V8 Business Logic, V9 Denial of Service, V10 Token, V11 Code Clarity, V12 Test Coverage, V13 Known Attacks, V14 DeFi [A24]. SCSVS is explicitly designed to be used "as a scoping document for penetration test or security audit of a smart contract" and as "a formal security requirement list for developers or third parties" [A24]. **What it proves:** that a named set of addresses was reviewed against a named checklist. **What it does not prove:** that the auditors found everything (see A6), or that the deployed code matches the audited source (see A3).

**2. Audit firm identity and reputation.** Auditor identity must be resolvable to a real firm with a public track record, not an anonymous "audit" PDF. Distinguish first-party audits (commissioned by the project) from community/crowdsourced reviews; a crowdsourced report has different evidentiary weight.

**3. Audit date vs deployment date — the highest-leverage, most-skipped check.** An audit dated *before* the deployed implementation is worth very little. This must be checked mechanically: compare the audit's stated commit/build date against `eth_getTransactionCount`-derived first-deploy block, or against the `Upgraded(address)` / `BeaconUpgraded` event log of the proxy. Euler is the canonical cautionary case: CertiK's own project page lists 12 available third-party audits while showing "Not Audited By CertiK" [A06c]; the March 2023 exploit post-dated the audit cycle.

**4. Source-code verification on the explorer.** Etherscan/Sourcify verification proves the published source compiles to the on-chain runtime bytecode. This is *bytecode identity*, not *semantic correctness*: it does not tell you the implementation is non-upgradeable, that the admin is a multisig, or that the logic is safe [B23].

**5. Formal verification / machine-checked invariants.** This is a distinct and higher-tier artifact than a manual audit because it produces a proof or a counterexample for a stated property. Compound is the canonical published example: from 2018 the team worked with Certora to formally verify code before deployment, writing formal specifications and using the Certora Prover to prove rules mathematically, with the prover generating a counterexample test case when a property does not hold [D12]. **What it proves:** the stated property holds for the modelled system. **What it does not prove:** the property was chosen well, or the model matches reality.

**6. Bug bounty with published economics.** Immunefi publishes per-program max bounty, total paid, and median resolution time as a live table [B25]; by June 2024 it had passed $100M in payouts across 3,000+ reports [D25]. A bounty with a published max bounty and a real history of payouts is evidence of a funded adversarial-research surface. A bounty page with zero paid reports is weaker.

**7. Published disclosure policy / security contact.** This is a prerequisite for #6 to be verifiable. Absence of one is a strong negative signal.

**8. Known-issue / risk-acknowledgement list.** Curve's Emergency DAO documentation is the best-in-class example: it explicitly enumerates its own scope ("designed very conservatively and cannot move or withdraw any user funds", permissions "strictly limited to reducing risk ... through parameter adjustments and pausing mechanisms") *and* names its members and deployments [B13]. **What it proves:** the project has modelled its own residual risk. A project with an audit but no known-issue list has usually not done threat modelling.

**Standards caveat that matters for scoring:** the SWC registry (SWC-101/104/105/107/114) is **no longer maintained** — its own pages state content "has not been thoroughly updated since 2020… It is known to be incomplete and may contain errors as well as crucial omissions," and redirect reviewers to the EEA EthTrust Security Levels specification and SCSVS instead [A06-A08]. A tool must not treat "addresses SWC-107" as a current-security signal. The successor is the EEA EthTrust Security Levels Specification v3 (EEA Specification, March 2025), which defines *certification levels* for "a defined set of security vulnerabilities" [D10].

**Verifiable checks**
- Locate a published audit PDF; extract scope, commit hash, compiler version, auditor legal entity, date.
- Compare audit date vs first-deploy block of the implementation (`eth_getCode` first seen at block; or earliest `Upgraded` log).
- Verify every contract holding or authorising user funds has Etherscan/Sourcify verification (or record it as unverifiable).
- Check for a named formal-verification programme with stated invariants (not just "formally verified" marketing).
- Check Immunefi listing: max bounty ≥ a meaningful fraction of TVL, and non-zero payouts.
- Check for a published security contact / disclosure policy and a known-issue list.
- Verify the audit's SWC/EthTrust/CSVS taxonomy references point to currently-maintained standards, not archived SWC.
- Measure audit coverage ratio: audited address set ÷ total contracts with authority over user funds.

**Sources:** [A24] [A06] [A07] [A08] [A09] [B23] [B25] [D12] [D25] [D10]

---

## A2 Verifying a claimed multisig / Safe / timelock instead of trusting the claim

This is where marketing claims and on-chain reality diverge most often. Below is a fully executed verification of a real protocol treasury control, plus the reference addresses needed to do it for any project.

### Authoritative reference addresses (Tier B, registry-sourced)
- **Safe v1.3.0 canonical singleton:** `0xd9Db270c1B5E3Bd161E8c8503c55cEABeE709552`, codeHash `0xbba688fbdb21ad2bb58bc320638b43d94e7d100f6f3ebaab0a4e4de6304b1c2e`; per-chain variant `eip155` = `0x69f4D1788e39c87893C980c06EdF4b7f686e2938` (same codeHash, used on ~250 chains incl. Ethereum, Arbitrum 42161, Base 8453, Polygon 137) [A01b].
- **Safe v1.4.1 canonical singleton:** `0x41675C099F32341bf84BFc5382aF534df5C7461a`, codeHash `0x1fe2df852ba3299d6534ef416eefa406e56ced995bca886ab7a553e6d0c5e1c4` [A01a].
- **Safe v1.4.1 zkSync variant:** `0xC35F063962328aC65cED5D4c3fC5dEf8dec68dFa` [A01a].
- **Compound v2 Timelock:** `0x6d903f6003cca6255d85cca4d3b5e5146dc33925`; Pause Guardian `0xbbf3f1421d886e9b2c5d716b5192ac998af2012c`; documented delay 2 days [B14].
- **OpenZeppelin `TimelockController`:** reference implementation with `PROPOSER_ROLE`, `EXECUTOR_ROLE`, `CANCELLER_ROLE`, `DEFAULT_ADMIN_ROLE`, `getMinDelay()`, `isOperation(id)`, `isOperationPending(id)`, `isOperationReady(id)`, `scheduleBatch`, `OperationState{Unset,Waiting,Ready,Done}` [B04]. Critically, the source comments that the optional `admin` "should be subsequently renounced in favor of administration through timelocked proposals," and that earlier versions auto-assigned admin to the deployer and it "should be renounced as well" [B04].
- **Aave v3 Ethereum:** `PoolAddressesProvider` `0x2f39d218133AFaB8F2B819B1066c7E434Ad94E9e`, `Pool` `0x87870Bca3F3fD6335C3F4ce8392D69350B4fA4E2`, `ACLManager` `0xc2aaCf6553D20d1e9d78E365AAba8032af9c85b0`, `ACL_ADMIN` = `EXECUTOR_LVL_1` `0x5300A1a15135EA4dc7aD5a167152C01EFc9b192A`, `EXECUTOR_LVL_2` `0x17Dd33Ed0e2dD2a80E37489B8A63063161BE6957`, `GRANULAR_GUARDIAN` `0x4457cA11E90f416Cc1D3a8E1cA41C0cdEcC251d4`, `GOVERNANCE_GUARDIAN` `0xCe52ab41C40575B072A18C9700091Ccbe4A06710` [B17].

### Critical trap: EIP-1967 detection produces FALSE NEGATIVES on Safe v1.3.0
The EIP-1967 implementation slot is `0x360894a13ba1a3210667c828492db98dca3e2076cc3735a920a3ca505d382bbc`; the admin slot is `0xb53127684a568b3173ae13b9f8a6016e243e63b6e8ee1178d6a717850b5d6103`; the beacon slot is `0xa3f0ad74e5423aebfd80d3ef4346578335a9a72aeaee59ff6cb3582b35133d50` [A03/B05].

But Safe v1.3.0's `GnosisSafeProxy` declares `address internal singleton;` as its **first** variable, and reads it from **storage slot 0** via `sload(0)`; `masterCopy()` is implemented in inline assembly with selector `0xa619486e` (`0xa619486e == keccak("masterCopy()")`, stated verbatim in the source) [A03b]. Safe v1.3.0 therefore **does not use EIP-1967 slots at all**.

**[ON-CHAIN, verified]** Curve Emergency DAO Safe, Ethereum, `0x467947EE34aF926cF1DCac093870f613C96B1E0c`:
| read | result |
|---|---|
| EIP-1967 impl slot | `0x0000…0000` ← **false "not a proxy"** |
| storage slot 0 | `0xd9db270c1b5e3bd161e8c8503c55ceabee709552` |
| `masterCopy()` (`0xa619486e`) | `0xd9db270c1b5e3bd161e8c8503c55ceabee709552` ✓ canonical v1.3.0 |
| `getThreshold()` (`0xe75235b8`) | `5` |
| `getOwners()` (`0xa0e67e2b`) | 9 owners (0xe9a65f…, 0x7a1057…, 0x2b47c5…, 0xdaa094…, 0x099bc0…, 0x0af175…, 0x0c6f3a…, 0xaac0aa…, 0x8a7dbc…) |
| runtime code length | 342 hex chars (~170 bytes) — minimal proxy |
| ETH / USDC / WETH balance | 0 / 0 / 0 |
| Safe Tx Service | `threshold: 5`, `owners: 9`, `masterCopy: 0xd9Db…9552`, `guard: 0x0`, `modules: []`, `nonce: 13` |
| recent executed txs | nonces 10–12, each with exactly **5 confirmations** |

This matches Curve's own documentation ("The EmergencyDAO is a **5-of-9 multisig**"; Ethereum at `0x467947EE34aF926cF1DCac093870f613C96B1E0c`; cross-chain at `0x6d447e544D01a59cb0774763bf15526574CffFeD`) [B13], and Curve ships a repo whose `validate_simple.py` "validates deployed multisigs onchain by checking owners and threshold" against that same baseline Safe [B12].

**Two analytically important conclusions from this single object:**
1. **It exists and it exercises real authority** (13 executed multisig transactions, each at full 5-of-9 threshold, to live protocol contracts), yet **it holds zero value** (0 ETH, 0 USDC, 0 WETH). So "is the Safe funded and moving funds" is the *wrong* question for a parameter-setting/pause authority — the right question is "does the Safe hold a recognised capability over material contracts?" Curve's Emergency DAO explicitly cannot touch user funds and instead holds pause + debt-ceiling-reduction + parameter rights [B13]. A scoring tool that only measures treasury balance would score this Safe as zero and a custodial Safe as maximal — exactly backwards for control risk.
2. **`modules: []` and `guard: 0x0` are positive findings, not neutral ones.** A Safe with an enabled module can bypass the threshold entirely; a Safe with a guard can block transactions. Curve's baseline Safe has neither, so its 5-of-9 threshold is the *only* gate. Any tool must read `getModulesPaginated` and `getGuard`, not just `getThreshold`.

### Cross-chain singleton divergence — a live example
**[ON-CHAIN, verified]** Curve's cross-chain Safe `0x6d447e544D01a59cb0774763bf15526574CffFeD` on Arbitrum One: storage slot 0 and `masterCopy()` both return `0xfb1bffc9d739b8d520daf37df666da4c687191ea` (confirmed on two independent Arbitrum RPCs). That address has code (47,602 hex chars) but **does not appear in the Safe deployments registry for either v1.3.0 (variants `canonical`/`eip155`/`zksync`) or v1.4.1 (`canonical`/`zksync`), and Arbitrum 42161 is mapped to `canonical`/`eip155` in v1.3.0 and `canonical` in v1.4.1** [A01a][A01b]. So a cross-chain "same Safe" can point at an unregistered singleton. Flag this as *unverified singleton*, not as malice — but it is a real, mechanically detectable divergence.

### Timelock verification
**[ON-CHAIN, verified]** Compound v2 Timelock `0x6d903f6003cca6255d85cca4d3b5e5146dc33925`:
- storage slot 0 (`admin`) = `0x309a862bbc1a00e45506cb8a802d1ff10004c8c0` (GovernorBravo) — i.e. self-administered via governance, no EOA escape hatch.
- storage slot 2 (`delay`) = `0x2a300` = **172,800 s = exactly 2 days**, matching Compound's documented "2 day review period … queued in the Timelock, and can be implemented 2 days later" [B14].
- Runtime code present (11,978 hex chars).

**[ON-CHAIN, verified]** Aave's `ACL_ADMIN` / `EXECUTOR_LVL_1` `0x5300A1a15135EA4dc7aD5a167152C01EFc9b192A` is **not** an OZ `TimelockController`: `getMinDelay()` and `delay()` both revert, `hasRole` reverts, EIP-1967 impl slot is zero, storage slot 0 = `0xdabad81af85554e9ae636395611c58f7ec1aaec5`, and the Safe Transaction Service returns 404 (i.e. it is not a Safe). Methodological consequence: a tool that only knows OZ `TimelockController`'s ABI will score every non-OZ governance executor as "no timelock." Always probe and treat revert as "unknown," not "absent."

### "Exists" vs "controls"
The decisive check is the *reverse* direction: for each contract the project claims the Safe controls, read that contract's admin/authority slot and confirm it equals the Safe address. Compound's Timelock slot-0 admin matches GovernorBravo [verified]; Curve's docs assert its Emergency DAO's pause/ceiling rights [B13]; Aave's docs state the `AaveOracle` "is owned by the Aave Governance" and that `setAssetSources` / `setFallbackOracle` are callable only by `POOL_ADMIN` or `ASSET_LISTING_ADMIN` via the `ACLManager` [B15].

**Verifiable checks**
- Detect Safe proxy: read `masterCopy()` (`0xa619486e`) **and** storage slot 0, not only EIP-1967 (avoids v1.3.0 false negative).
- Resolve singleton against the Safe deployments registry for that chain; flag unregistered singletons.
- `getThreshold()` vs `getOwners().length` → compute effective security as `(owners−threshold)+1` keys needed to lose control.
- `getModulesPaginated` non-empty → threshold can be bypassed by a module owner; `getGuard() != 0` → transactions can be vetoed.
- Safe Tx Service (`safe-transaction-<chain>.safe.global/api/v1/safes/<addr>/`) → `threshold`, `owners`, `masterCopy`, `guard`, `modules`, `nonce`, `txCount`; and `/multisig-transactions/?executed=true` to prove the Safe actually acts, and at how many confirmations.
- Reverse-authority check: for each material contract, read its admin/owner/role-admin slot and confirm it points at the claimed Safe/timelock, not an EOA.
- Timelock: `getMinDelay()` in seconds, `DEFAULT_ADMIN_ROLE` holder (must be the timelock itself or renounced), pending operations via `isOperationPending`/`isOperationReady` filtered on `CallScheduled` logs.
- Treasury liveness: native + stablecoin balances, and 30/90-day outbound transfer volume.
- Distinguish *capability* Safes (pause/parameter) from *custodial* Safes (hold value) and score each against the right criterion.

**Sources:** [A01a] [A01b] [A02] [A03a] [A03b] [A04] [A05] [A12] [A20] [B04] [B05] [B13] [B14] [B15] [B17] [B22] [A06]

---

## A3 Upgradeability and hidden admin control

**Three proxy patterns, three distinct risk shapes.**

*Transparent proxy (EIP-1967 + `ProxyAdmin`).* Upgrade logic and admin live **in the proxy**. Two properties go hand-in-hand: (1) any non-admin call is forwarded to the implementation even if it matches the admin's upgrade selector; (2) if the admin calls, it gets only the upgrade function and *cannot* fall through to the implementation [B05]. OpenZeppelin's docs therefore insist the admin "can only be used for upgrading the proxy, so it's best if it's a dedicated account that is not used for anything else" [B05]. Risk created: an admin-key compromise is a total, instant protocol takeover; and because OpenZeppelin sets the admin as an **immutable** while the ERC-1967 admin *slot* "can still be overwritten by the implementation logic," the on-chain slot can diverge from the real admin — "Relying on the value of the admin slot is generally fine if the implementation is trusted" [B05]. That is an explicit statement that the admin slot is not a reliable authority source once a malicious/buggy implementation is in place.

*UUPS (EIP-1822).* Upgrade logic lives **in the implementation**. Risks: (i) if you upgrade to an implementation that lacks the upgrade mechanism, the proxy is permanently bricked; (ii) the ERC-1822 `proxiableUUID` guard "can be bypassed by either … adding a flag mechanism … or upgrading to an implementation that features an upgrade mechanism without the additional security check, and then upgrading again" [B05]; (iii) critically, "since both proxies use the same storage slot for the implementation address, using a UUPS compliant implementation with a `TransparentUpgradeableProxy` might allow non-admins to perform upgrade operations" [B05] — a pattern-mismatch vulnerability that only appears when reading the source of *both* contracts.

*Beacon proxy.* The implementation address lives in a **separate beacon contract**; "all proxies that follow that beacon are automatically upgraded" [B05]. Blast radius is multiplied: one beacon admin key rewrites every proxy at once. **[ON-CHAIN, verified]** Aave v3's `Pool` (`0x87870Bca3F3fD6335C3F4ce8392D69350B4fa4E2`) has impl slot `0x728a138a4823392c2efa55e028d434f526fe03cf` — exactly the `POOL_IMPL` in Aave's published address book [B17] — with **admin slot = 0 and beacon slot = 0**, i.e. a UUPS proxy, not a transparent proxy. The proxy pattern is therefore mechanically determinable and materially changes who-can-upgrade analysis.

**Uninitialised-proxy / uninitialised-implementation.** OpenZeppelin: "An uninitialized contract can be taken over by an attacker. This applies to both a proxy and its implementation contract"; the remedy is `_disableInitializers()` in the implementation constructor, and "Uninitialized proxies might be susceptible to man-in-the-middle threats where the proxy is replaced with a malicious one" [B05]. Nomad's August 2022 bridge loss is the canonical real failure of exactly this class of "acceptable default" reasoning (see A5).

**What a verified source does and does not tell you.** Verified source proves: (a) the deployed runtime bytecode equals the compilation of the published source; (b) therefore the *current* logic can be read and reasoned about. It does **not** tell you: whether the address is a proxy; who the admin is; whether the admin is a multisig; whether a beacon or `ProxyAdmin` sits behind it; whether the *next* upgrade is benign; whether off-chain signers hold the keys. Conversely, a **verified source that looks clean is not evidence of safety** — Radiant Capital's January 2024 Arbitrum exploit was a flash-loan manipulation of `liquidityIndex` enabled by "the market's empty reserves upon launch," with ~1,190 ETH of bad debt repaid and ~720 ETH outstanding [B18]. Cream Finance's October 2021 loss was "a mix of economic and oracle exploits": the attacker flash-borrowed DAI from MakerDAO to mint yUSD while manipulating the multi-asset pool the yUSD price oracle relied on, then removed liquidity at artificial prices [B19]. KyberSwap's November 2023 Elastic exploit likewise "exploited a vulnerability in the" [implementation] [B20], later charged in a DOJ indictment at roughly $65M across KyberSwap and Indexed Finance [A02].

**The "unverified contract" red flag.** If runtime bytecode exists but no verified source is published, the entire artifact set in A1 is unverifiable and every claim about access control is hearsay. ethereum.org's own developer guidance frames contract security as a first-class concern precisely because "deployed contract code usually cannot be changed to patch security flaws, while assets stolen from smart contracts are extremely difficult to track and mostly irrecoverable due to immutability," and notes total value lost to smart-contract defects is "easily over $1 billion," citing the DAO hack (3.6M ETH), the Parity multisig wallet hack ($30M) and the Parity frozen wallet (over $300M in ETH locked forever) [B23].

**Why a multisig can still be a single-key compromise in practice.** On-chain state fixes *N* and *threshold*. It does not observe: whether the same EOA controls multiple signers; whether signers are hot wallets; whether signing happens on a compromised machine; whether hardware wallets and a documented key-rotation process exist; whether a signer set has drifted (added/removed owners, threshold changes) without public disclosure. Curve's own baseline is the reference standard here: it publishes a 5-of-9 signer set *with named members*, and ships tooling that validates deployed Safes' owners and threshold against the baseline [B12][B13]. An unnamed, unpinned, never-rotated signer set behind an "n-of-m Safe" claim is a materially different security object from a published one, and only the latter is auditable. Harmony's June 2022 Horizon bridge loss is the proof: Harmony described the keys as "encrypted … doubly encrypted via passphrase and a key management service, and no single machine had access to multiple plaintext keys" — and the attacker still "was able to access and decrypt a number of these keys," draining ~$100M across 11 transactions; afterwards they moved to a 4-of-5 multisig [B21]. Sophisticated key management is not the same as a robust threshold, and describing the former is a common substitute for evidencing the latter.

**Verifiable checks**
- Read all three EIP-1967 slots; classify transparent (admin set) vs UUPS (admin zero, upgrade fn in impl) vs beacon (beacon set).
- Call `proxiableUUID()` (`0x52d1902d`) on the implementation: must revert when called through the proxy (that is what prevents bricking), and must equal the EIP-1967 slot.
- Resolve the admin/owner/role-admin of the proxy, `ProxyAdmin`, or `UpgradeableBeacon` — recursively, until you reach an EOA or a Safe.
- Call `getImplementation()`/`getBeacon()` on the beacon; count how many proxies share the beacon (blast radius).
- Check for upgrade history: count `Upgraded` / `AdminChanged` / `BeaconUpgraded` events and list every implementation address ever live; diff each against the audit set.
- Check the implementation for `_disableInitializers()` in its constructor (or that `initialize` is unrevertable once used).
- Flag `eth_getCode != 0x` with no verified source as a hard negative.
- Detect pattern mismatch: UUPS implementation behind a transparent proxy, or vice versa.
- Extract Safe `getOwners()`, compare against a published signer list, and check whether multiple owners are the same address or trivially linked wallets.
- Check signer-rotation evidence: any owner add/remove or threshold change in history, and whether it was disclosed.

**Sources:** [A03a] [B05] [B10] [B17] [B18] [B19] [B20] [B21] [B23] [B12] [B13] [A02]

---

## A4 Oracle, price-manipulation and economic-attack surface

**Why flash loans change the security model.** Atomicity means an adversary can borrow, distort, and repay inside one transaction. Qin, Zhou, Livshits and Gervais formalise the oracle-manipulation pattern: borrow 7,500 ETH by flash loan, convert to 1,099,841 sUSD across Uniswap and Kyber (pushing the sUSD/ETH price down to 106.05 and 108.44 while Synthetix remains unaffected), then collateralise to borrow against the *depressed* on-chain rate — an ROI above 500,000%, which they show could be "boosted" to $829.5k and $1.1M respectively [D02]. The generalisable lesson: **any price feed sourced from an AMM pool is manipulable to approximately the depth of that pool**, and the mitigation is never "the pool is large" — it is *independent* sourcing (TWAP, multiple venues) plus a delay/liquidation buffer.

**Named oracle/economic attacks.**
- **Cream Finance (Oct 2021, protocol's own post-mortem):** "a mix of economic and oracle exploits" — flash-borrowed DAI to mint yUSD while manipulating the yDAI/yUSDC/yUSDT/yTUSD pool underlying the yUSD price oracle, in a single transaction; the inflated yUSD position "created sufficient borrow limit to remove the vast majority of the liquidity from C.R.E.A.M. Ethereum v1 markets" [B19].
- **Mango Markets (Oct 2022, $110M+, Tier A):** the CFTC's first "oracle manipulation" enforcement action, charging Avraham Eisenberg with a scheme to "unlawfully misappropriate over $110 million in digital assets from a purported decentralized digital asset exchange" on 11 October 2022, via two anonymous accounts holding large leveraged swap positions in MNGO/USDC [A03]. Note the *structural* lesson: the vulnerability was the price oracle's assumption that a thin, borrowable float could not be moved against a thin, borrowable derivative.
- **MakerDAO "Black Thursday" (12 March 2020):** ETH fell 43% ($194→$111) in a day; congestion prevented the Medianizer oracle from updating; when the feed did update the price dropped >20% and mass liquidations began; because auctions could not be bid on, liquidators "won these auctions with bids of zero DAI," extracting over $8M of ETH essentially for free, leaving ~$4.5M of DAI unbacked [D03]. Lesson: **liveness of the oracle and of the auction mechanism is part of the security model**; a correct oracle with a starved liquidation path is still a loss event.
- **KyberSwap Elastic (Nov 2023):** affected pools drained by a sophisticated primary exploit that was then "mimicked by front-run bots," rendering assets inaccessible to users [B20]; DOJ charged the same actor with exploiting KyberSwap and Indexed Finance for ~$65M by borrowing hundreds of millions in tokens to "engage in deceptive trading that he knew would cause the protocols' smart contracts to falsely calculate key variables," withdrawing "millions of dollars of investor funds… at artificial prices" [A02]. **Front-run-bot imitation is a distinct failure mode**: it converts a private exploit into a public one within one block, which is why "pause fast" and "low liquidity" both amplify loss.
- **Aave's own oracle design** is the counter-pattern worth scoring against: the `AaveOracle` is "owned by the Aave Governance"; `setAssetSources` and `setFallbackOracle` require `POOL_ADMIN` or `ASSET_LISTING_ADMIN` via `ACLManager` [B15]. i.e. feed substitution is a governance-gated event, not a runtime parameter.

**Flash-loan-assisted governance capture.** Beanstalk (16–17 April 2022): the attacker bought 212,858 BEAN with 73 ETH, took a flash loan of almost $1B, deposited into the silo to accumulate a ~67% "stalk" voting position, and passed two malicious "Bean Improvement Proposals" disguised as Ukraine-donation proposals that transferred protocol funds to the attacker's wallet; the attacker obtained just under 25,000 ETH (~$76M), with total protocol losses believed to reach $182M [D04]. KyberSwap adds a second variant: after the November 2023 exploit the attacker allegedly attempted **extortion** — "a sham settlement proposal in which he demanded complete control of the KyberSwap protocol and the decentralized autonomous organization that oversaw the KyberSwap protocol in exchange for returning 50 percent" of the assets [A02]. Governance-capture risk must therefore be scored on (a) whether flash-borrowable tokens carry voting weight, (b) whether proposals can execute immediately on passage (no timelock), and (c) whether quorum is expressed as a fraction of total supply rather than of circulating/voted supply.

**Sandwich/MEV exposure.** Value extractable by a sandwich is bounded by the slippage tolerance a transaction is willing to tolerate times the pool depth at the block the transaction lands in. Operational implication: a protocol that relies on AMM execution for user swaps inherits MEV risk it cannot audit away; the mitigations are private order flow, batch auctions, or slippage caps — none of which are visible in contract source. Treat sandwich exposure as a *design* property.

**Bridges.** Trust assumptions are the whole risk. Ronin (March 2022) — the largest digital-currency theft to date at the time, ~$615M+ in ETH and USDC, attributed by US officials to the Lazarus Group [D09] — was a signer-key compromise, not a contract bug, and Ronin ran a public Immunefi bounty. Wormhole-style validation failure is the other class. The generalisable check: count the parties whose keys/attestations are required, and ask whether the bridge is *verifiable* (light client, proof-based) or *trusted* (multisig/MPC). Harmony's Horizon bridge moved to a 4-of-5 multisig only *after* losing ~$100M [B21]; Multichain-style MPC designs concentrate the same risk in fewer parties.

**Quantifying exploit-to-TVL.** Loss magnitude is extremely fat-tailed and not well predicted by TVL: CertiK recorded $2,362,748,975.83 lost across **760** incidents in 2024 (avg $3,108,880; **median $150,925**), and $1.84B across **751** incidents in 2023 (avg $2.45M; median $101,132) [C01][C02]. With Euler's ~$197M representing ~70% of 2023's total losses [B06], the mean is dominated by a handful of events. A scoring tool should therefore use a **median-relative** or **worst-case-at-given-TVL** measure rather than an expected-loss one, and should weight *attack-vector* concentration: in 2024 phishing alone was $1,050,129,498 across 296 incidents (39.1% of incidents, ~half of all value) and private-key compromise $855,385,570 across just **65** incidents (8.6% of incidents, 36% of value) [C01]. In 2023, private key compromise was 6.3% of incidents but "nearly half of the year's total financial losses" [C02]. **Contract quality is therefore a minority determinant of realised loss in the current regime** — a fact a scoring tool must not hide.

**Verifiable checks**
- Map every price feed used by the protocol to its source; flag any sourced from a single AMM pool or from a thin-liquidity pair.
- Measure feed venue depth at the traded size; compute manipulation cost as a fraction of borrowable float for governance tokens.
- Compare oracle update cadence / deviation threshold / grace period against known congestion events.
- Read `setAssetSources` / `setFallbackOracle` authority on the oracle contract; require governance + timelock.
- Check voting token borrowability on the oracle source (e.g. lending markets listing the governance token) → flash-vote exposure.
- Compute `threshold_debt` vs `max_liquidation` to size liquidator-griefing exposure.
- Check whether governance proposals can execute on passage (no timelock) and whether quorum is % of total supply.
- For bridges: count required signatures/attesters, classify as trusted vs verifiable, list the multisig signer sets, and read their thresholds.
- Compute exploit-to-TVL using median-per-incident (~$0.1–0.15M) for calibration and a named-precedent worst case (Euler $197M; Ronin $615M+) for tail risk.
- Attribute loss history by vector (contract bug / oracle / key compromise / phishing / frontend) and treat key-compromise + phishing as a *separate* scored axis from code quality.

**Sources:** [D02] [B19] [A03] [D03] [B20] [A02] [B15] [D04] [D09] [B21] [B06] [C01] [C02]

---

## A5 Exploit & failure taxonomy with named examples and the prevention artifact each taught us

| # | Failure mode | Named example (year) | Loss | Prevention artifact the incident taught us |
|---|---|---|---|---|
| 1 | **Reentrancy** | The DAO (2016) | 3.6M ETH; "easily over $1 billion" cumulative for contract defects | SWC-107 remediation: **Checks-Effects-Interactions** ordering plus a reentrancy lock (OZ `ReentrancyGuard`); SWC-107's own worked example is a `SimpleDAO` whose fix moves `credit[msg.sender] -= amount` *before* the low-level call [A06] |
| 2 | **Reentrancy (via compiler, not app code)** | Curve pools pETH/msETH/alETH/CRV (30 Jul 2023) | ≈$61.7M extracted per LlamaRisk's pool-by-pool tally (6,106 WETH ≈$11M; 866 WETH ≈$1.6M + 959.71 msETH ≈$1.8M; 7,258 WETH ≈$13.6M + 4,821 alETH ≈$9M; 7,193,401 CRV ≈$5.1M + 7,680 WETH ≈$14.2M + 2,880 ETH ≈$5.4M) [D05] | **Compiler/lockfile pinning + advisory-aware dependency monitoring.** Vyper's advisory GHSA-5824-cm3x-3c38 / **CVE-2023-39363** (Critical, patched 0.3.1): "named re-entrancy locks are allocated incorrectly. Each function using a named re-entrancy lock gets a unique lock regardless of the key, allowing cross-function re-entrancy in contracts compiled with the susceptible versions" [A11]. The lesson: `@nonreentrant` in source is not evidence of a reentrancy guard if the compiler mis-allocates the lock — audit the *build*, not just the source |
| 3 | **Reentrancy (post-audit, live system)** | Cream Finance (Oct 2021) | Oracle + economic exploit draining most Ethereum-v1 liquidity [B19]; UwU Lend (Jun 2023) is a comparable reentrancy-driven lending-market collapse (loss figure ~$19.3M — **partially sourced, see note**) | ReentrancyGuard/CEI applied *across every* external-call path including token callbacks, plus an explicit economic-threat line item in the audit scope |
| 4 | **Oracle manipulation** | Mango Markets (11 Oct 2022) | $110M+ [A03]; KyberSwap Elastic + Indexed Finance (Nov 2023) | ~$65M [A02]; Cream (Oct 2021) [B19]; Black Thursday (Mar 2020) ~$4.5M unbacked DAI + >$8M ETH zero-bid [D03] | **Independent price sources + TWAP/deviation thresholds + governance-gated feed substitution** (`setAssetSources`/`setFallbackOracle` behind `POOL_ADMIN`/`ASSET_LISTING_ADMIN` [B15]); plus an economic-attack (not just code) audit scope item |
| 5 | **Proxy / upgrade hijack** | Nomad bridge (Aug 2022) | ~$190M across many chains (approximate; **figure not retrieved from a primary source in this pass — treat as approximate**) | **Uninitialised-default hardening.** Nomad's own root-cause analysis: an implementation bug made `Replica` fail to authenticate messages; because a `uint256` mapping default is `0` and "Any root that will not have been attested to will therefore have a `0` timestamp," the `acceptableRoot` check treated un-attested roots as acceptable — i.e. "any message that has not been proven will have a root of `bytes32(0)`" [B10]. The prevention artifact is **failing closed on default/uninitialised state** and asserting non-zero sentinels, plus `_disableInitializers()` [B05] |
| 6 | **Private-key compromise of admin** | Ronin (Mar 2022) | ~$615M+ ETH + USDC, attributed to Lazarus by US officials [D09]; Harmony Horizon bridge (Jun 2022) ~$100M via 11 transactions after decryption of stored keys [B21] | **Key hygiene + threshold discipline + published signer sets.** Harmony's post-incident fix was to move to a 4-of-5 multisig [B21]; Ronin's was an active Immunefi bounty. Prevention artifact is operational (HSM/hardware, rotation, published signers, on-chain drift monitoring), not code |
| 7 | **Governance attack** | Beanstalk (Apr 2022) | ~$76M taken by the exploiter; total protocol loss believed ~$182M [D04] | **Quorum/voting-delay hardening + timelock + borrow-aware voting.** The flash loan of ~$1B bought a ~67% stalk position and pushed malicious "Ukraine donation" BIPs through instantly [D04]. Prevention artifacts: proposal delay, quorum on circulating rather than total supply, and governance-token borrow monitoring |
| 8 | **Bridge validation failure** | Ronin / Harmony / Poly Network (Aug 2021) | Ronin ~$615M [D09]; Harmony ~$100M [B21]; Poly Network — SlowMist's analysis of the same-day attack attributes it to four root causes including that "the source chain did not check the initiated cross-chain operation," "the target chain did not check the parsed target call contract and call parameters," and a **hash collision** in `bytes4(keccak256(abi.encodePacked(_method,"(bytes,bytes,uint64)")))` [D11] | **Canonical message encoding / no selector-collision-prone type hashing**, plus independent per-chain input validation, plus signature-verification code written in a higher-level, audited form. Prevention artifact is a spec-level encoding rule plus third-party bridge audit |
| 9 | **Stablecoin depeg / issuer control** | USDC (Mar 2023) | Peg broke on SVB/Signature exposure | Circle: the "$3.3B USDC reserve deposit held at Silicon Valley Bank, about 8% of the USDC total reserve," with the reserve 77% ($32.4B) T-bills custodied at BNY Mellon and 23% ($9.7B) cash at BNY Mellon [D14] — **exposure concentration is the depeg mechanism**, so score reserve composition and single-custodian concentration. USDT (Oct 2023): Tether "proactively and voluntarily" froze ~$225M of USDT in self-custodied wallets linked to a trafficking syndicate, following a DOJ investigation, in the largest such freeze to date [D13] — i.e. the issuer's freeze capability is operationally decisive |
| 10 | **Centralisation-by-design** | FTX-style custodial collapse (adjacent, **not researched here — out of scope for this slice**) | — | For this slice the artifact is *disclosure*: Curve's Emergency DAO explicitly states it "is designed very conservatively and cannot move or withdraw any user funds" and enumerates exactly what it can do [B13]. A project that does not publish an equivalent scope statement is asserting centralisation implicitly |
| 11 | **Economic-attack / precision loss** | Radiant Capital (2 Jan 2024) | 1,190 ETH bad debt repaid, ~720 ETH outstanding | [B18] | **Launch-state invariants**: an empty-reserve market makes `liquidityIndex` attacker-settable. Prevention artifact is a post-deployment precondition check ("no market may launch with zero reserves and attacker-influenceable indexes") |
| 12 | **Frontend / Web2 compromise** | Badger DAO (Dec 2021) | ~$120M (approx.; **figure not retrieved from a primary source in this pass**) | [D14b] Badger's own recovery page is the artifact: a review with Mandemand/Mandiant "looking at everything from the smart contract layer, to the app infrastructure, to how we communicate," a Halborn audit of *new infrastructure*, and the explicit conclusion "even as Badger's core smart contracts were not impacted — phishing attacks, Web2 vulnerabilities, and user behaviors can interact in ways that pose major security threats." This is the canonical proof that **contract audits have zero coverage of the attack surface that now dominates losses** [C01] |

**Note on partial sourcing.** UwU Lend (~$19.3M), Nomad (~$190M), Badger DAO (~$120M) and The DAO's USD-at-the-time figure are widely reported but I did **not** retrieve a primary/authoritative source for those specific numbers in this pass. They are marked approximate and must not be scored on without confirmation. The DAO's *ETH* figure (3.6M ETH) and the cumulative "easily over $1 billion" are sourced to ethereum.org [B23]; Nomad's *mechanism* is sourced to Nomad's own root-cause analysis [B10]; Badger's *lesson* is sourced to Badger's own recovery page [D14b].

**Verifiable checks**
- Maintain a dated incident table per contract with vector, root cause, and whether a prevention artifact existed pre-incident.
- For each incident, ask: was the code audited? was the *specific path* audited? was the *dependency* (compiler) in the audit's threat model?
- Track recurring vectors to weight your signal set: reentrancy, oracle, key compromise, governance, bridge validation, depeg, centralisation.
- Flag any project with a live incident history *and* no published known-issue list.

**Sources:** [A06] [A11] [D05] [B19] [A03] [A02] [D03] [B05] [B10] [D09] [B21] [D04] [D11] [D14] [D13] [B13] [B18] [D14b] [B23] [C01] [A01a]

---

## A6 How much value does security genuinely protect? Audit outcomes vs post-audit incidents

**Audits are necessary and structurally insufficient.** Three independent lines of evidence:

1. **Tooling finds exploitable contracts en masse.** teEther (USENIX Security '18) analysed **all 38,757 unique Ethereum contracts** and generated working exploits for **815** of them, *completely automatically, from bytecode alone* [D09b]. Automated analysis therefore has a large non-zero hit rate on deployed code.
2. **But human and tool review both miss things, and the evaluation methodology itself distorts conclusions.** An ICSE 2021 study collected 46,186 source-available contracts from four influential organisations and evaluated nine tools under a unified standard, concluding that "different choices of experimental settings could significantly affect tool performance and lead to misleading or even opposite conclusions" [D13b]. Durieux & Ferreira's ICSE 2020 review covered 47,587 contracts [D13c]. The implication for scoring: *the presence of an audit report is evidence that a process ran; it is weak evidence about the residual defect count*, and the same weakness applies to automated scanners the tool might otherwise score highly.
3. **The dominant loss vectors are not code defects at all.** 2024: $1.05B phishing (296 incidents), $855M private-key compromise (65 incidents), against a $2.36B total across 760 incidents [C01]. 2023: private-key compromise was 6.3% of incidents but ~half of losses [C02]. Chainalysis' 2025 report records $40.9B received by identified illicit addresses in 2024 (a lower-bound estimate) [C03]. A scoring engine that treats "has an audit" as the dominant security term will systematically over-rank audited protocols whose losses actually come from keys, frontends and people.

**Named post-audit incidents (contract was audited before the loss):**
- **Euler (13 Mar 2023, ~$197M).** CertiK's incident analysis attributes the loss to a vulnerability in `donateToReserves()` present "within five separate pools," exploited with a 30M DAI Aave flash loan that built a leveraged insolvent position via Euler's recursive `mint()`, then liquidated and drained via withdrawals; stolen assets included 8,877,507 DAI, 8,080 WETH, 846.4 WBTC, 73,821 stETH and 34,224,863 USDC [B06]. Euler had a substantial audit history (CertiK's project page lists 12 available third-party audits [B06c]). **Lesson:** an audit reviewed the *invariant* "donations cannot create an insolvent position" as out of scope; the real invariant is "no sequence of operations may let a borrower reach a liquidatable-but-unliquidatable state that transfers value." Economic-adjacent invariants must be in the audit scope, and reentrancy-style guards do not address them.
- **Cream Finance (Oct 2021)** [B19], **KyberSwap Elastic (Nov 2023)** [B20][A02], **Radiant Capital (Jan 2024)** [B18], **Beanstalk (Apr 2022)** [D04] — all occurred in audited or heavily-reviewed systems.
- **Badger DAO (Dec 2021)** — the decisive counterexample: Badger's own statement says its **core smart contracts were not impacted** at all [D14b]. An audit could not have helped.
- **Ronin / Harmony** — code was not the failure mode [D09][B21].

**Documented limits of audits, stated by the standards themselves.** EthTrust Security Levels is explicitly a *certification against a defined set of vulnerabilities*, not a warranty: it defines requirements "for EEA EthTrust Certification, a set of certifications that a smart contract has been reviewed and found not to have a defined set of security vulnerabilities" [D10]. SCSVS is "a FREE 14-part checklist" usable "as a measure of your smart contract security and maturity" — a maturity measure, not a residual-risk bound [A24]. And OpenZeppelin's own proxy documentation concedes that reading the admin slot is "generally fine **if the implementation is trusted**" [B05] — an explicit dependency on trust outside the audited artifact.

**Verifiable checks**
- For each incident in the project's history, record whether an audit predated it and whether the incident was *within* the audit's declared scope.
- Compute post-audit incident rate across the protocol's own history (incidents after last audit / months since).
- Weight signal set toward vector-appropriate controls (key management, frontend integrity, disclosure policy) rather than audit presence alone.
- Require audit scope to include an explicit *economic/invariant* section before crediting an audit as covering oracle/AMM/gov attack surface.
- Check for continuous verification (formal verification in CI, property-based/fuzz testing in CI) as the only credible route to "audit coverage persists across upgrades."

**Sources:** [D09b] [D13b] [D13c] [C01] [C02] [C03] [B06] [B06c] [B19] [B20] [A02] [B18] [D04] [D14b] [D09] [B21] [D10] [A24] [B05]

---

## A7 Governance-linked security interfaces: who can pause, blacklist, upgrade or freeze user funds

**Censorship capability is mechanically detectable.** The interface surface is small and enumerable:
- **`owner()` (0x8da5cb5b)** → single-EOA-or-contract admin.
- **`paused()` (0x5c975abb)** / `pause()` → freeze of all transfers.
- **`blacklist(address)` / `isBlacklisted(address)` (0xfe575a87)** → per-address denial.
- **`minter()` / `burner()`** → ability to inflate supply.
- **Proxy admin / UUPS `_authorizeUpgrade`** → ability to replace all logic.
- **Timelock over the above** → delay before any of it.

**[ON-CHAIN, verified] USDC (Ethereum), `0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48`:** `owner()` = `0xfcb19e6a322b27c06842a71e8c725399f049ae3a` (a single contract address, not a Safe — the Safe Transaction Service does not index it as one and its EIP-1967 impl slot is zero). `paused()` = `0x00…00` (not paused). `isBlacklisted(0x7a250d5630b4cF539739dF2C5dACb4c659F2488D)` = `0x00…00`. `blacklist()` (0xef379d17) is **not** exposed as a public getter on the current implementation, so blacklist authority must be inferred from the `owner` address + verified source, not from a storage read.

**[ON-CHAIN, verified] USDT (Ethereum), `0xdAC17F958D2ee523a2206206994597C13D831ec7`:** `owner()` = `0xc6cde7c39eb2f0f0095f41570af89efc2c1ea828`; `paused()` = `0x00…00`.

**Interpretation — and this is the key scoring insight:** `owner()` on both major dollar stablecoins resolves to a **single contract address with no timelock and no publicly disclosed signer set**. Whether that contract is internally governed by a multi-party process is *not* observable from the token's own storage. Tether's demonstrated freeze of ~$225M of USDT [D13] proves the capability is real and operational, not theoretical. So a tool must classify a stablecoin's control model from **issuer-published terms plus issuer-published reserve/compliance documentation**, not from the token contract alone — and must score "issuer can freeze arbitrary user balances at request of a government" as a *disclosed, exercised* capability rather than a hypothetical.

**Aave as the decentralised comparison — and where it is still not fully decentralised.** Aave's oracle is "owned by the Aave Governance," with feed changes behind `POOL_ADMIN`/`ASSET_LISTING_ADMIN` in the `ACLManager` [B15]; governance runs on a Core Network / Voting Networks / Execution Networks split with an a.DI delivery infrastructure and a "Governance Emergency Guardian" that can "veto" an onchain payload if deemed malicious [B16b]. Two dedicated guardians exist in the address book: `GRANULAR_GUARDIAN 0x4457cA11E90f416Cc1D3a8E1cA41C0cdEcC251d4` and `GOVERNANCE_GUARDIAN 0xCe52ab41C40575B072A18C9700091Ccbe4A06710`, alongside `EXECUTOR_LVL_1/LVL_2` [B17]. **[ON-CHAIN, verified]** `ACL_ADMIN` = `EXECUTOR_LVL_1 0x5300A1a15135EA4dc7aD5a167152C01EFc9b192A` is not a Safe and not an OZ `TimelockController` (see A2) — so "who can pause Aave and after what delay" is **not answerable by generic ABI probing** and requires the governance-process documentation [B16]. This is a real, citable limitation and a legitimate negative signal about verifiability.

**The asymmetry to score.** USDC/USDT: single-address issuer admin, no on-chain delay, capability exercised at scale [verified][D13]. Curve Emergency DAO: 5-of-9 Safe, published signers, published narrow scope, timelocked behind DAO proposals [B13][verified]. Both are "centralised" in different, and mechanically distinguishable, ways. A binary "centralised/decentralised" flag destroys information; a capability tuple (who can pause, at what threshold, with what delay, over what scope, disclosed or not) is the right unit.

**Verifiable checks**
- For every token/contract a user can deposit into, enumerate: `owner()`, `paused()`, blacklist function presence, minter, proxy admin, and the authority behind each.
- Resolve each authority address one hop: EOA (worst), contract with published signer set (checkable), Safe (checkable), timelock (checkable, incl. `getMinDelay`).
- Confirm the admin is *not* reachable instantly: require a timelock with a non-trivial `getMinDelay()`, and that `DEFAULT_ADMIN_ROLE` is held by the timelock itself (OZ's own guidance: the admin "should be subsequently renounced in favor of administration through timelocked proposals") [B04].
- For Safes: threshold ≥3 with distinct signers, no modules, no guard, published signer list.
- Emit an explicit "censorship capability" object per project: {can_pause, can_blacklist, can_upgrade, can_freeze, delay_seconds, signer_set_disclosed, capability_exercised_hist}.
- Prefer issuer-published terms (e.g. Circle's USDC Terms) and reserve composition disclosures over inference from token source [D14].

**Sources:** [A16] [A17] [B15] [B16] [B16b] [B17] [B04] [B13] [D13] [D14]

---

## Sources

| Ref | Tier | Title | URL | Date accessed |
|---|---|---|---|---|
| A01a | B | Safe deployments registry — Safe v1.4.1 (`safe.json`): canonical singleton + codeHash + per-chain mapping | https://raw.githubusercontent.com/safe-global/safe-deployments/main/src/assets/v1.4.1/safe.json | 2026-10-06 |
| A01b | B | Safe deployments registry — Safe v1.3.0 (`gnosis_safe.json`): canonical / eip155 / zksync variants + codeHash | https://raw.githubusercontent.com/safe-global/safe-deployments/main/src/assets/v1.3.0/gnosis_safe.json | 2026-10-06 |
| A02 | B | Safe `Safe.sol` v1.4.1 source — threshold, owners, confirmations, EIP-712 tx hashing | https://raw.githubusercontent.com/safe-global/safe-smart-account/v1.4.1/contracts/Safe.sol | 2026-10-06 |
| A03a | B | EIP-1967: Proxy Storage Slots (official spec) | https://eips.ethereum.org/EIPS/eip-1967 | 2026-10-06 |
| A03b | B | `GnosisSafeProxy.sol` v1.3.0 source — singleton at storage slot 0, `masterCopy()` selector `0xa619486e` | https://raw.githubusercontent.com/safe-global/safe-smart-account/v1.3.0/contracts/proxies/GnosisSafeProxy.sol | 2026-10-06 |
| A04 | B | `TimelockController.sol` (OpenZeppelin, last updated v5.6.0) — roles, `getMinDelay`, operation states, admin-renounce guidance | https://raw.githubusercontent.com/OpenZeppelin/openzeppelin-contracts/master/contracts/governance/TimelockController.sol | 2026-10-06 |
| A05 | B | OpenZeppelin Contracts — Proxy API: ERC-1967 slot constants, Transparent vs UUPS, Beacon, `Initializable` | https://docs.openzeppelin.com/contracts/5.x/api/proxy | 2026-10-06 |
| A06 | B | SWC-107 Reentrancy (registry; page states content no longer maintained since 2020) | https://swcregistry.io/docs/SWC-107 | 2026-10-06 |
| A07 | B | SWC-105 Unprotected Ether Withdrawal | https://swcregistry.io/docs/SWC-105 | 2026-10-06 |
| A08 | B | SWC-104 Unchecked Call Return Value | https://swcregistry.io/docs/SWC-104 | 2026-10-06 |
| A09 | B | SWC-101 Integer Overflow and Underflow | https://swcregistry.io/docs/SWC-101 | 2026-10-06 |
| A10 | B | EIP-1822: Universal Upgradeable Proxy Standard (UUPS) | https://eips.ethereum.org/EIPS/eip-1822 | 2026-10-06 |
| A11 | B | Vyper advisory GHSA-5824-cm3x-3c38 / CVE-2023-39363 "Incorrectly allocated named re-entrancy locks" (Critical, patched 0.3.1) | https://github.com/vyperlang/vyper/security/advisories/GHSA-5824-cm3x-3c38 | 2026-10-06 |
| A12 | B | curvefi/emergency-msig-crosschain — baseline Safe, on-chain owner/threshold validation tooling, replay manifests | https://github.com/curvefi/emergency-msig-crosschain/tree/main | 2026-10-06 |
| A13 | B | Curve docs — Emergency DAO: 5-of-9, scope statement, named members, deployments | https://docs.curve.finance/user/dao/emergency-dao | 2026-10-06 |
| A14 | B | Compound v2 docs — Governance: Timelock address, 2-day delay, Pause Guardian address | https://docs.compound.finance/v2/governance/ | 2026-10-06 |
| A15 | B | Aave docs — `AaveOracle`: owned by Aave Governance, `setAssetSources`/`setFallbackOracle` role gates | https://www.aave.com/docs/aave-v3/smart-contracts/oracles | 2026-10-06 |
| A15b | B | Aave docs — Governance v3 architecture, Guardians, execution networks | https://aave.com/docs/ecosystem/governance | 2026-10-06 |
| A16 | B | Ethereum mainnet JSON-RPC `eth_call`/`eth_getStorageAt`/`eth_getBalance`/`eth_getCode` reads (USDC `0xA0b8…6eB48`, USDT `0xdAC1…1ec7`) via publicnode — analyst-executed 2026-10-06 | https://ethereum-rpc.publicnode.com | 2026-10-06 |
| A17 | B | Arbitrum One JSON-RPC reads of Curve cross-chain Safe `0x6d447e…ffFeD` (slot 0, `masterCopy()`) via two independent endpoints | https://arbitrum-one-rpc.publicnode.com · https://arb1.arbitrum.io/rpc | 2026-10-06 |
| A18 | B | Safe Transaction Service API (Safe-operated on-chain indexer): safe metadata + executed multisig transactions | https://safe-transaction-mainnet.safe.global/api/v1/safes/0x467947EE34aF926cF1DCac093870f613C96B1E0c/ | 2026-10-06 |
| A19 | B | bgd-labs/aave-address-book — `AaveV3Ethereum.sol` (POOL, POOL_IMPL, ACL_MANAGER, ACL_ADMIN) | https://raw.githubusercontent.com/bgd-labs/aave-address-book/main/src/AaveV3Ethereum.sol | 2026-10-06 |
| A19b | B | bgd-labs/aave-address-book — `GovernanceV3Ethereum.sol` (EXECUTOR_LVL_1/2, GRANULAR_GUARDIAN, GOVERNANCE_GUARDIAN) | https://raw.githubusercontent.com/bgd-labs/aave-address-book/main/src/GovernanceV3Ethereum.sol | 2026-10-06 |
| A20 | B | ethereum.org — Smart contract security (dev guidance; DAO 3.6M ETH, Parity $30M, Parity frozen >$300M, cumulative "easily over $1 billion") | https://ethereum.org/developers/docs/smart-contracts/security/ | 2026-10-06 |
| A21 | B | securing/SCSVS v1.2 — Smart Contract Security Verification Standard, 14-part checklist (repo status: archived) | https://github.com/securing/SCSVS | 2026-10-06 |
| A22 | B | Immunefi bug-bounty programme table (max bounty, total paid, median resolution) | https://immunefi.com/bug-bounty/ | 2026-10-06 |
| A23 | B | Nomad — "Nomad Bridge Hack: Root Cause Analysis" (protocol's own post-mortem: `Replica` authentication failure, zero-value defaults) | https://medium.com/nomad-xyz-blog/nomad-bridge-hack-root-cause-analysis-875ad2e5aacd | 2026-10-06 |
| A24 | D | EEA EthTrust Security Levels Specification v3 (Editor's Draft page, EEA Specification March 2025) | https://entethalliance.org/specs/ethtrust-sl/v3/ | 2026-10-06 |
| A25 | A | US DOJ — "Canadian Man Charged in $65M Cryptocurrency Hacking Schemes" (KyberSwap Elastic + Indexed Finance; flash-borrow-induced miscalculation; sham-settlement extortion of protocol/DAO control) | https://www.justice.gov/opa/pr/canadian-man-charged-65m-cryptocurrency-hacking-schemes | 2026-10-06 |
| A26 | A | US CFTC — "CFTC Charges Avraham Eisenberg with Manipulative and Deceptive Scheme to Misappropriate Over $110 million from Mango Markets" (Release 8647-23, 9 Jan 2023) | https://www.cftc.gov/PressRoom/PressReleases/8647-23 | 2026-10-06 |
| A27 | B | Cream Finance — "Post Mortem: Flash Loan Exploit Oct 27" (oracle + economic exploit via yUSD price manipulation) | https://medium.com/cream-finance/post-mortem-exploit-oct-27-507b12bb6f8e | 2026-10-06 |
| A28 | B | Radiant Capital — "Post-Mortem Report" (Jan 2024 Arbitrum flash-loan `liquidityIndex` manipulation on empty-reserve market; 1190 ETH repaid, ~720 ETH outstanding) | https://medium.com/@RadiantCapital/post-mortem-report-radiant-capital-aea46cb985ae | 2026-10-06 |
| A29 | B | KyberSwap — "KyberSwap Elastic Exploit Post Mortem and User Support with 100% Coverage via the Treasury Grant Program" | https://blog.kyberswap.com/post-mortem-kyberswap-elastic-exploit/ | 2026-10-06 |
| A30 | B | Harmony — "Harmony's Horizon Bridge Hack" (private keys decrypted; 11 txs; ~$100M; post-incident 4-of-5 multisig) | https://medium.com/harmony-one/harmonys-horizon-bridge-hack-1e8d283b6d66 | 2026-10-06 |
| A31 | B | Badger — "Recovery Phase" (core smart contracts not impacted; Mandiant review; Halborn infra audit; Web2/phishing lesson) | https://oldlandingpage.badger.com/recovery-phase | 2026-10-06 |
| A32 | B | Compound governance forum — "Security and Agility of Compound Smart Contracts via Continuous Formal Verification" (Certora Prover programme since 2018) | https://www.comp.xyz/t/security-and-agility-of-compound-smart-contracts-via-continuous-formal-verification/4007 | 2026-10-06 |
| A33 | B | Circle pressroom — "$3.3 Billion of USDC Reserve Risk Removed, Dollar De-peg Closes" (13 Mar 2023; SVB ~8% of reserves; 77%/$32.4B T-bills at BNY Mellon; 23%/$9.7B cash) | https://www.circle.com/pressroom/3-3-billion-of-usdc-reserve-risk-removed-dollar-de-peg-closes | 2026-10-06 |
| A34 | C | CertiK — "Hack3d: The Web3 Security Report 2024" ($2,362,748,975.83 across 760 incidents; phishing $1,050,129,498/296; private-key compromise $855,385,570/65; avg $3.11M, median $150,925) | https://www.certik.com/blog/hack3d-the-web3-security-report-2024 | 2026-10-06 |
| A35 | C | CertiK — "Hack3d: The Web3 Security Report 2023" ($1.84B across 751 incidents; private-key compromise 6.3% of incidents ≈ half of losses; cross-chain $799M/35 incidents) | https://www.certik.com/blog/hack3d-the-web3-security-report-2023 | 2026-10-06 |
| A36 | C | Chainalysis — "The 2025 Crypto Crime Report" ($40.9B to identified illicit addresses in 2024; lower-bound estimate) | https://www.chainalysis.com/wp-content/uploads/2025/02/the-2025-crypto-crime-report-release.pdf | 2026-10-06 |
| A37 | D | Qin, Zhou, Livshits, Gervais — "Attacking the DeFi Ecosystem with Flash Loans for Fun and Profit" (arXiv:2003.03810) | https://arxiv.org/abs/2003.03810 | 2026-10-06 |
| A38 | D | Krupp & Rossow — "teEther: Gnawing at Ethereum to Automatically Exploit Smart Contracts", USENIX Security '18 (815 working exploits auto-generated from bytecode across 38,757 unique contracts) | https://www.usenix.org/conference/usenixsecurity18/presentation/krupp | 2026-10-06 |
| A39 | D | Glassnode — "What Really Happened To MakerDAO?" (Black Thursday: ~$4.5M unbacked DAI; >$8M ETH via zero-bid auctions) | https://research.glassnode.com/what-really-happened-to-makerdao/ | 2026-10-06 |
| A40 | D | Elliptic — "$76 million stolen from Beanstalk Farms" (flash loan ≈$1B → ~67% stalk position → malicious BIPs; total protocol loss ≈$182M) | https://www.elliptic.co/insights/76-million-stolen-from-beanstalk-farms-defi-stablecoin-protocol/ | 2026-10-06 |
| A41 | D | LlamaRisk — "Curve Pool Reentrancy Exploit Postmortem" (30 Jul 2023; per-pool extracted amounts ≈$61.7M total) | https://llamarisk.com/research/curve-pool-reentrancy-exploit-postmortem | 2026-10-06 |
| A42 | D | SlowMist — "The Analysis and Q&A Of Poly Network Being Hacked" (Aug 2021 root causes incl. selector/hash collision) | https://slowmist.medium.com/the-analysis-and-q-a-of-poly-network-being-hacked-8112a35beb39 | 2026-10-06 |
| A43 | D | CNBC — "Ronin hack: North Korea linked to $615 million crypto heist, U.S. says" | https://www.cnbc.com/2022/04/15/ronin-hack-north-korea-linked-to-615-million-crypto-heist-us-says.html | 2026-10-06 |
| A44 | D | Ren et al. — "Empirical evaluation of smart contract testing: what is the best choice?", ISSTA 2021 (46,186 contracts, 9 tools; evaluation-setting sensitivity) | https://dl.acm.org/doi/10.1145/3460319.3464837 | 2026-10-06 |
| A45 | D | Durieux & Ferreira — "Empirical review of automated analysis tools on 47,587 Ethereum smart contracts", ICSE 2020 | https://dl.acm.org/doi/10.1145/3377811.3382384 | 2026-10-06 |
| A46 | D | Zooko Wilcox — "SoK: Decentralized Finance (DeFi) — Fundamentals, Taxonomy and Risks" (arXiv:2404.11281) | https://arxiv.org/html/2404.11281v1 | 2026-10-06 |
| A47 | D | The Block — "Tether freezes $225 million worth of stolen USDT after DOJ investigation" (20 Nov 2023) | https://www.theblock.co/news/regulation/2023-11-20-tether-freezes-225-million-worth-of-stolen-usdt-after-doj-investigation-263802 | 2026-10-06 |
| A48 | B | CertiK — "Euler Finance Incident Analysis" (13 Mar 2023, ~$197M; `donateToReserves()` in five pools; 30M DAI Aave flash loan; asset list) | https://www.certik.com/blog/euler-finance-incident-analysis | 2026-10-06 |
| A49 | B | CertiK Skynet — Euler project page ("Not Audited By CertiK"; 12 available third-party audits) | https://skynet.certik.com/projects/euler-finance | 2026-10-06 |
| A50 | D | Circle — USDC Terms (issuer's binding disclosure of control/compliance powers; last updated 12 Dec 2025) | https://www.circle.com/legal/usdc-terms | 2026-10-06 |

---

## Scoring signals extracted

| signal_id | what it measures | how to verify mechanically | evidence tier | failure mode | confidence |
|---|---|---|---|---|---|
| SEC-01 | Whether a full audit artifact set exists (scope + commit + compiler + date + named firm) | Locate published PDF/txt; regex-extract scope addresses, commit, compiler version, date, auditor entity; require all five fields | B | Marketing page lists "audited" with no artifact | high |
| SEC-02 | Audit predates the deployed implementation | Compare audit date to first-deploy block of the implementation, or to earliest `Upgraded`/`BeaconUpgraded` log for the proxy | B | Audit is for a superseded implementation (Euler class) | high |
| SEC-03 | Audit coverage ratio over authority-bearing contracts | Audited address set ÷ set of contracts that can move/pause/upgrade user funds | B | Token audited, lending core and oracle not | high |
| SEC-04 | Audit scope explicitly includes economic/invariant analysis | Text-match the scope section for oracle/invariant/economic-attack language | B | Pure code audit; economic invariants out of scope (Euler, Cream) | med |
| SEC-05 | Continuous formal verification in the build pipeline | Look for a CI config invoking a prover/fuzzer; or a published spec of verified invariants | B | "Formally verified" as a one-off marketing claim | med |
| SEC-06 | Funded bug bounty with real payout history | Immunefi listing: `max_bounty / TVL` ratio and `total_paid > 0` | B/C | Bounty page with zero payouts or negligible cap | high |
| SEC-07 | Published disclosure policy + security contact | Presence of a security.txt / security@ / disclosure policy with response SLA | B | Absence implies no adversarial-research channel | med |
| SEC-08 | Published known-issue / residual-risk list | Existence of a doc enumerating self-identified risks and non-goals (Curve Emergency DAO is the reference) | B | Audit without threat model = no self-identified residual risk | med |
| SEC-09 | Standards currency of the project's security taxonomy | Whether cited taxonomy is SWC (archived since 2020) or EthTrust/SCSVS (maintained) | B/D | Scoring off an archived standard set | high |
| SEC-10 | Contract source verification status for every authority-bearing address | Etherscan/Sourcify verified flag; `eth_getCode != 0x` but unverified ⇒ hard negative | B | Unverified contract ⇒ all access-control claims unverifiable | high |
| SEC-11 | Proxy pattern classification (transparent / UUPS / beacon / none) | Read EIP-1967 impl, admin, beacon slots; call `proxiableUUID()` (`0x52d1902d`) on the impl | B | Pattern drives who-can-upgrade; misclassification hides admin risk | high |
| SEC-12 | Safe-proxy detection done correctly (no EIP-1967 false negative) | Read storage slot 0 AND `masterCopy()` (`0xa619486e`); do not rely on EIP-1967 alone for Safe v1.3.0 | B | Reading EIP-1967 on a v1.3.0 Safe returns 0 ⇒ falsely "not a proxy" | high |
| SEC-13 | Safe singleton is registry-canonical for that chain | Resolve singleton against safe-deployments registry for the specific chainId/variant | B | Unregistered singleton (Curve Arbitrum `0xfb1bffc9…`) ⇒ unverifiable Safe logic | high |
| SEC-14 | Multisig threshold strength | `getThreshold()` and `getOwners().length`; compute `(n−t)+1` keys to lose control | B | 1-of-N or 2-of-9 claimed as "multisig" (Harmony pre-incident class) | high |
| SEC-15 | Safe has no bypass modules | `getModulesPaginated` length == 0 | B | An enabled module can execute without the threshold | high |
| SEC-16 | Safe has no veto guard | `getGuard() == address(0)` | B | A guard contract can block all Safe transactions | high |
| SEC-17 | Safe signer set is published and matches on-chain state | Compare `getOwners()` to a project-published member list | B | Unnamed signers; signer-state drift undetectable | high |
| SEC-18 | Safe actually exercises its authority (control ≠ existence) | Safe Tx Service executed-tx count > 0; sample confirmations == threshold; identify target contracts | B | A never-used Safe controls nothing | high |
| SEC-19 | Treasury liveness / value custody | Native + stablecoin balances and 30/90-day outbound volume | B | Dormant or empty treasury claimed as custody | high |
| SEC-20 | Capability-authority mapping (which contracts the Safe actually controls) | Reverse check: for each material contract, read its admin/owner/role-admin slot and confirm it equals the claimed Safe | B | Safe exists but controls nothing material | high |
| SEC-21 | Timelock delay on the real authority path | `getMinDelay()` in seconds; reject revert as "unknown" not "absent" | B | No timelock ⇒ instant upgrade/pause capability | high |
| SEC-22 | Timelock self-administration (no EOA admin escape hatch) | `hasRole(DEFAULT_ADMIN_ROLE, timelock)` == true; EOA admin absent | B | Retained admin = one-key bypass of the delay | high |
| SEC-23 | Timelock has genuinely queued, non-trivial operations | `isOperationPending`/`isOperationReady` over `CallScheduled` logs; count distinct executors | B | A timelock that never executes is theatre | med |
| SEC-24 | Censorship capability tuple is emitted and disclosed | Enumerate `owner()`, `paused()`, blacklist fn, minter, proxy admin; record whether issuer discloses | B/A | Hidden or undisclosed freeze capability | high |
| SEC-25 | Oracle is not a single thin AMM pool | Trace each feed to its source; flag single-venue AMM-sourced prices | B | Cream / Mango / KyberSwap / Black Thursday class | high |
| SEC-26 | Oracle feed substitution is governance-gated | `setAssetSources`/`setFallbackOracle` authority resolves to governance/timelock (Aave pattern) | B | Runtime-settable or permissionless feed swap | high |
| SEC-27 | Flash-loan-assisted governance-capture exposure | Oracle price source ÷ borrowable float for the governance token; quorum basis (total vs circulating supply); proposal delay before execution | B/D | Beanstalk class: ~$1B flash loan → 67% stake → instant malicious proposal | med |
| SEC-28 | Bridge trust model classification | Count required signers/attestors; classify trusted-multisig/MPC vs verifiable-light-client; read signer thresholds | B | Ronin / Harmony / Poly Network class | high |
| SEC-29 | Compiler/dependency pinning with advisory monitoring | Detect pinned compiler version; check version against known advisories (e.g. Vyper CVE-2023-39363) | B | Curve 30 Jul 2023 class: source-level guard silently ineffective | high |
| SEC-30 | Loss-vector attribution (contract vs key vs frontend vs phishing) | Classify each historical incident; compute share of realised loss outside code defects | C | Optimising for code quality while 2024 losses were ~80.6% phishing+key compromise | high |

### Sourcing gaps (do not score on these without further work)
- **UwU Lend (~$19.3M, Jun 2023)** — loss figure UNSOURCED in this pass.
- **Nomad (~$190M, Aug 2022)** — mechanism SOURCED [A23]; loss figure UNSOURCED.
- **Badger DAO (~$120M, Dec 2021)** — lesson SOURCED [A31]; loss figure UNSOURCED.
- **The DAO (2016)** — 3.6M ETH and cumulative "easily over $1 billion" SOURCED [A20]; contemporaneous USD figure UNSOURCED.
- **US DOJ Tether/OFAC settlement (Oct 2021, ~$162M MANA + 594M USDC)** — the canonical Tier A citation for issuer blacklist power; I could **not** retrieve the justice.gov press release in this pass. Tier D confirmation of Tether's freeze capability is captured via [A47]. Do not cite the DOJ Tether release until fetched.
- **Aave pause/freeze delay** — not answerable from on-chain state because `ACL_ADMIN` is a non-OZ, non-Safe executor [verified]; requires [A15b]/[A19b] process documentation. Treat as "unverifiable" rather than "no guardian."
- **Do NOT assert** that Euler was audited by CertiK — CertiK's own project page states "Not Audited By CertiK" and lists 12 third-party audits [A49].
