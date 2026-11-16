# Contributing to irori-templete

This is the contract for **developing the template**. It is not the contract the
template ships.

## The two contracts

`AGENTS.md` at the root is template content. It is the schema layer of a user's
knowledge base, read by the agents that work there after the user creates a
repository from this template. Do not put development instructions, workspace
permissions, provider accounts or machine paths in it, and do not describe it
as this repository's contributor guidance.

Guidance for working on the template belongs in this file. When this repository
is checked out inside the `dev-irori` development workspace, that workspace's
`AGENTS.md` still applies: shared development skills and runtime configuration
live at its root, and this repository's history stays independent from irori
and irori-extention. That path does not exist in a knowledge base created from
this template, which is why it is not linked.

## What the template is

[DESIGN.md](DESIGN.md) is the current design. Read it before changing the shape
of the knowledge layer, the declaration or the skills, and keep it current:
a change to the design updates `DESIGN.md` and adds a record to
[decisions/](decisions/). Steps an existing knowledge base needs to follow the
change go in [UPGRADING.md](UPGRADING.md).

Keep each fact with its owner ([ADR 010](decisions/010-a-small-contract-over-declared-data.md)):

- what may exist goes in `.property/property.json`, and skills read it rather
  than naming types or folders;
- irori's behaviour is cited from irori's documentation, not restated, and
  shipped files carry no per-version notes; the minimum irori version is in
  README alone;
- `AGENTS.md` holds invariants and boundaries only; a procedure goes in its
  skill.

Several decisions depend on what irori and the OKF specification actually say.
When you cite either, cite the file and check it still says what you claim.
The OKF specification is versioned; this template targets 0.2.

## Validation

There is no build, test or lint command for the template itself, and inventing
a toolchain for a Markdown repository is out of scope. For a change here:

- read the diff and check relative links resolve;
- for a skill: `name` equals the directory name, `description` is at most 400
  characters, and the file is under 16 KiB (irori's limits); `metadata.roles`
  and `metadata.projects`, when present, hold at most 20 names each of letters,
  digits, `-` and `_`, each at most 64 characters;
- for `.property/property.json`: it parses as JSON with `schemaVersion` 1;
  every key in `required` and in a type's `required` is a declared property;
  every `default` is one of its `options`; every type has a unique `heading`
  and a `folder` that is a relative path inside the knowledge layer (no leading
  `.`, no `..`, not `contents`); a relation's `from` names declared types or
  `any`, and its `to` is `page`, `url` or a list of declared types;
- for a retired skill: the directory holds `RETIRED.md` and no `SKILL.md`; its
  frontmatter has `retired` as `YYYY-MM-DD`, a `reason` of at most 400
  characters, an optional `replacement` naming an existing skill, and no `name`
  or `description`;
- for the root `index.md`, the declaration's folders or the `init` skill: copy
  the template to a disposable directory, perform `init` for each owner (a
  person, a project, an organization) by hand or with an agent, and check that
  every page other than `index.md` has frontmatter with the declared required
  keys, every folder with pages has the `index.md` irori generates, the root
  index keeps `okf_version`, `docs/` is gone, and no `log.md` exists (OKF §11);
- if you touched `.gitignore`, confirm `/contents/` is still ignored and
  `.irori/scope.json` is not. This repository itself never holds a
  `scope.json`: a copy made from it would share its `scopeId`.

Use a disposable knowledge base for anything that mutates files. Keep
credentials, account state, absolute machine paths and real user notes out of
the template.

## Language

Respond to the user in Japanese. Write identifiers, filenames, actor ids,
technical documents and commit messages in English. The shipped template
content is English; a user writes their own pages in their language.
