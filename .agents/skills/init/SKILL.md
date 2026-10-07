---
name: init
description: "Set up a new knowledge base once: create journal/ and wiki/ with their indexes, write the first identity page and irori's note settings, and remove the template's own records."
---

# init

Run once, on a fresh copy of the template. If `Knowledge_Base/wiki/` already
exists, stop and say the knowledge base is initialised.

## Ask

1. The category. Read it from `category` in `.irori/scope.json` when irori has
   registered this directory; ask only when that file is absent. `personal`,
   `team` and `organization` are presets; any other name, or none, is a category
   of the scope's own (`AGENTS.md`, **Folders**).
2. The identity that owns this scope:
   - personal: the person's handle, used as `human:<handle>`;
   - team: the project's name and a slug for it;
   - organization: the organization's name and a slug for it;
   - a category of its own: whether a person, a project or an organization owns
     it, then the answer above for that owner. Its rows below are the owner's:
     personal for a person, team for a project, organization for an
     organization.
3. The language of the pages (default: the language the person is writing in).

## Create

The layout is the same for every category (`AGENTS.md`, **Folders**). An
`index.md` has no frontmatter; the root one already carries `okf_version` and
keeps it. Write each index by the rule in `AGENTS.md`, **Indexes**.

1. `Knowledge_Base/journal/<this year>/` with an `index.md` that holds only
   `# Entries`, and `Knowledge_Base/journal/index.md` holding `# Folders` and
   the year's line, `* [<year>](<year>/) - records of <year>`.
2. `Knowledge_Base/wiki/` with the first identity page from the answers:
   `type: person` (personal), `type: project` (team) or `type: org`
   (organization), named after its slug. It carries `title`, a one-line
   `description`, `generated: { by: human:<handle or git author>, at: <now> }`
   and a body of what stays true about the identity. `wiki/index.md` lists it
   under the type's heading.
3. The body of the root `Knowledge_Base/index.md`: `# Folders` with
   `* [journal](journal/) - dated records written by people` and
   `* [wiki](wiki/) - what this scope knows, decided and made`, in the pages'
   language.
4. `.irori/notes.json` with `"schemaVersion": 1`, so irori puts notes where this
   scope keeps them:

   | Category | `newNoteDirectory` | `daily` |
   | --- | --- | --- |
   | personal | `Knowledge_Base/journal/<this year>` | `{ "path": "Knowledge_Base/journal/{{yyyy}}/{{date}}.md", "template": ".irori/templates/daily.md" }` |
   | team | `Knowledge_Base/journal/<this year>` | none |
   | organization | `Knowledge_Base/wiki` | none |

   For a personal scope also write `.irori/templates/daily.md`; irori fills
   `{{date}}` and `{{datetime}}` when it creates the day's entry:

   ```markdown
   ---
   type: journal
   title: {{date}}
   description: Daily record for {{date}}.
   generated: { by: human:<handle>, at: {{datetime}} }
   ---

   # {{date}}

   ## Log

   ## Decided

   ## Open
   ```

5. Remove the template's own records, which are not this knowledge base's:
   the `docs/` directory.
6. Report what was created and removed. Do not commit; the person reviews the
   diff.

## Boundaries

Do not create `contents/` or anything under it. Do not write example pages
beyond the one identity page: an empty index is honest, a fake page is not.
