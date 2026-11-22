---
title: Across on-chain refund lifecycle
type: project
tags: [bug-bounty, web3, security, defi]
created: 2026-11-22
summary: Distinguishes immediate payout, deferred credit, and whole-leaf reversion in the Base SpokePool refund path.
---

# Across on-chain refund lifecycle

The [Base deployment source](https://sourcify.dev/server/v2/contract/8453/0xf23C6C04A2B88e8651FE99bbDccbB5C9D306e6B0?fields=sources) separates three outcomes after a committed refund leaf is presented:

| Outcome | What the Base SpokePool records |
| --- | --- |
| Immediate payout | A successful token transfer fulfills that refund entry during leaf execution. |
| Deferred refund | A failed individual token transfer can credit the amount to the original beneficiary for that token. A later claim reads the caller's own credit, clears it before transfer, and sends the funds to the chosen recipient. A successful leaf event with deferred refunds is not a receipt for every payment. |
| Whole-leaf revert | If cumulative refunds exceed the token balance measured at the start of distribution, the call reverts. The claim mark and all changes from that execution roll back together. |

Refund distribution is distinct from the optional `amountToReturn` in the same leaf: the latter returns funds to L1 using the chain's bridge path. A leaf event is not proof that an asynchronous L1 return has completed.

The historical Ethereum WETH example in [[2026-11-22-across-historical-refund-proof]] has a successful transfer and no deferred-refund flag. It is not evidence about every refund. This is a source-only lifecycle map, not a live test or High/Critical finding. Off-chain work remains paused.

## Related

- [[2026-11-22-across-onchain-settlement-controls]]
- [[2026-11-22-across-historical-refund-proof]]
