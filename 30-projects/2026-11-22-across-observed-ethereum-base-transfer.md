---
title: Across observed Ethereum-to-Base transfer
type: project
tags: [bug-bounty, web3, security, defi]
created: 2026-11-22
summary: Matches an Ethereum deposit to a finalized Base fill and confirms recipient delivery without attributing a later repayment to that individual fill.
---

# Across observed Ethereum-to-Base transfer

This passive case study reads historical public chain records only. No transactions were sent and no Across API was called. One successful transfer does not establish a general protocol invariant or a vulnerability.

| Step | Evidence |
| --- | --- |
| Ethereum deposit | [Transaction and logs](https://eth.blockscout.com/tx/0xee0f1a057177ad288e85a1d3055806d04dc84a84336b2bea006556c78d839e15): block 26347586, 2026-11-05 08:58:23 UTC. Ethereum SpokePool emitted `FundsDeposited`, deposit ID 4880919, destination 8453, and input 5000000000000000 wei. The 0.005 ETH input was wrapped into WETH on Ethereum. |
| Base fill | [Transaction and internal transfers](https://basescan.org/tx/0x27a7041101fc905dfd6254c057ee084b8755e9c58266c4de51f1513495c15a24): block 53539879, 08:58:25 UTC. The Base SpokePool emitted `FilledRelay` with origin 1 and the same deposit ID; its block was below the RPC finalized head. The two event records agree on the input and output token identifiers and amounts, recipient, depositor, and deadline. |
| Recipient delivery | Base receipt logs show 4997633755473124 wei of WETH transferred from the relayer to the Base SpokePool and unwrapped. BaseScan's internal transfer shows the pool paid the recorded recipient `0x7072ab3A...2633B29FD` exactly 0.004997633755473124 native ETH. The chain block timestamps differ by two seconds; this is not a wall-clock latency measurement. |
| Relayer reimbursement | The fill records repayment chain ID 1 (Ethereum), not Base. That is a repayment destination, not proof of payment. No executed reimbursement leaf was independently matched to this deposit ID. An aggregate payout alone would not identify this constituent fill; attribution needs bundle-level accounting evidence. The input/output difference is not evidence of the relayer's net earnings or LP fee. |

Deposit identity and final delivery are confirmed for this example. Per-fill reimbursement is unverified, not shown to be missing. It is not a High/Critical finding. The observed repayment chain differs from the assumed Base repayment in the synthetic model; the synthetic model is not a claim about this transfer.

## Related

- [[2026-11-22-across-synthetic-settlement-model]]
- [[2026-11-22-across-passive-recon-closeout]]
