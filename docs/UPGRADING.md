# Bringing a knowledge base up to date

A knowledge base keeps the template it was created from. These steps bring one
made from an earlier version of this template up to the current one. Do them on
a branch, review the diff, then commit.

## To the declared-folder contract (ADR 010)

1. Copy `AGENTS.md` and the shipped skills (`init`, `ingest`, `query`, `lint`,
   `journal`, `promote`, `vocabulary`) from this template over yours. Keep
   skills of your own.
2. In `.property/property.json`, give each type a `folder`: `"journal"` for
   `journal` and `"wiki"` for the others, or wherever you keep them.
3. Move any rule you wrote into `AGENTS.md` or `CLAUDE.md` into a `policy`
   page, then delete `CLAUDE.md`.
4. If you relied on `lint` rejecting a `stable` page without `verified` in a
   team or organization knowledge base, write that as a `policy` page; `lint`
   no longer checks it.

`lint --vocabulary` is now the `vocabulary` skill, and `lint --for` is part of
`promote`.

## From pages in `Knowledge_Base/` (before irori 0.2.0)

Ask the irori agent to run `irori layer <vault> knowledge .`, or choose
**vault のルート** under **層** in the vault's settings. irori moves the pages to
the root and rewrites the links into them, `.irori/notes.json` and the
comments; it stops, moving nothing, when a name is already at the root. Paths
written as text in `AGENTS.md` or a skill stay as they were, which step 1 above
replaces.

## From an `ontology/` folder (before irori 0.2.66)

An earlier irori generated `ontology/entities.csv`, `ontology/relations.csv`
and `ontology/index.md` from the pages' relations. Nothing reads them now: the
ontology is `.property/property.json`. Delete the folder and run `irori index`.
A `.irori/ontology.json` is read by nothing either; the CSV files it named are
ordinary tables, kept or deleted as you like.
