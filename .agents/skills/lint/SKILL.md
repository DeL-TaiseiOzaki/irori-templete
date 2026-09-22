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
| vocabulary | every `type` and every `rel` is in `AGENTS.md` and none is a listed name not to use; a relation points the way its table line says; no page lists the same relation twice | replace a listed name with the entry listed for it and drop an exact duplicate; report the rest, including a name listed with "a link", whose fix is prose |
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
rewritten to a URL; every identity the page refers to exists there; the page's
`type` and every `rel` it uses are in the receiving scope's vocabulary. Report
what does not hold; do not redact silently.

## Vocabulary review (`lint --vocabulary`)

Run it when the person asks, and suggest it when pages keep landing under the
nearest type or an ordinary link because nothing fitted. It changes the page
types and `rel` names in `AGENTS.md` only as far as the person agrees, and
leaving the vocabulary as it is is always an acceptable outcome.

1. **Evidence.** From the indexes and the pages' frontmatter: pages per `type`
   and per `rel`, values outside the vocabulary, and index lines whose
   description does not fit the type they are filed under; by searching the
   bodies, the same relationship written as prose on several pages.
2. **Propose one change** with the whole vocabulary in view, including the
   names not to use: sharpen a definition the evidence shows is misread, add an
   entry the evidence needs and no entry covers, fold a name in use outside the
   vocabulary into the entry that already means it, merge two entries that mean
   the same, or retire one that no page uses although its folder exists here.
3. **Check it before anyone sees it.** Discard it and propose a different one,
   keeping the discarded one in view so it does not come back, unless it differs
   in meaning, not only in spelling, from every entry and every listed name;
   it names the pages it would change (at least three for a new entry) or the
   recurring question it answers; and every page stays conformant without
   moving. A proposal is never kept because others were discarded.
4. **Stop** after three kept proposals, or sooner when new ones repeat earlier
   ones.
5. **Show** each kept proposal with its evidence and its exact edits: the lines
   in `AGENTS.md`, the pages whose `type` or `rel` changes and the index lines
   that move. The person accepts or turns down each one.
6. **Apply** what was accepted as one change, add to the list in `AGENTS.md`
   each name the change folded or merged away and each new name the person
   turned down, with what to use instead, and run the other checks on the
   result. In a team or organization scope the change is a branch and a pull
   request, which you do not merge.

## irori graph (`lint --irori-graph`)

irori draws a graph from a declared CSV pair. On request, generate
`Knowledge_Base/ontology/entities.csv` with the columns
`id,label,note,parentId,group` (one row per identity page and per page that has
`relations`; `id` is the page's slug, `note` the page path from the repository
root, `group` the `type`) and `Knowledge_Base/ontology/relations.csv` with
`sourceId,relation,targetId` (one row per `relations` entry), and write
`.irori/ontology.json`:

```json
{
  "schemaVersion": 1,
  "entities": { "path": "Knowledge_Base/ontology/entities.csv", "id": "id", "label": "label", "note": "note", "parent": "parentId", "group": "group" },
  "relations": { "path": "Knowledge_Base/ontology/relations.csv", "source": "sourceId", "target": "targetId", "label": "relation" }
}
```

Both endpoints of every relation must exist as rows, ids must be unique, and
parents must not form a cycle, or irori rejects the whole file. These files are
generated; regenerate them rather than editing them.

## Boundaries

Do not rewrite prose. Do not delete a page; deprecate it. Do not change
`generated` on a page whose content you did not change.
