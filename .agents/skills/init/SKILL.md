---
name: init
description: "Set up a new knowledge base once: write the owner's identity page and irori's note settings, index the knowledge layer, and remove the template's own records."
---

# init

Run once, on a fresh copy of the template. If the knowledge layer already holds
a page whose `type` is declared, stop and say the knowledge base is
initialised.

## Ask

1. Who owns this knowledge base: a person, a project or an organization. Take
   it from `category` in `.irori/scope.json` when that is `personal` (a
   person), `team` (a project) or `organization`; ask otherwise.
2. The owner's handle for a person, used as `human:<handle>`; otherwise its
   name and a slug.
3. The language of the pages (default: the language the person is writing in).

## Create

1. The owner's identity page, of type `person`, `project` or `org`, named
   `<slug>.md` in the `folder` that type declares in `.property/property.json`.
   It carries the keys the declaration requires, with
   `generated: { by: human:<handle or git author>, at: <now> }`, and a body of
   what stays true about the owner.
2. `.irori/notes.json`, which tells irori where notes go. Take the journal
   folder from the `journal` type's `folder`:

   | Owner | Content |
   | --- | --- |
   | a person | `{ "schemaVersion": 1, "newNoteDirectory": "<journal folder>/<this year>", "daily": { "path": "<journal folder>/{{yyyy}}/{{date}}.md", "template": ".irori/templates/daily.md" } }` |
   | a project or an organization | `{ "schemaVersion": 1, "newNoteDirectory": "<the identity page's folder>" }` |

   For a person also write `.irori/templates/daily.md`; irori fills `{{date}}`
   and `{{datetime}}` when it creates the day's entry:

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

3. Remove the template's own records, which are not this knowledge base's: the
   `docs/` directory.
4. Run `irori index`, then give each folder line of the root `index.md` a short
   description in the pages' language of what the folder holds.
5. Report what was created and removed. Do not commit; the person reviews the
   diff.

## Boundaries

Do not create `contents/` or anything under it. Do not write example pages
beyond the identity page: an empty index is honest, a fake page is not. A
folder with no pages yet is created by the first page written into it.
