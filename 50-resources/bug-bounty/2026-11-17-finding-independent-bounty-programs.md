---
title: Finding and vetting independent web3 bug bounty programs
type: resource
owner: dev
tasks: []
tags: [bug-bounty, web3, security, dorking, osint]
created: 2026-11-17
---

# Finding and vetting independent web3 bug bounty programs

An independent program takes reports on the project's own channel: an email address, its own
form, or its own portal. Fewer hunters look at these than at Immunefi listings, but no platform
sits in the middle if the team goes quiet or lowballs. Current shortlist:
[[2026-11-17-independent-web3-bounty-shortlist]].

## Where to look

- **Lissy93/bug-bounties**, `independent-programs.yml`: 240 programs, mostly not web3. Good for
  names and contacts, but the reward figures go stale. It listed Obol at $100k; the live page
  says $25k.
- **Protocol docs:** GitBook pages, and `SECURITY.md` or `bug-bounty.md` files on GitHub.
- **Dorks:**

```
intitle:"bug bounty" ("smart contract" OR protocol) -immunefi -hackenproof -cantina -sherlock -code4rena
site:gitbook.io "bug bounty program" "USDC" "critical"
site:github.com "bug-bounty.md" "critical" "security@"
site:github.com inurl:SECURITY.md "bounty" "smart contract" "up to"
"bug bounty" "do not use external sites" OR "only through our" email USDC
"bug bounty" "paid in USDC" "mailto:security@" -site:immunefi.com
site:xyz intitle:"bug bounty" "proof of concept"
inurl:".well-known/security.txt" "bounty"
"launching our bug bounty" "security@" site:mirror.xyz OR site:medium.com
```

## Vetting a candidate

1. **Open the program page itself.** Many docs pages titled "bug bounty" only link out to
   Immunefi or HackenProof. Aave's GitHub bounty repo, for example, was archived in 2023.
2. **Read the reward table on that page**, not an aggregator's summary of it.
3. **Size the real payout.**
   - A percent-of-funds Critical depends on TVL. Alto pays 10% of funds affected, with a $20k
     floor and a $100k cap; at ~$55k TVL every Critical lands on the floor. TVL comes from
     `https://api.llama.fi/tvl/<slug>`.
   - Token-denominated rewards need a price, and a lockup cuts their value (Agave pays SOL
     locked for 12 months). Prices come from
     `https://api.coingecko.com/api/v3/simple/price?ids=solana,ethereum,bitcoin&vs_currencies=usd`.
4. **Look for time-limited multipliers.** Fork windows (EF Glamsterdam) and boosts (Firedancer)
   can double or triple a payout for a few weeks.
5. **Check the track record.** Published payouts (Safe's past-bounties page, Curve's $250k) are a
   good sign. Discretionary rewards with public disputes (Ledger, Parity) are not.
6. **Read a portal's config API, not its landing page.** `bounty.anza.xyz/api/config` gave the
   burn amount and the programs in scope when the page text was ambiguous.

Red flags: rewards paid in the project's own new token, discretionary-only rewards, unpublished
amounts, and policy pages untouched for years.

## Reporting in the AI-flood era (late 2026)

- Solana portals now charge a SOL burn per report (Anza 0.5 SOL, Firedancer 1 SOL) because
  AI-generated reports swamped triage.
- Firedancer's 2026-04-09 to 2026-05-09 contest drew 809 reports from 162 researchers and
  produced 127 unique findings. Nearly half of the valid reports were duplicates.
- EF warns that replies can take up to a week.

So: report fast (on a duplicate, the first report wins), one finding per report, and an
end-to-end PoC on a local fork, local cluster or testnet. Never test on mainnet.

With no platform to appeal to, keep timestamped evidence of what was sent and when, and get the
severity and payout decision in writing.

## Gotchas

- From my Indonesian home ISP, kraken.com refuses connections (ECONNREFUSED), so agent web
  fetches fail. Use web search snippets or a VPN.

## Sources

- [Lissy93 independent programs list](https://github.com/Lissy93/bug-bounties/blob/main/independent-programs.yml)
- [CoinDesk: Solana bug reports now cost 1 SOL](https://www.coindesk.com/tech/2026/10/27/solana-bug-reports-now-cost-1-sol-as-ai-floods-security-teams-with-spam)
- [Alto bug bounty](https://github.com/altomoney/security-review-engagements)
- [Anza bounty portal](https://bounty.anza.xyz/)

Related: [[2026-11-17-independent-web3-bounty-shortlist]], [[index]]
