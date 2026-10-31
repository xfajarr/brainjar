---
title: Agent Harness & Tooling
type: moc
tags: [harness, opencode, 9router, mcp, agents]
date: 2026-08-03
---

# 🔧 Agent Harness & Tooling

Everything about how my coding agents are set up, routed, and maintained.

## Setup & Architecture

- [[2026-08-03-deepseek-v4-routing-opencode-harness|DeepSeek V4 routing (ADR)]] — Flash primary, Pro standby
- [[2026-10-31-opencode-v2-config-layout|opencode V2 config layout]] — what loads, secrets, hidden skills, backups
- [[2026-10-31-agent-coding-workflow-guide|Agent coding workflow guide]] — lanes, Opus vs Flash, token habits

## Gotchas

- [[2026-08-03-9router-commandcode-stream-cut|"Upstream stream ended" — 9router routes via commandcode]]
- [[2026-10-31-opencode-permissions-match-rewritten-command|Shell permissions see the rtk-rewritten command]]
- [[2026-10-31-opencode-skill-transform-misses-file-skills|skill.transform cannot hide disk skills]]

## Areas to cover

- [ ] opencode config & agents (web3-assistant, subagents)
- [ ] MCP servers (codebase-memory, context7, exa, cavemem)
- [ ] Wiki vaults (monkies vs brainjar routing)
- [ ] Model routing & reasoning effort
- [ ] 9router proxy internals
- [ ] Skills (graphify, ponytail, wiki-*)

## Related

- [[index|Home]]
