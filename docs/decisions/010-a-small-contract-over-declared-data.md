# ADR 010 — A small contract over declared data

Date: 2026-11-15. Status: owner decision. Amends [ADR 001](001-kb-structure.md)
D12 (no `CLAUDE.md`), [ADR 003](003-vocabulary-review.md) D3 (the review is its
own skill and opens no pull request of its own),
[ADR 006](006-one-layout.md) D1 (the category sets no discipline) and D5 (the
index rule is irori's), [ADR 007](007-free-categories.md) (no category has a
discipline), and [ADR 009](009-the-ontology-is-the-declaration.md) decision 4
(no reading fallback outside irori). Nothing here has been exercised against a
real knowledge base.

> ADR 011 adds the `review` skill and moves the `stale_after` check from `lint`
> to it; D6's one procedure per skill stands.

## Context

On 2026-11-15 the owner asked for a more refined template: simpler, more
concise and easier to extend. Read against that, the shipped template had
grown by accretion over nine decisions:

- `AGENTS.md` was 424 lines. Beside the contract it carried irori's
  implementation detail (the byte-level index rule, the `RETIRED.md` format,
  limits on skill metadata) and at least seventeen notes of the form "irori
  0.1.86 and later". README and `lint` carried migrations from earlier layouts.
- The same facts were stated in several places. The keys a type requires were
  in `.property/property.json`, the frontmatter section of `AGENTS.md`, the
  `lint` table and `CONTRIBUTING.md`; "a team `concept` needs `sources`" was in
  `AGENTS.md` and `lint` although the declaration already required `sources` of
  every `concept`.
- Where pages go was prose. `journal/` and `wiki/` were named 26 times in the
  skills, so adding a type with its own folder meant editing `AGENTS.md` and
  four skills.
- The category's discipline lived in a table in `AGENTS.md`, a row in `lint`
  and a branch in `init`.
- `lint` held three procedures: the health check, the pre-promotion check and
  the vocabulary review.
- The current design could be assembled only by reading ADR 001 to 009 with
  their partial supersessions.

irori already owns most of what the template restated. It generates the
indexes (irori ADR 028) and applies the declaration (irori ADR 060), and
agents running inside it have `irori index` and `irori check`. It reads its
declaration with a schema that keeps unknown keys (`src/domain/properties.ts`)
and names no folder of the knowledge layer itself. Claude Code reads
`AGENTS.md` from 2.1.277, and irori gives it to an older build or one with
hooks off as text (irori ADR 058) and links `.agents/skills/` for it (irori
ADR 049).

The owner settled three questions: operation outside irori is no longer
supported, so the template stops restating irori's rules; team and
organization operation is left to each user for now; and `CLAUDE.md` is not
needed by any CLI the knowledge base will be used with.

## Decisions

### D1 — One owner per fact; `AGENTS.md` holds the invariants

| Fact | Owner |
| --- | --- |
| what may exist: properties, types and their folders, relations | `.property/property.json` |
| how irori behaves: index bytes, note settings, skill limits, versions | irori's own documentation, cited and not restated |
| how a task is done | the skill |
| what always holds, and the boundaries | `AGENTS.md` |
| how to start and extend | `README.md` |

`AGENTS.md` keeps the layers, the page invariants, how to read the vocabulary,
that indexes are generated, the file-to-page join, how skills relate to pages,
and what agents must not do.

### D2 — irori is the runtime

The template needs irori 0.2.66 or later and says so once, in README. Skills
run `irori index` after writing pages and `irori check` to apply the
declaration; `lint` no longer re-implements either by reading. The index rule
is cited from irori ADR 028. Shipped files carry no per-version notes; how to
bring an earlier knowledge base up to date is in `docs/UPGRADING.md`.

### D3 — No `CLAUDE.md`

Every CLI irori runs reads `AGENTS.md`, so a second entry file only duplicates
it. The repository ships `AGENTS.md` alone.

### D4 — A type declares its folder

Each type in `.property/property.json` carries `folder`, where its new pages are
created: `journal` for `journal`, `wiki` for the rest. Skills and `init` read it
instead of naming folders, so adding a type with a folder of its own is one
entry in the declaration. It decides where a page is created, not where it may
be: what a page is still comes from its `type`, and a person may move pages and
add folders. irori ignores the key today.

`init` therefore writes only the identity page, in its type's folder, and lets
`irori index` write the indexes. A folder comes into existence with its first
page; the description on its line in the parent index is the only index text
written by hand.

### D5 — The category is a label

The template attaches no rule to `personal`, `team`, `organization` or a name of
the knowledge base's own. `init` reads it only to guess who owns the knowledge
base. `lint` has no category rules, and a knowledge base that wants pages
confirmed before they count writes that as a `policy` page. `verified`,
`sensitivity` and `promote` stay, since copying a page into another knowledge
base needs them whatever the category.

### D6 — One procedure per skill

The vocabulary review leaves `lint` as the `vocabulary` skill, unchanged in
substance and without its own pull request rule. The pre-promotion check moves
into `promote`, the only skill that uses it. `lint` is the health check alone.

### D7 — Template-owned and knowledge-base-owned files

`AGENTS.md` and the shipped skills belong to the template: a newer template can
be copied over them. The declaration, the pages and `.irori/` belong to the
knowledge base. Rules of the knowledge base's own are `policy` pages, which
carry sources, can be checked and can be promoted, instead of edits to
`AGENTS.md`.

### D8 — The current design is one document

`docs/DESIGN.md` states the design as it is now. The ADRs stay as the record of
how it got there; a later change updates `DESIGN.md` and adds an ADR.

## Consequences

- `AGENTS.md` goes from 424 to about 150 lines, and the shipped Markdown and
  JSON from about 1,020 to about 600.
- irori's `src/domain/folder-index.ts` describes its rule as "the one
  irori-templete states for its `lint`". The rule's owner is now irori; that
  comment should say so.
- A team or organization knowledge base loses lint's `verified` check on
  `stable` pages until it writes a policy for it. A project's or
  organization's new notes go to its identity page's folder, not
  `journal/<year>`.
- An existing knowledge base keeps working: irori ignores the new `folder` key,
  and a declaration without it leaves agents to the folders they see. Bringing
  it up to date is a copy and three edits (`docs/UPGRADING.md`).
- Not done here: no change to the nine types or four relations, which still
  wait for evidence from use.
