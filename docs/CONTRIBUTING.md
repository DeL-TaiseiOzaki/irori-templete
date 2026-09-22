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
is checked out inside the `KB_design` development workspace, that workspace's
`AGENTS.md` still applies: shared development skills and runtime configuration
live at its root, and this repository's history stays independent from irori
and irori-extention. That path does not exist in a knowledge base created from
this template, which is why it is not linked.

## What the template is

A knowledge base that a person clones, registers in irori and initialises with
the `init` skill. The structure and its reasoning are in
[ADR 002](decisions/002-okf-bundle.md), which supersedes the folder, navigation,
ontology, frontmatter and skill decisions of
[ADR 001](decisions/001-kb-structure.md) and keeps the rest. Read both before
changing the shape of `Knowledge_Base/`, the frontmatter contract or the skills,
and update the ADR rather than leave it stale.

Several decisions depend on what irori and the OKF specification actually say.
When you cite either, cite the file and check the line still says what you
claim; the references were verified on 2026-09-17 and will drift. The OKF
specification is versioned; this template targets 0.2.

## Validation

There is no build, test or lint command for the template itself, and inventing
a toolchain for a Markdown repository is out of scope. For a change here:

- read the diff and check relative links resolve;
- for a skill: `name` equals the directory name, `description` is at most 400
  characters, and the file is under 16 KiB (irori's limits); `metadata.roles`
  and `metadata.projects`, when present, hold at most 20 names each of letters,
  digits, `-` and `_`, each at most 64 characters;
- for a retired skill: the directory holds `RETIRED.md` and no `SKILL.md`; its
  frontmatter has `retired` as `YYYY-MM-DD`, a `reason` of at most 400
  characters, an optional `replacement` naming an existing skill, and no `name`
  or `description`;
- for anything under `Knowledge_Base/` or in the `init` skill: copy the
  template to a disposable directory, perform `init` for each category by hand
  or with an agent, and check that every page other than `index.md` has
  frontmatter with a non-empty `type`, every folder has an `index.md`, the root
  index keeps `okf_version`, and no `log.md` exists (OKF §11);
- if you touched `.gitignore`, confirm `/contents/` and `/.irori/scope.json`
  are still ignored. irori appends `/contents/` itself when it is missing, and a
  committed `scope.json` breaks registration and pulls for every copy.

Use a disposable knowledge base for anything that mutates files. Keep
credentials, account state, absolute machine paths and real user notes out of
the template.

## Language

Respond to the user in Japanese. Write identifiers, filenames, actor ids,
technical documents and commit messages in English. The shipped template
content is English; a user writes their own pages in their language.
