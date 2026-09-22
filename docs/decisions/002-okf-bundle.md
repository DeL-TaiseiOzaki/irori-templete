# ADR 002 — Knowledge_Base as an OKF bundle maintained by agents

Date: 2026-09-17. Status: accepted by the owner in design and implemented in
this repository. Supersedes ADR 001 D3, D4, D5, D6, D8 and D9; keeps D1, D2,
D7, D10, D11 and D12. Nothing here has been exercised against a real knowledge
base.

## Context

ADR 001 built a knowledge base that people file by hand: role folders decided at
a note's birth, an ontology CSV as the only map, ULID identifiers, and skills
that move notes out of an in-tray. On 2026-09-17 the owner judged it unfit and
asked for a redesign grounded in current practice for knowledge bases that
agents maintain, and in Google's Open Knowledge Format (OKF).

That practice converges on one lineage. Karpathy's LLM wiki (2026-04) keeps
immutable raw sources, an agent-owned wiki of Markdown pages and a schema
document, with three operations, ingest, query and lint, and `index.md` as the
catalogue. OKF 0.1 (2026-06-12) "formalizes the LLM-wiki pattern into a
portable, interoperable format"; OKF 0.2 (2026-07) adds provenance, trust and
lifecycle fields for knowledge that agents rewrite. Producers and consumers
exist outside Google: LangChain OpenWiki emits OKF 0.2 and two Obsidian plugins
validate it. Operating reports agree on per-folder indexes past about 150
pages, mandatory source citations so a wiki does not read its own output,
scoped edits agreed before writing, and periodic lint. The research is kept in
the development workspace (`_research/kb-practices-2026-09-17/`); this ADR
keeps the decisions.

irori's constraints did not change: three layers by path
(`src/domain/scopes.ts`), skills read from `.agents/skills/` and prepended to
the request (irori 0.1.6), substring search, a graph drawn from a declared CSV
pair (`src/host/ontology.ts`), a fixed new-note directory
(`src/host/files.ts:361`), and device-local run and artifact records that are
deliberately not exported (`docs/KNOWLEDGE-NAVIGATION.md`). Cloud mount names
are portable through `.irori/cloud-mounts.json`
(`docs/WORKSPACE-CONNECTIONS.md`).

## Decisions

### D1 — Files and knowledge are split by kind

`contents/` holds files: source material, deliverables and incoming items,
mounted from cloud folders and never committed. `Knowledge_Base/` holds
knowledge: what was learned, what was decided, and one page per file that
matters. Source material is never a folder inside `Knowledge_Base/`; what a
file taught us is a `reference` page bound to it. (Owner premise, 2026-09-17.)

### D2 — `Knowledge_Base/` is one OKF 0.2 bundle

