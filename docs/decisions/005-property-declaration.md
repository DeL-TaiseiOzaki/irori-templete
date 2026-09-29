# ADR 005 — Free folders, declared properties, and `generated` as the last change

Date: 2026-09-29. Status: accepted by the owner in design. Builds on ADR 004.
Supersedes the vocabulary's place in `AGENTS.md` (ADR 003 D1, D3 and D5, whose
procedure stays) and ADR 002's rule that a page's `generated` changes only with
its content. Nothing here has been exercised against a real knowledge base.

## Context

Frontmatter carries everything the bundle knows about a page, but writing it as
YAML by hand is tedious, and the owner wants irori to show it as properties
above the page, the way Notion does. A property editor needs to know which keys
exist, which values a select offers, and which keys a type requires. Until now
that lived only in `AGENTS.md` tables written for people and agents, which
irori cannot read.

The owner also settled what is free and what is not (2026-09-29):

- the contents of each layer's folders are free; this template designs a folder
  set, and irori thinks only in the three layers;
- frontmatter is required on every page of the knowledge layer;
- `generated` means who touched the page last, leaning on OKF;
- files in `contents/` are free, Markdown included, and carry no required
  frontmatter.

## Decisions

### D1 — Folders are free

The folder set per category is this template's design, which `init` creates. A
person may add folders, subject folders included, and rename or drop the ones
they do not use; the **Folders** block declares what the knowledge base uses.
What a page is comes from its `type`. Every folder still has an `index.md`, and
`lint` writes one for a folder a person added without it, so index-first
navigation reaches every folder.

### D2 — The vocabulary is declared in `.property/property.json`

One JSON file in the schema layer declares the page properties (key, kind,
options, default), the keys every page requires, the page types (meaning, index
heading, required keys), the `rel` names (meaning, from which types, to a page
or a URL) and the names to avoid, each as `{ "name", "use" }`. `AGENTS.md` explains how to read it and no
longer repeats it; `init`, `journal`, `ingest` and `lint` read it; a vocabulary
review edits it and nothing else does.

It is a dot directory at the repository root because irori classifies a
top-level dot entry as schema (`src/domain/scopes.ts`), while a top-level
`property.json` would fall into the knowledge layer. JSON rather than YAML
matches irori's other declarations (`.irori/notes.json`,
`.irori/cloud-mounts.json`). It lives outside `.irori/` because it is the
knowledge base's contract, read by every agent, not a setting for irori. The
directory leaves room for later declarations, such as per-type templates.

Property kinds: `type`, `text`, `select`, `multi-select`, `datetime`,
`actor-time` (`{ by, at }`), `actor-time-list`, `link` (a `contents/` path, a
page or a URL), `sources` and `relations`. irori's page-properties design owns
how each is shown and edited; this template only declares them.

### D3 — `generated` is the last change

`generated` names whoever last changed the page, its body or its properties,
and when. An agent that changes a page names itself; irori names the person
when they save. Retyping a page in a vocabulary review changes `generated` too.
A journal entry an agent types for a person in their own words keeps naming the
person.

### D4 — Frontmatter is required in the knowledge layer only

Every `.md` in `Knowledge_Base/` other than `index.md` has frontmatter with the
declared required keys (OKF §11 for `type`). Skills keep their own `SKILL.md`
frontmatter. Files in `contents/` are free.

## Consequences

- An agent reads one more file before writing a page. The file is small and
  the same for every scope.
- A person who edits `.property/property.json` by hand bypasses the vocabulary
  review; lint's `property declaration` check reports a broken file but cannot
  tell a reviewed change from an unreviewed one. Git history can.
- Personal scopes see organization types (`policy`, `standard`) in the file.
  Pruning per category was not done: the vocabulary stays the same in every
  scope, which keeps promotion simple.

## Open

- How irori shows each kind, what it does when the file is absent or broken, and
  when it sets `generated`: irori's design.
- Per-type page templates under `.property/`.
