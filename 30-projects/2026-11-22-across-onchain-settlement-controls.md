---
title: Across on-chain settlement controls
type: project
tags: [bug-bounty, web3, security, defi]
created: 2026-11-22
summary: Maps settlement timing, cross-chain root authentication, and refund execution controls in the Ethereum HubPool and Base SpokePool registry source.
---

# Across on-chain settlement controls

This source-only control map uses the Ethereum HubPool and Base SpokePool deployment references in [[2026-11-22-across-onchain-release-baseline]]. No live testing was performed.

| Stage | On-chain control | What it establishes |
| --- | --- | --- |
| Ethereum HubPool: `executeRootBundle` | Requires liveness to have elapsed, checks the pool rebalance Merkle proof and whether the leaf is already claimed, and uses the configured adapter and SpokePool for the destination. | The executed rebalance leaf is part of the proposed commitment and is consumed once in that proposal. It does not recompute cross-chain settlement accounting. |
| Base SpokePool: `relayRootBundle` | Admin-only root registration. The `Ovm_SpokePool` override authenticates the cross-domain sender through the messenger against the configured cross-domain admin. | Only the authorized cross-domain administration path can register a root. The SpokePool assigns a local bundle ID. |
| Base SpokePool: `executeRelayerRefundLeaf` | Checks the leaf's chain ID, verifies inclusion in the selected local bundle's refund root, and records the claim before distributing the refunds. | The claimed leaf is committed for this chain and can be consumed once per local bundle and leaf ID. This is not a guarantee of one reimbursement per fill across bundles. |

Keep on-chain and off-chain guarantees separate. Merkle proofs authenticate committed data, not the economic correctness of each constituent fill. The [bounty policy](https://docs.across.to/introduction/bug-bounty) excludes scenarios that require an invalid bundle to pass without a challenge. No vulnerability or High/Critical issue is established by this map. Off-chain work remains paused.

## Related

- [[2026-11-22-across-onchain-release-baseline]]
- [[2026-11-22-across-historical-refund-proof]]
