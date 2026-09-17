# Claude Code entry point

@AGENTS.md

The contract above owns this knowledge base. Nothing is repeated here.

Skills for this KB live in `.agents/skills/`, which Codex reads natively as
repository-scope skills. Claude Code does not read that directory on its own.
irori 0.1.6 and later supply the chosen skill with the request; earlier builds do
not, so read `.agents/skills/<name>/SKILL.md` directly when you need one and it
was not given to you. See [ADR 001](docs/decisions/001-kb-structure.md) (D9).

Working on the template itself rather than using it? Read
[docs/CONTRIBUTING.md](docs/CONTRIBUTING.md) — the contract above is shipped
content, not this repository's contributor guidance.
