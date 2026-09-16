---
name: distill
description: Turn journal entries and unsorted Notes into reusable library notes, and empty the in-tray.
---

# distill

Two jobs, and they are not the same.

**From `journal/`** — a dated record contains a claim that will be worth reading
detached from its date. You write a **new** note in `Knowledge_Base/library/`.
The journal entry is not touched except to record what came out of it.

**From `Notes/`** — a person wrote something by hand into irori's default
directory. It has no home yet. You give it one and the in-tray gets shorter.

## From a journal entry

1. Read the entry. Find claims that would still be true and still be useful next
   quarter. A meeting happening is not a claim; what the meeting settled is.
2. For each, write `Knowledge_Base/library/<slug>.md` from `templates/note.md`
   or `templates/decision.md`. One claim per note. If you are writing "and also",
   start another note.
3. Set `derived_from` to the journal entry's `id`. Generate a fresh ULID for the
   new note; never reuse the source's.
4. Add the id of each new note to the journal entry's `## Distilled` section.
   That is the only edit you make to the entry.
5. If an existing `library/` note already makes the claim, strengthen that note
   instead of writing a near-duplicate, and add the journal entry's id to its
   `derived_from`.
6. If the claim is about a person, org or repo that has no record, hand over to
   the `entity` skill.

## From `Notes/`

1. Read the file and ask the placement question from `AGENTS.md`: is it tied to a
   date, is it a reusable claim, or is it an identity?
2. Move it to the folder that answers, rename it to the convention for that
   folder, and fill in the frontmatter it is missing — `id`, `type`, `created`
   from when it was written, `sensitivity`.
3. If it contains several unrelated things, split it. Each piece gets its own id.
4. Leave nothing behind in `Notes/`. This is the one move the contract allows, so
   finish it.

## Boundaries

Do not move a note out of `journal/` or `library/` — maturity is not a location.
Do not rewrite a person's wording into your own in the journal. Do not distil
material the person has not asked you to look at.
