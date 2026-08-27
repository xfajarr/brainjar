---
type: gotcha
title: "Upstream stream ended before terminal chunk" — 9router routes DeepSeek via commandcode
owner: dev
tasks: []
tags: [opencode, 9router, commandcode, deepseek, streaming, proxy]
created: 2026-08-03
---

# "Upstream stream ended before terminal chunk" — 9router routes DeepSeek via commandcode

**Symptom:** In opencode, requests to `9router/cmc/deepseek/*` intermittently fail with
`CommandCode error: {"type":"server_error","message":"Upstream stream ended before terminal
chunk"}`. Looks like a DeepSeek problem, but it is not.

**Why:** The opencode config is fine — the failure is in the 9router proxy layer.
1. 9router (PID from `node .../9router`, port 20128) maps model id `cmc/deepseek/*` to the
   **`commandcode`** provider connection (the only non-grok provider connected; all grok-cli
   accounts are 429 rate-limited with backoff levels up to 15).
2. `requestDetails` in `~/.9router/db/data.sqlite` shows the pattern: requests with
   **~240K prompt tokens** (AGENTS.md + rules + long history) get `providerResponse:
   "[Empty streaming response]"` / `"[Streaming - raw response not captured]"` and the stream
   cuts mid-chunk. Success rate is ~128/131, so it is intermittent and correlated with large
   contexts.
3. 9router settings force `commandcode.mode = max` thinking, compounding latency on big
   requests.

**Fix:**
- Treat it as transient: retry the request. It is not an opencode config bug.
- Reduce context pressure: keep AGENTS.md concise, cap `tool_output` (default is unlimited),
  avoid dumping huge files into context.
- Long-term: add a direct DeepSeek provider connection in 9router (or another healthy
  provider) so `cmc/deepseek/*` does not depend on commandcode's stream reliability.
- Do not misdiagnose as a model/config problem — check `requestDetails` first.

**Also noticed:** `launchctl list com.9router.autostart` shows `LastExitStatus = 9` and the
launchd logs are empty — the tray app crashed at some point; the current 9router process is
running under a different parent, so autostart may not relaunch it after a reboot. Verify the
port after restarts.

**Status update (2026-08-27):** Both `cmc/deepseek/deepseek-v4-flash` and
`cmc/deepseek/deepseek-v4-pro` now return `400 You have insufficient credits` from
commandcode. The stream-cut issue is moot — the account is out of credits. Workaround: use
`bai/deepseek-v4-flash` (registered in omp models.yml, works). See
[[2026-08-27-omp-providers-from-opencode]] for the omp setup.
