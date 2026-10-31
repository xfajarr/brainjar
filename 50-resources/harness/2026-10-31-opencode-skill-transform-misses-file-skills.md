---
title: opencode skill.transform cannot see skills discovered from disk
type: gotcha
owner: dev
tasks: []
tags: [opencode, skills, plugins, context]
created: 2026-10-31
---

# opencode skill.transform cannot see skills discovered from disk

**Symptom:** a plugin setting `autoinvoke = false` through `ctx.skill.transform` loaded as "active", but
every targeted skill stayed advertised. `/api/skill` showed no `autoinvoke` on them.

**Why:** in opencode 2.2.5 the transform editor lists only the built-in skills (`editor.list()` returned 2),
not the ~300 found in `~/.claude/skills`, `~/.agents/skills`, `~/.opencode/skills`. The docs imply it
sees all of them.

**Fix:** override by ID instead. An explicit `skills` config entry has the highest precedence, so
`~/.config/opencode/skills-unadvertised/<id>.md` with `metadata.opencode/autoinvoke: false` replaces the
original in opencode only, without touching files Claude Code and Codex share. The stub's body points at the
real `SKILL.md`, so `@id` and skills that name it still work (one extra read). Reads under the skill roots
need an `external_directory` allow, or every load prompts.

Check what the model is actually offered: ask a fresh `opencode run` to list its skill IDs. `/api/skill`
lists hidden skills too.

## Related

- [[2026-10-31-opencode-v2-config-layout]]
- [[harness-moc]]
