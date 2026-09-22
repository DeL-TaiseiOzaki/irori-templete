---
name: lint
description: "Check the bundle and repair what is mechanical: OKF conformance, missing descriptions, index drift, broken links and resources, orphans, stale and duplicate pages, contradictions. Also runs the pre-promotion check and checks the graph index irori generates."
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

irori 0.1.24 and later generate the graph index, `Knowledge_Base/ontology/`,
from the pages' `type`, `title` and `relations` when the person presses
**グラフ索引を更新** in its ontology panel, and the person commits it, so every
device shows the same graph. Never write or edit those files. On request, check
that the index is current:

- every page with a `relations` entry that resolves to a page of the bundle
  other than an `index.md`, and every page such an entry points at, has one row
  in `entities.csv` with its `type` and its `title`, or its file name when it
  has none;
- every such entry has one row in `relations.csv`, and no row names a page or a
  relation that is gone.

Report the differences and ask the person to update the index in irori. A
knowledge base with `.irori/ontology.json` keeps the tables it declares; leave
them to the person.

## Boundaries

Do not rewrite prose. Do not delete a page; deprecate it. Do not change
`generated` on a page whose content you did not change.
