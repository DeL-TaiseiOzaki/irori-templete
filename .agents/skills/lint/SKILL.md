---
name: lint
description: "Check the bundle and repair what is mechanical: OKF conformance, missing descriptions, index drift, broken links and resources, orphans, stale and duplicate pages, contradictions. Also runs the pre-promotion check and the optional irori graph export."
---

# lint

Lint reports first and fixes only what is mechanical. A judgement, such as
merging two pages, resolving a contradiction or retiring a claim, is proposed,
not made.

## Checks

Walk every `.md` under `Knowledge_Base/` except `index.md`.

| Check | Rule | Fix |
| --- | --- | --- |
| conformance | parseable frontmatter with a non-empty `type`; `index.md` has no frontmatter except `okf_version` at the root; no `log.md` anywhere | add a missing `type` only when the folder makes it obvious; otherwise report |
| required keys | `title`, `description`, `generated` present; `sources` present on concept, synthesis, artifact and decision pages | report; `generated` may be filled from Git history with `by: process:lint` |
| index drift | every page appears once in its folder's `index.md`, under the heading for its type, with its current `description`; every subdirectory is listed; no entry points at a missing page | rewrite the entry lines additively, keeping the person's headings, order and prose |
| links | every relative link resolves inside this repository | report; fix only a link broken by a move you can see in Git history |
| resources | every `resource` and `sources[].resource` under `contents/` resolves when its mount is present; when the mount is absent, report "not checked" | never create anything under `contents/` |
| orphans | a page that nothing links to and no index lists | report |
| lifecycle | `stale_after` in the past; a `deprecated` page still linked as current | report |
| identity | two identity pages that name the same person, org, repo, product or project | propose a merge with a `deprecated` stub |
| contradictions | two pages that state incompatible claims; check by re-reading the cited sources, not by comparing the pages | report both, with the source that supports each |
| size | a page over 800 lines; a folder index over about 150 entries | propose a split |
| category rules | organization: a `stable` page without `verified`; team and organization: a concept without `sources` | report as an error |
| irori declaration | `.irori/notes.json` parses; `newNoteDirectory` exists in the knowledge layer and, when it names a year, names the current one; `daily.template` exists | advance the year folder and create it with an index; otherwise report |

Report by folder, errors first. Then apply the mechanical fixes as one change
the person can read in the diff.

## Before a promotion (`lint --for <receiving scope>`)

For the pages to be promoted: `sensitivity` is `internal`, or the person said
otherwise for this page in this conversation; every link and every
`sources[].resource` either resolves in the receiving repository or is
rewritten to a URL; every identity the page refers to exists there. Report what
does not hold; do not redact silently.

## irori graph (`lint --irori-graph`)

irori draws a graph from a declared CSV pair. On request, write two CSV files,
quoting any field that holds a comma, a double quote or a line break:

- `Knowledge_Base/ontology/entities.csv`, columns `id,label,note,parentId,group`:
  one row per page that is an identity page, has `relations` or is where such a
  relation points, each page once. `id` is the page's path in the bundle without
  `.md` (`wiki/retry-budget`); `label` its `title`, or its file name when it has
  none; `note` its path from the repository root; `parentId` empty; `group` its
  `type`.
- `Knowledge_Base/ontology/relations.csv`, columns `sourceId,relation,targetId`:
  one row per `relations` entry whose target, resolved from its page, is a page
  of the bundle other than an `index.md`. A URL, such as the record a `same_as`
  names, and a missing page get no row; say how many were left out.

Write both files with their header lines even when one has no rows, give the
folder an `index.md` saying its files are generated, list it in the root index,
and write `.irori/ontology.json`:

```json
{
  "schemaVersion": 1,
  "entities": { "path": "Knowledge_Base/ontology/entities.csv", "id": "id", "label": "label", "note": "note", "parent": "parentId", "group": "group" },
  "relations": { "path": "Knowledge_Base/ontology/relations.csv", "source": "sourceId", "target": "targetId", "label": "relation" }
}
```

irori rejects the whole file when a relation's endpoint is not a row, when two
rows share an `id` or a `label` is empty, when parents form a cycle, and past
2,000 rows or 10,000 relations; past those limits, say so instead of writing.
These files are generated; regenerate them rather than editing them.

## Boundaries

Do not rewrite prose. Do not delete a page; deprecate it. Do not change
`generated` on a page whose content you did not change.
