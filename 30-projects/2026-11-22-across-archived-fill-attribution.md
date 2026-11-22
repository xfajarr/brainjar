---
title: Across archived fill attribution
type: project
tags: [bug-bounty, web3, security, defi]
created: 2026-11-22
summary: Matches a public Across bundle archive to the observed Base fill and reconciles its contribution to an Ethereum WETH payout.
---

# Across archived fill attribution

This historical reconciliation extends [[2026-11-22-across-observed-settlement-bundle]] with per-fill evidence. No transactions or protocol endpoint calls were made.

- The published `@across-protocol/sdk` 4.5.1 package documents bundle archive lookup by the Ethereum end block. The SDK's default archive owner and topic `bundles-26347665` yielded [two](https://arweave.net/XcmFebvxeTFzFUlc4vyPseoLL2ybbAvvkS00-bh4-MQ) [records](https://arweave.net/ond4UGZ85YaT1aV4vvSS-dgE6hBqbikcBLT8x52V6WA) with identical payload SHA-256 `f1fede6d6f7a00c1b1b5236635b2b2aec04bb1e6cf1189bc875fce87165f2267`. These are two copies of one dataset, not independent attestations. The npm package is evidence of the lookup convention, not the production build identity.
- The archive records 31 chain ranges, including Ethereum `[26347516, 26347665]` and Base `[53539423, 53540322]`. Under Ethereum repayment and WETH, it lists deposit `4880919` (origin 1) with the exact Base transaction hash, fill block 53539879, and relayer from [[2026-11-22-across-observed-ethereum-base-transfer]].
- The archival record provides a zero LP fee for this fill. Applying the [UMIP-179](https://github.com/UMAprotocol/UMIPs/blob/master/UMIPs/umip-179.md) repayment formula (multiply then integer-divide per fill) to the five entries for that relayer gives 208151214821557928 wei of WETH in total. Deposit `4880919` contributes 5000000000000000 wei (0.005 WETH). The five-fill sum matches the archived refund amount, the executed leaf entry, and the [Ethereum WETH transfer](https://eth.blockscout.com/tx/0x352dfc071a115601658ccd343b766d0891d68a9af51d0b478bec02d01dced4c3).

This reconciles the individual contribution using the public archive and the observed payout. It is not an independent reconstruction of the entire refund Merkle root, validation of historical fee inputs, confirmation of the production Dataworker build, or a High/Critical finding.
