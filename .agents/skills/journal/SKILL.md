---
name: journal
description: "Write or append a person's dated record, such as today's entry, a meeting record or a weekly summary, in journal/<year>/, in the person's own words and without rewriting what is already there."
---

# journal

`journal/<year>/` holds records written by people. It is append-only: you add,
you do not rewrite or delete, and what turned out wrong is corrected in a later
entry. An organization scope has no journal.

## Which file

| Ask | File | `type` |
| --- | --- | --- |
| today's log | `journal/<year>/<yyyy-mm-dd>.md` | `daily` (personal only) |
| a meeting | `journal/<year>/<yyyy-mm-dd>--mtg-<slug>.md` | `meeting` |
| a week's summary | `journal/<year>/<yyyy>-W<nn>.md` | `weekly` |

In irori 0.1.8 and later, **今日のノート** opens or creates today's daily entry
from `.irori/templates/daily.md`; this skill then appends to it. Create the
file yourself only when the person is not using that button.

## Steps

1. Work out the date: the day it happened, not today, if the person is
   recording something earlier.
2. If the file exists, append under the right heading and leave the rest
   untouched. If not, create it with `type`, `title`, a one-line `description`,
   `generated: { by: human:<id>, at: <now> }` (the person is the author even
   when you type) and the headings `## Log`, `## Decided`, `## Open` for a daily
   or weekly entry, or `## Present`, `## Discussed`, `## Decided`, `## Actions`
   for a meeting. List attendees with links to their identity pages where they
   exist.
3. Keep the person's wording. An entry is evidence; smoothing it destroys what
   it is for.
4. Add the file to `journal/<year>/index.md` with its description. If the year
   folder is new, create it with an `index.md` (`# Entries`) and add the year to
   `journal/index.md`.
5. A decision that will outlive the week belongs in a `decision` page as well;
   offer `ingest`, do not do it silently.

## Boundaries

Do not promote a journal entry; what travels is a distilled page that cites it.
Do not edit an entry to record that something was distilled from it: the
distilled page's `sources` carries that.
