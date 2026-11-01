# ADR 009 — A GitHub repository has a page and is cited at a commit

Date: 2026-11-01. Status: follows the owner's request to make the GitHub
repositories a knowledge base rests on easy to trace later. Builds on irori's
[ADR 046](https://github.com/DeL-TaiseiOzaki/irori/blob/main/docs/decisions/046-github-repositories-in-contents.md).
Keeps [ADR 006](006-one-layout.md)'s vocabulary unchanged.

## Context

irori 0.2.24 connects a GitHub repository to a vault: each device clones it
and links it at `contents/<mount>`, and `.irori/repositories.json` records the
URL and branch. The template said nothing about it. Its contract listed the
other `.irori/` records as tracked but not this one, gave every mount
`Inbox/`, `source/` and `output/`, and had pages cite files as
`contents/<mount>/<path>`. For a repository that leaves three gaps:

- no page says which repositories the knowledge rests on or which project each
  serves, so finding them means reading a JSON record or every page;
- `contents/<mount>/<path>` names whatever the clone holds today, so a claim
  read from code cannot be checked against the code it was read from;
- a page promoted to another scope, or opened where the repository is not
  cloned, keeps a path that resolves nowhere.

## Decisions

1. **`.irori/repositories.json` is tracked** like the other connection records.
2. **One `reference` page per repository**, connected or not, with `resource`
   `https://github.com/<owner>/<repo>` and `about` to its project or
   organization. The References index and the graph are then where the
   repositories are found; no new type or relation is needed, since a
   repository is a thing someone else keeps, which is what `reference` means.
3. **Code is cited by a permalink at a full commit hash**
   (`/blob/<commit>/<path>`, `/tree/<commit>`), not by a branch or by
   `contents/<mount>/...`. The page that cites it also reaches the repository's
   page through `sources` or `uses`.
4. **The skills carry it.** `ingest` offers a connected repository without a
   page; `lint` reports a missing page, a GitHub source at a branch or tag, and
   a source into a repository's mount; `query` reads cited code at its commit.

## Rejected

- **A `repository` type or a `repository` relation.** It would change the
  vocabulary, which happens through a review on evidence (ADR 003); a
  `reference` page whose `resource` is a GitHub URL already says the same.
- **Citing `contents/<mount>/<path>` with a `commit` key.** It still fails on a
  device without the clone and after promotion; the permalink is one value that
  works everywhere and opens in a browser.
- **Mirroring repository history in pages.** Git history is GitHub's; a page
  records what was learned or decided from it.

## Consequences

- An older knowledge base gains the rule without moving anything; `lint` lists
  what to bring in line, and the person agrees before a source is rewritten.
- A private repository's permalink opens only for those with access to it; the
  page's own text must still say what the code shows.
