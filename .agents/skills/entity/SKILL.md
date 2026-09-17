---
name: entity
description: Create or update the record of a person, organisation or repository together with its ontology rows.
---

# entity

The ontology graph is the only way back into this knowledge base — there is no
map index. An entity record and its CSV rows are written in the same change, or
the graph rots.

## What is allowed to be an entity file

`person`, `org` and `repo`, under `Knowledge_Base/entities/<type>/<slug>.md`.
Nothing else. A topic, a project, a product or a recurring meeting is a
`library/` note; it still gets an ontology row, with `group` set to `topic` or
`project` and `note` pointing at that note. If you are unsure, write the
`library/` note — an entity file can be created later, a wrong one has to be
merged.

## Steps

1. Check `Knowledge_Base/ontology/entities.csv` for an existing row. Names
   collide; ids do not. Search for the id, the label, and plausible spellings
   before creating anything.
2. Write the record from `templates/entity.md`, with a fresh ULID. Keep it to
   what stays true — naming, responsibility, how to reach them. Judgements go in
   `library/`.
3. Add the row: `id,label,note,parentId,group`. The `id` is a stable slug, not
   the ULID, and it is what relations refer to. `note` is the path to the record
   from the repository root. `parentId` is hierarchy only — a person's employer
   is a `works_at` relation, not a parent.
4. Add relations to `relations.csv` for what you actually know. Do not invent
   `relates_to` edges to make the graph look connected.
5. In a **personal or project** scope, if a higher scope already owns this
   identity, keep the local record thin and add a `same_as` relation to the upper
   scope's id. The upper scope is authoritative; do not copy its content down.

## Validation before you finish

irori rejects the whole file if any of these fail, so check them:

- ids unique and nonempty, labels nonempty;
- every `parentId` exists, and the hierarchy has no cycle;
- both endpoints of every relation exist;
- every `note` path exists and is inside `Knowledge_Base/`;
- unknown columns, quoting and existing row order preserved.

A missing note file is allowed and shows as a missing link, but do not leave one
on purpose.
