---
title: opencode V2 config layout after the 2026-10-31 audit
type: resource
owner: dev
tasks: []
tags: [opencode, harness, skills, agents, plugins, permissions]
created: 2026-10-31
---

# opencode V2 config layout after the 2026-10-31 audit

## What loads, in precedence order

`opencode api get "/api/config?location%5Bdirectory%5D=<dir>"` lists the documents actually merged.

1. `~/.config/opencode/opencode.json`: global. Providers (bai, gmi, tokenrouter, 9router), DigitalOcean
   MCPs, codebase-memory MCP, `vision` agent, `experimental.todo`, the `skills-unadvertised` source.
2. `~/.opencode/opencode.json`: a V1 leftover, but `~` is an ancestor of every project, so it loads as a
   **project** `.opencode` config and outranks the global file. Holds `default_agent`, exa/context7 MCPs,
   notifier + goal plugins, the shell ask rules, ClinePass/ollama providers.
3. `~/.opencode/agents/*.md`: the V1 agents (web3-assistant, code-reviewer, …). V2 defaults a custom
   agent to `primary`, so each needs `mode: all` to be launchable as a subagent.

Only `AGENTS.md` is read in V2: `~/.config/opencode/AGENTS.md` plus the project's. `~/.opencode/AGENTS.md`
and `CLAUDE.md` are not.

## Pieces added in the audit

- **Secrets** live in `~/.config/opencode/secrets/` (mode 700/600), referenced as `{file:~/.config/opencode/secrets/<name>}`.
- **Skill advertising**: rarely used skills are hidden by stub overrides in
  `~/.config/opencode/skills-unadvertised/<id>.md` (`metadata.opencode/autoinvoke: false`, body points at
  the real `SKILL.md`). Delete a stub to advertise that skill again. See
  [[2026-10-31-opencode-skill-transform-misses-file-skills]].
- **rtk plugin** `~/.config/opencode/plugins/rtk.ts`, V2 API, keeps a rewrite only when it does not change
  the permission decision. See [[2026-10-31-opencode-permissions-match-rewritten-command]].
- `external_directory` allows reads under the skill roots; edits there still ask.
- `cli.json`: leader `ctrl+o`, `open.menu` moved to `<leader>o`.

## Backups

`~/.local/share/opencode-config-backups/`: `pre-audit-2026-10-31.tar.gz` (full pre-change state) and
`archive-2026-10-31/` (archived files: old backups, V1 leftovers, shadowed skills, `skill-creator` agent).

## Still open

- `plugins/vision-proxy.ts` fails to load (SDK mismatch) and reads keys from the config file, which now
  holds `{file:}` refs. Port to V2 and read from `secrets/` if still wanted.
- `~/.opencode/tools/*.ts` are V1 custom tools and do not load in V2.

## Related

- [[harness-moc]]
