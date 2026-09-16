---
id: scope/00000000000000000000000002
title: How this knowledge base works
type: knowledge
status: active
created: 2026-09-16
sensitivity: internal
derived_from: []
---

# How this knowledge base works

Four places, and one question each.

- **`journal/<year>/`** — anything tied to a date: a daily entry, a weekly
  summary, a meeting record. It is append-only. You do not go back and tidy it;
  you write a new note instead.
- **`library/`** — one claim per note, written to be read again later. Flat, no
  subject folders. This note is one.
- **`entities/`** — the record of a person, an organisation or a repository.
  Identity only; what you *think* about them goes in `library/`.
- **`Notes/`** — where irori's new-note button puts things. Treat it as an
  in-tray, not a home.

Nothing moves between these to show that it matured. When a journal entry turns
out to contain a reusable claim, you write a new `library/` note and set its
`derived_from` to the journal entry's `id`. Both survive, and you can see where
the claim came from.

`contents/` is not part of this. It is where irori mounts your Google Drive:
`Inbox/` for things arriving, `source/` for references you did not write,
`output/` for deliverables. It is never committed, and large files stay there.

The **ontology** — `ontology/entities.csv` and `ontology/relations.csv` — is how
you get back to things. irori draws it as a graph you can filter and click
through to the underlying note. There is no map index to maintain instead, which
means the CSV is the one thing worth keeping honest: when you write a note
someone would look for in the graph, add its row at the same time.

The full contract, including what to write in frontmatter and how to promote a
note to a shared knowledge base, is in `AGENTS.md` at the root.
