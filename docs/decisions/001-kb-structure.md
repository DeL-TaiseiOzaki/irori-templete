# ADR 001 — Knowledge base structure for the irori main-KB template

Date: 2026-09-16. Status: structure confirmed by the owner and implemented in
this repository. One decision (D9) depends on unimplemented irori work and is
marked. Nothing here has been exercised against a real knowledge base.

## Context

The owner's organisation picture is fixed: a member's personal vault feeds a
project sharing filter, a project KB feeds a company-wide filter, and an
enterprise KB is referenced back on members' machines. Deliverables live in each
project's Google Drive, not in a KB.

A prior investigation produced "structure G" for `claudian-orchestra-template`
running under the irori VS Code extension. That checkout no longer exists and
irori, the desktop application, is now the runtime. The role split G recommended
survives; the container, the ontology format, the provenance owner and the
promotion mechanism are re-fitted here against what irori actually implements.
Cited line numbers are in the sibling `irori` repository at this date.

## Decisions

### D1 — One template for all three categories

A single template serves personal, team and organization KBs. irori's `Category`
is chosen when a directory is registered (`src/domain/types.ts:9`), so the
structure must not encode it. The three differences are stated as conditions in
`AGENTS.md`, not as different folders:

- a team or organization `journal/` holds meeting records, decisions and weekly
  summaries; members' personal dailies are not carried up;
- the authoritative record of a shared identity belongs to the highest scope that
  holds it, and lower scopes keep a `same_as` reference;
- ontology consistency is a warning in a personal KB and a rejection above it.

### D2 — The knowledge layer is wrapped in `Knowledge_Base/`

irori classifies a path as `schema` (`.irori`, `.claude`, `.codex`, `.opencode`,
`.pi`, `.agents`, `.cursor`, `.gemini`, `.hermes`, `schema/`, and root
`AGENTS.md`/`CLAUDE.md`/`.mcp.json`/`opencode.json[c]`), as `contents` when the
path is a declared contents root, and as `Knowledge_Base` otherwise
(`src/domain/scopes.ts:12-33`). A literal folder is therefore not required, but
irori's new-note dialog defaults to `Knowledge_Base/Notes`
(`src/app/main.tsx:270`, `src/host/files.ts:361`) and the extension's design
documents use the same name. The wrapper is kept so the default lands inside the
template's structure rather than beside it.

### D3 — Role folders, decided at a note's birth

```text
Knowledge_Base/
├── journal/<year>/      # records tied to a date; append-only
├── library/             # reusable claims; flat, type in frontmatter
├── entities/{person,org,repo}/
├── Notes/               # landing place for hand-written notes (D6)
├── ontology/{entities.csv,relations.csv}
├── templates/
└── attachments/
```

A folder answers one question — is this a dated record, a reusable claim, an
identity record, or an unsorted capture — and the answer does not change when a
note matures or is promoted. Subject, note type, status and audience live in
frontmatter. Subject subfolders inside `library/` are not created.

### D4 — The ontology graph is the only navigation surface

No `maps/` directory. irori has no backlinks, note properties or saved queries,
and its KB search is a literal substring search, so a hand-written map index
would be a second thing to maintain with no mechanism keeping it honest. The
declared ontology, which irori renders as a filterable graph with links to the
associated note, is the entry point instead.

The `note` column of an ontology entity may address any Markdown file in the
knowledge layer. Rows therefore cover more than identities: a `topic` or
`project` row points at a `library/` note. The `entities/` folder stays narrow
and holds only identity records for `person`, `org` and `repo`.

### D5 — Ontology format follows irori's declaration

`.irori/ontology.json` declares `Knowledge_Base/ontology/entities.csv` and
`Knowledge_Base/ontology/relations.csv` with explicit column mapping. The single
`ontology.csv` of the earlier research is not used. The reader requires each
declared path to resolve inside this KB's knowledge layer and to not be an alias
(`src/host/ontology.ts:19-25`), which the paths above satisfy. The template ships
seed rows so the graph view is populated on first open.

### D6 — `Knowledge_Base/Notes/` is the landing place, and is emptied

