---
type: decision
id: ADR-004
title: DeepSeek V4 routing for the opencode harness — Flash primary, Pro standby
status: accepted
owner: dev
tasks: []
supersedes: null
date: 2026-08-03
tags: [opencode, deepseek, v4, agentic, routing, harness, 9router]
---

# ADR-004 — DeepSeek V4 routing for the opencode harness

## Context

The opencode harness (global config at `~/.config/opencode/`, agent runtime at `~/.opencode/`)
was rebuilt to run Chinese frontier models through a local 9router proxy (`127.0.0.1:20128`).
Target: get DeepSeek V4-class results close to Claude Opus on agentic coding, at 1/3 the cost.

Two DeepSeek V4 tiers exist on the API (model ids `deepseek-v4-pro`, `deepseek-v4-flash`):

- **DeepSeek-V4-Flash-0731** (official public beta, 31 Jul 2026): 284B/13B-active MoE, 1M ctx,
  re-post-trained for agentic work. DeepSeek-reported agent benchmarks beat the Pro *preview* on
  **9/9** agent benchmarks (Terminal-Bench 2.1: Flash 82.7 vs Pro-preview 72.1; DeepSWE 54.4 vs
  12.8). Native Responses API + Codex adaptation. $0.14 / $0.28 per M in/out.
- **DeepSeek-V4-Pro** (still preview, unchanged since Apr): 1.6T/49B-active. Stronger world
  knowledge / factual precision, but trails Flash-0731 on agentic work. $0.435 / $0.87 per M.

Independently measured caveat: V4 has a very high "answers anyway" rate (AA-Omniscience 94-96%),
so verification must live outside the model regardless of tier.

## Decision

**1. `deepseek-v4-flash` is the default and primary model for the harness.** Used for the
main agent (`web3-assistant`), code review, investigation, planning, refactoring — i.e. the
agentic-heavy majority of work. Flash-0731 is the best agentic model in the DeepSeek family
today and costs 1/3 of Pro.

**2. `deepseek-v4-pro` stays configured but is standby only.** Escalate to it when a task
needs deep world knowledge or factual precision, or when Flash demonstrably fails (long chains,
10+ tool calls). Re-evaluate when the official Pro release lands (expected early Aug 2026).

**3. Reasoning effort defaults to `high`, not `max`.** Per-task mode map: non-think for
routing/lookup/summarization, high for planning, max only for hard codegen + error recovery.
Thinking mode ignores temperature/top_p (DeepSeek-documented no-op), so prompt tuning is the
lever, not sampling.

**4. Diversify review/thinking roles to other Chinese models via 9router.** code-simplifier +
effort-estimator → Kimi-K2.7-Code; deep-thinker + web-researcher + find-next-crypto-idea →
GLM-5.2; talk → Qwen3.7-Plus. Reduces single-vendor bias in the maker/checker split.

**5. Verification loop is non-negotiable.** Agent must: ground via codebase-memory-mcp /
graphify before grep; run code-reviewer (fresh context, diff-only) + code-simplifier after each
chunk; run real tests before claiming done; never edit tests to force a pass.

## Consequences

**Cost**: Flash-first routing cuts token spend to ~1/3 of a Pro-default setup; cache-hit input
is 98% off ($0.0028 vs $0.14) with a static system-prompt prefix, which dominates agent-loop
cost.

**Risk**: all agent-benchmark numbers above are DeepSeek-reported (own harness, unreleased);
Pro-preview measured independently at 8% pass@1 on DeepSWE shows vendor numbers run optimistic.
Mitigation is the verification loop, not model trust.

**Maintenance**: when DeepSeek ships the official V4-Pro release, re-benchmark Flash vs Pro on
the real workload and flip the default in one config edit if Pro wins.

**Reversal trigger:** official Pro release that beats Flash-0731 on agentic work, or a measured
regression in Flash on real tasks.

---
## 🔗 Related
[[opencode-harness-setup]] · [[9router-gotcha]]
