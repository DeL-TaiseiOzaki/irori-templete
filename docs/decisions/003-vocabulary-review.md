# ADR 003 — The vocabulary changes through reviewed proposals

Date: 2026-09-22. Status: proposed. Keeps ADR 002 and adds to its D2, D4, D7,
D9 and D10.

## Context

ADR 002 fixed a table of page types and made `relations` an extension key, but
said nothing about how either changes. OKF registers no types (SPEC §4.1) and
its consumers tolerate unknown ones, so nothing stopped a knowledge base from
drifting: an agent meeting a page no type fits invents a type, and `uses`,
`depends_on` and `relies_on` come to mean the same thing on different pages.
`lint` already proposed identity merges and folder splits; the vocabulary had no
such path. `uses` was also left undefined beside `sources`, so an agent could
not tell which of the two to write.

The owner pointed at EvoAgent (Yuan et al., arXiv 2406.14228v3, 2025) as a
method to learn from. It generates task-specific expert agents rather than
maintaining an ontology: from one human-written agent, a model writes one new
expert description per round with the earlier ones in view, a check keeps it
only if it is distinct from them and able to help, and its answer is merged
into the current one critically. Its evidence is about answers to tasks, and
it is thinner than its framing: the check shows an effect only when several
candidates are proposed at once, and none with one candidate per round, where
the released code re-asks an unchanged prompt at temperature 0 and keeps the
fifth candidate whatever the check says. Nothing carries over from one task to
the next. The reading is kept in the development workspace
(`_research/ontology-evolution-2026-09-22/`).

## Decisions

### D1 — The relation vocabulary is declared

`AGENTS.md` lists the `rel` names, each with a meaning and a direction,
starting from the two the contract already used. `uses` is a dependency
between what two pages describe; what a page was written from stays in
`sources`. A relationship none of the names covers is an ordinary link whose
sentence says what it is, which any OKF consumer reads (SPEC §6.1).

### D2 — Pages use the vocabulary and do not extend it

When nothing fits, `ingest` takes the nearest type, or an ordinary link, and
says so. A name invented while writing is exactly the unchecked candidate the
review exists to stop.

### D3 — The vocabulary changes through `lint --vocabulary`

One proposal at a time, written with the whole vocabulary and the names not to
use in view; checked before the person sees it for a distinct meaning, for
evidence (three pages for a new entry, or a recurring question) and for pages
that stay conformant without moving; at most three per review; shown with its
exact edits and accepted or turned down on its own; applied as one change and
re-checked, and in team and organization scopes opened as a pull request.

What is taken from EvoAgent is the shape of the loop: one candidate with the
existing set in view, a distinctness and usefulness check before anything is
integrated, and integration that may keep what is there. The check is our
choice rather than a finding, since the paper measured no effect of it in this
configuration, and it is built to work where the code's did not: a discarded
candidate stays in view, nothing is kept by default, and a person decides. The
cap of three keeps a review to one sitting; the stop that matters is proposals
repeating one another. Not taken: autonomy, since a model's judgement is not
`verified`, and a population discarded after each task, since here the
vocabulary and the names not to use persist in `AGENTS.md`: those the person
turned down and those a review folded into an entry. They are the only record
kept, because they are what stops a rejected name returning and lets lint
replace a folded one mechanically.

### D4 — Promotion checks the receiving vocabulary

Two scopes' vocabularies can now differ, so the pre-promotion check requires
the page's `type` and `rel` names to exist in the receiving scope, and
`promote` either takes the nearest entry there and says so or stops.

### D5 — Each type names its index heading

The decided one-section-per-type rule was not what `init` created: journal
years had only `# Entries`, team entities lacked `# Projects` for the first
identity page, and no wiki had `# Syntheses` for answers filed by `query`.
Personal identities and organization policies also shared headings across
types. The organization's `# Patterns and lessons`, `# Glossary` and
`# People and teams` named no type, so they became type headings; patterns,
lessons and glossary entries are `concept` pages. Separate types remain a
proposal through `lint --vocabulary`.

The **Page types** table in `AGENTS.md` now names each type's index heading.
The folder rule uses those headings in table order, adds a heading with the
first page of its type, and keeps year folders under `# Years`. `init` declares
directories and types and creates their headings from that table; `journal`
does the same for new years and files entries under their type's heading.
`lint` adds a missing type heading while keeping existing headings, order and
prose. A new type proposed in a vocabulary review must name its heading.

## Consequences

- A review costs an agent turn over indexes and frontmatter and one decision
  per proposal from the person.
- Nothing here is evidence that the review works. What would show it: no two
  names with the same meaning after months of use, listed names neither
  proposed nor written again, and the proportion of proposals kept and accepted.

## Open

The thresholds (three pages, three proposals), and whether a large ingest
should end by suggesting a review.
