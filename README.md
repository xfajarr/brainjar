---
title: README
---

# Brainjar — Second Brain

Personal knowledge vault. Plain markdown + git. Accessible from phone (web app), laptop, and AI agents.

## Structure

```
brainjar/
├── 00-inbox/      # All new notes land here first (capture, no decisions)
├── 10-concepts/   # Atomic notes: one idea per file (the real knowledge)
├── 20-areas/      # Ongoing topics you're learning (ai, crypto, dev, life)
├── 30-projects/   # Time-bound work
├── 40-archive/    # Finished/dormant (move, never delete)
├── 50-resources/  # Reference: setup, tooling, configs, how-tos
├── 60-daily/      # Daily journal (YYYY-MM-DD.md)
├── 70-mocs/       # Maps of Content: navigation hubs
├── _templates/    # Note templates
└── _attachments/  # Images, PDFs, embedded files
```

## Why this shape

- **Shallow folders** (2 levels max) — the link graph does the organizing, not folders.
- **Frontmatter** (`type`, `title`, `tags`, `date`) — makes notes queryable by AI agents.
- **Atomic notes** — small, single-topic, link-dense. Compound over time.
- **MOCs over deep trees** — navigate by reading a map, not browsing folders.

## Agent access

Agents can query and save to this vault. Flow:
- **Query**: search `index.md` + MOCs first, then grep/frontmatter search, then read.
- **Save**: new knowledge = new file in the right folder (inbox for quick capture, concepts for atomic knowledge, resources for tooling).
- **Routing**: company projects → `@monkies` vault; personal harness/tooling → `@brainjar` (this vault).

## Sync

- Git repo: `git@github.com:xfajarr/brainjar.git` (private).
- Commit + push after meaningful changes.
- Web app reads this repo (read-only mirror for phone access).
