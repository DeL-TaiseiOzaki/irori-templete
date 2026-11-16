# ADR 011 — The knowledge base is read again in a review

Date: 2026-11-16. Status: owner decision. Adds to
[ADR 010](010-a-small-contract-over-declared-data.md) D6 (one procedure per
skill): a `review` skill, and the `stale_after` check moves to it from `lint`.
Nothing here has been exercised against a real knowledge base.

## Context

The seven shipped skills write pages and check them. `ingest`, `journal` and
`query` turn what arrives, happens and is asked into pages; `lint` checks that
the pages conform and hang together; `vocabulary` and `promote` change and share
them. None of them brings the knowledge base back to the person over time: an
open item in a journal entry, a claim past its `stale_after` or a page that
should link to last month's stays unseen until someone happens to look. A
second brain that is only written becomes an archive
([the collector's fallacy](https://goodluckman.substack.com/p/the-collectors-fallacy-why-most-notetaking)).

On 2026-11-16 the owner asked which skills a template for a second brain in
irori should ship. What others ship and practise:

- Three starter kits for a second brain kept by Claude Code in an Obsidian vault
  all ship a recurring review:
  [claudesidian](https://github.com/heyitsnoah/claudesidian) (`daily-review`,
  `weekly-synthesis`), [brain](https://github.com/vinipx/brain) (`/closeday`,
  `/weekly-review`) and
  [obsidian-claude-pkm](https://github.com/ballred/obsidian-claude-pkm)
  (`/daily`, `/weekly`, `/monthly`, routed by `/review`).
- The GTD weekly review, every seven to ten days, has three phases: get clear
  (empty the inboxes), get current (bring every list up to date) and get
  creative (look ahead)
  ([checklist](https://gettingthingsdone.com/wp-content/uploads/2014/10/Weekly_Review_Checklist.pdf)).
- [Karpathy's LLM wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f),
  whose ingest, query and lint loop this template follows (ADR 002), has its
  periodic health check also suggest "new questions to investigate and new
  sources to look for".

Of the candidates (a review, a skill that makes deliverables, capture from the
web in `ingest`, a thinking partner, task and goal tracking) the owner chose the
review alone.

## Decisions

### D1 — `review` reads the knowledge base back to the person

Over a period, the skill lists four groups: **Waiting** (what `ingest` would
offer), **Open** (journal items no later entry settles), **Due** (pages past or
near `stale_after`, idle drafts, decisions whose condition for changing may now
hold) and **Connections** (older pages the period's pages should link to,
recurring themes without a page, questions the knowledge base cannot answer).
They follow GTD's clear, current and creative.

### D2 — It proposes, and hands work to the skill that owns it

The person decides each item. `review` itself writes only accepted links,
corrections to due pages after re-reading their sources, and the week's journal
entry in the person's words. Material goes to `ingest`, questions to `query`,
structure to `lint` and missing types to `vocabulary`, so each procedure keeps
one owner (ADR 010 D6).

### D3 — No review log

`AGENTS.md` forbids a work log or a timeline page. The record of a review is the
week's journal entry, written as `journal` describes, and Git history.

### D4 — The period is the knowledge base's own

A `policy` page may say how often to review, over what period and what to look
at; without one the period is the seven days up to today. irori runs nothing on
its own: a routine is started by the person with a button
([irori ROUTINES](https://github.com/DeL-TaiseiOzaki/irori/blob/main/docs/ROUTINES.md)).
So a review happens when the person asks for it, or presses a routine of their
own that asks for it.

### D5 — What time has made due is `review`'s

`lint` stops reporting pages past `stale_after`; `review` lists them and, with
the person, re-checks them against their sources. `lint` keeps what is wrong in
the files whatever the date, including a `deprecated` page still linked as
current. `query` still says when a page it cites is past `stale_after`.

## Consequences

- Eight shipped skills. README's weekly step is `review`, then `lint`.
- A knowledge base that runs only `lint` no longer hears about pages past
  `stale_after` (`docs/UPGRADING.md`).
- Not done: a skill that makes deliverables, capture from the web, a thinking
  partner, task or goal tracking. A knowledge base adds its own skill or type
  when it needs one; a deliverables skill is the next candidate once reviews
  have been used.
- What would show this works: open items get settled in later entries, due
  pages get re-checked rather than ignored, and older pages gain links from new
  ones.
