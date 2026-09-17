---
name: init
description: "Set up a new knowledge base once. Choose personal, team or organization, create the folder set and its indexes, fill the Folders block in AGENTS.md, and write the first identity page."
---

# init

Run once, on a fresh copy of the template. If the **Folders** block in
`AGENTS.md` already shows a category, stop and say the knowledge base is
initialised.

## Ask

1. The category: `personal`, `team` or `organization`. irori registered this
   directory with one of these; the person knows which.
2. The identity that owns this scope:
   - personal: the person's handle, used as `human:<handle>`;
   - team: the project's name and a slug for it;
   - organization: the organization's name and a slug for it.
3. The language of the pages (default: the language the person is writing in).

## Create

Directories, each with an `index.md` that has the headings below and no entries
yet. An `index.md` has no frontmatter; the root one already carries
`okf_version` and keeps it.

| Category | Directories and index headings |
| --- | --- |
| personal | `journal/` (`# Years`), `journal/<this year>/` (`# Entries`), `wiki/` (`# Concepts`, `# References`, `# Artifacts`, `# Decisions`, `# People and organizations`) |
| team | `journal/` (`# Years`), `journal/<this year>/` (`# Entries`), `decisions/` (`# Decisions`), `wiki/` (`# Concepts`, `# References`, `# Artifacts`), `entities/` (`# People`, `# Organizations`, `# Repositories`, `# Products`) |
| organization | `entities/` (`# People and teams`, `# Organizations`, `# Repositories`, `# Products`, `# Projects`), `policies/` (`# Policies and standards`), `wiki/` (`# Patterns and lessons`, `# Glossary`) |

Then:

1. Write the first identity page from the answers: `type: person` in `wiki/`
   (personal), `type: project` in `entities/` (team) or `type: org` in
   `entities/` (organization). It carries `title`, a one-line `description`,
   `generated: { by: human:<handle or git author>, at: <now> }` and a body of
   what stays true about the identity. Add its line, with the description,
   under the matching heading of the folder's `index.md`.
2. Rewrite the body of the root `Knowledge_Base/index.md`: a `# Folders`
   section with one line per directory, for example
   `* [journal](journal/) - dated records written by people` and
   `* [wiki](wiki/) - concepts, references, artifacts and decisions`. Keep the
   `okf_version` frontmatter.
3. Replace everything between `<!-- init:folders ... -->` and
   `<!-- /init:folders -->` in `AGENTS.md` with `Category: <category>` and a
   table of the directories created, each with the types it holds and who
   writes there. Take the types from the **Page types** table and the discipline
   from the category table that follows the block. Edit nothing else in
   `AGENTS.md`.
4. Write `.irori/notes.json` so irori puts notes where this scope keeps them:

   | Category | `newNoteDirectory` | `daily` |
   | --- | --- | --- |
   | personal | `Knowledge_Base/journal/<this year>` | `{ "path": "Knowledge_Base/journal/{{yyyy}}/{{date}}.md", "template": ".irori/templates/daily.md" }` |
   | team | `Knowledge_Base/journal/<this year>` | none |
   | organization | `Knowledge_Base/wiki` | none |

   with `"schemaVersion": 1`. For a personal scope also write
   `.irori/templates/daily.md`; irori fills `{{date}}` and `{{datetime}}` when it
   creates the day's entry:

   ```markdown
   ---
   type: daily
   title: {{date}}
   description: Daily record for {{date}}.
   generated: { by: human:<handle>, at: {{datetime}} }
   ---

   # {{date}}

   ## Log

   ## Decided

   ## Open
   ```

5. Report what was created. Do not commit; the person reviews the diff.

## Boundaries

Do not create `contents/` or anything under it. Do not create folders for
another category "just in case". Do not write example pages beyond the one
identity page: an empty index is honest, a fake page is not.
