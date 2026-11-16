---
name: query
description: "Answer a question from the knowledge base with citations, reading indexes first and pages second, and file a useful answer back as a concept page so it is not re-derived."
---

# query

## Answer

1. If the question can be read in ways that lead to different answers, and the
   knowledge base does not settle which, ask one question first or answer
   under a reading you state.
2. Read the root `index.md`, then the index of each folder that could hold the
   answer. Open only the pages whose `description` matches. Search within a
   folder when the indexes are not enough.
3. Prefer `stable` pages over `draft`, and a page with `verified` over one
   without; say when a page is `deprecated` or past its `stale_after`.
4. When a claim matters to the answer and its page cites a file in `contents/`
   that is mounted, re-read the cited passage before relying on it.
5. Write the answer with footnotes. Each names the page (relative link) or,
   through the page's `sources`, the file the claim rests on. Say what the
   knowledge base does not cover instead of filling the gap from memory, and
   ask whether the person knows; an answer they give is kept as `AGENTS.md`,
   **Asking**, describes and can be filed back below.

## File back

If the answer took real work to assemble and would be asked again, offer to
file it as a `concept` page whose title is the question: `sources` list every
page and file used, and the body is the answer with its footnotes. Then run
`irori index`. Do not file an answer the person did not accept, and do not turn
a one-off lookup into a page.

## Boundaries

Do not modify any page other than the one you were asked to file. Do not
promote, ingest or lint from here; name the skill that would.
