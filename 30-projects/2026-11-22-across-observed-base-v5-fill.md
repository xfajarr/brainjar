---
title: Across observed Base V5 fill
type: project
tags: [bug-bounty, web3, security, defi]
created: 2026-11-22
summary: Verifies a historical V5 fill on Base through the Gateway, SpokePool witness, and USDC delivery in one receipt.
---

# Across observed Base V5 fill

[Base transaction `0x5638cd59...b9a35`](https://basescan.org/tx/0x5638cd59056de5ed2dd73241af7424bf56e2dd221c9afb04b04d4d244dcb9a35) succeeded at block `52217882`. Its top-level call targeted the Gateway proxy from [[2026-11-22-across-base-gateway-binding]]. The receipt contains a `FilledRelay` log from the Base SpokePool and a Gateway log with the same step ID as the transaction input.

The fill's `messageHash` exactly matches `keccak256(V5_MAGIC_PREFIX || stepId)` as defined by the [SpokePool source](https://github.com/across-protocol/contracts/blob/v5.1.0/contracts/spoke-pools/SpokePool.sol). The USDC logs show `525835006` base units passing from the relayer to the intermediate recipient and then to the final address in that same receipt. A historical read of the Gateway proxy's ERC-1967 slot at the transaction block returned the same implementation address, `0x64cfa63acd51ab86ff1f4f2adb3c8da1d7d42ab0`, as the later snapshot.

This establishes historical use on Base, not merely deployment. It does not establish when the route first opened, current activity across routes, reimbursement, or verified Gateway source identity. No live test or transaction submission took place. No High/Critical finding is established. Off-chain work remains paused.

## Related

- [[2026-11-22-across-base-gateway-binding]]
- [[2026-11-22-across-onchain-fill-lifecycle]]
