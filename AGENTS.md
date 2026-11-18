# Brainjar: instructions for agents

Personal second brain: plain markdown in PARA folders. Git is the source of truth
(`github.com/xfajarr/brainjar`, branch `main`). The search index in the brainjar-app Worker
picks up every push within about 2 minutes, and the phone app reads from that index.

## Rules

- Notes are in English. Captures in `00-inbox/` may be in any language; translate them when filing.
- New knowledge goes in a new file. Do not rewrite existing notes. The index computes backlinks, so a
  one-way `[[link]]` from the new note is enough.
- Frontmatter: `title`, `type`, `tags`, `created`, and `summary`. The summary is one sentence of at
  most 200 characters; search results and agents show it instead of the body.
- Link by file name with `[[wikilink]]`; folders are location, links are relation.
- Never delete. Move finished projects and dead topics to `40-archive/`.
- Commit and push after meaningful changes, without Co-Authored-By footers.

## Folders

| Folder | Holds |
|---|---|
| `00-inbox/` | Raw captures from the phone and agents, waiting to be processed |
| `10-concepts/` | One atomic idea per note |
| `20-areas/` | Ongoing topics being learned |
| `30-projects/` | Active projects |
| `40-archive/` | Finished or dead material |
| `50-resources/` | Reference material, grouped in subfolders by topic |
| `60-daily/` | Daily notes |
| `70-mocs/` | Maps of content, created once a topic is busy |

## Finding things

- `brain search "<query>"` returns path, title, summary and the best matching passage. Queries may be
  in Indonesian; the index matches meaning across languages.
- `brain related <path>` lists notes linked to it, similar to it, or sharing its tags.
- Read the local file for full text. Do not paste whole notes into answers when a path and summary do.
- `brain capture "<text>"` saves something worth remembering to the inbox.

## Processing the inbox ("proses inbox")

1. `git pull --ff-only`.
2. List the inbox with `brain inbox` (or `ls 00-inbox`). Work oldest first. For each capture:
   1. Read it. When it has a `url`, fetch the page for the substance.
   2. Look at its `related:` candidates and run `brain search` on its gist. Read the summaries of the
      top matches.
   3. Pick the folder: an atomic idea goes to `10-concepts/`, reference material to `50-resources/<topic>/`,
      project work to `30-projects/`, an ongoing topic to `20-areas/`.
   4. `git mv` the capture to `<folder>/<YYYY-MM-DD>-<slug>.md`, then rewrite it as an English note with
      full frontmatter, `summary:`, and `[[links]]` to the related notes. Keep the source URL.
   5. When it contradicts an existing note, write `Contradicts [[note]]: ...` in the new note.
   6. When it duplicates a note, is unclear, or looks not worth keeping, leave it in the inbox and ask.
3. When a topic reaches five or more notes without a MOC, suggest one; do not create it unasked.
4. Append one line to `log.md`: `- [YYYY-MM-DD] INBOX processed N: <old path> -> <new path>, ...`.
5. Stage the moved files and `log.md` by path, commit `inbox: process N captures`, and push.
6. Report which capture became which note, any contradictions, and the open questions.
