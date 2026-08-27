---
title: omp setup — custom providers from opencode + env-var keys
type: resource
owner: dev
tasks: []
tags: [omp, opencode, 9router, models, env-vars, harness]
created: 2026-08-27
---

# omp setup — custom providers from opencode + env-var keys

## What

`omp` (can1357/oh-my-pi, v18.0.6 via `brew install can1357/tap/omp`) is the
coding agent on this machine. Its custom OpenAI-compatible providers were
copied from the opencode global config (`~/.config/opencode/opencode.json`)
into `~/.omp/agent/models.yml`, then hardened: mimo removed, grok bumped to
4.6, and every API key moved from the config file into env vars.

## Models.yml

All providers speak `openai-completions` (opencode used
`@ai-sdk/openai-compatible`). `apiKey` is an **env-var name**, resolved by omp
at runtime (literal fallback if the var is unset — keep keys out of the file).

```yaml
# ~/.omp/agent/models.yml
providers:
  9router:            # local proxy 127.0.0.1:20128
    baseUrl: http://127.0.0.1:20128/v1
    api: openai-completions
    apiKey: OMP_9ROUTER_API_KEY
    models:
      - id: gcli/grok-4.6
        input: [text, image]
      - id: cmc/deepseek/deepseek-v4-flash
        reasoning: true
        contextWindow: 1000000
        maxTokens: 384000
        input: [text, image]
      - id: cmc/deepseek/deepseek-v4-pro
        reasoning: true
        contextWindow: 1000000
        maxTokens: 384000
        input: [text, image]
  gmi:                # https://api.gmi-serving.com/v1
    baseUrl: https://api.gmi-serving.com/v1
    api: openai-completions
    apiKey: OMP_GMI_API_KEY
    models:
      - id: MiniMaxAI/MiniMax-M3
        reasoning: true
        input: [text, image]
  tokenrouter:        # https://api.tokenrouter.com/v1
    baseUrl: https://api.tokenrouter.com/v1
    api: openai-completions
    apiKey: OMP_TOKENROUTER_API_KEY
    models:
      - id: qwen/qwen3.8-max-free
        reasoning: true
        input: [text]
  bai:                # https://api.b.ai/v1
    baseUrl: https://api.b.ai/v1
    api: openai-completions
    apiKey: OMP_BAI_API_KEY
    models:
      - id: deepseek-v4-flash          # opencode default model
        reasoning: true
        contextWindow: 1048576
        maxTokens: 1048576
        input: [text]
      - id: deepseek-v4-flash-vision-exp
        reasoning: true
        contextWindow: 1048576
        maxTokens: 1048576
        input: [text, image]
```

Secrets live in `~/.zshrc` (`OMP_9ROUTER_API_KEY`, `OMP_GMI_API_KEY`,
`OMP_TOKENROUTER_API_KEY`, `OMP_BAI_API_KEY`). New shells pick them up;
running shells need `source ~/.zshrc`.

## Smoke test results (2026-08-27)

| Model | Result |
| --- | --- |
| `9router/gcli/grok-4.6` | ✅ OK |
| `9router/cmc/deepseek/deepseek-v4-flash` | ❌ 400 **insufficient credits** (commandcode upstream) |
| `9router/cmc/deepseek/deepseek-v4-pro` | ❌ 400 **insufficient credits** (commandcode upstream) |
| `gmi/MiniMaxAI/MiniMax-M3` | ✅ OK |
| `tokenrouter/qwen/qwen3.8-max-free` | ✅ OK |
| `bai/deepseek-v4-flash` | ✅ OK |
| `bai/deepseek-v4-flash-vision-exp` | ✅ OK |

Validation: `omp models find <provider>`; one-shot smoke:
`omp -p --model <provider>/<id> --thinking off "Reply with exactly: OK"`.

## Gotchas

- **9router `cmc/deepseek/*` → commandcode credits**: both deepseek models via
  9router now fail with `400 You have insufficient credits` from
  commandcode.ai. Auth works — it's a billing wall, not config. Top up
  commandcode or use `bai/deepseek-v4-flash` (works, and it's the opencode
  default). See [[2026-08-03-9router-commandcode-stream-cut]].
- **mimo (xiaomimimo) returned 401** on `/v1/models` — key looked invalid,
  provider dropped.
- **gcli/grok-4.5-high no longer exists** on 9router (now `gcli/grok-4.6`,
  `cmc/xai/grok-4.5/4.6`). Bumped to `gcli/grok-4.6`.
- Env vars are read per-process — omp started before the vars are exported
  won't see them.

## How it connects

- [[2026-08-03-deepseek-v4-routing-opencode-harness]] — routing ADR (flash
  primary, pro standby)
- [[2026-08-03-9router-commandcode-stream-cut]] — commandcode upstream gotcha
- [[harness-moc|🔧 Agent Harness & Tooling]]
