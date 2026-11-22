---
title: Across Ethereum HubPool and Base SpokePool deployment evidence
type: project
tags: [bug-bounty, web3, security, defi]
created: 2026-11-22
summary: Pins public verification evidence for the Ethereum HubPool and Base SpokePool, and separates fill, reimbursement, and challenge ownership.
---

# Across Ethereum HubPool and Base SpokePool deployment evidence

Passive snapshot on 2026-11-22. Only public explorer, verification registry, and GitHub records were read; no protocol endpoints were probed and no transactions were sent. This is scope and architecture evidence, not vulnerability evidence.

| Asset | Version evidence | Still open |
| --- | --- | --- |
| Ethereum HubPool | The [tagged manifest](https://github.com/across-protocol/contracts/blob/v5.0.28/broadcast/deployed-addresses.json) and [Sourcify](https://sourcify.dev/server/v2/contract/1/0xc186fA914353c44b2E33eBE05f21846F1048bEda?fields=compilation,deployment,proxyResolution) agree on address and block 14819537. Sourcify identifies deployed `HubPool`, Solidity 0.8.13, partial source match (`match`), and no proxy. Blockscout also lists no proxy and a partial match. Etherscan labels an adapter as an implementation; do not substitute the adapter for HubPool source. | The registry's `HubPool.sol` does not equal the corresponding file at `v5.0.28` or at the historical GitHub snapshot checked. Exact Git revision unverified. |
| Base SpokePool | The manifest and [Sourcify proxy record](https://sourcify.dev/server/v2/contract/8453/0x09aea4b2242abC8bb4BB78D537A67a245A7bEC64?fields=proxyResolution,deployment) agree on address and creation block 2164878. Sourcify identifies an exact-matched ERC-1967 proxy and resolves implementation `0xf23C6C04A2B88e8651FE99bbDccbB5C9D306e6B0`, agreeing with BaseScan. The creation receipt identifies a different initial implementation; it is not the current source reference. | The [implementation record](https://sourcify.dev/server/v2/contract/8453/0xf23C6C04A2B88e8651FE99bbDccbB5C9D306e6B0?fields=compilation,deployment) identifies `OP_SpokePool`, Solidity 0.8.30, and partial match. Its registry copies of `OP_SpokePool.sol` and `SpokePool.sol` match `v5.0.28` after newline normalization. This compares two files, not the full build or exact Git revision. |

## Invariant ownership

- **Fill integrity:** Destination SpokePool records fill status and enforces the local fill lifecycle. Dataworker, not the destination status alone, reconciles deposit and fill evidence across chains to determine settlement eligibility.
- **Reimbursement once:** Dataworker accounting determines which obligations go into a bundle. The destination SpokePool tracks execution of claimed refund leaves; once-per-leaf execution is not the same as proof of once-per-fill accounting across bundles.
- **Settlement accounting:** The HubPool hosts bonded proposals, disputes, and execution; the validity of the full cross-chain accounting is independently recomputed off-chain under the [documented validation model](https://legacy.docs.across.to/introduction/security/validating-root-bundles). The [bounty policy](https://docs.across.to/introduction/bug-bounty) excludes cases that only work if an invalid bundle goes unchallenged, and delays without loss or lasting freeze. Keep High/Critical only.

Use the chain-specific verification registry source for this snapshot. Do not replace it with the latest tag by assumption. Off-chain production Dataworker identity remains unverified.

## Related

- [[2026-11-22-across-recon-deployment-and-settlement]]
- [[2026-11-22-across-bounty-scope-and-invariants]]
