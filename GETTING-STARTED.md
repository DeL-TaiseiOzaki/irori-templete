# Getting started

What to do with this repository once you have your own copy of it, and what the
first weeks look like.

## 1. Make it yours

1. Create your repository from this template and clone it.
2. Set your Git author email in the clone (`git config user.email`). Its local
   part is your actor id, `human:<id>`, on every page you write.

## 2. Register it in irori

1. Open or create a workspace and add this directory as a knowledge base,
   choosing **personal**, **team** or **organization**.
2. irori writes `.irori/scope.json` with a random identifier. It is already in
   `.gitignore` and must stay there: a shared identifier makes a second copy of
   this template unregisterable, and an incoming change to that file stops a
   pull.
3. The left navigation should show `AGENTS.md` and `.agents/` under the schema
   pane and `Knowledge_Base/` under the knowledge pane.

## 3. Run `init`

Select the knowledge base, pick `init` in the composer's skill selector (irori
0.1.6 or later; with Codex the skill is available without it), and answer three
questions: the category, your handle (or the project's or organization's name),
and the language you write in. The agent creates the folders and their indexes,
fills the **Folders** block in `AGENTS.md`, writes one identity page and
`.irori/notes.json` (with a daily template for a personal scope), and stops for
you to review the diff. Commit it.

## 4. Connect the cloud folder

Use **クラウド接続** in the header, or **接続** in the workspace's Google Drive
section, to attach the folder this scope exchanges files through, and choose
its name under `contents/`. Keep `Inbox/`, `source/` and `output/` inside it.
irori records the name in `.irori/cloud-mounts.json`, which is committed, so a
page can name a file as `contents/<name>/output/...` and everyone who attached
the same folder resolves it.

## 5. The first weeks

**Every day (personal).** Press **今日のノート** (irori 0.1.8 or later): the
entry for today opens, created from the template on first use. Write into it
during the day, or ask for `journal` to append. Do not tidy it. With an older
irori, create `Knowledge_Base/journal/<year>/<date>.md` by hand.

**When something arrives.** Put it in the cloud folder's `Inbox/` and ask for
`ingest`. The file stays where it is; the knowledge base gets a `reference`
page that says what it is and what it claims.

**When you make something with an agent.** The deliverable goes to `output/`
in the cloud folder. The agent writes its `artifact` page: what it is, what it
was made from, what was decided along the way. If you made it outside irori,
`ingest` finds it and asks.

**When you catch yourself explaining something twice.** Ask for `query`. If the
answer was worth the work, it is filed back as a `synthesis` page.

**Once a week.** Ask for `lint`. It reports what drifted and repairs the
mechanical part: index lines, broken links, missing descriptions.

**When something should be shared.** Ask for `promote`. It copies the page into
the receiving knowledge base with its provenance and opens a pull request there.
It does not merge it, and it stops if the page is `confidential` or `restricted`
and you have not said otherwise.

## Which agent

irori runs Codex, Claude Code, OpenCode and Pi. Every one of them reads
`AGENTS.md`; that is where the rules are, and it is worth reading yourself
before the first week rather than after it.
