# ADR 008 — The knowledge sits at the repository root

Date: 2026-10-12. Status: follows the owner's request in irori
([ADR 035](https://github.com/DeL-TaiseiOzaki/irori/blob/main/docs/decisions/035-knowledge-at-the-vault-root.md)).
Amends [ADR 006](006-one-layout.md) D1, which put the folders in
`Knowledge_Base/`.

## Context

Every page lived one level down, in `Knowledge_Base/journal/…` or
`Knowledge_Base/wiki/…`. irori hides that level, but Finder, an editor and
GitHub always show it: opening the repository showed `Knowledge_Base/` before
any page. The owner asked for the knowledge to sit at the root, and the
personal vaults made from this template to follow.

irori 0.2.0 lets a vault declare its root as the knowledge folder
(`"knowledge": "."` in `.irori/scope.json`). It keeps classifying as before:
`AGENTS.md`, `CLAUDE.md`, the top `README.md` and dot folders are schema,
`contents/` is contents, and everything else is knowledge. It registers a
folder with an OKF `index.md` at its root and no `Knowledge_Base/` this way by
itself, and `irori layer <vault> knowledge .` moves an existing vault, links
included.

## Decisions

1. **The bundle is rooted at the repository root.** `index.md` (with
   `okf_version`), `journal/`, `wiki/` and `ontology/` sit beside `AGENTS.md`,
   `README.md`, `.agents/`, `.irori/`, `.property/` and `contents/`. The
   folders, types, indexes and disciplines of ADR 006 are unchanged.
2. **The knowledge layer is what is neither schema nor contents.** OKF's
   "every `.md` but `index.md` carries frontmatter" and lint's walk apply to
   it, not to `AGENTS.md`, `CLAUDE.md`, `README.md` or a dot folder.
3. **Paths drop the prefix.** Skills, `.irori/notes.json` and the contract
   name `journal/…`, `wiki/…` and `ontology/` from the repository root.
4. **Existing knowledge bases move with irori**, not by hand: README says how,
   and what irori does not rewrite (paths written as text in their own
   `AGENTS.md` or skills).

## Consequences

- A copy registered with irori older than 0.2.0 is declared with
  `Knowledge_Base` and shows no folder indexes; README asks for 0.2.0.
- Until `init` removes it, the template's own `docs/` is knowledge too; it
  carries no frontmatter, so lint before `init` reports it.
