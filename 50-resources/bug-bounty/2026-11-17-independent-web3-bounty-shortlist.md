---
title: Independent web3 bug bounties worth hunting for High and Critical
type: resource
owner: dev
tasks: []
tags: [bug-bounty, web3, security, defi, ethereum, solana]
created: 2026-11-17
---

# Independent web3 bug bounties worth hunting for High and Critical

Self-hosted programs only: the report goes to the project's own email, form or portal, never to
Immunefi, HackenProof, Cantina, Sherlock, Code4rena, HackerOne or Bugcrowd. Every figure here was
read off the program's own page on 2026-11-16. Programs change fast, so re-read the page before
sending a report. How the list was built: [[2026-11-17-finding-independent-bounty-programs]].

Conversions use CoinGecko prices from 2026-11-16: SOL $71, ETH $1,810, BTC $64,395. TVL figures
are from DefiLlama on the same day.

## Deadlines

**Firedancer** (`bounty.firedancer.io`, terms effective 2026-10-26)

- Until **2026-11-20**, every payout is doubled and Alpenglow-specific findings are tripled.
- Standard: Critical $50k–100k, High $10k–50k, Medium $5k–10k, Low $1k–5k.
- Boosted: Critical $100k–200k (Alpenglow $150k–300k), High $20k–100k (Alpenglow $30k–150k).
- Paid in USDC to a Solana wallet. KYC. 1 SOL burned per report. First reply comes on the GitHub
  Security Advisory within 96 hours.
- Only the Alpenglow config counts (`[consensus] alpenglow = true`), and the PoC must run end to
  end on a local cluster.

**Ethereum Foundation, Glamsterdam fork** (`ethereum.org/en/bug-bounty`)

- Glamsterdam bugs get a multiplier that depends on when they are reported:
  - 0.5x from the release-candidate blog post until the Sepolia upgrade (Medium and up only).
  - **2x** from 24 hours after the Sepolia upgrade finalizes until the Hoodi upgrade.
  - 1.5x from the Hoodi upgrade until one week before mainnet.
