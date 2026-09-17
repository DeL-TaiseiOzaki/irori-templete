# Getting started

What to do with this repository once you have your own copy of it, and what the
first week looks like.

## 1. Make it yours

1. Create your repository from this template and clone it.
2. Decide the scope's identity. It is a slug you will see in every note's `id`:
   `p-<handle>` for a personal vault, `pjt-<slug>` for a project, `ent` for an
   organization. Nothing enforces it; consistency is the point.
3. Open `Knowledge_Base/entities/person/self.md`, replace it with your own
   record, and change the `label` of the `self` row in
   `Knowledge_Base/ontology/entities.csv`.

## 2. Register it in irori

1. Open or create a workspace, and add this directory as a knowledge base.
   Choose **personal**, **team** or **organization** — the choice affects a few
   rules in `AGENTS.md`, not the structure.
2. irori writes `.irori/scope.json` with a random identifier. It is already in
   `.gitignore` and must stay there: a shared `scopeId` makes a second copy of
   this template unregisterable, and an incoming change to that file stops a
   pull.
3. Confirm the layers look right. The left navigation should show `AGENTS.md`
   and `.agents/` under the schema pane, `Knowledge_Base/` under the knowledge
   pane, and nothing under contents yet.

## 3. Connect the exchange surface

Use **クラウド接続** in the header, or **接続** in the workspace's Google Drive
section, to attach the folder this scope exchanges files through. Inside it,
keep three folders:

- `Inbox/` — things arriving that you have not dealt with
- `source/` — references you did not write and will not edit
- `output/` — deliverables

Deliverables live there, not in `Knowledge_Base/`. irori records which run
produced which file, so you do not track that yourself.

## 4. Check the ontology loads

Select the knowledge base and open **オントロジー**. You should see two nodes,
`Me` and `How this knowledge base works`, with one edge between them. Click a
node to reach its note.

If irori reports an error instead, the CSV is rejected as a whole — usually a
duplicate id, a `parentId` that does not exist, a relation endpoint that does
not exist, or a `note` path outside `Knowledge_Base/`.

## 5. The first week

**Day one.** Create today's journal entry from
`Knowledge_Base/templates/daily.md`. Write into it during the day. Do not tidy
it.

**When something arrives.** Put it in Drive's `Inbox/`, then ask an agent for
the `capture` skill. References stay in Drive; the knowledge base gets a note
about them.

**When you catch yourself explaining something twice.** That is a claim worth
keeping. Ask for `distill`: it writes a new note in `library/` and records which
journal entry it came from. The journal entry stays as it was.

**When a new person, team or repository shows up.** Ask for `entity`. It writes
the record and the ontology rows together, which is the only way the graph stays
usable.

**At the end of the week.** Empty `Knowledge_Base/Notes/` — `distill` files
whatever is left. Open the ontology graph and see whether it still describes
what you have been working on.

**When something should be shared.** Ask for `promote`. It copies the note into
the receiving knowledge base with a new identifier and opens a pull request
there. It does not merge it, and it stops if the note is marked `confidential`
or `restricted` and you have not said otherwise.

## Which agent

irori runs Codex, Claude Code, OpenCode and Pi. Codex reads `.agents/skills/`
natively as repository-scope skills, so the five skills are available there
without setup.

From 0.1.6, irori also offers them itself: with this KB selected, a skill
selector sits beside the agent selector in the composer, and the chosen skill is
sent with your request to whichever harness is running. If your build does not
show that selector, ask the agent to read `.agents/skills/<name>/SKILL.md`
directly, or use Codex for skill-driven work. See
[ADR 001](docs/decisions/001-kb-structure.md).

Every agent reads `AGENTS.md` — that is where the rules are, and it is worth
reading yourself before the first week rather than after it.
