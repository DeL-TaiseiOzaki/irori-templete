# Contributing to irori-templete

This is the contract for **developing the template**. It is not the contract the
template ships.

## The two contracts

`AGENTS.md` at the root is template content. It is read by the agents working in
a user's knowledge base after they create a repository from this template, and
it is the schema layer of that knowledge base. Do not put development
instructions, workspace permissions, provider accounts or machine paths in it,
and do not describe it as this repository's contributor guidance.

Guidance for working on the template belongs in this file. When this repository is checked out inside the
`KB_design` development workspace, that workspace's `AGENTS.md` still applies:
shared development skills and runtime configuration live at its root, and this
repository's history stays independent from irori and LayeredKB. That path does
not exist in a knowledge base created from this template, which is why it is not
linked.

## What the template is

A knowledge base that a person clones and registers in irori. The confirmed
structure and the reasoning behind it are in
[ADR 001](decisions/001-kb-structure.md). Read it before changing the shape of
`Knowledge_Base/`, the frontmatter contract, or where skills live — each of
those is a decision with a recorded alternative, and reversing one should update
the ADR rather than leave it stale.

Several decisions depend on what irori actually implements. When you cite irori
behaviour, cite the file and check the line still says what you claim; the ADR's
references were verified on 2026-09-16 and will drift.

## Validation

There is no build, test or lint command, and inventing a Node or Python
toolchain for a Markdown and CSV repository is out of scope. For a change here:

- read the diff;
- check relative links resolve;
- if you touched `.irori/ontology.json` or `Knowledge_Base/ontology/*.csv`,
  register a disposable copy of the template in irori and open **オントロジー**
  to confirm the declaration still loads — irori rejects the whole file on a
  single structural error;
- if you touched `.gitignore`, confirm `/contents/` and `/.irori/scope.json` are
  still ignored. irori appends `/contents/` itself when it is missing, and a
  committed `scope.json` breaks registration and pulls for every copy.

Use a disposable knowledge base for anything that mutates files. Keep
credentials, account state, absolute machine paths and real user notes out of
the template, including out of the ontology seed.

## Language

Respond to the user in Japanese. Write identifiers, filenames, ontology ids,
technical documents and commit messages in English. The shipped template content
is English; a user localises their own notes.
