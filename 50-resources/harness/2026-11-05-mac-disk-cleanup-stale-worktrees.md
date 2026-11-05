---
type: gotcha
title: Deleting stale worktrees frees far less than du says
owner: dev
tasks: []
tags: [disk, macos, git-worktree, pnpm, caches, cleanup]
created: 2026-11-05
---

# Deleting stale worktrees frees far less than du says

**Symptom:** On 2026-11-05 the Mac's data volume was 97% full (13.6 GB free of 460 GB). `du`
put the 63 unused asend worktrees at ~108 GB. Removing all of them freed only 11 GB.

**Why:** pnpm's `node_modules` are hard links into the store (`~/Library/pnpm`), so each
worktree's 1.6 GB is mostly the same blocks counted again. The real bytes per worktree were
the Rust `target/` dirs and other build output. Estimate a cleanup by the `df` free-space
delta, never by summing `du`.

**What the cleanup actually freed** (13.6 GB to 44.6 GB free):

- 63 worktrees + their local branches: 11.2 GB
- `go clean -cache`: 8.8 GB
- `pnpm store prune` (run after the worktrees, so their packages are unreferenced): 4.3 GB
- `npm cache clean --force`: 2.6 GB
- `~/Library/Caches/*.ShipIt` and `*-updater` (old app update downloads): 3.7 GB
- `brew cleanup -s`: 0.4 GB

**Still large, not touched:** Chrome cache 9.5 GB, Brave 2.3 GB, Telegram 1.9 GB (clear in
app), `~/.gradle` 4.7 GB and `~/Library/Android` 4.5 GB (needed for Android builds).

**How to tell a worktree is safe to delete:**

1. One `gh pr list --state all --limit 1000 --json headRefName,state,number` call, matched to
   each worktree's branch. MERGED or CLOSED is a candidate. Do not call `gh` per worktree.
2. No tracked changes, no untracked files, and `git rev-list --count origin/<b>..<b>` is 0.
3. Local-only branches: every commit subject must appear in `git log origin/develop`
   (squash merges change the hashes, so `ahead_by` lies).
4. **Check ignored files too.** `git status --porcelain --ignored=matching`, minus build output.
   Plain `git status` hides these. This run found one worktree `.env` with ~45 keys absent from
   the main `.env`, and two plans that existed only in worktrees (plans are never committed).
   Rescued to `asend-monorepo/.env.asend-perf` and `asend-monorepo/docs/plans/`, both ignored.
5. No process has its cwd there: `lsof -a -d cwd -Fn`.

Then `git worktree remove --force` each, `git branch -D` the branch, `git worktree prune`.
Run the deletions and the cache commands one after another, not in parallel.

Related: [[harness-moc]]