The bundle root is `Knowledge_Base/`, which OKF allows ("a subdirectory within a
larger repository", SPEC §3). Every page except `index.md` carries frontmatter
with `type`; the root index carries `okf_version: "0.2"`. Consumers that speak
OKF read the knowledge base without translation. Rejected: the bespoke
frontmatter of ADR 001 D8, which carried no provenance, trust or description
and which no external tool read; and making the repository root the bundle,
which would force frontmatter onto `README.md`, `AGENTS.md` and `docs/`.

### D3 — Identity is the path; links are relative

A page's identity is its path in the bundle (SPEC §2). ULIDs and the `<scope>`
prefix of ADR 001 are dropped: they were opaque to people and agents, a real
agent turn composed one by hand, and a cross-scope reference needs a URL in any
case. A renamed or merged page leaves a `status: deprecated` stub, as OKF's own
sample bundle does. Links are relative Markdown links because the bundle root
is not the repository root; a page in another scope is referred to by its
GitHub URL.

### D4 — `index.md` in every folder is the navigation; there is no `log.md`

Agents read the root index, then a folder index, then pages (SPEC §8, Karpathy,
and Taktile's production wiki). Producers keep indexes current; `lint` repairs
drift additively. The ontology graph of ADR 001 D4 is no longer the map; it can
be generated on request (D8). There is no `log.md` and no work diary (D5).

### D5 — Files and knowledge are joined by pages, not by a timeline

Work with an agent yields files and knowledge, many to many. The relation is
recorded on knowledge pages: an `artifact` page per deliverable (`resource`
names the file, `sources` what it was made from, the body the background, the
work and the decisions); a `decision` page when the outcome is a decision; and
`sources` on every derived page. No chronological log is kept. W3C PROV records
derivation without the activity; OKF makes `log.md` optional and carries time
only as `generated.at`; Git holds history. The join is written by the CLI agent
working in irori, under the standing rule in `AGENTS.md`, not by irori; irori's
device-local run records stay device-local. (Owner decision, 2026-09-17.)

### D6 — Files are named by their path under `contents/`

`resource` and `sources[].resource` name a file as `contents/<mount>/<path>`,
relative to the repository root. The mount name is recorded in the tracked
`.irori/cloud-mounts.json`, so everyone who attached the same folder resolves
the same path. Absolute machine paths are never written. `hash` may be added
when the exact version matters; irori's reconnection follows a moved file by
hash on the device. Rejected: Drive URLs, in favour of the mounted path the
owner chose; irori's device-local source ids, which are not shareable.

### D7 — One template, three folder sets, created by `init`

The category chosen at registration determines the folder set:

| Category | Folders |
| --- | --- |
| personal | `journal/<year>/`, `wiki/` |
| team | `journal/<year>/`, `decisions/`, `wiki/`, `entities/` |
| organization | `entities/`, `policies/`, `wiki/` |

The template ships only the contract, the skills and the root index; the `init`
skill creates the folders, their indexes and the **Folders** block in
`AGENTS.md`. A folder answers who writes there and under what discipline;
`type` distinguishes pages within it. Rejected: one folder set for all three
(ADR 001 D1 and D3), because practice differs (personal vaults are journal-led,
project wikis are decision-led, organization bases are curated catalogues).
Deferred: three template repositories or branches; `init` is one skill and
irori already knows the category.

### D8 — The ontology CSV is a generated view

irori draws a graph from a declared CSV pair. The template no longer ships
`.irori/ontology.json` or `Knowledge_Base/ontology/`; `lint --irori-graph`
generates both from identity pages and `relations`, so the graph is available
without a second thing to maintain. This changes the 2026-09-11 premise that
the ontology CSV is hand-kept, and is recorded here for the owner to confirm.

2026-09-22, decided (irori ADR 008): irori 0.1.24 and later generate
`Knowledge_Base/ontology/` deterministically from the pages, with no
declaration, and the person commits it so every device shows the same graph.
`lint --irori-graph` checks that it is current and no longer writes it. A
declared `.irori/ontology.json` still wins, for tables people maintain.

### D9 — Six skills form the loop

`init`, `ingest`, `query`, `lint`, `journal` and `promote`. The `capture`,
`distill` and `entity` skills of ADR 001 are absorbed into `ingest` and `lint`.
Skills read the **Folders** block rather than assuming paths, so one skill set
serves all three categories.

### D10 — Promotion raises the trust tier

Promotion copies a page into the receiving repository with `sources[0]` naming
the source page, `status: draft` and a pull request; the reviewer's
`verified: [{ by: human:<id> }]` and `status: stable` are the sharing filter.
`sensitivity`, an extension key, remains the disclosure input, because OKF
trust tiers are advisory and not access control (SPEC §5.3).

## Consequences

- irori needs no change for the format. irori 0.1.8 adds `.irori/notes.json`,
  a tracked declaration of the new-note directory and of today's note (path
  with date tokens plus a template), which `init` writes for the category; that
  is what makes `journal/` the place hand-written notes land. Drawing the graph
  from the bundle was decided on 2026-09-22: irori 0.1.24 generates the graph
  index from the pages (D8).
- OKF is a 0.2 draft from one vendor and renamed a field within three months.
  Extension keys are kept to two (`sensitivity`, `relations`) and the version is
  declared in the root index.
- `index.md` maintenance is the recurring cost; it is what `lint` is for.

## Open

Whether the three folder sets hold up in use; whether 150 entries is the right
threshold for splitting a folder; and whether substring search suffices past
about a hundred pages. D8 was settled on 2026-09-22.
