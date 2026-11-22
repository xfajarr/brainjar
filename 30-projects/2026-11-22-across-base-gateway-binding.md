---
title: Across Base Gateway binding
type: project
tags: [bug-bounty, web3, security, defi]
created: 2026-11-22
summary: Confirms Base SpokePool references a deployed Gateway proxy, without establishing route activation or the Gateway source revision.
---

# Across Base Gateway binding

At [finalized Base block 60745935](https://basescan.org/block/60745935) (2026-11-22 09:26:02 UTC), two public RPCs agreed on the SpokePool proxy implementation (`0xf23c6c04a2b88e8651fe99bbdccbb5c9d306e6b0`) and its `gateway()` value (`0x998d7c178a1f9607b47ea3a8e22426451b3ce707`). Both returned the same nonempty Gateway proxy runtime. Reading the Gateway's ERC-1967 implementation slot at the same block returned `0x64cfa63acd51ab86ff1f4f2adb3c8da1d7d42ab0`.

The Gateway reference is configured on Base, not just present in the release source. This does not establish route activation, actual V5 fill activity, or audited build identity. Sourcify returned no match for the Gateway implementation; a source match elsewhere was not established. The check used read-only public state, sent no transaction, and established no High/Critical finding. Off-chain review remains paused.

## Related

- [[2026-11-22-across-onchain-fill-lifecycle]]
- [[2026-11-22-across-onchain-release-baseline]]
