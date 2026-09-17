---
name: journal
description: Write or append to a dated record — today's entry, a weekly summary, or a meeting record.
---

# journal

`Knowledge_Base/journal/<year>/` is append-only. You add to it. You do not go
back and rewrite what is already there, and you do not delete an entry because
it turned out to be wrong — you write what you now know, in today's entry.

## Which file

| Ask | File |
| --- | --- |
| today's log | `journal/<year>/<yyyy-mm-dd>.md`, type `daily` |
| a week's summary | `journal/<year>/<yyyy>-W<nn>.md`, type `weekly` |
| a meeting | `journal/<year>/<yyyy-mm-dd>--mtg-<slug>.md`, type `meeting` |

In a **team or organization** scope, only meetings, decisions and weekly
summaries belong here. Members' personal dailies stay in their own vaults.

## Steps

1. Work out the date. If the person is recording something that happened
   earlier, use the date it happened, not today.
2. If the file exists, append under the right heading and leave everything above
   untouched. If it does not, start from the matching template, with an `id` of
   this scope's name from `AGENTS.md`, a slash and a fresh ULID.
3. Keep the entry in the person's own words where they gave you words. A journal
   entry is evidence, and smoothing it out destroys what it is for.
4. Record decisions as decisions: what was decided, and what it rules out. A
   decision that survives the week is usually worth a `library/` note — offer
   `distill`, do not do it silently.
5. For a meeting, list who was present using their ontology ids where they have
   one.

## Boundaries

Do not promote a journal entry to another scope. Journal entries are the raw
record of one scope; what travels is a distilled note. Do not add journal entries
to the ontology — they are reached by date, not by graph.
