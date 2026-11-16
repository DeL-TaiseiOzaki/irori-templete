---
name: lint
description: "Check the knowledge base and repair what is mechanical: declaration conformance, index drift, broken links and resources, orphans, stale and duplicate pages, contradictions, and the pages skills name."
---

# lint

Lint reports first and fixes only what is mechanical. A judgement, such as
merging two pages, resolving a contradiction or retiring a claim, is proposed,
not made.

## Checks

Run `irori check` first. It applies `.property/property.json` to every page and
lists each that differs: an undeclared type or relation, a missing required
key, a relation from a type that may not carry it or to the wrong kind of
target. For each finding, replace a name the declaration's `avoid` lists with
the entry it names and drop an exact duplicate relation; report the rest. A
missing `generated` may be filled from Git history with `by: process:lint`.

Then check by reading, over the pages (every `.md` of the knowledge layer except
`index.md`) and the skills (`.agents/skills/*/SKILL.md`):

| Check | Rule | Fix |
| --- | --- | --- |
| values | a `select` value is one of its `options` | report |
| bundle | no `log.md`; no `AGENTS.md` in the knowledge layer; no `index.md` has frontmatter except the root one's `okf_version` | for an `AGENTS.md`, propose moving its rules to a `policy` page |
| indexes | `irori index` changes nothing | run it and say which lines changed |
| links | every relative link resolves inside this repository | report; fix only a link broken by a move you can see in Git history |
| resources | every `resource` and `sources[].resource` under `contents/` resolves when its mount is present; when it is absent, report "not checked" | never create anything under `contents/` |
| orphans | a page that nothing links to and no index lists | report |
| lifecycle | `stale_after` in the past; a `deprecated` page still linked as current | report |
| skills | every page path a skill names resolves to a page that is not `deprecated`; a skill does not state a criterion, fact or reason that a page states or should state | repoint a path to the page its `deprecated` stub links to; propose moving a stated claim to a page, never move it yourself |
| identity | two `person`, `org` or `project` pages that name the same identity | propose a merge with a `deprecated` stub |
| contradictions | two pages that state incompatible claims; check by re-reading the cited sources, not by comparing the pages | report both, with the source that supports each |
| size | a page over 800 lines; a folder index over about 150 entries | propose a split, by topic into subfolders |
| note settings | in `.irori/notes.json`, a `newNoteDirectory` that names a year names the current one, and `daily.template` exists | advance the year; otherwise report |

Report by folder, errors first. Then apply the mechanical fixes as one change
the person can read in the diff.

## Boundaries

Do not rewrite prose. Do not delete a page; deprecate it. Do not edit
`.property/property.json`; a vocabulary problem goes to the `vocabulary` skill.
When you change a page, set its `generated` to yourself and now; leave it alone
on a page you did not change.
