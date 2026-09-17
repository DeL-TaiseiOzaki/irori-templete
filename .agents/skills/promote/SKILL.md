---
name: promote
description: "Share a page with a higher scope by copying it into the receiving repository with its provenance and opening a pull request there. The receiving reviewer's verification is the sharing filter."
---

# promote

Promotion copies. The source page is not moved, deleted or rewritten. Journal
entries are not promoted; distil first, then promote the distilled page.

## Steps

1. Identify the receiving repository and confirm it is registered in irori and
   writable. If it is not available, prepare the copy and say what could not be
   checked rather than guessing at its contents.
2. Run the `lint` skill's pre-promotion check for the page. `confidential` and
   `restricted` pages do not leave a scope unless the person says so, in this
   conversation, for this page. Do not infer permission from an earlier
   promotion.
3. Read the page for what does not belong in the wider scope: people who are
   not part of it, unreleased plans, quotes from private material, credentials
   of any kind. Report them; do not redact silently.
4. Copy the page into the receiving scope's folder for its type (the receiving
   `AGENTS.md` **Folders** block says which). Set `sources[0]` to the source
   page's GitHub URL with its title; keep the other sources, rewriting any
   `contents/` path the receiving scope cannot see into a URL or prose. Set
   `generated` to yourself, now, and `status: draft`; remove `verified`. From an
   organization scope, `sources` name the project page, not the personal page
   behind it.
5. Resolve every relative link in the receiving scope or rewrite it. Every
   identity the page refers to must exist there or be added in the same change.
   Add the page to the folder's `index.md`.
6. Create a branch in the receiving repository, commit the copy, and open a
   pull request that states what is promoted, from where, and what the reviewer
   should check. Leave it open. The reviewer adds `verified` and sets
   `status: stable`.
7. In the source page, change nothing unless asked. The pull request is the
   record, and a rejected promotion should leave no claim behind.

## Boundaries

Do not merge the pull request. Do not promote in bulk. Do not copy anything
from `contents/`: files are shared through the cloud folder, and the artifact
page travels as a page.
