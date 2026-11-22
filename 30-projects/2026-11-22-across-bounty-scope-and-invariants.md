---
title: Across Protocol bounty scope and fund invariants
type: project
tags: [bug-bounty, web3, security, defi]
created: 2026-11-22
summary: Tracks Across repositories by scope confidence and records the authorization, fill, settlement, and refund invariants for a High/Critical-only review.
---

# Across Protocol bounty scope and fund invariants

The [Across bug bounty](https://docs.across.to/introduction/bug-bounty) accepts private reports directly at `bugs@across.to`. This review tracks **High ($10,000) and Critical (up to $1,000,000)** only. Across rates impact and likelihood; failure to uphold an invariant does not establish severity by itself. No vulnerabilities are identified in this note.

## Repository inventory

| Repository | Role | Review status |
| --- | --- | --- |
| [`across-protocol/relayer`](https://github.com/across-protocol/relayer) | Relayer, Dataworker, and Finalizer | Off-chain priority. The README describes V3; confirm which components and exact versions remain deployed before review. |
| [`across-protocol/contracts`](https://github.com/across-protocol/contracts) | HubPool, SpokePool, chain adapters, and SVM programs | Core protocol and on-chain reference for the off-chain review. Map the relevant release or commit to the deployed component. |
| [`across-protocol/sdk`](https://github.com/across-protocol/sdk-v3) | Shared relayer logic, including LP fee calculations | Supporting dependency only; check the exact version used by the active component. This is not the App SDK in `toolkit`. |
| `across-protocol/contracts-v5` | V5 Gateway and Executor above the core protocol | Candidate named in the [OpenZeppelin audit](https://www.openzeppelin.com/news/across-protocol-v5-bridging-system-audit). Source access and deployed commit are unverified. |
| [`across-protocol/quotes-api`](https://github.com/across-protocol/quotes-api) | Public quote API repository | On hold; not verified to back the current Across API `/v1`. |
| [`across-protocol/frontend-v1`](https://github.com/across-protocol/frontend-v1) | Legacy frontend repository | On hold; not verified to represent the current production frontend. |
| [`across-protocol/toolkit`](https://github.com/across-protocol/toolkit) | App SDK and integration examples | On hold; the [current migration guide](https://docs.across.to/introduction/migration-guide) describes the legacy API replacement. Do not infer current production use from the public repo. |

Start with `relayer` plus `contracts`, once the relevant Dataworker version is confirmed. Keep the other repos in the inventory without treating unverified or legacy code as eligible.

## Fund invariants

1. **Order authorization:** Execution stays within the paths, chains, assets, recipients, and amounts the user authorized.
2. **Fill integrity:** A fill recognized for settlement corresponds to a valid deposit and satisfies its intent parameters. A successful delivery respects the committed minimum output where applicable.
3. **No duplicate economic claims:** An eligible fill is counted once for reimbursement. Fill and user refund do not generate two claims on the same escrowed funds.
4. **Settlement accounting:** A bundle reflects the correct chain activity and block ranges. Reimbursements, fees, and rebalancing reconcile to valid obligations without creating an unbacked claim.
5. **Refund entitlement:** For refundable routes, refund eligibility, asset, chain, and recipient follow the route's terms. Irreversible routes are not required to offer automatic refunds.

See [root bundle validation](https://legacy.docs.across.to/introduction/security/validating-root-bundles) for the documented settlement model, the [V5 audit](https://www.openzeppelin.com/news/across-protocol-v5-bridging-system-audit) for execution authorization, and the [refund policy](https://docs.across.to/introduction/refunds) for route-specific behavior.

## High/Critical filter

Prioritize unauthorized movement or loss of funds, invalid payouts with material financial impact, and lasting freezes on eligible deployed code. Exclude known issues, temporary delays without loss or lasting freeze, assumed compromise or misbehavior of trusted parties, and scenarios that require an invalid bundle to pass the challenge period undisputed. Confirm scope and version before any work; the initial review is read-only.

## Related

- [[2026-11-17-independent-web3-bounty-shortlist]]
- [[2026-11-17-finding-independent-bounty-programs]]
