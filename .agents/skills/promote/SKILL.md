---
name: promote
description: Prepare a note for a shared knowledge base as a pull request against the receiving repository.
---

# promote

A sharing filter is a pull request against the receiving scope's repository.
Promotion **copies**: the receiving scope gets a new note with a new `id` and a
`derived_from` naming the source. Nothing is moved, nothing is deleted, and no
link crosses repositories by path.

Journal entries are not promoted. Distil first, then promote the distilled note.

## Steps

1. Identify the receiving scope and confirm it is registered and writable. If it
   is not available, prepare the copy and say what could not be verified rather
   than guessing at its contents.
2. Check `sensitivity`. `confidential` and `restricted` notes do not leave a
   scope without the person saying so explicitly, in this conversation, for this
   note. Do not infer permission from an earlier promotion.
3. Read the note for things that do not belong in the wider scope: names of
   people who are not part of it, unreleased plans, quotes from private
   material, credentials of any kind. Report them; do not silently redact.
4. Copy the note into the receiving scope's `library/` (or `entities/`, for an
   identity the receiving scope should own). Generate a **new** ULID under the
   receiving scope's namespace. Set `derived_from` to the source note's `id`.
   From an organization scope, `derived_from` names the project note, not the
   personal note behind it.
5. Resolve the links. Every relative link must resolve in the receiving scope or
   be rewritten; every id in `derived_from` must be a real id. A reference to a
   note that was not promoted becomes prose, not a broken link.
6. Check the vocabulary. Every ontology id the note refers to must exist in the
   receiving scope, or be added there in the same change through the `entity`
   skill.
7. Create a branch in the receiving repository, commit the copy, and open a pull
   request that states what is being promoted, from which scope, and what the
   review should check. Leave it open.
8. In the source scope, add `promoted_to:` with the new id under the source
   note's frontmatter only if the person asks — the pull request is the record,
   and a rejected promotion should not leave a claim behind.

## Boundaries

Do not merge the pull request. Do not promote in bulk. Do not copy anything from
`contents/` into a receiving scope — a deliverable is shared through Drive, and
its provenance is irori's record.
