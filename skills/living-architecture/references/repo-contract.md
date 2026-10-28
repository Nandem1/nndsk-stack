# Repository integration contract (copy/adapt)

Use this short section in the project's `AGENTS.md`. For Claude Code, ensure its `CLAUDE.md` also points to the same policy or relevant AGENTS content when needed. Do not duplicate the project's existing rules or overwrite its context.

```md
## Living Architecture

The repository's `docs/` architecture map is the external mental model; the implementation, migrations, contracts and verified behavior are operational truth.

Before non-trivial work, identify affected architecture areas and read only the relevant docs. Start from `docs/00-home.md` if the area is unclear. Apply the `living-architecture` skill in MAINTAIN mode when boundaries, data contracts, auth, integrations, runtime, business rules or deployment change.

When those changes materially affect the documented model, update only the relevant Markdown/Mermaid files **in the same change**. Cosmetic edits and internal refactors without behavioral impact do not need doc updates.

For non-trivial changes, finish with a short Architecture Impact report. For complex or high-risk changes, request an independent `living-architecture REVIEW` before merge. Never claim tests or production deployment succeeded without verifying them.
```

Suggested human prompt after implementing:

```text
Using living-architecture REVIEW, compare this diff against the actual code and docs. Do not modify files. Separate new drift from pre-existing drift and report PASS/PARTIAL/FAIL.
```

**Rule:** One copy of the docs per project, not one copy per model. Install the skill globally for personal environments or at the project level for remote agents and team usage. Avoid duplicate global+project installations of the same name if model selection becomes ambiguous.
