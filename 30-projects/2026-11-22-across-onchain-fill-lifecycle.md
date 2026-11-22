---
title: Across on-chain fill lifecycle
type: project
tags: [bug-bounty, web3, security, defi]
created: 2026-11-22
summary: Maps standard and slow fill transitions in the Base SpokePool source, separating requested delivery from completed delivery and settlement.
---

# Across on-chain fill lifecycle

The [SpokePool source at v5.1.0](https://github.com/across-protocol/contracts/blob/v5.1.0/contracts/spoke-pools/SpokePool.sol) matches the Base registry copy for this file, as shown in [[2026-11-22-across-onchain-release-baseline]].

- A standard fast fill uses the payer's funds. A slow-fill request records `RequestedSlowFill` and does not pay the recipient; executing a slow fill requires a proof against the registered slow-fill root and uses SpokePool reserves.
- A fast fill after a slow-fill request is classified as `ReplacedSlowFill`. Fast and slow paths share a terminal `Filled` state for the relay hash. The status update and delivery are part of the same transaction; a revert in delivery rolls back the status and event.
- The legacy fast and slow paths exclude V5-tagged deposits. This is a source-level distinction, not evidence that a V5 Gateway route is enabled in the selected deployment.

A fill records destination delivery, not later reimbursement. See [[2026-11-22-across-onchain-settlement-controls]]. This read-only map establishes no High/Critical finding. Off-chain review remains paused.
