---
title: Across passive recon closeout
type: project
tags: [bug-bounty, web3, security, defi]
created: 2026-11-22
summary: Closes the public-source Across recon and separates the Ethereum/Base baseline from off-chain components that lack production build evidence.
---

# Across passive recon closeout

The public-source recon pass is complete as of 2026-11-22. This is not an exhaustive system audit, proof of security, or a vulnerability report. No active probes, contract transactions, or messages to Across were sent.

| Component | Closeout status |
| --- | --- |
| Ethereum HubPool and Base SpokePool | Documented deployment and registry source baseline. The exact Git revisions were not established. See [[2026-11-22-across-ethereum-base-deployment]]. |
| Dataworker, Relayer, Finalizer | Production source and build remain unverified. The `relayer`, `relayer-v2`, and `relayer-v3` GitHub URLs returned unauthenticated 404s, including the historical paths referenced in Across and UMA docs. This does not determine whether the repo is private, moved, or retired. The queried `@across-protocol/relayer/latest` npm metadata endpoint also returned 404. |
| Shared SDK and contracts packages | npm metadata returned `@across-protocol/sdk` 4.5.1 and `@across-protocol/contracts` 5.1.0 as `latest`; the indexed relayer snapshot showed SDK 4.4.8 and contracts 5.0.22. Registry tags and historical lockfiles do not prove what an operator runs or what is deployed on-chain. |
| V5 Gateway and Solana V5 | Audit and prerelease evidence only for the source/build mapping in this recon; no verified mapping to eligible current deployments. |
| API, frontend, and Toolkit | `/v1` is documented. Production source is not mapped to `quotes-api`, `frontend-v1`, or Toolkit. npm metadata for `@across-protocol/app-sdk` 0.6.1 points to Toolkit, not to the production API implementation. |

The invariant model distinguishes on-chain fill state, once-per-leaf execution, and once-per-fill accounting across bundles. The third is not verified against a production Dataworker build. The [program rules](https://docs.across.to/introduction/bug-bounty) exclude invalid bundles that only pass because nobody challenges them. No High or Critical issue was established in this pass.

The [official security contact](https://across.to/.well-known/security.txt) is `bugs@across.to`. The Dataworker scope question is recorded but unsent in [[2026-11-22-across-synthetic-settlement-model]]. Reopen that workstream on first-party source or build confirmation. Keep all earlier notes as dated evidence; this closeout does not replace them.

## Related

- [[2026-11-22-across-recon-deployment-and-settlement]]
- [[2026-11-22-across-bounty-scope-and-invariants]]
