---
title: Across on-chain release baseline
type: project
tags: [bug-bounty, web3, security, defi]
created: 2026-11-22
summary: Compares the latest stable contracts release to registry source for the Ethereum HubPool and Base SpokePool while keeping off-chain work paused.
---

# Across on-chain release baseline

The public [v5.1.0 release](https://github.com/across-protocol/contracts/releases/tag/v5.1.0) resolves to commit `fc2edbd3dc417ab6e640e2925e745da7b469aaa6`. The [GitHub comparison](https://github.com/across-protocol/contracts/compare/v5.0.28...v5.1.0) lists 8 commits and 64 changed paths, none ending in `.sol`. The release notes describe dependency, packaging, and tooling changes. This does not establish identical build artifacts or deployment.

| Component | Source comparison | Baseline decision |
| --- | --- | --- |
| Base SpokePool | Sourcify still resolves the proxy to `OP_SpokePool` at `0xf23C6C04A2B88e8651FE99bbDccbB5C9D306e6B0`. The registry copies of `OP_SpokePool.sol` and `SpokePool.sol` equal the `v5.1.0` files after CRLF normalization. | Use the registry as deployment evidence and the tag for these matching source files; no full build match established. |
| Ethereum HubPool | The Sourcify `contracts/HubPool.sol` does not equal the tag's `contracts/hub-pool/HubPool.sol`, even after CRLF normalization. | Keep the registry source as the deployment baseline; exact Git revision unverified. |

There is no new Solidity delta to investigate between these tags based on the comparison. Continue distinguishing the public release from the associated deployed implementation, and keep the Dataworker, Relayer, Finalizer, API, and frontend parked. This was a public source comparison; no live contract calls or vulnerability findings.

## Related

- [[2026-11-22-across-onchain-focus]]
- [[2026-11-22-across-ethereum-base-deployment]]
