---
name: review
description: "Look back over a period with the person, a week unless a policy page says otherwise: what waits to be ingested, what is still open, which pages time has made due, and what the period's pages should connect to. Proposes, and writes only what the person agrees to."
---

# review

Pages that are written and never read again are an archive, not knowledge. A
review reads the knowledge base again with the person, over a period, and ends
with what to do next. It proposes; it changes nothing the person did not agree
to.

If a `policy` page says how this knowledge base reviews (how often, over what
period, what to look at), apply it. Otherwise the period is the seven days up to
today.

## Gather

Read the root `index.md`, the indexes of the folders that changed in the period
(`git log --since` shows which), and the period's entries in the `folder` the
`journal` type declares. Then list four groups:

1. **Waiting**: what `ingest` would offer as new (its *Find what is new*). Name
   and count each item; do not read the files.
2. **Open**: each item under `## Open` or `## Actions` in the period's entries,
   and in earlier entries, that no later entry settles. Quote it as written and
   link its entry; ask when you cannot tell whether it is settled.
3. **Due**: pages whose `stale_after` has passed or falls before the next
   review; `draft` pages nobody changed in the period; `decision` pages whose
   stated condition for changing may now hold, given what the period's pages
   say.
4. **Connections**: for each page written or changed in the period, older
   pages it should link to or that make the same claim; a theme that recurs in
   the period's entries with no page of its own; a question the period raised
   that the knowledge base cannot answer, with sources that might.

Mention a group only when it has items.

## Decide with the person

Show the groups, each item on one line with its link. For each, the person says
act now, later or drop. Name the skill that acts: `ingest` for waiting material
and for a recurring theme worth a page, `query` for an unanswered question,
`lint` for anything structural, `vocabulary` when pages keep missing a type.
Hand over only what the person chose.

## Write

Only what was agreed:

- **Connections**: add the accepted links to the pages they belong in.
- **Due**: re-read each cited source, then correct the claim and set a new
  `stale_after` or remove it. A page the person no longer stands behind becomes
  `draft` or `deprecated`, as they say.
- **The week's record**: offer to record the period in the week's entry
  (`<yyyy>-W<nn>.md`) as `journal` describes. Ask the person what was done,
  decided and left open, using the groups as prompts, and write their words,
  not your summary.

Set `generated` on each page you change, then run `irori index` and
`irori check`.

## Boundaries

Do not keep a review log or write a page about the review itself; the week's
entry and Git history are the record. Do not settle an Open item yourself: a
later entry in the person's words settles it. Do not read files in `contents/`
or do another skill's work from here.
