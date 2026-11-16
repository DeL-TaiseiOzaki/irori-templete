# ADR 012 — Agents ask for what only the person knows

Date: 2026-11-16. Status: owner decision. Adds an **Asking** section to
`AGENTS.md`, an `interview` skill, a discussion round to `ingest` and a
clarifying question to `query`. Keeps [ADR 010](010-a-small-contract-over-declared-data.md)
D6 (one procedure per skill) and works with
[ADR 011](011-reading-the-knowledge-base-again.md)'s `review`. Nothing here
has been exercised against a real knowledge base.

## Context

On 2026-11-16 the owner stated what an agent in a second brain, in irori or
elsewhere, is for: making clear what was left unexplained by asking the person,
organizing what they know, and the quality of both. The shipped skills asked
only in passing: `init` asks three questions, `ingest` asks which items to take
and what a deliverable was made from, and every skill asks agreement on the
pages it will touch. Nothing said how to ask, what not to ask, or where an
answer goes.

What others ship:

- Matt Pocock's [`grill-me`](https://github.com/mattpocock/skills) wraps a
  `grilling` skill: decisions form a tree; each round asks every question whose
  prerequisites are settled, numbered, each with a recommended answer worded so
  "yes" accepts it; the agent finds facts itself and puts only decisions to the
  user; it ends when no branch is left. It writes nothing down.
  `grill-with-docs` adds `domain-modeling`, which challenges a term that
  conflicts with the glossary at once and records it when resolved.
- [superpowers' `brainstorming`](https://github.com/obra/superpowers) asks only
  when the answer would change what gets built and neither the request nor the
  project answers it, one question at a time, open until the "why" is known
  because options steer, and plays back each understood part before the next.
- [spec-kit's `clarify`](https://github.com/github/spec-kit) scans a spec
  against a list of commonly missed areas, picks at most five questions by
  impact times uncertainty, asks them one at a time with a recommended option,
  and writes each answer back as a dated `Q → A`.
- [Claude Code's best practices](https://code.claude.com/docs/en/best-practices)
  suggest asking Claude to interview you until it could write the spec, then
  writing it to a file.
- In [Karpathy's LLM wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f),
  the agent "discusses key takeaways with you" while ingesting, and good
  answers are filed back so they do not disappear into chat history.

Research on agents that may ask, all on coding, SQL or reasoning tasks with
simulated users, points the same way. Silent guessing is the common failure,
and pushing models to ask produces over-asking that varies by model
([Ambig-SWE](https://arxiv.org/abs/2502.13069),
[Ask or Assume?](https://arxiv.org/abs/2603.26233)). Gaps surface while
looking: without environment tools one model's recall of blockers fell from 61%
to 11% ([HiL-Bench](https://arxiv.org/abs/2604.09408)). A few answerable,
high-impact questions beat many: success plateaus as questions grow, and a
model asking 3.0 questions on average outperformed one asking 5.1
([Asking What Matters](https://arxiv.org/abs/2604.14624)). Applying this to a
knowledge base is an inference.

## Decisions

### D1 — `AGENTS.md` says how every skill asks

Look before asking: pages, indexes, files and Git history answer facts. Ask
about intent, judgement, preference and what happened, and only when the answer
changes what will be written; otherwise state the assumption. Ask a round of
independent questions, few, most important first, each with a recommended
answer; ask for what can be answered now; point out a contradiction with a page
and ask which holds; play back before writing; use the CLI's question tool when
it has one.

### D2 — Answers that pages rest on are journal evidence

An answer a page will rest on goes into the day's journal entry under
`## Asked`, as `- Q: … → A: …` with the answer in the person's words, and the
page cites the entry in `sources`. The journal already holds what people said;
the wiki holds what was distilled from it, so an answer gets provenance without
a new type or file. Answers that only steer a task, such as which item to
ingest, are not recorded.

### D3 — `interview` is the skill for getting something straight

It takes a subject in the person's words, reads what the knowledge base
already says, proposes the pages the subject would become, and asks in rounds.
The questions come from the gaps between what is known and what those pages
need: what the type's description says such a page holds and the keys it
requires, what a claim rests on, until when it holds, whom it concerns, where
the person and a page disagree, and a concrete example. At most five questions
a round. It stops when no question would change a page or the person says
enough; unanswered questions go under `## Open`, where `review` finds them.
Because the questions follow the declaration, a new type brings its own.

### D4 — `ingest` discusses and `query` clarifies

`ingest` plays back each item it takes deep and asks one round: why it was
kept, what to emphasize, and which page holds where they disagree. `query`
asks one question first when the question can be read in ways that change the
answer, and asks whether the person knows what the knowledge base does not
cover.

### D5 — Not adopted

An unbounded, "relentless" interview, which a person in capture mode does not
want and the research does not support; options by default; a separate
glossary file, since terms are `concept` pages here; and a fixed limit per
session.

## Consequences

- Nine shipped skills.
- irori shows Claude Code's and OpenCode's questions as a form with choices and
  a free answer; Codex in its default mode and Pi ask in text. The skills work
  either way.
- How often an agent asks differs by model. Whether the rules hold for each CLI
  irori runs is to be seen in use.
- What would show this works: pages cite `## Asked` answers instead of guesses,
  no answer is asked for twice, and `review` finds open questions settled in
  later entries.
