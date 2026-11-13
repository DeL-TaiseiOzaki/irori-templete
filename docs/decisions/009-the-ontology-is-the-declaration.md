# ADR 009 — The ontology is the declaration

Date: 2026-11-13. Status: owner decision, taken in irori
([ADR 060](https://github.com/DeL-TaiseiOzaki/irori/blob/main/docs/decisions/060-the-ontology-is-the-declaration.md)).
Replaces [ADR 006](006-one-layout.md) D4 and [ADR 002](002-okf-bundle.md) D8,
and amends ADR 006 D3 and [ADR 008](008-knowledge-at-the-root.md) decision 1.
Nothing here has been exercised against a real knowledge base.

## Context

ADR 006 D4 said the ontology has three parts: the vocabulary in
`.property/property.json`, the instances on the pages, and an index, the graph
irori generated into `ontology/`. Only the first is an ontology. The pages are
what the ontology is about, and the generated graph was a view of them. The
research of 2026-09-11 had it right before the name moved: the ontology is the
definitions, not a list of notes.

The mix-up had costs. `about` and `part_of` were explained by what they did for
the graph (ADR 006 D3). A relation said which types it goes from, but only
"a page or a URL" for what it points at, so "`about` names a person,
organization or project" lived in a sentence that nothing could check. irori
read the types of the declaration and none of its relations, so a misspelt
relation name went into the graph unnoticed, and only an agent running `lint`
could catch it.

The owner asked for the simplest mechanism and for an ontology rather than a
knowledge graph. irori 0.2.66 applies the declaration to the pages, lists the
pages that differ, draws the declaration itself, and keeps no graph of the
pages.

## Decisions

1. **The ontology is `.property/property.json`, and nothing else.** Types with
   a meaning and required keys; relations with a meaning, the types they go
   `from` and what they point `to`; names to `avoid`. The pages are instances.
2. **A relation names the types it points at.** `to` is `page` for any page,
   `url`, or a list of types. The seed says: `about` from any page to `person`,
   `org` or `project`; `part_of` from `person`, `org` or `project` to `org` or
   `project`; `uses` from any page to any page; `same_as` from `person`, `org`
   or `project` to a URL. The meanings of `about` and `part_of` no longer
   mention a graph.
3. **There is no `ontology/` folder.** irori neither generates nor reads one.
   `lint --irori-graph` is gone; `lint` proposes deleting a folder an earlier
   irori left. A table kept by hand is a CSV file, shown as a table;
   `.irori/ontology.json` is read by nothing.
4. **The declaration is applied where irori is, and by `lint` where it is
   not.** Inside irori, **オントロジー** lists the differing pages for the
   person and `irori check` prints them for an agent. `ingest` runs the check on
   what it wrote; `lint` runs it first and reads the pages itself only when the
   command is absent. The rules are the same either way: declared names, none
   avoided, required keys, a relation from a type that may carry it and to a
   target of the declared kind.
5. **The vocabulary still changes only by review** ([ADR 003](003-vocabulary-review.md)).
   Nothing about who may change `.property/property.json`, or how, is altered.

## Consequences

- The declaration needs irori 0.2.66 or later: an earlier irori cannot read a
  list in `to`, falls back to generic property rows and offers no folder
  indexes.
- A knowledge base made from an earlier template keeps working. Its `to: "page"`
  reads as any page, so nothing is reported until it adopts the lists, which is
  a vocabulary review like any other.
- Nothing lists, on a record's page, the pages that say they are `about` it.
  Body links still show as backlinks.
- Not added, until a knowledge base needs them: a hierarchy of types, labels in
  two languages, a mapping of types between scopes (`promote` still takes the
  nearest entry and says so), and a place for the concepts of a domain as a
  controlled list.
