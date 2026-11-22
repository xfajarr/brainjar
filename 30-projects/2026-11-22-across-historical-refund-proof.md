---
title: Across historical refund proof
type: project
tags: [bug-bounty, web3, security, defi]
created: 2026-11-22
summary: Verifies the historical WETH refund leaf against the HubPool commitment and distinguishes leaf inclusion from individual fill attribution.
---

# Across historical refund proof

The WETH refund leaf for Ethereum bundle `30766`, leaf `1`, is independently verified against the HubPool refund root using the already executed transaction's data. No new proof was generated, no transaction was sent, and no live behavior was tested.

| Check | Result |
| --- | --- |
| Executed leaf | The [historical execution transaction](https://eth.blockscout.com/tx/0x352dfc071a115601658ccd343b766d0891d68a9af51d0b478bec02d01dced4c3) contains the leaf and its two proof siblings in an Ethereum SpokePool subcall. The leaf contains six refund entries, including `208151214821557928` wei of WETH for the relayer identified in the preceding notes. |
| Leaf hash | `0xec080db7178adc64feae058f2d3f5493eee355c362e920f194d15955708bc9fc` |
| Recomputed root | `0x3ec476c3b30a4990c8de90943a96eb4e8bfcc793881eb99ee6a3b82eb386b673`, an exact match to the [HubPool proposal's](https://eth.blockscout.com/tx/0xb436cfb1574551c2003d49721edb39c9918baf0b934aed1e24c0da4967069177) refund root. |

The proof authenticates the aggregate leaf, not a direct mapping from deposit ID to reimbursement. The `0.005 WETH` contribution of deposit `4880919` is separately supported by the archived fill records and their arithmetic reconciliation. This does not reconstruct all leaves, verify fee inputs independently, or verify production Dataworker behavior across bundles. No High/Critical finding is established. The historical sample is complete at this evidence level.

## Related

- [[2026-11-22-across-archived-fill-attribution]]
- [[2026-11-22-across-observed-settlement-bundle]]