- Sepolia forked on 2026-10-06. Hoodi was set for 2026-10-26, then postponed on 2026-10-22
  (ACDE #248) with no new date, so the 2x window is still open. ACDE #249 (around 2026-11-19)
  may set the date.

## Tier 1

| Program | Top payouts | Money at risk | Submit to |
|---|---|---|---|
| Curve | Up to $250k (matrix below) | $1.11B TVL | `security@curve.finance` |
| Extended (perp DEX, Starknet) | Critical up to $500k, High up to $50k | $96.8M TVL | `security@extended.exchange` |
| Safe | High (top tier) up to $1M, Medium up to $50k | Large, not tracked by DefiLlama | `bounty@safefoundation.org` |
| EF Glamsterdam | Critical $1M, High $50k (2x now: $2M, $100k) | Ethereum L1 | Own form, `bbp-form.ethereum.org` |
| Agave (Solana) | Loss of funds 6,250–25,000 SOL | Solana L1 | Own portal, `bounty.anza.xyz` |

**Curve.** Payout is likelihood times severity:

| Likelihood | Low | Moderate | High |
|---|---|---|---|
| Almost certain | $10k | $50k | $250k |
| Possible | $1k | $10k | $50k |
| Unlikely | $250 | $1k | $5k |

- Scope is anything that can cause a substantial loss of money, a broken liveness condition, or
  an irreversible loss of funds.
- The reporter must be able to sign a message from the reporting address.
- Precedent: paid the full $250k in 2024 to Marco Croc (Kupia Security) for a reentrancy bug.

**Extended.**

- Critical is "bugs or exploits causing unintended financial consequences". High is network
  downtime or liveness failure. Medium up to $5k. Paid in USDC.
- KYC/KYB. A bug must be reported within 24 hours of discovery.
- Scope only says "live environment", and the SDK is out. Email to pin the scope down first.
- Mainnet since 2025-08-12. The tier wording is copied from Hyperliquid's program.

**Safe.**

- Email in plain text, with no attachments and no links. Paid in USDC on mainnet. No KYC
  mentioned.
- The core Safe Smart Account (v1.1.1 to v1.5.0) is heavily audited. The realistic targets are
  the newer modules: Passkey v0.2.1 (WebAuthn signers, P256), Safe 4337 Module v0.3.0, Social
  Recovery v0.1.0, Allowance Module v1.0.0 and Safenet Beta Staking v0.1.0.
- History: 7 paid bounties, $41.5k in total, the largest $30k.

**EF Glamsterdam.**

- Base caps: Critical $1M, High $50k, Medium $10k, Low $2k. Paid in ETH or DAI.
- KYC: real name and ID, sent PGP-encrypted to EF legal. Replies can take a week because of the
  AI report flood.
- New code in the fork: ePBS (EIP-7732), block-level access lists (EIP-7928) and gas repricing.
  ChainSecurity flagged a gas-griefing issue in EIP-8037/8038 before Hoodi was postponed.
- Scope: EL clients (Besu, Erigon, ethrex, Geth, Nethermind, Reth), CL clients (Grandine,
  Lighthouse, Lodestar, Nimbus, Prysm, Teku), Solidity, Vyper, the deposit contract, C-KZG-4844,
  Go-ETH-KZG, and the specs including the Fast Confirmation Rule.

**Agave.**

| Category | SOL | USD at $71 |
|---|---|---|
| Loss of funds | 6,250–25,000 | $444k–1.78M |
| Consensus or safety violation | 3,125–12,500 | $222k–888k |
| Liveness or loss of availability | 1,250–5,000 | $89k–355k |
| DoS (non-RPC) and supply chain | 315–1,250 | $22k–89k |

- Paid into stake accounts locked for 12 months, so the payout carries SOL price risk. KYC.
- Each report costs a 0.5 SOL burn. `bounty.anza.xyz/api/config` gives the burn address
  (`1nc1nerator11111111111111111111111111111111`, mainnet-beta). The portal files each report as
  a GitHub Security Advisory.
- Alpenglow consensus is in scope since 2026-09-16. The Alpenglow competition (2026-08-05 to
  2026-08-19) already drew 300+ reports, so that code has been combed.
- SPL Token, Token-2022 and Stake Pool use the same portal. Stake Pool loss of funds pays
  1,000–10,000 SOL.
- On 2026-11-16 the portal showed "Competition not yet live". Check that the submit form opens
  after GitHub login before burning any SOL.

## Tier 2

| Program | Top payout | Next tier | Submit to |
|---|---|---|---|
| Hyperliquid | Critical under 1M USDC | High under 50k USDC | `bugbounty@hyperfoundation.org` |
| Across | Critical up to $1M | High flat $10k | `bugs@across.to` |
| Trezor | Firmware Core up to $100k | Suite and Connect up to $20k | `security@satoshilabs.com` |
| Pacifica | Critical 10k–25k USDC | High 2.5k–10k USDC | `ops@pacifica.fi` or Discord ticket |
| Alto | Critical $20k–100k | High $5k–20k | `security@altofoundation.org` |

- **Hyperliquid:** ~$7.39B TVL. High means network downtime without incorrect state; Medium is
  under 10k. The node is closed source as far as I know, so the work is mostly black box.
  Testnet or local fork only. KYC/KYB. No public payout record found.
- **Across:** $12.8M TVL. Scope covers the contracts plus the relayer and dataworker bots and the
  front-end code, all TypeScript. Medium $1k, Low $250, UMA terms apply. The 2026-11-10 domain
  hijack (Namecheap social engineering, ~$350k lost) was not a code bug.
- **Trezor:** caps are per component: Firmware Core $100k (uncapped in exceptional cases), Model
  One $50k, Trezor.io $25k, Suite $20k, Connect $20k, Blockbook $10k. Paid in BTC. Send the
  report as a .md file.
- **Pacifica:** $24.2M TVL. Backend and infra are in scope, not just contracts. Low 500 USDC.
  KYC. Test on `test-app.pacifica.fi`.
- **Alto:** Critical pays 10% of funds affected, with a $20k floor and a $100k cap. TVL is ~$55k,
  so in practice every Critical pays $20k. Medium flat $3k. KYC above $10k. 12 audit reports,
  but new code ships often (a new IRM went live 2026-11-12).

## Skipped for High and Critical

- **Obol:** Critical now tops out at $25k and High at $2.5k. The $100k on Lissy93's list is
  stale.
- **Low ceilings:** EFP ($10k, no funds at risk), Range (Critical $5k), Foundation.xyz (web
  Critical $500), SpectroCoin ($15k, web only). Request Finance returned 403, so unverified.
- **Ledger Donjon:** discretionary, with 2026 disputes: EUR 200 offered for a Low
  (1SEAL-2026-005) and the TestMachine case.
- **Parity:** discretionary. A report rated Medium in June 2026 was exploited for ~$1.4M in
  October 2026.
- **Kraken:** amounts unpublished (minimum $500 in BTC), needs an intermediate-verified Kraken
  account, and kraken.com is blocked from my home ISP.
- **BitBox:** discretionary, policy last updated 2022.
- **Flashbots:** the docs still say the bounty is "coming soon".
- **BlazePhoenix:** pays in its own token, which only launches on 15 Dec. Red flag.

## Checked, but platform-hosted

Aave (bounty repo archived 2023, now Immunefi), Compound (Immunefi), 1inch (Immunefi and
HackenProof), Ripple/XRPL (Bugcrowd, private), Scallop (HackenProof), Lido (Immunefi), RedStone
(HackenProof and Cantina), TON (HackenProof), Paradex (Sherlock).

## Where to start (DeFi and trading, Solidity and TypeScript)

1. **Curve:** AMM and lending (crvUSD, LlamaLend, scrvUSD). Closest to my LP work, and the max
   payout has been paid before.
2. **Extended:** perp DEX, Critical $500k, probably few hunters. Email to confirm scope first.
3. **Safe:** the newer modules, not the core account.
4. **Across:** the contracts plus the off-chain bots.
5. **EF Glamsterdam:** protocol level, while the 2x lasts.

## Sources

- [ethereum.org bug bounty](https://ethereum.org/en/bug-bounty/)
- [EF blog: Glamsterdam on Hoodi postponed](https://blog.ethereum.org/2026/10/22/glamsterdam-hoodi-delay)
- [BigGo: Glamsterdam pulled from Hoodi](https://finance.biggo.com/news/094d80db-dd23-45bd-ab03-3d7b1afefd8c)
- [Firedancer bounty terms](https://bounty.firedancer.io/terms.html)
- [CoinDesk: Solana bug reports now cost 1 SOL](https://www.coindesk.com/tech/2026/10/27/solana-bug-reports-now-cost-1-sol-as-ai-floods-security-teams-with-spam)
- [Agave SECURITY.md](https://github.com/anza-xyz/agave/blob/master/SECURITY.md)
- [Anza bounty portal](https://bounty.anza.xyz/)
- [Cryptobriefing: Alpenglow bounty closes](https://cryptobriefing.com/solana-alpenglow-bug-bounty-closes/)
- [Curve bug bounty](https://docs.curve.finance/user/security/bug-bounty)
- [Cointelegraph: Curve awards $250k](https://cointelegraph.com/news/curve-finance-awards-250k-finding-reentrancy-vulnerability)
- [Extended bounty program](https://docs.extended.exchange/extended-resources/more/bounty-program)
- [Safe bug bounty](https://docs.safefoundation.org/security/bug-bounty)
- [Safe past bounties](https://docs.safefoundation.org/security/past-bounties)
- [Hyperliquid bug bounty](https://hyperliquid.gitbook.io/hyperliquid-docs/bug-bounty-program)
- [Across bug bounty](https://docs.across.to/introduction/bug-bounty)
- [TokenPost: Across domain takeover](https://www.tokenpost.com/news/technology/54491)
- [Trezor bug bounty terms](https://trezor.io/other/partner-portal/for-developers/trezor-bug-bounty-program-scope-rules-and-rewards)
- [Pacifica bug bounty](https://pacifica.gitbook.io/docs/programs/bug-bounty-program)
- [Alto bug bounty](https://github.com/altomoney/security-review-engagements)
- [Obol bug bounty](https://docs.obol.org/advanced-and-troubleshooting/security/bug-bounty)
- [EFP bug bounty](https://docs.efp.app/bugbounty/)
- [Range bug bounty](https://docs.range.org/resources/bug-bounty)
- [Foundation disclosure](https://foundation.xyz/responsible-disclosure/)
- [1SEAL-2026-005 (Ledger)](https://1seal-org.pages.dev/research/advisories/1SEAL-2026-005)
- [crypto.news: Ledger signing flaw](https://crypto.news/ledger-says-ethereum-signing-flaw-was-already-fixed/)
- [Parity bug bounty](https://parity.io/bug-bounty)
- [Cryptoast: Polkadot XCM bug](https://cryptoast.fr/polkadot-bug-xcm-dotusd-tresorerie-perd-1-4-million-dollars/)
- [Kraken bug bounty](https://www.kraken.com/features/security/bug-bounty)
- [BitBox bug bounty policy](https://bitbox.swiss/policies/bug-bounty-policy/)
- [Flashbots security docs](https://docs.flashbots.net/flashbots-mev-boost/security)
- [Aave bug-bounty repo (archived)](https://github.com/aave/bug-bounty)
- [HackenProof: TON bounty](https://hackenproof.com/blog/ton-foundation-bug-bounty-reward)
- [Cryptobriefing: Paradex on Sherlock](https://cryptobriefing.com/paradex-bug-bounty-program-500k-sherlock/)
- [DefiLlama](https://defillama.com), [CoinGecko](https://www.coingecko.com)

Related: [[2026-11-17-finding-independent-bounty-programs]], [[index]]
