# ADR 004 — Knowledge is the center; skills apply it, contents hold its files

Date: 2026-09-29. Status: accepted by the owner in design. Keeps ADR 002 and
sharpens its D1; adds a check to `lint`. Nothing here has been exercised against
a real knowledge base.

> ADR 006 folds `synthesis` into `concept` and `standard` into `policy`.

## Context

ADR 002 D1 split files from knowledge "by kind, not by importance" and left the
schema layer as the place for the contract and the skills. It did not say how a
skill differs from a page, and the two drift together: know-how, such as how a
proposal is reviewed, reads as a procedure and as a set of criteria at once.
Written into a skill, the criteria have no `sources`, `verified` or
`stale_after`, lint does not see them, and promotion cannot share them.

irori 0.1.49 and later make both easier to write. Its Schema settings define a
skill by name, description, instructions and attached files, and offer an
`AGENTS.md` in any knowledge folder. Attached files are a natural home for
reference material, and an `AGENTS.md` under `Knowledge_Base/` has no
frontmatter, which breaks OKF 0.2 conformance: every non-reserved `.md` file in
the bundle needs frontmatter with a non-empty `type` (SPEC §11).

On 2026-09-29 the owner stated the relation they intend: knowledge is the
center, the most important information of this world, used by people and agents
alike, tied to contents, accumulating what must be understood in that
knowledge base; a skill is knowledge made into a concrete procedure; what a
procedure produces goes to contents.

## Decisions

### D1 — Knowledge_Base is the center

`AGENTS.md` describes `Knowledge_Base/` as what anyone working in the scope must
understand, and the other two layers as serving it: the schema turns knowledge
into procedures, and `contents/` holds the files knowledge is about, including
what those procedures produce. This replaces "by kind, not by importance".

### D2 — Contents and knowledge are split by authority

A file whose original lives elsewhere is in `contents/`, whatever its format; a
document edited together in a cloud folder is contents although it is text.
What it taught us, what it is, and what was decided about it is a page, and the
scope answers for whether the page is true. A page states its meaning without
its file at hand, because a mount may be absent on a device and a promoted page
leaves `contents/` behind; a page that only points at a file is a catalogue
entry. The rule that a `reference` page is written only when knowledge comes
out of a file stays as ADR 002 set it.

### D3 — A skill applies pages and does not restate them

Three questions place a text: whom it addresses (an agent, or anyone reading
about the world), how it goes wrong (a bad outcome, or becoming false or out of
date), and whether a person reading it learns something. Know-how is split: its
criteria, facts and reasons go on a page, and the skill names that page by its
repository path and says how to apply it. Files packaged with a skill are tools
for the procedure, such as a template or an output format, not reference
material.

### D4 — No `AGENTS.md` inside the bundle

Rules for working in one folder belong in the root `AGENTS.md`. `lint` reports
an `AGENTS.md` under `Knowledge_Base/` as a conformance error and proposes
moving its rules to the root.

### D5 — `lint` checks the pages skills name

A new `skills` check: every `Knowledge_Base/` path a skill names resolves to a
page that is not `deprecated`, and a path to a deprecated stub is repointed to
the page the stub links to. A skill that states a criterion, fact or reason a
page states or should state is reported with a proposal to move the claim; lint
never moves it.

## Consequences

- A skill that encodes a practice needs a page to exist first, or the same
  change that adds the skill adds the page.
- irori's Schema settings still offer an `AGENTS.md` in a knowledge folder. A
  knowledge base made from this template gets a lint error for one; whether
  irori should stop offering it for an OKF bundle is irori's decision.

## Open

- Personal and team scopes have no `standard` or `policy` type; their criteria
  go on `decision` or `concept` pages. Whether they need one is a question for
  `lint --vocabulary` once such pages exist.
- Whether a `synthesis` page reused as a procedure should suggest a skill, the
  opposite direction of D3.
