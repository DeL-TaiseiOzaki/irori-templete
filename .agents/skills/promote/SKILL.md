---
name: promote
description: "Share a page with another knowledge base by copying it there with its provenance and opening a pull request. The receiving reviewer's verification is the sharing filter."
---

# promote

Promotion copies. The source page is not moved, deleted or rewritten. Journal
entries are not promoted; distil first, then promote the distilled page.

## Check

1. Identify the receiving repository and confirm it is registered in irori and
   writable. If it is not available, prepare the copy and say what could not be
   checked rather than guessing at its contents.
2. `sensitivity` is `internal`, or the person said otherwise for this page in
   this conversation. Do not infer permission from an earlier promotion.
3. Read the page for what does not belong in the receiving knowledge base:
   people who are not part of it, unreleased plans, quotes from private
   material, credentials of any kind.
4. Every link and every `sources[].resource` resolves there or can be rewritten
   to a URL; every identity the page refers to exists there or is added with
   it; its `type` and every `rel` it uses are in the receiving vocabulary.

Report what does not hold; do not redact silently.

## Copy

1. Write the copy where the receiving declaration puts its type. When the
   receiving vocabulary lacks its `type` or a `rel` it uses, take the nearest
   entry there and say so in the pull request, or stop and suggest the
   `vocabulary` skill in that repository.
2. Set `sources[0]` to the source page's GitHub URL with its title. Keep the
   other sources, rewriting what the receiving knowledge base cannot see into a
   URL or prose. Set `generated` to yourself and now, `status: draft`, and
   remove `verified`.
3. Rewrite every relative link for the receiving repository and run
   `irori index <receiving vault>`.
4. Create a branch there, commit the copy, and open a pull request that states
   what is promoted, from where, and what the reviewer should check. Leave it
   open. The reviewer adds `verified` and sets `status: stable`.
5. In the source page, change nothing unless asked. The pull request is the
   record, and a rejected promotion should leave no claim behind.

## Boundaries

Do not merge the pull request. Do not promote in bulk. Do not copy anything
from `contents/`: files are shared through the connected folder, and the
artifact page travels as a page.
