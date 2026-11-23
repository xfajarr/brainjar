---
title: Across Base Gateway verified source
type: project
tags: [bug-bounty, web3, security, defi]
created: 2026-11-23
summary: Finds BaseScan exact-match source for the active Base Gateway implementation while leaving its mapping to audited Git history unresolved.
---

# Across Base Gateway verified source

[BaseScan](https://basescan.org/address/0x64cfa63acd51ab86ff1f4f2adb3c8da1d7d42ab0#code) marks the Gateway implementation at `0x64cfa63acd51ab86ff1f4f2adb3c8da1d7d42ab0` as **Source Code Verified, Exact Match** and exposes `src/Gateway.sol` and compiler input. The explorer reports Solidity `v0.8.33+commit.64118f21`, optimization enabled with 2,000,000 runs, and the Prague EVM target. This is explorer verification, not an independent rebuild.

This resolves the source availability gap from [[2026-11-22-across-base-gateway-binding]]: Sourcify's no-match result was registry-specific. The address is the same implementation observed when checking the historical fill in [[2026-11-22-across-observed-base-v5-fill]].

The [OpenZeppelin Contracts V5 audit](https://www.openzeppelin.com/news/contracts-v5-audit) names commit `f9578f0`. Its GitHub tree and the raw Gateway file at that revision returned 404 without authentication. No mapping between the BaseScan source and that audit revision has been established. Use BaseScan for deployed source and the audit for documented scope, not as interchangeable evidence. This was a read-only comparison, not a High/Critical finding. Off-chain work remains paused.
