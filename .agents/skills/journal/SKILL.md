---
name: journal
description: "Write or append a person's dated record, such as today's entry, a meeting record or a weekly summary, as a journal page, in the person's own words and without rewriting what is already there."
---

# journal

Journal entries are records written by people. They are append-only: you add,
you do not rewrite or delete, and what turned out wrong is corrected in a later
entry.

## Which file

Every entry is `type: journal`, in `<folder>/<year>/` where `<folder>` is the
`folder` the `journal` type declares; the file name says which kind it is.

| Ask | File |
| --- | --- |
| today's log | `<yyyy-mm-dd>.md` |
| a meeting | `<yyyy-mm-dd>--mtg-<slug>.md` |
| a week's summary | `<yyyy>-W<nn>.md` |

**今日のノート** opens or creates today's entry from `.irori/notes.json`; this
skill then appends to it.

## Steps

1. Work out the date: the day it happened, not today, if the person is
   recording something earlier.
2. If the file exists, append under the right heading and leave the rest
   untouched. If not, create it with the keys the declaration requires,
   `generated` naming the person, and the headings `## Log`, `## Decided`,
   `## Open` for a daily or weekly entry, or `## Present`, `## Discussed`,
   `## Decided`, `## Actions` for a meeting. Link attendees to their identity
   pages where they exist. Answers to an agent's questions go under
   `## Asked` (`AGENTS.md`, **Asking**).
3. Keep the person's wording. An entry is evidence; smoothing it destroys what
   it is for.
4. Run `irori index`.
5. A decision that will outlive the week belongs in a `decision` page as well;
   offer `ingest`, do not do it silently.

## Boundaries

Do not promote a journal entry; what travels is a distilled page that cites it.
Do not edit an entry to record that something was distilled from it: the
distilled page's `sources` carries that.
