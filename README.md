# irori-templete

The recommended starting point for a knowledge base used inside
[irori](https://github.com/DeL-TaiseiOzaki/irori). Create a repository from it,
register the copy in irori, run the `init` skill, and you have a knowledge base
that agents compile and keep consistent, in a format any agent can read.

## What it is

- **Knowledge is the center.** `Knowledge_Base/` holds what anyone working
  here must understand: what was learned and decided, including one page per
  file that matters, saying what it is and what it was made from. Skills turn
  that knowledge into procedures and name the pages they apply. `contents/` is
  where irori connects your folders: incoming items, source material,
  deliverables. Nothing there is committed.
- **`Knowledge_Base/` is an [Open Knowledge Format](https://github.com/GoogleCloudPlatform/open-knowledge-format)
  0.2 bundle.** Every page has typed frontmatter, its properties as
  `.property/property.json` declares them: a one-line description, provenance
  (`sources`) and lifecycle (`status`, `verified`, `stale_after`). Every folder
  has an `index.md` that follows from its pages, so an agent reads indexes first
  and pages second. Tools that speak OKF read it as is.
- **Agents do the bookkeeping.** Six skills, `init`, `ingest`, `query`, `lint`,
  `journal` and `promote`, cover the loop of Karpathy's LLM wiki: material
  comes in, pages are written with citations, questions are answered from the
  pages and filed back, and a lint pass keeps links and claims honest.
- **One layout for every scope.** A personal vault, a project knowledge base
  and an organization knowledge base have the same folders; the category you
  register in irori decides who confirms pages.

```text
AGENTS.md                the contract every agent reads
CLAUDE.md                points at it
README.md                this page
.agents/skills/          init · ingest · query · lint · journal · promote
.property/property.json  the vocabulary: page properties, types and relations
.irori/                  scope.json (irori), notes.json and templates/ (init)
Knowledge_Base/
  index.md               the root index (okf_version 0.2)
  journal/<year>/        records people write: a day, a meeting, a week
  wiki/                  everything else: concepts, decisions, policies,
                         pages about files, people, organizations, projects
  ontology/              the graph index irori generates
contents/<folder>/       a connected folder; never committed
  Inbox/                 arrived, not yet read
  source/                originals someone else wrote
  output/                deliverables made here
```

The reasoning is in the template's
[decisions](https://github.com/DeL-TaiseiOzaki/irori-templete/tree/main/docs/decisions),
the current layout in
[ADR 006](https://github.com/DeL-TaiseiOzaki/irori-templete/blob/main/docs/decisions/006-one-layout.md).

## Getting started

1. **Make it yours.** Create your repository from this template and clone it.
   Set your Git author email in the clone (`git config user.email`); its local
   part is your actor id, `human:<id>`, on every page you write.
2. **Register it in irori.** Add the directory as a hibachi and choose
   **personal**, **team** or **organization**, or type a category of your own
   (irori 0.1.87 and later; it keeps the personal discipline). irori writes
   `.irori/scope.json`; commit it, so every device sees the same knowledge base.
   The schema pane shows `AGENTS.md`, `README.md`, `.agents/` and `.property/`;
   the knowledge pane shows `Knowledge_Base/`.
3. **Run `init`.** Pick it in the composer's skill selector and answer two
   questions: your handle (or the project's or organization's name) and the
   language you write in. The agent creates `journal/` and `wiki/` with their
   indexes, one identity page and `.irori/notes.json` (with a daily template for
   a personal scope), removes `docs/`, and stops for you to review the diff.
   Commit it.
4. **Connect a folder.** In the hibachi's connections, connect the folder this
   scope exchanges files through, usually one Drive for desktop or another sync
   app keeps, and name it under `contents/`. Keep `Inbox/`, `source/` and
   `output/` inside it.

## The first weeks

**Every day (personal).** Press **今日のノート**: today's entry opens, created
from the template on first use. Write into it during the day, or ask for
`journal` to append. Do not tidy it.

**When something arrives.** Put it in `Inbox/` and ask for `ingest`. The file
stays where it is; the knowledge base gets a `reference` page that says what it
is and what it claims, and you move the file to `source/` if you keep it.

**When you make something with an agent.** The deliverable goes to `output/`.
The agent writes its `artifact` page: what it is, what it was made from, what
was decided along the way. If you made it outside irori, `ingest` finds it and
asks.

**When you catch yourself explaining something twice.** Ask for `query`. If the
answer was worth the work, it is filed back as a `concept` page.

**After a batch of changes.** Press **索引を更新** in the ontology panel and
commit: the folder indexes and the graph follow the pages.

**Once a week.** Ask for `lint`. It reports what drifted and repairs the
mechanical part.

**When something should be shared.** Ask for `promote`. It copies the page into
the receiving knowledge base with its provenance and opens a pull request there.
It does not merge it, and it stops if the page is `confidential` or `restricted`
and you have not said otherwise.

irori runs Codex, Claude Code, OpenCode and Pi; every one of them reads
`AGENTS.md`. Read it yourself before the first week rather than after it.

## Status

The contract, the skills and the root index are here. Folder indexes are
generated by irori 0.1.86 and later; with an earlier build an agent writes them
by the same rule. Nothing has been exercised against a real knowledge base over
time; the first months will tell whether two folders, nine types and the
150-entry index rule hold.

irori provides the desktop environment and this repository the knowledge
structure used within it; the two have independent histories and releases.
To work on the template itself, read
[docs/CONTRIBUTING.md](https://github.com/DeL-TaiseiOzaki/irori-templete/blob/main/docs/CONTRIBUTING.md).
