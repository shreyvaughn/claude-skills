---
name: vaughn-wiki
description: Maintain a structured local markdown knowledge base so decisions, facts, and context persist and can be recalled across sessions.
---

# Vaughn Wiki

A persistent-memory engine built from plain markdown files. It lets an assistant remember decisions, facts, and context across sessions instead of starting cold every time. There is no database and no service: the knowledge base is a folder of small, linked markdown notes with frontmatter, versioned in git alongside (or near) the work it describes.

The folder is named `vaughn-wiki` by convention, but this is a generic personal-memory pattern. Point it anywhere with a `WIKI_ROOT` (default: `./vaughn-wiki/`).

## When to use this

Two moments matter:

- **Recall (before answering).** When a question depends on prior decisions, project context, or stated preferences, check the wiki first so the answer is consistent with what was decided before.
- **Capture (after producing knowledge).** When a session yields a decision, a durable fact, a new preference, or a solved problem, write it down before moving on. Do this as a natural closing action, not as a question.

Skip capture for purely ephemeral work (one-off debugging, throwaway lookups). The test: would a fresh session benefit from knowing this? If yes, capture it.

## Folder layout

```
vaughn-wiki/
  index.md            # catalog of notes, grouped by domain
  hot.md              # short live snapshot: current focus, open threads, recent decisions
  decisions/          # one file per decision, with rationale
  facts/              # granular durable facts
  projects/           # per-project context notes
  notes/              # catch-all for anything that does not fit yet
  log.md              # reverse-chronological session log
```

Create domain folders as needed (for example `people/`, `clients/`, `tools/`). Do not over-structure up front. Start with `notes/` and graduate items into proper folders as patterns emerge.

## Note format

Every note is a small markdown file with frontmatter. Keep notes short and single-topic. One idea per file makes linking and recall precise.

```markdown
---
title: Chose Postgres over MongoDB for the billing service
type: decision
created: 2025-01-14
updated: 2025-01-14
tags: [database, billing, architecture]
links: [projects/billing-service.md, facts/billing-needs-transactions.md]
status: active
---

## Context
The billing service needs multi-row transactional writes for invoice generation.

## Decision
Use Postgres.

## Rationale
Strong transactional guarantees and a relational shape fit the invoice and line-item
model. The team already operates Postgres elsewhere, so no new operational burden.

## Supersedes
(none)
```

Recommended frontmatter fields:

- `title`: a full sentence, not a label. It should make sense out of context.
- `type`: `decision`, `fact`, `project`, `note`, `preference`.
- `created` / `updated`: ISO dates.
- `tags`: lowercase, reused across notes for grouping.
- `links`: relative paths to related notes (the graph edges).
- `status`: `active` or `superseded`. Never delete a wrong note; mark it superseded and link forward.

## Linking notes

The value compounds when notes reference each other.

- When a new note relates to an existing one, add a `links` entry in both directions where it makes sense.
- When a decision changes, do not edit history into a lie. Create a new note, set the old one's `status: superseded`, and add a `Superseded by` link. The trail of how thinking evolved is itself valuable.
- Keep `index.md` current: add a one-line entry per note under its domain so the catalog stays browseable without opening every file.

## Recall workflow

Before answering a question that could depend on prior context:

1. Read `hot.md` for the current snapshot (focus, open threads, recent decisions). This is the cheapest, highest-value read.
2. Scan `index.md` for notes whose titles match the topic.
3. Open ONLY the specific notes that look relevant. Do not load the whole wiki.
4. Follow `links` from those notes one hop out if needed.

Be frugal. The point of `hot.md` and `index.md` is to avoid reading everything. Loading the entire knowledge base on every question defeats the purpose and burns tokens.

## Capture workflow

At a natural stopping point, when knowledge was produced:

1. Write or update the relevant note(s) in the right domain folder, using the frontmatter format above.
2. For granular durable facts, append to or create a file under `facts/`. Mark any contradicted fact as superseded rather than overwriting silently.
3. Update `links` on related notes so the new note is reachable.
4. Add or update the one-line entry in `index.md`.
5. Prepend a short entry to `log.md` (date, what changed, why).
6. Refresh `hot.md`: update current focus, open threads, and recent decisions. Keep it short (a few hundred words). It is a cache, not an archive.

Do all of this silently as a closing action. Do not ask permission to remember.

## What to capture

- Decisions and their rationale (the why outlives the what)
- Durable facts about projects, systems, people, and constraints
- Stated preferences and conventions worth applying again
- Problems solved and the approach that worked
- Ideas explored but not pursued, and why they were set aside

## What NOT to capture

- Ephemeral debugging state and one-off task scratch
- Anything code or git already records well: implementation details, line-level changes, commit history. Link to the code instead of restating it.
- Secrets, credentials, tokens, or private keys. Never. Reference where a secret lives, never its value.
- Large pasted blobs. Summarize and link to the source.

## Maintenance habits

- Keep notes small and single-topic. Split a note that has grown two subjects.
- Prefer marking `superseded` over deleting, so the history of decisions stays intact.
- Reuse tags rather than inventing near-duplicates (`db` vs `database`). Glance at existing tags before adding one.
- Periodically file loose `notes/` items into proper domains and prune anything that turned out to be noise.
- Keep `hot.md` honest. If it no longer reflects current reality, it is worse than nothing.

## Why this works

Plain markdown means the knowledge base is portable, diffable, greppable, and reviewable in git. Frontmatter gives structure without a database. Links turn isolated notes into a navigable graph. The `hot.md` plus `index.md` pair keeps recall cheap, so memory stays useful without slowing every answer down.
