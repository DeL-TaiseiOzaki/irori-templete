---
name: lint
description: "Check the bundle and repair what is mechanical: OKF conformance, missing descriptions, index drift, broken links and resources, orphans, stale and duplicate pages, contradictions, and the pages skills name. Also runs the pre-promotion check and checks the graph index irori generates."
---

# lint

Lint reports first and fixes only what is mechanical. A judgement, such as
merging two pages, resolving a contradiction or retiring a claim, is proposed,
not made.

## Checks

Walk every `.md` under `Knowledge_Base/` except `index.md`, and for the skills
check every `.agents/skills/*/SKILL.md`.

| Check | Rule | Fix |
| --- | --- | --- |
| conformance | parseable frontmatter with a non-empty `type`; `index.md` has no frontmatter except `okf_version` at the root; no `log.md` anywhere; no `AGENTS.md` under `Knowledge_Base/` | add a missing `type` only when the folder makes it obvious; otherwise report, and for an `AGENTS.md` propose moving its rules into the root `AGENTS.md` |
| required keys | the keys `.property/property.json` requires of every page and of the page's type are present; a `select` value is one of its `options` | report; `generated` may be filled from Git history with `by: process:lint` |
| index drift | every folder with pages in it or below it has an `index.md` that is exactly what the rule in `AGENTS.md`, **Indexes**, gives from its pages | rewrite the index by that rule, keeping each folder line's description; say which lines changed |
| links | every relative link resolves inside this repository | report; fix only a link broken by a move you can see in Git history |
| resources | every `resource` and `sources[].resource` under `contents/` resolves when its mount is present; when the mount is absent, report "not checked" | never create anything under `contents/` |
| orphans | a page that nothing links to and no index lists | report |
| lifecycle | `stale_after` in the past; a `deprecated` page still linked as current | report |
| vocabulary | every `type` and every `rel` is declared in `.property/property.json` and none is in its `avoid` list; a relation goes from a type and to a `page` or `url` as its declaration says; no page lists the same relation twice | replace an avoided name with the entry listed for it and drop an exact duplicate; report the rest, including a name listed with "a link", whose fix is prose |
| skills | every `Knowledge_Base/` path a skill names resolves to a page that is not `deprecated`; a skill does not state a criterion, fact or reason that a page states or should state (AGENTS.md, Skills and knowledge) | repoint a path to the page its `deprecated` stub links to; report the rest; propose moving a stated claim to a page, never move it yourself |
| identity | two `person`, `org` or `project` pages that name the same identity | propose a merge with a `deprecated` stub |
| contradictions | two pages that state incompatible claims; check by re-reading the cited sources, not by comparing the pages | report both, with the source that supports each |
| size | a page over 800 lines; a folder index over about 150 entries | propose a split |
| category rules | from `category` in `.irori/scope.json`: organization, a `stable` page in `wiki/` without `verified`; team and organization, a concept without `sources`; any other category, or none, adds no rule | report as an error |
| property declaration | `.property/property.json` parses; every key a type requires is a declared property; every `default` is one of its `options`; every `heading` is unique; `avoid` names no declared entry | report; never edit the file outside a vocabulary review |
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
types and `rel` names in `.property/property.json` only as far as the person
agrees, and
leaving the vocabulary as it is is always an acceptable outcome.

1. **Evidence.** From the indexes and the pages' frontmatter: pages per `type`
   and per `rel`, values outside the vocabulary, and index lines whose
   description does not fit the type they are filed under; by searching the
   bodies, the same relationship written as prose on several pages.
2. **Propose one change** with the whole vocabulary in view, including the
   `avoid` list: sharpen a definition the evidence shows is misread, add an
   entry the evidence needs and no entry covers, fold a name in use outside the
   vocabulary into the entry that already means it, merge two entries that mean
   the same, or retire one that no page uses although its folder exists here.
3. **Check it before anyone sees it.** Discard it and propose a different one,
   keeping the discarded one in view so it does not come back, unless it differs
   in meaning, not only in spelling, from every entry and every avoided name;
   it names the pages it would change (at least three for a new entry) or the
   recurring question it answers; and every page stays conformant without
   moving. A proposal is never kept because others were discarded.
4. **Stop** after three kept proposals, or sooner when new ones repeat earlier
   ones.
5. **Show** each kept proposal with its evidence and its exact edits: the
   entries in `.property/property.json`, the pages whose `type` or `rel` changes and the index lines
   that move. The person accepts or turns down each one.
6. **Apply** what was accepted as one change, add to `avoid` each name the change folded or merged away and each new name the person
   turned down, with what to use instead, and run the other checks on the
   result. In a team or organization scope the change is a branch and a pull
   request, which you do not merge.

## irori graph (`lint --irori-graph`)

irori generates the graph index, `Knowledge_Base/ontology/`, from the pages'
`type`, `title` and `relations` when the person presses **索引を更新** in its
ontology panel (**グラフ索引を更新** before 0.1.86), and the person commits it,
so every device shows the same graph. Never write or edit those files. On
request, check that the index is current:

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

Do not rewrite prose. Do not delete a page; deprecate it. When you change a
page, set its `generated` to yourself and now; leave it alone on a page you did
not change.
