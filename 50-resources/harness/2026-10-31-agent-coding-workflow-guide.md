---
title: Agent coding workflow — lanes, models, token habits
type: resource
owner: dev
tasks: []
tags: [workflow, opencode, compound-engineering, models, tokens, web3, smart-contracts]
created: 2026-10-31
---

# Agent coding workflow — lanes, models, token habits

One spine (Compound Engineering), a fresh session per phase, Flash by default, Opus where a
mistake is expensive, and every change proven by real behavior before it counts as done.

Why not pstack or full SDD: pstack has no OpenCode host and leans on several premium models running
in parallel; spec-kit-style SDD writes a stack of documents per feature. Both cost more tokens than
they save for daily work. Two ideas are worth keeping and are folded in below: prove real behavior,
and settle types before code. A light spec is kept for smart contracts, where it pays.

## Models

| Model | ID | Use for |
| --- | --- | --- |
| DeepSeek Flash | `bai/deepseek-v4-flash` (default) | exploring, implementing a written plan, tests, small fixes, refactors, docs |
| Claude Opus | `claude-agent/opus#high` (`#xhigh` for contract audits) | plans for non-trivial work, architecture, contract specs and security review, infra touching prod, final review of risky diffs |

Switching in the TUI (leader is `ctrl+o`): `<leader>m` model list, `ctrl+f` in that dialog to mark a
favorite, `f2` previous model, `ctrl+t` cycle variant, `shift+tab` cycle agent, `<leader>n` new
session, `<leader>c` compact. One-off from the shell: `opencode run -m claude-agent/opus#high "…"`.

Rule of thumb: if a wrong answer costs more than an hour or touches money, use Opus for that step.

## Pick the lane first

| Lane | When | Steps |
| --- | --- | --- |
| Small | one file or an obvious fix | Flash: do it → run checks → commit |
| Feature | backend, API, AI agent | Opus plan session → Flash work session → review → capture |
| Bug | something misbehaves | Flash `ce-debug`; switch to Opus after two failed hypotheses |
| Contract | Solidity / Anchor | spec (Opus) → invariant tests → implement (Flash) → security review (Opus) |
| Infra | deploy, CI, Docker, cloud | Opus read-only plan → dry-run → Flash executes → runbook |
| Architecture | crosses modules, new subsystem | Opus: types and data structures first → ADR |

## Lanes in detail

### Feature (backend, AI agents)

1. **Plan session — Opus.** `ce-brainstorm` only if the goal is fuzzy, then `ce-plan`. The plan is
   a file, not chat. In asend it lives in `docs/plans/` and opens with a **Status:** line (repo rule).
   For an AI agent, the plan lists eval cases: input → expected behavior, and a cost ceiling per run.
2. **Work session — new session, Flash.** `ce-work <plan file>`. Test first where the behavior is
   clear (`tdd`). After each chunk, launch the `code-reviewer` subagent; for a risky diff, switch to
   Opus before launching it (subagents inherit the session model).
3. **Prove it.** Run the project's real check (`pnpm check` in asend) and exercise the behavior:
   hit the endpoint, run the agent against its eval cases, read the actual output.
4. **Ship.** `ce-commit`, or `ce-commit-push-pr`.
5. **Capture.** Decision or gotcha → the vault (`wiki-capture --quick` is enough).

### Bug

`ce-debug`: reproduce → find the cause with evidence → report → plan → fix. In asend this is the
mandatory investigate → report → plan → act gate. If two hypotheses fail on Flash, stop patching:
start an Opus session with the evidence so far.

### Smart contracts (light spec)

Money is on the line, so this lane is not vibe coding.

1. **Spec — Opus.** One file, `docs/specs/<contract>.md`: state variables, roles and access control,
   every external call, invariants ("total shares == sum of balances", "only owner can pause"),
   failure modes, upgrade path.
2. **Tests from the spec.** Foundry fuzz and `invariant_*` tests, or Anchor / LiteSVM tests, one per
   invariant. They must fail before the implementation exists.
3. **Implement — Flash** until the tests pass. Load `solidity-security-patterns` or
   `anchor-best-practices` for the stack.
4. **Security review — Opus `#xhigh`.** `security-audit`, plus `solana-vulnerability-scanner` for
   Solana. Fix, re-run the tests.
5. **Deploy** devnet / testnet first; `deploy-to-mainnet` checklist before mainnet.

### Infra / devops

1. **Plan — Opus, Plan agent** (`shift+tab` to `plan`, read-only).
2. **Dry-run** — `terraform plan`, `docker compose config`, `kubectl diff`, staging first.
3. **Execute — Flash**, step by step, against the plan.
4. **Runbook** — update the living doc in the vault (`04-engineering/runbook/` for company projects).

### Architecture

Opus. Settle types, data structures and module boundaries before any implementation; compare two
designs when the choice matters (`design-an-interface`). Record the result as an ADR
(`02-decisions/YYYY-MM-DD-slug.md`).

## Which skill for what

Use the CE set and skip its duplicates; they all write plans, and loading two costs double.

| Need | Use | Skip |
| --- | --- | --- |
| clarify requirements | `ce-brainstorm` | `brainstorming`, `write-a-prd` |
| plan | `ce-plan` | `writing-plans`, `prd-to-plan` |
| execute a plan | `ce-work` | — |
| debug | `ce-debug` | `diagnose` |
| review | `code-reviewer` subagent, `ce-code-review` for big PRs | — |
| stress-test a plan | `grill-me` | — |
| simplify | `ce-simplify-code` | — |

Name the skill you want (`@ce-plan`) instead of describing the task and letting several load.

## Token habits

- **One phase, one session.** Hand off through the plan file, not the chat history.
- **Explore in a subagent** (`explore`) for wide searches; only the summary returns to the main context.
- **Graph before files** in indexed repos: codebase-memory `search_graph` / `trace_path` beat reading
  ten files.
- **Point, don't paste.** Give a log path or a command to run, not 2,000 pasted lines.
- **Re-plan instead of re-patching.** Three failed fixes in a row means the plan is wrong.
- **Compact long phases** with `<leader>c` rather than letting the context run full.

## Session checklist

Start: right lane → right model → read the vault `index.md` for a known project → plan file if the lane has one.

End: real check passed → reviewed → committed → captured in the vault.

## Not set up yet

- `brainstorming` still says "MUST use before any creative work" and loads on its own; hiding it or
  rewording it would stop that.
- Agents have no model pins, so model choice is manual per session (see Models).
- No `/verify` or `/contract-spec` commands yet; the steps above are the manual version.
- `web3-assistant`'s prompt still says to invoke `write-a-prd` and `prd-to-plan`, which contradicts
  the skill table above.

## Related

- [[2026-10-31-opencode-v2-config-layout]]
- [[harness-moc]]
