# ADR 007 — A category of the scope's own keeps the lightest discipline

Date: 2026-10-08. Status: follows the owner's request in irori
([ADR 029](https://github.com/DeL-TaiseiOzaki/irori/blob/main/docs/decisions/029-free-categories.md)).
Amends [ADR 006](006-one-layout.md) D1, where the category was one of three.

## Context

The owner asked that categories be set up freely. irori 0.1.87 stores any
short name as `category` in `.irori/scope.json`, keeping `personal`, `team` and
`organization` as presets. This template reads the category for two things:
who confirms pages and what lint requires (AGENTS.md, **Folders**), and what
`init` writes for the owner's identity and `.irori/notes.json`.

## Decision

- **Discipline**: a category other than the presets, or none, follows the
  personal row: the person who wrote a page confirms it, `verified` is not
  required, and lint adds no category rule. A stricter discipline is chosen by
  choosing `team` or `organization`, so a name never changes what lint rejects.
- **init**: for such a category it asks whether a person, a project or an
  organization owns the scope, and writes that owner's identity page and
  `notes.json` row.

## Consequences

- No new field: lint keeps reading only `category`, and a scope's own name is
  free to change without changing its rules.
- A team that names its scope `lab` gets no `sources` check until it chooses
  `team`. That is deliberate; AGENTS.md says so next to the table.
