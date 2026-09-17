# irori-templete

The recommended starting point for a knowledge base used inside
[irori](https://github.com/DeL-TaiseiOzaki/irori). Use this repository as a template, register the copy in
irori, and you have a knowledge base with a structure, an operating contract for
the agents that work in it, and five skills that keep it from drifting.

The same template serves all three levels of the picture: your personal vault, a
project's shared knowledge base, and an organization's. They have the same shape
and differ only in a few rules, which [AGENTS.md](AGENTS.md) states.

## What you get

```text
AGENTS.md                     the contract every agent in this KB reads
CLAUDE.md                     points at it
.agents/skills/               capture · journal · distill · entity · promote
.irori/ontology.json          tells irori which CSVs are the ontology
Knowledge_Base/
  journal/<year>/             records tied to a date; append-only
  library/                    one reusable claim per note; flat
  entities/{person,org,repo}/ identity records
  Notes/                      where irori's new-note button lands
  ontology/                   entities.csv · relations.csv — the graph
  templates/                  daily · meeting · note · decision · entity
  attachments/
contents/                     mounted Google Drive; never committed
```

Two ideas hold it together. **A folder answers one question, decided when a note
is created** — is this tied to a date, is it a reusable claim, is it an identity
— and the answer never changes, so notes do not migrate as they mature.
Distilling writes a new note and records where it came from. **The ontology is
the way back in**: irori draws `ontology/*.csv` as a graph you filter and click
through to notes, and there is no hand-written index competing with it.

Start with [GETTING-STARTED.md](GETTING-STARTED.md).

## Status

The structure, contract, skills and ontology seed are here. Two limits remain:

- The skills reach every harness only from irori 0.1.6 (`v0.1.6-preview.1`),
  whose composer offers them. With irori 0.1.5 or earlier, only Codex — which
  reads `.agents/skills/` natively — sees them.
  [ADR 001](docs/decisions/001-kb-structure.md) records why they are not
  duplicated per runtime instead.
- Nothing here has been exercised against a real knowledge base over time.
  `Knowledge_Base/Notes/` in particular is a compromise with irori's fixed
  new-note default, and ADR 001 records what would justify removing it.

irori provides the desktop environment;
[irori for VS Code](https://github.com/DeL-TaiseiOzaki/irori-extention) provides
the same capabilities as a VS Code extension. This repository owns the knowledge
structure used within them. The three have independent histories and releases.

To work on the template itself, read [docs/CONTRIBUTING.md](docs/CONTRIBUTING.md).
