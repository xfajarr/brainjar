---
title: Across Ethereum to Base synthetic settlement model
type: project
tags: [bug-bounty, web3, security, defi]
created: 2026-11-22
summary: Models normal delivery, depositor refund, and once-per-leaf settlement with synthetic amounts, separating lifecycle guarantees from unverified production accounting.
---

# Across Ethereum to Base synthetic settlement model

This is a paper model for recon, not a simulation, live test, or vulnerability finding. Use one hypothetical refundable Ethereum-to-Base route and one deposit. All quantities and fee assumptions are illustrative, not Across fee quotes.

| Case | Before settlement | Expected payment and beneficiary | What the model establishes |
| --- | --- | --- | --- |
| Valid fill | User escrows 100 units on Ethereum. Relayer delivers the agreed 98 units to the recipient on Base. Assume a 1-unit LP fee and an eligible repayment on Base. | One reimbursement of 99 units to the relayer on Base; no expiry refund to the depositor for this filled order. | Delivery to the user and repayment to the relayer are two different transfers, not duplicate claims. |
| Expiry without a fill | User escrows 100 units on Ethereum. No eligible fill occurs before the deadline. Assume a route with a full origin refund and no deductions. | One refund of 100 units to the entitled refund recipient on Ethereum; zero reimbursement because no relayer filled. | An expired, refundable order has a different obligation from a filled order. This is not a guarantee of refund for every route. |
| An already paid refund leaf | The valid 99-unit relayer reimbursement leaf from case one has already executed on Base. | Zero additional payment for the same leaf in its bundle. Cumulative repayment from that leaf remains 99 units. | Once-per-leaf execution does not prove that the same fill cannot be counted in a different bundle. That requires separate accounting evidence. |

## Evidence boundaries

- **Base SpokePool:** The selected registry source is the reference for local fill status and claimed-leaf tracking. See [[2026-11-22-across-ethereum-base-deployment]]. This model does not execute either path.
- **Dataworker:** The deployed build and dependency version are still unverified. Use [UMIP-179](https://github.com/UMAprotocol/UMIPs/blob/master/UMIPs/umip-179.md) as a specification reference, not evidence that a production build implements it exactly. Cross-bundle deduplication remains unverified in this review.
- **Refunds and challenge:** Apply the [route refund policy](https://docs.across.to/introduction/refunds) and [bounty scope](https://docs.across.to/introduction/bug-bounty). An invalid bundle that only succeeds if nobody challenges it is excluded. Keep High/Critical only; this model assigns no severity.

## Scope question for Across (not sent)

Which repository and release, commit, or build reference should researchers use for the currently deployed, bounty-eligible Dataworker? If the source is not public, is there an official scope or provenance reference? Do not infer production identity from an indexed copy of `master`.

## Related

- [[2026-11-22-across-ethereum-base-deployment]]
- [[2026-11-22-across-recon-deployment-and-settlement]]
