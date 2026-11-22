---
title: Across recon - deployment evidence and settlement boundaries
type: project
tags: [bug-bounty, web3, security, defi]
created: 2026-11-22
summary: Separates verified Across source and release evidence from production unknowns, and maps the boundaries that protect order authorization, settlement, and refunds.
---

# Across recon: deployment evidence and settlement boundaries

This is a passive, public-source review. It supersedes the initial review order in [[2026-11-22-across-bounty-scope-and-invariants]] because live source availability is different from indexed visibility. No vulnerabilities are claimed. Continue saving material evidence, corrections, and unresolved scope questions to Brainjar as new notes.

## Source and deployment evidence

| Component | Evidence | What it does not establish |
| --- | --- | --- |
| `contracts` | The public [v5.0.28 tag](https://github.com/across-protocol/contracts/releases/tag/v5.0.28) serves its [deployment manifest](https://github.com/across-protocol/contracts/blob/v5.0.28/broadcast/deployed-addresses.json). The manifest contains 42 chain records, including test networks, and labels Ethereum HubPool/SpokePool and Base SpokePool artifacts. | 42 entries are not 42 live routes. An address and deployment block do not identify the current implementation of an upgradeable proxy. A release tag alone does not prove the tag is deployed. |
| `relayer`, `sdk`, `sdk-v3`, `quotes-api` | Search indexes showed historical GitHub snapshots. Direct unauthenticated GitHub page and API checks returned 404 for `relayer`, `sdk`, and `quotes-api`; the `sdk-v3` GitHub page also returned 404. The indexed relayer lockfile contained SDK 4.4.8 and contracts 5.0.22. | Neither an indexed snapshot nor a 404 establishes the currently operated Dataworker version, why the source is unavailable, or the deployment behind the current API. |
| `contracts-v5` | [OpenZeppelin's V5 audit](https://www.openzeppelin.com/news/contracts-v5-audit) identifies Gateway and Executor source at an audit snapshot. Direct unauthenticated GitHub page and API checks returned 404. | An audit snapshot does not establish current source access, live build identity, or complete coverage. |
| `solana-v5` | The [contracts release notes](https://github.com/across-protocol/contracts/releases) reference a separate Gateway/adapter repository. Its GitHub page and API returned 404. The `v6.0.0-beta.5` release explicitly performs no deployment or route activation. | Historical Solana support and a beta package do not prove SVM V5 is active. |
| `frontend-v1`, `toolkit` | GitHub pages/API are accessible; [current API docs](https://docs.across.to/api-reference) distinguish `/v1` from the legacy Swap API. | No evidence ties these repos to the current production frontend or backend. Hold both. |

## Where the invariants live

1. **Authorization and delivery:** The [V5 bridging audit](https://www.openzeppelin.com/news/across-protocol-v5-bridging-system-audit) describes V5 as a Gateway/Executor layer over the core bridge, not a replacement for settlement. The Gateway authenticates the order and validates the chosen path against the user's commitment; the settlement core has a separate job. An API response accepting an authorization is not evidence that funds arrived.
2. **Fill and reimbursement:** SpokePool records a fill on the destination, while the Dataworker reconciles eligible fills with deposits and calculates reimbursements. Destination delivery and reimbursement of the relayer are separate outcomes.
3. **Bundle accounting:** [UMIP-179](https://github.com/UMAprotocol/UMIPs/blob/master/UMIPs/umip-179.md) defines the V3 settlement rules, retaining specified sections of [UMIP-157](https://github.com/UMAprotocol/UMIPs/blob/master/UMIPs/umip-157.md). The bonded bundle commits to separate roots for reimbursements, rebalancing, and slow fills. The [validation model](https://legacy.docs.across.to/introduction/security/validating-root-bundles) has independent validators recompute the roots from chain events and dispute mismatches. EVM block ranges and SVM slot ranges are distinct inputs; an eligible SVM end slot must have a produced block. These are specification rules, not proof of behavior for every V5 route.
4. **Refund and finality:** [Refunds](https://docs.across.to/introduction/refunds) depend on the route. A quoted refund ETA is indicative; some rails do not support automatic refunds. A transfer can be marked terminal before its chain state is finalized; a completed transfer with destination actions does not by itself establish that each action succeeded. Use [tracking semantics](https://exclusive.docs.across.to/introduction/tracking-deposits) and [destination-action outcomes](https://preview.docs.across.to/introduction/quickstart/destination-actions) to distinguish these states.

## Review order and boundaries

Use the public `contracts` tag and manifest as the first architecture baseline. Establish the relevant chain, contract or program family, and verified implementation/version before treating it as an active bounty asset. Keep off-chain Dataworker review on hold until the responsible team or public build provenance confirms the active source and version; do not substitute indexed `master`. Do not treat a beta or a legacy integration as an eligible deployment. The [bounty policy](https://docs.across.to/introduction/bug-bounty) excludes invalid bundles that only succeed if nobody challenges them, assumed trusted-party compromise or misbehavior, and temporary delays without loss or lasting freeze. Keep High/Critical only.

## Related

- [[2026-11-22-across-bounty-scope-and-invariants]]
- [[2026-11-17-independent-web3-bounty-shortlist]]
