---
title: Across reported scope clarification
type: project
tags: [bug-bounty, web3, security, defi]
created: 2026-11-22
summary: Records the reported latest-version guidance and separates the public contracts release from unresolved Dataworker source access.
---

# Across reported scope clarification

The researcher reports that Across directed them to the available repositories and the latest version. The original reply was not reviewed here. The guidance does not name a Dataworker repository, identify a production build, or say whether a prerelease qualifies.

- The public [contracts release](https://github.com/across-protocol/contracts/releases/tag/v5.1.0) was returned as latest by GitHub. The npm `latest` package is also `@across-protocol/contracts` 5.1.0. Use this as the public release baseline, not as proof of deployed bytecode on every chain. Keep the previous chain-specific versions as historical evidence.
- The public GitHub URLs for `relayer`, `relayer-v3`, `sdk`, `contracts-v5`, and `solana-v5` still returned 404 without authentication. This does not prove those repositories are absent or out of scope. If the original reply included a current Dataworker link, use that as the access reference rather than guessing from an indexed copy.
- Keep the existing [published bounty exclusions](https://docs.across.to/introduction/bug-bounty) and the High/Critical filter. No vulnerability or severity is established by the clarification.

## Related

- [[2026-11-22-across-passive-recon-closeout]]
- [[2026-11-22-across-ethereum-base-deployment]]
