# Evidence Ledger — Crypto Legitimacy & Use-Case Viability research

Reconstructed from the per-agent `## Sources` tables after agent D overwrote the
shared ledger mid-run. Sources are deduplicated by URL; where two agents cited the
same URL the first ref is kept.

Tier key: **A** primary regulatory/government · **B** primary technical/registry ·
**C** primary measurement · **D** established media/research/law-firm alerts.
Tier **E** (blogs, SEO, aggregators, price predictions) is **excluded by policy**.

| Ref | Tier | Title | URL | Accessed |
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
| A16 | B | Ethereum mainnet JSON-RPC `eth_call`/`eth_getStorageAt`/`eth_getBalance`/`eth_getCode` reads (USDC `0xA0b8…6eB48`, USDT `0xdAC1…1ec7`) via publicnode  | https://ethereum-rpc.publicnode.com | 2026-10-06 |
| A17 | B | Arbitrum One JSON-RPC reads of Curve cross-chain Safe `0x6d447e…ffFeD` (slot 0, `masterCopy()`) via two independent endpoints | https://arbitrum-one-rpc.publicnode.com | 2026-10-06 |
| A18 | B | Safe Transaction Service API (Safe-operated on-chain indexer): safe metadata + executed multisig transactions | https://safe-transaction-mainnet.safe.global/api/v1/safes/0x467947EE34aF926cF1DCac093870f613C96B1E0c/ | 2026-10-06 |
| A19 | B | bgd-labs/aave-address-book — `AaveV3Ethereum.sol` (POOL, POOL_IMPL, ACL_MANAGER, ACL_ADMIN) | https://raw.githubusercontent.com/bgd-labs/aave-address-book/main/src/AaveV3Ethereum.sol | 2026-10-06 |
| A19b | B | bgd-labs/aave-address-book — `GovernanceV3Ethereum.sol` (EXECUTOR_LVL_1/2, GRANULAR_GUARDIAN, GOVERNANCE_GUARDIAN) | https://raw.githubusercontent.com/bgd-labs/aave-address-book/main/src/GovernanceV3Ethereum.sol | 2026-10-06 |
| A20 | B | ethereum.org — Smart contract security (dev guidance; DAO 3.6M ETH, Parity $30M, Parity frozen >$300M, cumulative "easily over $1 billion") | https://ethereum.org/developers/docs/smart-contracts/security/ | 2026-10-06 |
| A21 | B | securing/SCSVS v1.2 — Smart Contract Security Verification Standard, 14-part checklist (repo status: archived) | https://github.com/securing/SCSVS | 2026-10-06 |
| A22 | B | Immunefi bug-bounty programme table (max bounty, total paid, median resolution) | https://immunefi.com/bug-bounty/ | 2026-10-06 |
| A23 | B | Nomad — "Nomad Bridge Hack: Root Cause Analysis" (protocol's own post-mortem: `Replica` authentication failure, zero-value defaults) | https://medium.com/nomad-xyz-blog/nomad-bridge-hack-root-cause-analysis-875ad2e5aacd | 2026-10-06 |
| A24 | D | EEA EthTrust Security Levels Specification v3 (Editor's Draft page, EEA Specification March 2025) | https://entethalliance.org/specs/ethtrust-sl/v3/ | 2026-10-06 |
| A25 | A | US DOJ — "Canadian Man Charged in $65M Cryptocurrency Hacking Schemes" (KyberSwap Elastic + Indexed Finance; flash-borrow-induced miscalculation; sham | https://www.justice.gov/opa/pr/canadian-man-charged-65m-cryptocurrency-hacking-schemes | 2026-10-06 |
| A26 | A | US CFTC — "CFTC Charges Avraham Eisenberg with Manipulative and Deceptive Scheme to Misappropriate Over $110 million from Mango Markets" (Release 8647 | https://www.cftc.gov/PressRoom/PressReleases/8647-23 | 2026-10-06 |
| A27 | B | Cream Finance — "Post Mortem: Flash Loan Exploit Oct 27" (oracle + economic exploit via yUSD price manipulation) | https://medium.com/cream-finance/post-mortem-exploit-oct-27-507b12bb6f8e | 2026-10-06 |
| A28 | B | Radiant Capital — "Post-Mortem Report" (Jan 2024 Arbitrum flash-loan `liquidityIndex` manipulation on empty-reserve market; 1190 ETH repaid, ~720 ETH  | https://medium.com/@RadiantCapital/post-mortem-report-radiant-capital-aea46cb985ae | 2026-10-06 |
| A29 | B | KyberSwap — "KyberSwap Elastic Exploit Post Mortem and User Support with 100% Coverage via the Treasury Grant Program" | https://blog.kyberswap.com/post-mortem-kyberswap-elastic-exploit/ | 2026-10-06 |
| A30 | B | Harmony — "Harmony's Horizon Bridge Hack" (private keys decrypted; 11 txs; ~$100M; post-incident 4-of-5 multisig) | https://medium.com/harmony-one/harmonys-horizon-bridge-hack-1e8d283b6d66 | 2026-10-06 |
| A31 | B | Badger — "Recovery Phase" (core smart contracts not impacted; Mandiant review; Halborn infra audit; Web2/phishing lesson) | https://oldlandingpage.badger.com/recovery-phase | 2026-10-06 |
| A32 | B | Compound governance forum — "Security and Agility of Compound Smart Contracts via Continuous Formal Verification" (Certora Prover programme since 2018 | https://www.comp.xyz/t/security-and-agility-of-compound-smart-contracts-via-continuous-formal-verification/4007 | 2026-10-06 |
| A33 | B | Circle pressroom — "$3.3 Billion of USDC Reserve Risk Removed, Dollar De-peg Closes" (13 Mar 2023; SVB ~8% of reserves; 77%/$32.4B T-bills at BNY Mell | https://www.circle.com/pressroom/3-3-billion-of-usdc-reserve-risk-removed-dollar-de-peg-closes | 2026-10-06 |
| A34 | C | CertiK — "Hack3d: The Web3 Security Report 2024" ($2,362,748,975.83 across 760 incidents; phishing $1,050,129,498/296; private-key compromise $855,385 | https://www.certik.com/blog/hack3d-the-web3-security-report-2024 | 2026-10-06 |
| A35 | C | CertiK — "Hack3d: The Web3 Security Report 2023" ($1.84B across 751 incidents; private-key compromise 6.3% of incidents ≈ half of losses; cross-chain  | https://www.certik.com/blog/hack3d-the-web3-security-report-2023 | 2026-10-06 |
| A36 | C | Chainalysis — "The 2025 Crypto Crime Report" ($40.9B to identified illicit addresses in 2024; lower-bound estimate) | https://www.chainalysis.com/wp-content/uploads/2025/02/the-2025-crypto-crime-report-release.pdf | 2026-10-06 |
| A37 | D | Qin, Zhou, Livshits, Gervais — "Attacking the DeFi Ecosystem with Flash Loans for Fun and Profit" (arXiv:2003.03810) | https://arxiv.org/abs/2003.03810 | 2026-10-06 |
| A38 | D | Krupp & Rossow — "teEther: Gnawing at Ethereum to Automatically Exploit Smart Contracts", USENIX Security '18 (815 working exploits auto-generated fro | https://www.usenix.org/conference/usenixsecurity18/presentation/krupp | 2026-10-06 |
| A39 | D | Glassnode — "What Really Happened To MakerDAO?" (Black Thursday: ~$4.5M unbacked DAI; >$8M ETH via zero-bid auctions) | https://research.glassnode.com/what-really-happened-to-makerdao/ | 2026-10-06 |
| A40 | D | Elliptic — "$76 million stolen from Beanstalk Farms" (flash loan ≈$1B → ~67% stalk position → malicious BIPs; total protocol loss ≈$182M) | https://www.elliptic.co/insights/76-million-stolen-from-beanstalk-farms-defi-stablecoin-protocol/ | 2026-10-06 |
| A41 | D | LlamaRisk — "Curve Pool Reentrancy Exploit Postmortem" (30 Jul 2023; per-pool extracted amounts ≈$61.7M total) | https://llamarisk.com/research/curve-pool-reentrancy-exploit-postmortem | 2026-10-06 |
| A42 | D | SlowMist — "The Analysis and Q&A Of Poly Network Being Hacked" (Aug 2021 root causes incl. selector/hash collision) | https://slowmist.medium.com/the-analysis-and-q-a-of-poly-network-being-hacked-8112a35beb39 | 2026-10-06 |
| A43 | D | CNBC — "Ronin hack: North Korea linked to $615 million crypto heist, U.S. says" | https://www.cnbc.com/2022/04/15/ronin-hack-north-korea-linked-to-615-million-crypto-heist-us-says.html | 2026-10-06 |
| A44 | D | Ren et al. — "Empirical evaluation of smart contract testing: what is the best choice?", ISSTA 2021 (46,186 contracts, 9 tools; evaluation-setting sen | https://dl.acm.org/doi/10.1145/3460319.3464837 | 2026-10-06 |
| A45 | D | Durieux & Ferreira — "Empirical review of automated analysis tools on 47,587 Ethereum smart contracts", ICSE 2020 | https://dl.acm.org/doi/10.1145/3377811.3382384 | 2026-10-06 |
| A46 | D | Zooko Wilcox — "SoK: Decentralized Finance (DeFi) — Fundamentals, Taxonomy and Risks" (arXiv:2404.11281) | https://arxiv.org/html/2404.11281v1 | 2026-10-06 |
| A47 | D | The Block — "Tether freezes $225 million worth of stolen USDT after DOJ investigation" (20 Nov 2023) | https://www.theblock.co/news/regulation/2023-11-20-tether-freezes-225-million-worth-of-stolen-usdt-after-doj-investigation-263802 | 2026-10-06 |
| A48 | B | CertiK — "Euler Finance Incident Analysis" (13 Mar 2023, ~$197M; `donateToReserves()` in five pools; 30M DAI Aave flash loan; asset list) | https://www.certik.com/blog/euler-finance-incident-analysis | 2026-10-06 |
| A49 | B | CertiK Skynet — Euler project page ("Not Audited By CertiK"; 12 available third-party audits) | https://skynet.certik.com/projects/euler-finance | 2026-10-06 |
| A50 | D | Circle — USDC Terms (issuer's binding disclosure of control/compliance powers; last updated 12 Dec 2025) | https://www.circle.com/legal/usdc-terms | 2026-10-06 |
| B1 | B | `GovernorBravoDelegate.sol` (Compound Governor Bravo source) | https://github.com/compound-finance/compound-governance/blob/main/contracts/GovernorBravoDelegate.sol | 2026-10-06 |
| B2 | B | https://stage.compound.finance/docs/governance | https://stage.compound.finance/docs/governance | 2026-10-06 |
| B3 | B | OpenZeppelin Docs — How to manage roles of a TimelockController | https://docs.openzeppelin.com/defender/guide/timelock-roles | 2026-10-06 |
| B4 | B | OP Stack — Privileged Roles in OP Stack Chains | https://docs.optimism.io/op-stack/protocol/privileged-roles | 2026-10-06 |
| B5 | B | Optimism Security Council Charter v0.1 (OPerating-manual) | https://github.com/ethereum-optimism/OPerating-manual/blob/main/Security%20Council%20Charter%20v0.1.md | 2026-10-06 |
| B6 | B | Arbitrum DAO — Security Council: A conceptual overview | https://docs.arbitrum.foundation/concepts/security-council | 2026-10-06 |
| B7 | B | ArbitrumDAO — The Amended Constitution | https://docs.arbitrum.foundation/dao-constitution | 2026-10-06 |
| B9 | D | SEAL — Secure Multisig Best Practices | https://frameworks.securityalliance.org/wallet-security/secure-multisig-best-practices/ | 2026-10-06 |
| B10 | B | Uniswap Governance — Technical Reference | https://developers.uniswap.org/docs/ecosystem/governance/technical-reference | 2026-10-06 |
| B11 | D | Kani, Fritsch, Vonlanthen, Wattenhofer (ETH Zurich) — Analyzing Voting Power in Decentralized Governance: Who controls DAOs? arXiv:2204.01176 | https://ar5iv.labs.arxiv.org/html/2204.01176 | 2026-10-06 |
| B12 | D | Feichtinger, Fritsch, Vonlanthen, Wattenhofer (ETH Zurich) — The Hidden Shortcomings of (D)AOs: An Empirical Study of On-Chain Governance. arXiv:2302. | https://ar5iv.labs.arxiv.org/html/2302.12125 | 2026-10-06 |
| B13 | D | Hall & Miyazaki (a16z crypto) — What happens when anyone can be your representative? Testing liquid democracy in web3 | https://a16zcrypto.com/posts/article/testing-liquid-democracy/ | 2026-10-06 |
| B14 | D | Frontiers in Blockchain (2026) — Auditing governance concentration beyond token allocation: a live-governance study of 52 token protocols | https://www.frontiersin.org/journals/blockchain/articles/10.3389/fbloc.2026.1853465/full | 2026-10-06 |
| B15 | C | DefiLlama — DeFi Governance & DAO Proposals dashboard | https://defillama.com/governance | 2026-10-06 |
| B16 | C | DefiLlama — DeFi Data Definitions & Metrics Glossary | https://defillama.com/data-definitions | 2026-10-06 |
| B17 | D | Bongaerts, Lambert, Liebau, Roosenboom (RSM/Erasmus) — Vote Delegation in DeFi Governance. arXiv:2503.11940 | https://arxiv.org/pdf/2503.11940 | 2026-10-06 |
| B18 | A | IOSCO — Final Report with Policy Recommendations for Decentralized Finance (DeFi), FR14/23 | https://www.iosco.org/library/pubdocs/pdf/IOSCOPD754.pdf | 2026-10-06 |
| B19 | C | Keyrock — Realising Crypto's $5.6 Billion Treasury Opportunity | https://keyrock.com/assets/uploads/2026/03/Realising-Cryptos-5.6-Billion-Treasury-Opportunity.pdf | 2026-10-06 |
| B20 | D | Schellinger, Fiedler, Steinmetz (Blockchain Research Lab) — How Are You DAOing? The State of DAO Treasuries (SSRN 4604968) | https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4604968 | 2026-10-06 |
| B21 | B | Uniswap Foundation: Summary FY'2024 Financials (Uniswap Governance forum) | https://gov.uniswap.org/t/uniswap-foundation-summary-fy-2024-financials/25486 | 2026-10-06 |
| B22 | B | Ethereum Foundation — 2024 Annual Report | https://ethereum.foundation/report-2024.pdf | 2026-10-06 |
| B23 | C | DefiLlama — Net Treasury Rankings (Excl. Native Token) | https://defillama.com/net-project-treasury | 2026-10-06 |
| B24 | D | Moser, S. — Do whitepapers matter? Investigating the long-term effects of cryptocurrency whitepapers (2025) | https://ideas.repec.org/a/eee/finlet/v85y2025ipbs1544612325010050.html | 2026-10-06 |
| B25 | A | SEC — Quantstamp, Inc. administrative proceeding (Release 33-11215) | https://www.sec.gov/enforcement-litigation/administrative-proceedings/33-11215-s | 2026-10-06 |
| B26 | A | SEC Press Release 2023-229 — Charges SafeMoon and its Executive Team for Fraud and Unregistered Offering | https://www.sec.gov/newsroom/press-releases/2023-229 | 2026-10-06 |
| B27 | A | SEC Press Release 2023-32 — Charges Terraform and CEO Do Kwon | https://www.sec.gov/newsroom/press-releases/2023-32 | 2026-10-06 |
| B28 | A | SEC Press Release 2024-91 — Charges Nader Al-Naji with Fraud and Unregistered Offering | https://www.sec.gov/newsroom/press-releases/2024-91 | 2026-10-06 |
| B29 | A | DOJ — Sealed Indictment, *U.S. v. Mashinsky and Cohen-Pavon* | https://www.justice.gov/d9/2023-07/u.s._v._mashinsky_and_cohen-pavon_indictment.pdf | 2026-10-06 |
| B30 | C | Nansen — What is Smart Money in Crypto? A Detailed Look into Our Methodology | https://nansen.ai/post/what-is-smart-money-in-crypto-a-detailed-look-into-our-methodology | 2026-10-06 |
| B31 | D | Calzada, I. — The illusion of the Web3 decentralization (Data & Policy, Cambridge UP, 2026) | https://www.cambridge.org/core/services/aop-cambridge-core/content/view/AAC73AD95284C123F83BE0A67CAD2AE9/S2632324926100558a.pdf/the-illusion-of-the-web3-decentralization.pdf | 2026-10-06 |
| B33 | B | Radiant Capital — Radiant Capital Post-Mortem (18 Oct 2024) | https://medium.com/@RadiantCapital/radiant-post-mortem-fecd6cd38081 | 2026-10-06 |
| B34 | B | Harmony Community — Summary of the Horizon Bridge Incident | https://talk.harmony.one/t/summary-of-the-horizon-bridge-incident/20990 | 2026-10-06 |
| B34b | A | FBI — Lazarus Group Responsible for Harmony's Horizon Bridge Theft (23 Jan 2023) | https://www.fbi.gov/news/press-releases/fbi-confirms-lazarus-group-cyber-actors-responsible-for-harmonys-horizon-bridge-currency-theft | 2026-10-06 |
| B35 | B | Wormhole — Wormhole Incident Report 02/02/22 | https://wormholecrypto.medium.com/wormhole-incident-report-02-02-22-ad9b8f21eec6 | 2026-10-06 |
| B36 | B | Ronin — Back to Building: Ronin Security Breach Postmortem (27 Apr 2022), read via mirror | https://lazarus.day/reports/back-to-building-ronin-security-breach-postmortem-bbMOK/ | 2026-10-06 |
| B37 | D | SEAL — Multisig Implementation Checklist | https://frameworks.securityalliance.org/multisig-for-protocols/implementation-checklist/ | 2026-10-06 |
| B38 | B | OP Stack — Pausing the bridge (Guardian role scope) | https://docs.optimism.io/op-stack/security/pause | 2026-10-06 |
| B39 | B | Uniswap Labs — Uniswap v2 Technical Whitepaper (admin-key admission) | https://blog.uniswap.org/whitepaper.pdf | 2026-10-06 |
| B40 | B | Uniswap Labs — Uniswap v2 Overview | https://blog.uniswap.org/uniswap-v2 | 2026-10-06 |
| B41 | D | CoinDesk — SushiSwap Migration Ushers in Era of 'Protocol Politicians' (9 Sep 2020) | https://www.coindesk.com/tech/2020/09/09/sushiswap-migration-ushers-in-era-of-protocol-politicians | 2026-10-06 |
| B42 | B | Arbitrum Foundation forum — DVP-Quorum for ArbitrumDAO (quorum design discussion) | https://forum.arbitrum.foundation/t/dvp-quorum-for-arbitrumdao/29996/1 | 2026-10-06 |
| C1 | A | CFTC Charges Two Individuals with Multi-Million Dollar Digital Asset Pump-and-Dump Scheme (Release 8366-21) | https://www.cftc.gov/PressRoom/PressReleases/8366-21 | 2026-10-06 |
| C2 | A | CFTC Customer Advisory: Beware Virtual Currency Pump-and-Dump Schemes | https://www.cftc.gov/LearnAndProtect/AdvisoriesAndArticles/beware_virtual_currency_pump_dump.html | 2026-10-06 |
| C3 | A | SEC Press Release 2024-166: SEC Charges Three So-Called Market Makers and Nine Individuals in Crackdown on Manipulation of Crypto Assets | https://www.sec.gov/newsroom/press-releases/2024-166 | 2026-10-06 |
| C4 | A | SEC Litigation Release LR-26156: Gotbit Consulting LLC and Fedor Kedrov (Robo Inu) | https://www.sec.gov/enforcement-litigation/litigation-releases/lr-26156 | 2026-10-06 |
| C5 | A | SEC v. Armand et al. Complaint (Saitama Inu / SaitaRealty), D. Mass. | https://www.sec.gov/files/litigation/complaints/2024/comp-pr2024-166-saitama.pdf | 2026-10-06 |
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
| E1-B1 | B | The Depegging of UST — transaction-level on-chain trace of the Curve pool and Anchor outflows (Jump Crypto research) | https://jumpcrypto.com/resources/the-depegging-of-ust | 2026-10-06 |
| E1-C1 | C | Uniswap — TVL, Fees, Revenue & Volume (DeFiLlama) | https://defillama.com/protocol/uniswap | 2026-10-06 |
| E1-D1 | D | Anatomy of a Run: The Terra Luna Crash — Schoar, Makarov & Liu, Harvard Corporate Governance Forum | https://corpgov.law.harvard.edu/2023/05/22/anatomy-of-a-run-the-terra-luna-crash/ | 2026-10-06 |
| E1-A2 | A | USA v. Kohli et al., Superseding Indictment, No. 24cr10189 (D. Mass.) — DOJ PDF | https://www.justice.gov/d9/2024-10/kohli_et_al._superseding_indictment_1.pdf | 2026-10-06 |
| E1-B2 | B | Analysis – Terraform Labs' and Luna Foundation Guard's Defense of the UST Price Peg (expert report by J.S. Held / Jenner & Block, filed in litigation) | https://lfg.org/audit/LFG-Audit-2022-11-14.pdf | 2026-10-06 |
| E1-C2 | C | Uniswap V3 — TVL, Fees, Revenue & Volume (DeFiLlama) | https://defillama.com/protocol/uniswap-v3 | 2026-10-06 |
| E1-D2 | D | US charges 3 companies, 15 people with cryptocurrency fraud — Nate Raymond, Reuters | https://www.reuters.com/legal/us-charges-18-people-companies-cryptocurrency-fraud-2024-10-09/ | 2026-10-06 |
| E1-A3 | B | Independent Accountants' Report — USDC Reserve Report, July 8 & July 31 2026 (Circle) | https://6778953.fs1.hubspotusercontent-na1.net/hubfs/6778953/USDCAttestationReports/2026/2026%20USDC_Examination%20Report%20July%2026%20(1 | 2026-10-06 |
| E1-B3 | B | Public Version – Investigation IOC-Beanstalk (Intelligence on Chain, commissioned by the Beanstalk DAO) | https://img1.wsimg.com/blobby/go/84879eef-009a-49e3-bec7-d2ea2f962a66/downloads/Public%20Version%20-%20Investigation%20IOC-Beanstalk.pdf | 2026-10-06 |
| E1-C3 | C | Uniswap V4 — TVL, Fees, Revenue & Volume (DeFiLlama; states the distinct-address wash-trading exclusion rule) | https://defillama.com/protocol/uniswap-v4 | 2026-10-06 |
| E1-D3 | D | How did a hacker steal over $600 million from a crypto gaming blockchain? — Ars Technica | https://arstechnica.com/gaming/2022/03/how-did-a-hacker-steal-over-600-million-from-a-crypto-gaming-blockchain/ | 2026-10-06 |
| E1-A4 | B | Transparency & Stability — reserves composition, weekly disclosure, monthly assurance (Circle) | https://www.circle.com/transparency | 2026-10-06 |
| E1-B4 | B | DeFi targeted by State Sponsored Adversaries: The Ronin Hack (Forta Network technical post-mortem) | https://forta.org/blog/ronin-hack | 2026-10-06 |
| E1-C4 | C | Aave V3 — TVL, Fees & Revenue (DeFiLlama) | https://defillama.com/protocol/aave-v3 | 2026-10-06 |
| E1-D4 | D | Examining UST's Collapse — Alex Thorn, Galaxy Research | https://www.galaxy.com/insights/research/examining-ust-collapse | 2026-10-06 |
| E1-A5 | A | SEC v. NovaTech Ltd. et al., Litigation Release No. 26072 — $650M crypto fraud | https://www.sec.gov/enforcement-litigation/litigation-releases/lr-26072 | 2026-10-06 |
| E1-B5 | B | Unpacking the $625M Ronin Network Heist: Independent Postmortem (LCTO) | https://articles.lcto.org/articles/unpacking-the-625m-ronin-network-heist-independent-postmortem | 2026-10-06 |
| E1-C5 | C | Aave totals — supplied and borrowed across all markets, monthly snapshots (Aavescan) | https://aavescan.com/protocol/totals | 2026-10-06 |
| E1-A6 | A | USA v. Lee et al. — HyperFund / HyperTech indictment, ~$1.89B securities and wire fraud (DOJ PDF) | https://www.justice.gov/criminal/media/1337526/dl?inline= | 2026-10-06 |
| E1-B6 | B | Iron Finance Post-Mortem, 17 June 2021 (official project incident report) | https://ironfinance.medium.com/iron-finance-post-mortem-17-june-2021-6a4e9ccf23f5 | 2026-10-06 |
| E1-C6 | C | Lido — TVL, Fees & Revenue (DeFiLlama; states the treasury/operator split methodology) | https://defillama.com/protocol/lido | 2026-10-06 |
| E1-D6 | D | In Token Crash Postmortem, Iron Finance Says It Suffered Crypto's 'First Large-Scale Bank Run' — Kevin Reynolds, CoinDesk | https://www.coindesk.com/markets/2021/06/17/in-token-crash-postmortem-iron-finance-says-it-suffered-cryptos-first-large-scale-bank-run | 2026-10-06 |
| E1-A7 | A | USA v. Russell Armand — Information, Saitama/VZZN market manipulation and unlicensed money transmission (DOJ PDF) | https://www.justice.gov/d9/2024-10/armand_information_0.pdf | 2026-10-06 |
| E1-C7 | C | stETH pool — TVL and 629.0k holder count (DeFiLlama yields) | https://defillama.com/yields/pool/747c1d2a-c668-4682-b9f9-296708a3dd90 | 2026-10-06 |
| E1-D7 | D | Analysis of the TITAN fall — Ivan Kuznetsov (independent on-chain arbitrage-profit reconstruction of the stabiliser failure) | https://jeiwan.net/posts/analysis-titan-fall/ | 2026-10-06 |
| E1-A8 | A | USA v. Bankman-Fried — Indictment, 13 Dec 2022 (DOJ PDF) | https://www.justice.gov/d9/press-releases/attachments/2022/12/13/u.s._v._bankman-fried_indictment_0.pdf | 2026-10-06 |
| E1-C8 | C | Total Value Secured by L2, with associated tokens excluded (L2BEAT) | https://l2beat.com/layer2s/tvs/ | 2026-10-06 |
| E1-D8 | D | Sam Bankman-Fried convicted of multi-billion dollar FTX fraud — Luc Cohen, Jody Godoy, Reuters | https://www.reuters.com/legal/ftx-founder-sam-bankman-fried-thought-rules-did-not-apply-him-prosecutor-says-2023-11-02/ | 2026-10-06 |
| E1-C9 | C | Arbitrum One — TVS by asset, UOPS, 38.3% trust assumptions, upgrade log (L2BEAT) | https://l2beat.com/layer2s/projects/arbitrum | 2026-10-06 |
| E1-D9 | D | Sam Bankman-Fried trial: FTX founder convicted of fraud — Associated Press ("at least $10 billion"; **not a primary figure**) | https://apnews.com/article/sam-bankman-fried-ftx-crypto-bitcoin-baa4c94f2c4237c860475ff92e6bcf42 | 2026-10-06 |
| E2-A1 | D | "Blockchain Lifecycle Prediction — Dead Coins", Kuehn & Adnan, arXiv:2610.01379 (preprint) — source of the >52% token-death base rate and the 0.98→0.5 | https://arxiv.org/abs/2610.01379 | 2026-10-06 |
| E2-C1 | C | Electric Capital — **Open Dev Data** platform documentation (methodology for Monthly Active Developers, developer segments, commit fingerprinting, can | https://opendevdata.org/ | 2026-10-06 |
| E2-D1 | D | 1kx — **Onchain Revenue Report H1 2025** (full draft): 1,244 protocols, 2020–Q3 2025; ~400 protocols >$1M annualised fees; 20 protocols >$10M value to | https://www.datocms-assets.com/65672/1761822830-1kx-revenue-report-full-draft.pdf | 2026-10-06 |
| E2-A2 | A | FBI press release: "Cryptocurrency and AI Scams Bilk Americans of Billions — FBI releases annual internet crime complaint report" (2025 IC3 report: 1, | https://www.fbi.gov/news/press-releases/cryptocurrency-and-ai-scams-bilk-americans-of-billions | 2026-10-06 |
| E2-C4 | C | CoinGecko — **Trust Score Methodology** (5 components: Liquidity 50%, Cybersecurity 20%, Regulation 15%, Incident 10%, Proof of Reserves 5%; rank-rela | https://support.coingecko.com/hc/en-us/articles/36442561461657-Trust-Score-Methodology | 2026-10-06 |
| E2-D4 | D | Cong, Li, Tang & Yang — "Crypto Wash Trading", *NBER Working Paper 30783* (2022), published *Management Science* 69(11), 6427–6454 (29 exchanges; wash | https://www.nber.org/papers/w30783 | 2026-10-06 |
| E2-D5 | D | Falk, Tsoukalas & Zhang — "Can AI Detect Wash Trading? Evidence from NFTs", arXiv:2311.18717v3 (three major NFT exchanges; ~38% (30–40%) of trades and | https://arxiv.org/abs/2311.18717 | 2026-10-06 |
| E2-D6 | D | Beyer (Oak Security) — "The Audit Gap in Blockchain Security: A Four-Year Empirical Study of Public Audit Findings and Real-World Exploit Incidents",  | https://arxiv.org/abs/2606.15465 | 2026-10-06 |
| E2-D7 | D | Bourveau, Brendel & Schoenfeld — "Decentralized Finance (DeFi) assurance: early evidence", *Review of Accounting Studies* 29, 2209–2253 (2024), open a | https://link.springer.com/article/10.1007/s11142-024-09834-8 | 2026-10-06 |
| E2-D8 | D | David, Zhou, Qin, Song, Cavallaro & Gervais — "Do you still need a manual smart contract audit?", arXiv:2306.12338 (benchmark of 52 previously comprom | https://arxiv.org/abs/2306.12338 | 2026-10-06 |
| E2-D9 | D | Messias, Yaish & Livshits — "Airdrops: Giving Money Away Is Harder Than It Seems", arXiv:2312.02752 (rev. 2026-08) (first comprehensive empirical stud | https://arxiv.org/abs/2312.02752 | 2026-10-06 |
| E1-A10 | A | USA v. Andean Medjedovic — indictment, KyberSwap ~$65M and Indexed Finance ~$16.5M exploits plus attempted extortion of the DAO multisig (EDNY PDF) | https://s3.documentcloud.org/documents/26511007/2024-12-30-indictment-usa-v-andean-medjedovic.pdf | 2026-10-06 |
| E1-D10 | D | Sushi Tries to Pick Up the Pieces: A DeFi Governance Case Study — CoinDesk (cites $5.2B TVL, Dec 2021) | https://www.coindesk.com/tech/2021/12/30/sushi-tries-to-pick-up-the-pieces-a-defi-governance-case-study | 2026-10-06 |
| E1-A11 | A | USA v. Rhoden & Nowlin — indictment, "Undead Tombstone" NFT scheme abandoned before mint completion (M.D. Fla. PDF) | https://www.justice.gov/usao-mdfl/media/1339176/dl?inline= | 2026-10-06 |
| E1-C11 | C | Chainlink — Fees & Revenue, cumulative fees and adapter breakdown (DeFiLlama) | https://defillama.com/protocol/chainlink | 2026-10-06 |
| E1-D11 | D | SushiSwap's Lingering Troubles — Genevieve Yeoh, Delphi Digital (traders −60%, LPs −70% YoY) | https://members.delphidigital.io/reports/sushiswaps-lingering-troubles | 2026-10-06 |
| E1-C12 | C | Chainlink CCIP — bridge volume (DeFiLlama bridge page) | https://defillama.com/bridge/chainlink-ccip | 2026-10-06 |
| E1-D12 | D | EigenLayer Outflows of $2.3B Signal Restaking Sector Slide — CoinDesk (citing DeFiLlama; $15.1B TVL, Renzo −45%, Kelp −22%) | https://www.coindesk.com/business/2024/07/25/eigenlayer-outflows-of-23b-signal-restaking-sector-slide | 2026-10-06 |
| E1-C13 | C | Pendle — TVL, Fees, Revenue & Volume (DeFiLlama; states the 5% yield fee and 80% trading-fee share) | https://defillama.com/protocol/pendle | 2026-10-06 |
| E1-D13 | D | U.S. crypto firm Harmony hit by $100 million heist — Elizabeth Howcroft, Tom Wilson, Hannah Lang, Reuters | https://www.reuters.com/technology/us-crypto-firm-harmony-hit-by-100-million-heist-2022-06-24/ | 2026-10-06 |
| E1-C14 | C | State of Filecoin Q3 2025 — 1,110 PiB stored, 2,491 datasets, 925 over 1,000 TiB, 36% utilisation (Messari / Blockworks) | https://messari.io/report/state-of-filecoin-q3-2025 | 2026-10-06 |
| E1-D14 | D | Curve Finance Drained of $50M While CRV Token Sinks 12% in Latest DeFi Exploit — CoinDesk (TVL >$3B → $1.7B; $100M founder position) | https://www.coindesk.com/business/2023/07/30/curve-finance-exploit-puts-100m-worth-of-crypto-at-risk | 2026-10-06 |
| E1-C15 | C | State of Filecoin Q2 2025 — dataset onboarding and named paid deals: Cornell/Ramo, The Defiant/Akave, Recall, Humanode (Blockworks Intel) | https://app.blockworks.com/report/state-of-filecoin-q2-2025 | 2026-10-06 |
| E1-D15 | D | Curve's crvUSD depegs as market reacts to shock events — CoinTelegraph (0.35% deviation, recovered) | https://cointelegraph.com/news/curve-crvusd-depegs-market-reacts-shock-events | 2026-10-06 |
| E1-C16 | C | Revisiting Beanstalk Farms Exploit — CertiK incident analysis | https://www.certik.com/blog/revisiting-beanstalk-farms-exploit | 2026-10-06 |
| E1-D16 | D | Ethereum and Solana NFT Scammers Charged in $22 Million Rug Pull Scheme — Jason Nelson, Decrypt (reporting the DOJ action; the indictment itself is E1 | https://decrypt.co/298427/ethereum-solana-nft-scammers-charged-22m-rug-pulls | 2026-10-06 |
| E1-C17 | C | Cornell astrophysics simulation data archived on Filecoin via Ramo (Filecoin Foundation case note; counterparty-side confirmation) | https://fil.org/blog/unlocking-the-cosmos-how-cornell-astrophysicist-uses-ramo-to-store-the-universe-on-filecoin | 2026-10-06 |
| E1-D17 | D | MakerDAO: What went wrong and how it was fixed — Colin Platt, Decrypt (mechanism of the zero-bid auctions) | https://decrypt.co/23027/makerdao-what-went-wrong-and-how-it-was-fixed | 2026-10-06 |
| E1-C18 | C | Sky — TVL, Fees & Revenue: $13.45M 30d revenue, $204.6M annualised, $764.76M cumulative (DeFiLlama) | https://defillama.com/protocol/sky | 2026-10-06 |
| E1-D18 | D | Circle assures market after stablecoin USDC breaks dollar peg — Elizabeth Howcroft, Rishabh Jaiswal, Reuters (11 Mar 2023, low $0.88) | https://www.reuters.com/business/crypto-firm-circle-reveals-33-bln-exposure-silicon-valley-bank-2023-03-11/ | 2026-10-06 |
| E1-C19 | C | ENS — Fees & Revenue: $241,164 30d fees = 100% protocol revenue, registration vs renewal split (DeFiLlama) | https://defillama.com/protocol/ens | 2026-10-06 |
| E1-D19 | D | Axie Infinity gaming network Ronin sets date for Ethereum L2 migration — Andrew Hayward, Decrypt (RON inflation >20% → <1%, May 2026 migration) | https://decrypt.co/365131/axie-infinity-gaming-network-ronin-ethereum-layer-2-migration | 2026-10-06 |
| E1-C20 | C | Ondo RWA dashboard — USDY and OUSG AUM, issuer entities, and the note that ONDO carries no fee entitlement (DeFiLlama) | https://defillama.com/rwa/platform/ondo | 2026-10-06 |
| E1-D20 | D | USDC Stablecoin Momentarily Depegs to $0.74 on Binance — CoinDesk (3 Jan 2024, order-book imbalance) | https://www.coindesk.com/markets/2024/01/03/usdc-stablecoin-momentarily-depegs-to-074-on-binance | 2026-10-06 |
| E1-C21 | B | OUSG portfolio composition as of 5 Oct 2026 — BUIDL / BENJI / FYOXX / USDC, with holdings shown (Ondo) | https://ondo.finance/ousg | 2026-10-06 |
| E1-C22 | C | SushiSwap — TVL $41.75M, 30d volume $31.21M, cumulative volume $251.6B, 30d revenue $6,510 (DeFiLlama) | https://defillama.com/protocol/sushiswap | 2026-10-06 |
| E1-C23 | C | EigenCloud (ex-EigenLayer) — TVL $7.03B and restaking category share (DeFiLlama) | https://defillama.com/protocol/eigencloud | 2026-10-06 |
| E1-C24 | C | Harmony Incident Analysis — CertiK on-chain trace, ~$97M across 12 transactions, multisig-owner bypass | https://www.certik.com/blog/harmony-incident-analysis | 2026-10-06 |
| E1-C25 | C | Ethena USDe — TVL ~$4.9B, quarterly revenue series (DeFiLlama) | https://defillama.com/protocol/ethena-usde | 2026-10-06 |
| E1-C26 | C | Curve Finance Pools Exploited Due to Code Vulnerabilities — Vyper versions, ~$70M, contagion to Alchemix and Metronome (Chainalysis) | https://www.chainalysis.com/blog/curve-finance-liquidity-pool-hack/ | 2026-10-06 |
| E1-C27 | B | CCIP Metrics — cumulative transfer volume and fees (Chainlink, on-chain measured) | https://www.chainlinkecosystem.com/ccip-metrics | 2026-10-06 |
| E2-D10 | D | Li, Chen & Cai — "From Slang to Standards: Consensus-Driven Airdrop Hunter Definition as a Baseline for Cryptocurrency Ecosystem Security and Governan | https://dl.acm.org/doi/abs/10.1145/3772318.3790777 | 2026-10-06 |
| E2-D11 | D | Ovezik, Karakostas, Milad, Kiayias & Woods — "SoK: Measuring Blockchain Decentralization", arXiv:2501.18279 (ACAC 2025 / Springer LNCS chapter) (measu | https://arxiv.org/abs/2501.18279 | 2026-10-06 |
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
| F51 | D | Chen, Lin & Wu — Do cryptocurrency exchanges fake trading volumes? (Physica A 586, 2022) | https://www.sciencedirect.com/science/article/abs/pii/S0378437121006786 | 2026-10-06 |
| F52 | D | Albo, Lanir, Bak & Rafaeli — Off the Radar: Comparative Evaluation of Radial Visualization Solutions | https://exa.ai/library/publication/n830v4ng3fw | 2026-10-06 |
| F53 | D | Kantabutra & Tangmanee — Perceptual Bias in Mobile Radar Chart Visualization (2025) | https://exa.ai/library/publication/m24bx6s0mqj | 2026-10-06 |
| F54 | D | Evaluating Design Features that Enhance Radar Chart Comprehension on Smartphones (2019) | https://www.aasmr.org/jsms/Vol14/No.7/Vol.14.No.7.31.pdf | 2026-10-06 |
| F55 | D | Keep Radar Graphs Below the Radar — Stephen Few (2005) [practitioner, illustration only] | https://perceptualedge.com/articles/dmreview/radar_graphs.pdf | 2026-10-06 |
| F56 | D | Nguyen, Tran, Bethke, Westmoreland, Nelson, Barve — Combating Sybil attacks in cooperative systems (Gatekeeper, SumUp, Credo) | https://cs.nyu.edu/media/publications/tran_nguyen.pdf | 2026-10-06 |
| F57 | D | SybilLimit: A Near-Optimal Social Network Defense against Sybil Attacks (NUS) | https://www.comp.nus.edu.sg/~yuhf/yuh-sybillimit.pdf | 2026-10-06 |
| F58 | D | Baghaee et al. — Sybil in the Haystack (Algorithmic 16(1):34, MDPI) | https://www.mdpi.com/1999-4893/16/1/34 | 2026-10-06 |
| F59 | A | FATF/OECD — API evangelist OpenAPI spec for OFAC SLS search & lists endpoints | https://raw.githubusercontent.com/api-evangelist/department-of-the-treasury/refs/heads/main/openapi/department-of-the-treasury-search-api-openapi.yml | 2026-10-06 |
| T-A1 | A | SEC Fact Sheet, *Application of the Federal Securities Laws to Certain Types of Crypto Assets and Certain Transactions Involving Crypto Assets* (Rel.  | https://www.sec.gov/files/33-11412-fact-sheet.pdf | 2026-10-06 |
| T-B1 | B | Registrant statements on SEC EDGAR: SEC closed Uniswap Labs and OpenSea investigations (Feb 2025); ~89 crypto enforcement cases dropped/frozen in 2025 | https://www.sec.gov/Archives/edgar/data/2078856/000119312526272886/d50758ds1a.htm | 2026-10-06 |
| T-D1 | D | Wilson Sonsini, *No Commission Without Permission* (3 Mar 2025) — transaction-based compensation as hallmark of broker-dealer activity; citing Rel. 10 | https://www.wsgr.com/en/insights/no-commission-without-permission-sec-reinforces-focus-on-sales-activities-and-transaction-based-compensation-as-hallmarks-of-broker-dealer-status-in-recent-settlements.html | 2026-10-06 |
| T-A2 | A | SEC Commission Interpretation 33-11412 (full PDF) | https://www.sec.gov/files/rules/interp/2026/33-11412.pdf | 2026-10-06 |
| T-B2 | B | MAS Financial Institutions Directory (filter: Digital Payment Token Service) | https://eservices.mas.gov.sg/fid/institution?category=Major+Payment+Institution&activity=Digital+Payment+Token+Service | 2026-10-06 |
| T-D2 | D | *Business & Finance Law Review* note, J. Berkun — self-custody wallet platforms and the "finder's exception" (analytic, not a holding) | https://gwbflr.org/wp-content/uploads/2025/05/J.Berkun-Note_FINAL.pdf | 2026-10-06 |
| T-A3 | A | SEC Press Release 2026-30, *SEC Clarifies the Application of Federal Securities Laws to Crypto Assets* (17 Mar 2026) | https://www.sec.gov/newsroom/press-releases/2026-30-sec-clarifies-application-federal-securities-laws-crypto-assets | 2026-10-06 |
| T-B3 | B | FinCEN *MSB Registrant Search* (BSA ID / entity lookup; ~39,713 registered MSBs) | https://www.fincen.gov/resources/msb-state-selector | 2026-10-06 |
| T-D3 | D | AGIO Ratings, *Why Proof of Reserves Is Not Enough for Crypto Counterparty Risk* (4 Jul 2026) — PoR limitations | https://www.agioratings.io/insights/why-proof-of-reserves-is-not-enough-for-crypto-counterparty-risk | 2026-10-06 |
| T-A4 | A | SEC Press Release 2026-76 + Proposed Rule 33-11434, *Regulation Crypto Assets* (18 Aug 2026) | https://www.sec.gov/newsroom/press-releases/2026-76-sec-proposes-new-regulation-crypto-assets | 2026-10-06 |
| T-B4 | B | SEC *Investment Adviser Public Disclosure* (IAPD; Form ADV + FINRA BrokerCheck) | https://adviserinfo.sec.gov/ | 2026-10-06 |
| T-A5 | A | H.R. 3633, *Digital Asset Market Clarity Act of 2025* — House Report 119-168 (23 Jun 2025) | https://www.congress.gov/119/crpt/hrpt168/CRPT-119hrpt168.pdf | 2026-10-06 |
| T-B5 | B | IAF CertSearch — ISO/IEC 27001 certificate validation | https://www.iafcertsearch.org/ | 2026-10-06 |
| T-A6 | A | SEC, Statement of Chairman Atkins on Regulation Crypto Assets — "support Congress in delivering the CLARITY Act" (18 Aug 2026) | https://www.sec.gov/newsroom/speeches-statements/atkins-statement-regulation-crypto-assets-081826 | 2026-10-06 |
| T-B6 | B | ICANN *Registration Data Policy* and *RDAP* (registrant-data redaction post-GDPR) | https://www.icann.org/en/contracted-parties/consensus-policies/registration-data-policy | 2026-10-06 |
| T-A7 | A | SEC Proposed Rule 33-11434, *Regulation Crypto Assets* (full PDF) | https://www.sec.gov/files/rules/proposed/2026/33-11434.pdf | 2026-10-06 |
| T-B7 | B | State Secretary of State corporate registries (entity-existence verification) | https://bizfileonline.sos.ca.gov/search | 2026-10-06 |
| T-A8 | A | SEC Press Release 2025-47, *SEC Announces Dismissal of Civil Enforcement Action Against Coinbase* (27 Feb 2025) | https://www.sec.gov/newsroom/press-releases/2025-47 | 2026-10-06 |
| T-B8 | B | FCA Register — cryptoasset firms registered under the Money Laundering Regulations (entry format) | https://register.fca.org.uk/s/firm?id=001b000003O1uMmAAJ | 2026-10-06 |
| T-A9 | A | SEC-filed Joint Stipulation of Dismissal, *SEC v. Coinbase* (2025) | https://www.sec.gov/files/litigation/complaints/2025/stipulation-pr2025-47.pdf | 2026-10-06 |
| T-B9 | B | ESMA *Databases and Registers* (MiCA registers under Arts. 109–110) | https://www.esma.europa.eu/publications-and-data/databases-and-registers | 2026-10-06 |
| T-A10 | A | SEC Litigation Release 26278, *Payward, Inc. and Payward Ventures, Inc. (d/b/a Kraken)* — dismissed with prejudice (27 Mar 2025) | https://www.sec.gov/enforcement-litigation/litigation-releases/lr-26278 | 2026-10-06 |
| T-A11 | A | SEC-filed Stipulation of Dismissal, *SEC v. Binance* (29 May 2025) | https://www.sec.gov/files/litigation/litreleases/2025/stipulation-dismissal-26316.pdf | 2026-10-06 |
| T-A12 | A | SEC Newsroom, SEC Crypto Task Force chairman letter, 13 Mar 2025 (context for CASP/discovery posture) | https://www.sec.gov/files/ctf-input-andreesen-horowitz-2025-03-13.pdf | 2026-10-06 |
| T-A13 | A | US Treasury / OFAC, *U.S. Treasury Sanctions Notorious Virtual Currency Mixer Tornado Cash* (8 Aug 2022) | https://home.treasury.gov/news/press-releases/jy0916 | 2026-10-06 |
| T-A14 | A | CFTC Press Release 8822-23, *CFTC Releases FY 2023 Enforcement Results* (7 Nov 2023) — Ooki DAO "person"/unincorporated association holding; 47 digita | https://www.cftc.gov/PressRoom/PressReleases/8822-23 | 2026-10-06 |
| T-A15 | A | ESMA, *Markets in Crypto-Assets Regulation (MiCA)* — Interim MiCA Register (5 CSVs incl. Non-compliant entities); last update 30 Sep 2026 | https://www.esma.europa.eu/esmas-activities/digital-finance-and-innovation/markets-crypto-assets-regulation-mica | 2026-10-06 |
| T-A16 | A | ESMA Q&A on MiCA scope (Apr 2026): fully decentralised services without an intermediary out of scope | https://www.esma.europa.eu/print/view/pdf/esma_q_a_search_page/page_2?&page=75 | 2026-10-06 |
| T-A17 | A | ESMA75-453128700-438, *Second Consultation Paper on MiCA* (5 Oct 2023) | https://www.esma.europa.eu/sites/default/files/2023-10/ESMA75-453128700-438_MiCA_Consultation_Paper_2nd_package.pdf | 2026-10-06 |
| T-A18 | A | DOJ SDNY, *Tornado Cash Founders Charged With Money Laundering and Sanctions Violations* (23 Aug 2023) | https://www.justice.gov/usao-sdny/pr/tornado-cash-founders-charged-money-laundering-and-sanctions-violations | 2026-10-06 |
| T-A18b | A | ESMA, *Disclosure items for the crypto-asset white paper* (name, legal form, registered address, LEI) | https://www.esma.europa.eu/publications-and-data/interactive-single-rulebook/mica/disclosure-items-crypto-asset-white-paper-e | 2026-10-06 |
| T-A19 | A* | DOJ SDNY, *Founder of Tornado Cash Crypto Mixing Service Convicted of Knowingly Transmitting Criminal Proceeds* (6 Aug 2025) — verdict before Judge Ka | https://www.justice.gov/usao-sdny/pr/founder-tornado-cash-crypto-mixing-service-convicted-knowingly-transmitting-criminal | 2026-10-06 |
| T-A20 | A | FCA, *Cryptoasset firms: Authorisation, supervision and enforcement* (final rules 30 Jun 2026; applies to firms authorised on/after 25 Oct 2027) | https://www.fca.org.uk/firms/new-regime-cryptoasset-regulation/authorisation-supervision-enforcement | 2026-10-06 |
| T-A21 | A | US Treasury / OFAC, *Treasury Sanctions Cryptocurrency Exchange and Network Enabling Sanctions Evasion and Cyber Criminals* — Garantex re-designation, | https://home.treasury.gov/news/press-releases/sb0225 | 2026-10-06 |
| T-A22 | A | ASIC Media Release 25-194MR, *High Court grants ASIC special leave to appeal Block Earner decision* (5 Sep 2025) | https://www.asic.gov.au/about-asic/news-centre/find-a-media-release/2025-releases/25-194mr-high-court-grants-asic-special-leave-to-appeal-block-earner-decision | 2026-10-06 |
| T-A23 | A | ASIC Media Release 26-124MR, *ASIC successful in High Court Block Earner appeal* — unanimous 7-0; Corporations Amendment (Digital Assets Framework) Ac | https://www.asic.gov.au/about-asic/news-centre/find-a-media-release/2026-releases/26-124mr-asic-successful-in-high-court-block-earner-appeal | 2026-10-06 |
| T-A24 | A | FinCEN, *Guidance on Application of FinCEN's Regulations to Certain Business Models Involving Convertible Virtual Currencies* (FIN 2019-G001, 9 May 20 | https://www.fincen.gov/system/files/2019-05/FinCEN%20CVC%20Guidance%20FINAL.pdf | 2026-10-06 |
| T-A25 | A | ESMA35-1872330276-1899, *Final Report on the Guidelines on reverse solicitation under MiCA* (17 Dec 2024) — Art. 59(1) authorisation requirement | https://www.esma.europa.eu/sites/default/files/2024-12/ESMA35-1872330276-1899_-_Final_report_on_GLs_on_reverse_solicitation_under_MiCA.pdf | 2026-10-06 |
| T-A26 | A | Regulation (EU) 2023/1114 (MiCA), Article 61 — provision of services at client's exclusive initiative; anti-disclaimer rule | https://www.esma.europa.eu/publications-and-data/interactive-single-rulebook/mica/article-61-provision-crypto-asset-services | 2026-10-06 |
| T-A27 | A | ESMA Public Statement ESMA75-113276571-1710, *ESMA calls on unauthorised CASPs to wind down… as MiCA transitional period ends* (23 Jun 2026) | https://www.esma.europa.eu/sites/default/files/2026-06/ESMA75-113276571-1710_Public_Statement_MiCA_transitional_period_ends.pdf | 2026-10-06 |
| T-A28 | A | US Treasury, *2024 National Proliferation Financing Risk Assessment* — "OFAC sanctions compliance works on a strict liability standard" | https://home.treasury.gov/system/files/136/2024-National-Proliferation-Financing-Risk-Assessment.pdf | 2026-10-06 |
| T-A29 | C | Chainalysis, *2026 Crypto Crime Report — Introduction* (8 Jan 2026): ≥$154bn illicit 2025, +162%, sanctioned +694%, <1% of attributed volume, stableco | https://www.chainalysis.com/blog/2026-crypto-crime-report-introduction/ | 2026-10-06 |
| T-A30 | C | Chainalysis, *Crypto Sanctions: 2026 Crypto Crime Report* (5 Mar 2026): A7A5 $93.3bn; Grinex/Meer; Tornado Cash SDN delisting Mar 2025 | https://www.chainalysis.com/blog/crypto-sanctions-2026/ | 2026-10-06 |
| T-A31 | A | Regulation (EU) 2023/1113 (TFR) — EUR-Lex summary; applies from 30 Dec 2024 | https://eur-lex.europa.eu/EN/legal-content/summary/information-accompanying-transfers-of-funds-and-certain-crypto-assets.html | 2026-10-06 |
| T-A32 | A | Regulation (EU) 2024/1624 (AMLR) | https://eur-lex.europa.eu/eli/reg/2024/1624/oj/eng | 2026-10-06 |
| T-A33 | A | FinCEN, *Requests for Information on Existing Registrant Status Regarding Money Services Business Activities Relating to Mixing* (311 NPRM, 19 Oct 202 | https://www.fincen.gov/system/files/federal_register_notices/2023-10-19/FinCEN_311MixingNPRM_FINAL.pdf | 2026-10-06 |
| T-A34 | A | FinCEN, *TD Bank Consent Order, Number 2024-02* (10 Oct 2024) | https://www.fincen.gov/system/files/enforcement_action/2024-10-10/FinCEN-TD-Bank-Consent-Order-508FINAL.pdf | 2026-10-06 |
| T-A35 | A | EDPB, *Guidelines 02/2025 on processing of personal data through blockchain technologies* (final version, 7 Jul 2026) | https://www.edpb.europa.eu/documents/guideline/guidelines-022025-on-processing-of-personal-data-through-blockchain_en | 2026-10-06 |
| T-A36 | A | Regulation (EU) 2023/1113, Art. — personal data transmitted in accordance with GDPR | https://eur-lex.europa.eu/eli/reg/2023/1113/oj/eng | 2026-10-06 |
| T-A37 | A | US Treasury, *2026 National Money Laundering Risk Assessment* (2026-NMLRA) — DASPs "may claim not to be regulated financial institutions" | https://home.treasury.gov/system/files/246/2026-NMLRA.pdf | 2026-10-06 |
| T-A38 | A | US Treasury, *Illicit Finance Risk Assessment of Decentralized Finance* (DeFi-Risk-Full-Review) | https://home.treasury.gov/system/files/136/DeFi-Risk-Full-Review.pdf | 2026-10-06 |
| T-A39 | A | US Treasury, *Report to Congress from the Secretary of the Treasury* on GENIUS Act illicit finance (Mar 2026) | https://home.treasury.gov/system/files/246/GENIUS-Act-Illicit-Finance-Innovation-Congressional-Report-March-2026.pdf | 2026-10-06 |
| T-A40 | A | SEC, Statement on President Trump Signing the GENIUS Act into Law (18 Jul 2025) | https://www.sec.gov/newsroom/speeches-statements/atkins-statement-genius-act-071825 | 2026-10-06 |
| T-A41 | A | SEC, *Frequently Asked Questions Relating to Crypto Asset Activities Distributed Ledger Technology* (15 May 2025) — references GENIUS Act, 12 U.S.C. 5 | https://www.sec.gov/rules-regulations/staff-guidance/trading-markets-frequently-asked-questions/frequently-asked-questions-relating-crypto-asset-activities-distributed-ledger-technology | 2026-10-06 |
| T-A42 | A | Regulation (EU) 2023/1114 (MiCA) full text (EUR-Lex) | https://eur-lex.europa.eu/legal-content/EN/TXT/PDF/?uri=CELEX:32023R1114 | 2026-10-06 |
| T-A43 | A | ASIC Information Sheet 225 (INFO 225), *Digital assets: Financial products and services* | https://www.asic.gov.au/regulatory-resources/digital-transformation/digital-assets-financial-products-and-services | 2026-10-06 |
| T-A44 | A | MAS Media Release, *MAS Strengthens Regulatory Measures for Digital Payment Token Services* (23 Nov 2023) | https://www.mas.gov.sg/news/media-releases/2023/mas-strengthens-regulatory-measures-for-digital-payment-token-services | 2026-10-06 |
| T-A45 | A | CFTC Press Releases 9059-25 / 9060-25, withdrawal of Staff Advisories on virtual currency (28 Mar 2025) | https://www.cftc.gov/PressRoom/PressReleases/9059-25 | 2026-10-06 |

## Totals

- Unique sources: **331** (from 346 raw citations; 14 URL duplicates collapsed)
- By tier: A=80, A*=1, B=93, C=70, D=84, E=3
- Per slice: A=54, B=43, C=50, D=58, E1=64, E2=18, F=59
