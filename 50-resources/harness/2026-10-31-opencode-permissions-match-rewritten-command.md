---
title: opencode matches shell permissions against the rewritten command
type: gotcha
owner: dev
tasks: []
tags: [opencode, rtk, permissions, plugins, security]
created: 2026-10-31
---

# opencode matches shell permissions against the rewritten command

**Symptom:** with the rtk plugin active, `git push origin main` ran without the ask that
`git push *` should trigger.

**Why:** in opencode 2.2.5 the permission scanner runs on the command *after* plugin hooks. Both
`ctx.tool.hook("execute.before")` and `ctx.shell.hook("create.before")` run first, so rtk's
`rtk git push …` is what the rules see, and the ask never matches. V1 behaved the same (old logs show
`pattern="rtk …"`). Proven with a scratch project: a deny on `rtk git status` blocked a plain
`git status`, and a deny on `git status` let it through.

A second trap in the same plugin: `rtk rewrite` (0.43) exits 0 for `find … -exec …`, then
`rtk find` refuses to run it. Exit 3 is undocumented; the hook contract is exit 0 only.

**Fix:** the plugin reads the agent's merged rules (`ctx.agent.get({ agentID })`), evaluates the original
and the rewritten command the way opencode does (last match wins, no match asks, worst segment of a
compound decides), and keeps the rewrite only when both decisions agree. It also skips `find` with actions
or compound predicates. Regression check: `bun ~/.config/opencode/scripts/rtk-check.ts` (covers
`git push`, `git branch -D`, `git status && git push`, `find -exec`). Re-run it after upgrading rtk.

## Related

- [[2026-10-31-opencode-v2-config-layout]]
- [[harness-moc]]