irori's new-note default is fixed, so the template provides the directory it
points at. `Notes/` holds hand-written captures until the `distill` skill files
them into `journal/` or `library/`. This is close to the maturity-stage folder
the earlier research rejected, and it does make moving a file part of normal
operation. It is accepted because the alternative is either an unconfigurable
default writing outside the structure, or asking the user to retype the
directory on every note.

### D7 — irori owns artifact provenance; the KB owns note derivation

Which note produced which deliverable is answered by irori's run and artifact
records and its source/artifact navigation. Those records are device-local and
deliberately not exported into Git, and the template does not keep a parallel
tracked `manifest/`. Note-to-note derivation is different in kind and is not
covered by those records, so `derived_from` stays in frontmatter.

### D8 — Frontmatter contract

`id` (`<scope>/<ULID>`, generated by the writing agent), `title`, `type`,
`status`, `created`, `sensitivity`, `derived_from`. No `updated`, because Git
holds it, and no `tags`, because nothing in irori can query them. irori does not
read frontmatter identifiers today; this contract is for people and agents.

### D9 — One canonical skill set in `.agents/skills/` — depends on irori

Skills are written once as `.agents/skills/<name>/SKILL.md` and are not
duplicated per runtime. Five skills: `capture`, `journal`, `distill`, `entity`,
`promote`.

This follows [claudian](https://github.com/YishenTu/claudian), which owns a
provider-neutral store at `AGENT_SKILLS_ROOT = '.agents/skills'`
(`src/core/skills/AgentSkillRepository.ts`) and lets each provider resolve from
it — Codex is asked for its own repository-scope skills over the app-server RPC
(`src/providers/codex/skills/CodexSkillListingService.ts`) rather than being
given generated files.

**Dependency, now implemented in review.** Claude Code reads only
`.claude/skills`, so without host support the skills here would be invisible to
three of irori's four harnesses. The owner chose to implement that in irori
rather than commit duplicate copies here, and irori pull request 27 does it: the
composer lists `.agents/skills/` packages and prepends the chosen one to the
request, so the same skill reaches Codex, Claude Code, OpenCode or Pi. Until that
pull request is merged and released, only Codex — which reads the directory
natively — sees these skills in an installed build.

### D10 — What the template must not ship

`.irori/scope.json` is never committed. irori generates it with a random UUID at
registration (`src/host/files.ts:127-175`), a duplicate `scopeId` makes a second
KB from the same template unregisterable, and incoming Git changes to that file
abort a pull by design (`src/git/service.ts:481`). The shipped `.gitignore`
therefore carries `/contents/` and `/.irori/scope.json`.

`.obsidian/` is not shipped either: it is not in irori's schema list, so it would
be classified as knowledge and appear in the KB's own search results.

### D11 — Promotion is separate repositories, not submodules

An irori workspace registers several independent KB repositories side by side and
each remains its own Git repository. A personal, project or enterprise KB is a
separate registration, and the sharing filters of the organisation picture are
pull requests against the receiving repository. The submodule arrangement of the
earlier research is dropped.

Promotion copies: the receiving scope gets a new note with its own `id` and a
`derived_from` pointing at the source. Nothing is moved and nothing is linked
across repositories by path.

### D12 — The root contract is shipped content, not contributor guidance

A person creates their knowledge base from this repository, so the repository's
files are the knowledge base's files. Root `AGENTS.md` is therefore the operating
contract that ships and that irori's agents read in the user's KB, and root
`CLAUDE.md` points at it. Guidance for developing the template moved to
`docs/CONTRIBUTING.md`, which states the distinction so the two do not merge back
together.

No `.agents/rules/` directory. The earlier research placed rules beside skills,
but `AGENTS.md` is already resident in every runtime and a second always-read
location competes with it. Detail that only matters while performing a task lives
in the skill that performs it.

## Open

`AGENTS.md`, the five skills, the ontology seed and `GETTING-STARTED.md` are
written. None of it has been exercised against a real knowledge base, and two
things should be reviewed once it has been:

- whether `Notes/` survives contact with use, or whether the in-tray simply fills
  up. D6 records why it is there, so the review has something to argue with.
- whether the ontology alone is enough to find things again without a map index
  (D4), or whether a person ends up keeping one somewhere anyway.

D9's host support exists in irori pull request 27 but is not merged or released.
Until it ships, only Codex sees these skills in an installed build.
