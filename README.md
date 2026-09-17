# irori-templete

The recommended starting point for a knowledge base used inside
[irori](https://github.com/DeL-TaiseiOzaki/irori). Create a repository from it,
register the copy in irori, run the `init` skill, and you have a knowledge base
that agents compile and keep consistent, in a format any agent can read.

## What it is

- **Files and knowledge are kept apart.** `contents/` is where irori mounts
  your cloud folders: source material, deliverables, incoming items. Nothing
  there is committed. `Knowledge_Base/` holds what was learned and decided,
  including one page per deliverable that says what it was made from.
- **`Knowledge_Base/` is an [Open Knowledge Format](https://github.com/GoogleCloudPlatform/open-knowledge-format)
  0.2 bundle.** Every page has typed frontmatter with a one-line description,
  provenance (`sources`) and lifecycle (`status`, `verified`, `stale_after`).
  Every folder has an `index.md`, so an agent reads indexes first and pages
  second. Tools that speak OKF read it as is.
- **Agents do the bookkeeping.** Six skills, `init`, `ingest`, `query`, `lint`,
  `journal` and `promote`, cover the loop of Karpathy's LLM wiki: material
  comes in, pages are written with citations, questions are answered from the
  pages and filed back, and a lint pass keeps indexes, links and claims honest.
- **One template, three shapes.** A personal vault, a project knowledge base and
  an organization knowledge base share the contract and the format; `init`
  creates the folder set for the category you register in irori.

```text
AGENTS.md                the contract every agent reads; init fills its Folders block
CLAUDE.md                points at it
.agents/skills/          init · ingest · query · lint · journal · promote
.irori/                  notes.json and templates/daily.md, written by init (irori 0.1.8+)
Knowledge_Base/
  index.md               the root index (okf_version 0.2); init adds the folders
  journal/<year>/        dated records people write               (personal, team)
  wiki/                  concept · reference · artifact · synthesis pages
  decisions/             decision pages                           (team)
  entities/              person · org · repo · product · project  (team, organization)
  policies/              policies and standards                   (organization)
contents/                mounted cloud folders; never committed
```

Start with [GETTING-STARTED.md](GETTING-STARTED.md). The reasoning is in
[ADR 002](docs/decisions/002-okf-bundle.md).

## Status

The contract, the skills and the root index are here. The skills reach every
harness from irori 0.1.6 (`v0.1.6-preview.1`); with an earlier build only Codex,
which reads `.agents/skills/` natively, sees them. Nothing has been exercised
against a real knowledge base over time; the first months will tell whether the
three folder sets and the 150-entry index rule hold.

irori provides the desktop environment;
[irori for VS Code](https://github.com/DeL-TaiseiOzaki/irori-extention) provides
the same capabilities as a VS Code extension. This repository owns the knowledge
structure used within them. The three have independent histories and releases.

To work on the template itself, read [docs/CONTRIBUTING.md](docs/CONTRIBUTING.md).
