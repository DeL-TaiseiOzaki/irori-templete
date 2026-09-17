---
name: query
description: "Answer a question from the knowledge base with citations, reading indexes first and pages second, and file a useful answer back as a synthesis page so it is not re-derived."
---

# query

## Answer

1. Read the root `Knowledge_Base/index.md`, then the index of each folder that
   could hold the answer. Open only the pages whose `description` matches. Use
   search within a folder when the indexes are not enough.
2. Prefer `stable` pages over `draft`; say when a page is `deprecated` or past
   its `stale_after`. Prefer a page with `verified` over one without, and say
   which tier the answer rests on.
3. When a claim matters to the answer and its page cites a file in `contents/`
   that is mounted, re-read the cited passage before relying on it.
4. Write the answer with footnotes. Each footnote names the page (relative link)
   or, through the page's `sources`, the file the claim rests on. Say what the
   knowledge base does not cover instead of filling the gap from memory.

## File back

If the answer took real work to assemble and would be asked again, offer to
file it as a `synthesis` page in `wiki/`: `sources` list every page and file
used, the body is the answer with its footnotes, and the folder index gets its
line. Do not file an answer the person did not accept, and do not turn a
one-off lookup into a page.

## Boundaries

Do not modify any page other than the synthesis you were asked to file. Do not
promote, ingest or lint from here; name the skill that would.
