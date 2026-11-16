# irori-templete

The recommended starting point for a knowledge base used in
[irori](https://github.com/DeL-TaiseiOzaki/irori) 0.2.66 or later. Agents turn
what you read, decide and make into pages with citations; irori shows them,
checks them against one declaration and keeps their indexes.

```text
AGENTS.md                the contract every agent reads
.agents/skills/          init · ingest · query · lint · review · journal · promote · vocabulary
.property/property.json  the vocabulary: properties, page types (and their folders), relations
index.md                 the root index of an Open Knowledge Format 0.2 bundle
journal/  wiki/          the pages, created as they are written
contents/<folder>/       folders irori connects: Inbox/, source/, output/; never committed
```

- **Knowledge is the center.** The repository root holds what was learned,
  decided and made, including one page per file that matters. Files stay in
  `contents/`.
- **One file declares what can exist.** `.property/property.json` is the
  ontology: each page type with its meaning, required keys, index heading and
  folder; each relation with the types it goes from and points at. Skills read
  it rather than naming types themselves.
- **The pages are portable.** They are an
  [Open Knowledge Format](https://github.com/GoogleCloudPlatform/open-knowledge-format)
  0.2 bundle that any agent or OKF tool can read: typed frontmatter, provenance
  in `sources`, and an `index.md` per folder.

## Getting started

1. Create your repository from this template and clone it. Set your Git author
   email in the clone (`git config user.email`); its local part is your actor
   id, `human:<id>`.
2. Add the directory as a vault in irori and pick any category; it is a label.
   Commit the `.irori/scope.json` irori writes.
3. Pick `init` in the composer's skill selector and answer who owns the
   knowledge base and which language you write in. Review the diff and commit.
4. In the vault's connections, connect the folder you exchange files through,
   usually one a sync app keeps, and keep `Inbox/`, `source/` and `output/`
   inside it.

## Using it

| When | Do |
| --- | --- |
| Every day | Press **今日のノート** and write; ask `journal` to append |
| Something arrives | Put it in `Inbox/` and ask `ingest` |
| You make something with an agent | The deliverable goes to `output/`; the agent writes its `artifact` page |
| A question worth keeping | Ask `query`; a good answer is filed back as a `concept` page |
| After a batch of changes | Open **オントロジー**, press **索引を更新**, commit |
| Once a week | Ask `review`, then `lint` |
| Pages keep not fitting a type | Ask `vocabulary` |
| A page should be shared with another knowledge base | Ask `promote`; it opens a pull request there |

## Extending it

| To | Change |
| --- | --- |
| add or change a page type or relation | `.property/property.json`, through the `vocabulary` skill |
| put a type's pages somewhere else | that type's `folder`; existing pages may be moved by hand |
| add a rule of your own | a `policy` page, not `AGENTS.md` |
| add a procedure | `.agents/skills/<name>/SKILL.md` (below) |
| take a newer template | copy its `AGENTS.md` and skills over yours; your declaration, pages and `.irori/` stay yours |

A skill's `name` equals its directory, its `description` is at most 400
characters, and the file is under 16 KiB. It names the pages whose criteria it
applies rather than restating them. Optional `metadata.roles` and
`metadata.projects` (comma- or space-separated names) narrow whom irori lists it
for; this is a view, not a permission. To retire a skill, replace its
`SKILL.md` with a `RETIRED.md` holding `retired` (`YYYY-MM-DD`), `reason` (at
most 400 characters) and an optional `replacement`, but no `name` or
`description`; never reuse the name.

## Status

Nothing has been exercised against a real knowledge base over time. The design
and its reasoning are in
[docs/DESIGN.md](https://github.com/DeL-TaiseiOzaki/irori-templete/blob/main/docs/DESIGN.md);
a knowledge base made from an earlier version of this template is brought up to
date with
[docs/UPGRADING.md](https://github.com/DeL-TaiseiOzaki/irori-templete/blob/main/docs/UPGRADING.md).
To work on the template itself, read
[docs/CONTRIBUTING.md](https://github.com/DeL-TaiseiOzaki/irori-templete/blob/main/docs/CONTRIBUTING.md).
