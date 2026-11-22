---
title: Across observed settlement bundle and aggregate reimbursement
type: project
tags: [bug-bounty, web3, security, defi]
created: 2026-11-22
summary: Links an Across refund root to its executed WETH payment and distinguishes aggregate payout evidence from attribution to an individual fill.
---

# Across observed settlement bundle and aggregate reimbursement

This follow-up reads historical Ethereum logs only. The earlier [[2026-11-22-across-observed-ethereum-base-transfer]] verified delivery for deposit 4880919 and identified repayment on Ethereum for relayer `0x20c58c01cc73da90d5ca4dfb4357228c636422d6`.

| Evidence | What it establishes |
| --- | --- |
| [HubPool proposal](https://eth.blockscout.com/tx/0xb436cfb1574551c2003d49721edb39c9918baf0b934aed1e24c0da4967069177), Ethereum block 26347710 (09:23:11 UTC on 2026-11-05) | `ProposeRootBundle` committed refund root `0x3ec476c3b30a4990c8de90943a96eb4e8bfcc793881eb99ee6a3b82eb386b673`. The previous proposal's array value at index 6 was 53539422; this proposal's was 53540322. The Base fill block 53539879 lies between them. |
| [Historical chain configuration](https://eth.blockscout.com/tx/0x2c63979d6105ce67d2ba4652f11e337a29d84380fefdb2ffe64b71b9ecd875c2) | The latest `CHAIN_ID_INDICES` update returned by the historical event query places Base (8453) at zero-based index 6. The observed list has 30 entries and the proposal array has 31; the full ordering and preceding finalized range were not independently reconciled. Treat the block bracket as a candidate, not proof of inclusion. |
| [Bundle execution and payout](https://eth.blockscout.com/tx/0x352dfc071a115601658ccd343b766d0891d68a9af51d0b478bec02d01dced4c3), block 26347864 (09:54:11 UTC) | The Ethereum SpokePool emitted `RelayedRootBundle` for the same refund root, locally numbered 30766. `ExecutedRelayerRefundRoot` for leaf 1 lists the relayer and 208151214821557928 wei of WETH, with no deferred refunds. A WETH `Transfer` in the same receipt confirms that exact amount left the Ethereum SpokePool for the relayer. |

The proposal-to-execution link and aggregate payout are confirmed. The refund leaf has no deposit ID for the constituent fills. [UMIP-179](https://github.com/UMAprotocol/UMIPs/blob/master/UMIPs/umip-179.md) describes aggregation by repayment chain, token, and relayer. Without authenticated per-fill bundle data or independent reconstruction against the committed root, the share attributable to deposit 4880919 remains unverified. This is not evidence of missing repayment or a High/Critical issue.

## Related

- [[2026-11-22-across-observed-ethereum-base-transfer]]
- [[2026-11-22-across-passive-recon-closeout]]
