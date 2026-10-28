---
name: living-architecture
description: Maintain a repository's visual architecture documentation as living external memory. Use to bootstrap architecture maps from existing code, keep Markdown/Mermaid diagrams aligned with implementation changes, independently review code-to-doc drift, or audit stale architecture knowledge. Invoke for architecture, data contracts, authentication, integrations, runtime flows, and material domain-rule changes; not for cosmetic edits or routine refactors without behavioral impact.
license: MIT
compatibility: Requires repository filesystem access and Git for diff-based reviews. Markdown and Mermaid work in Obsidian without additional plugins.
---

# Living Architecture

Keep the **mental model of a software system** recoverable and synchronized with its implementation. This skill is a workflow, not an architecture framework, mandatory folder taxonomy, or code generator.

## Principles

1. **Operational truth:** inspected code, migrations, tests, and verified runtime behavior determine what currently exists. Documentation records the architectural mental model; it is not automatically authoritative over code.
2. **Visual first:** use small Markdown/Mermaid diagrams for boundaries, data paths, state machines, and meaningful runtime interactions. Supplement with short invariants and links to implementation.
3. **Minimum useful knowledge:** document *why*, boundaries, responsibilities, contracts, business semantics, and essential flows. Avoid inventories and code details that can be derived automatically.
4. **Selective context:** read only the relevant documentation and source files. Start from the home/index note if the affected area is unclear.
5. **One change, one model:** when a meaningful architecture or behavior change is implemented, update only impacted documentation in the same diff.
6. **Evidence, not guesses:** distinguish verified facts, inferred behavior, unvalidated deployment claims, and unknowns. Do not describe planned features as implemented.
7. **Project independence:** do not prescribe React, FSD, Go, microservices, Supabase, or any particular stack. Capture and respect the *actual* project boundaries.
8. **Respect authority:** never publish, deploy, commit, clean, or modify project code unless the user or parent task authorized it. In REVIEW and AUDIT, remain read-only.

## Select a mode

Use the explicit mode if supplied; otherwise infer from the task:

| Mode | Trigger | Allowed changes | Outcome |
|------|---------|-----------------|---------|
| `BOOTSTRAP` | Project lacks a navigable architecture map | Docs and minimal opt-in agent contract, as authorized | Accurate entry point + essential diagrams |
| `MAINTAIN` | Coding task with potential structural/behavioral impact | Code requested by user + only affected docs | Implementation and docs aligned |
| `REVIEW` | Independent check of an implementation/diff | None | PASS/PARTIAL/FAIL with evidence |
| `AUDIT` | Inspect existing documentation for drift | None | Prioritized drift inventory |

For a purely cosmetic or local change with no architectural/behavioral impact, do not create or update diagrams; report `unchanged` or `not applicable` briefly. Never add artificial features solely to generate documentation unless the user explicitly requests a disposable test.

## BOOTSTRAP — reconstruct before writing

1. Inspect the repository's top-level organization, source entry points, existing documentation, package manifests, migrations, API schemas, authorization implementation, integrations, tests, and deployment configuration. Inspect selectively; avoid reading the entire repository blindly.
2. Identify runtime systems and external dependencies, domain modules, real boundaries, data ownership, the actual read/write paths, key journeys, auth/permissions, and non-obvious invariants.
3. Contrast existing documentation with code; distinguish implemented, planned, and uncertain behavior. Preserve existing documentation and conventions.
4. **Classify before archiving:** distinguish active architecture notes, normative contracts, operational guides, historical plans/audits, and tool-owned/generated paths. Check inbound references and maintenance obligations in `README.md`, `AGENTS.md`, `CLAUDE.md`, scripts, and workflows. A file is not historical merely because it was moved to `legacy/`. Do not claim `legacy/` is unmaintained while other instructions require maintaining files inside it. Verify current contracts/guides against code before moving or declaring them authoritative. Preserve user changes and avoid moving hardcoded paths without addressing their consumers. Resolve conflicts explicitly; do not silently rewrite authority.
5. Propose a **small** documentation map, scaled to the project. Do not manufacture empty categories or rigidly create six files. If reorganization is needed, name which docs stay active, which become historical, and the references that must change. Never wholesale-archive existing docs by default.
6. Unless the user requests immediate implementation, present the proposed files/diagrams first. If authorized, write the minimal docs, an entry-point `docs/00-home.md`, and navigable links to existing README/AGENTS/other notes.
7. Verify link targets, references from agent instructions/scripts/workflows, and Mermaid syntax where tooling is available. Do not claim runtime validation without running it.

Suggested topics **only if they add value**: architecture/boundaries, data plane, integrations/sync, auth/runtime flows, domain semantics, operations, and ADRs. See [starter-docs.md](references/starter-docs.md) for a small starting layout.

## MAINTAIN — apply to normal feature work

**Before implementation**

1. Inspect the relevant implementation, tests, and local `AGENTS.md` or equivalent agent rules.
2. Identify affected boundaries and read only the corresponding docs. Use the home/index note when needed.
3. Record expected impact: module responsibilities, dependencies, data/schema, public API/RPC, auth/authorization, external integrations, jobs, runtime journeys, business rules, and infrastructure/deployment.

**After implementation**

4. Run relevant tests, linters, type checks, builds, and validation permitted by the task. State explicitly what was and was not run.
5. Compare the resulting diff to the docs. Update only documentation that would otherwise give a returning developer a **wrong model** of the system. Keep diagrams concise and visual-first.
6. Keep code modifications and their necessary docs together; do not create speculative ADRs or documentation-only churn.
7. Return an `Architecture Impact` summary using the report format below. If behavior is unverified in production, label it unverified.

If docs and code disagree *before* the change, distinguish pre-existing drift from drift introduced by the implementation; do not silently broaden feature scope to fix unrelated discrepancies.

## REVIEW — independent, read-only judgement

1. Establish the base ref and inspect all changes, including uncommitted files when available. Do not assume `git diff main` includes untracked files; inspect status and those files separately.
2. Trace each claimed boundary, API, auth, data, integration, runtime, and infra change back to source code and relevant tests/configuration.
3. Compare updated docs/diagrams against the implemented control flow. Check missing and unnecessary doc updates. For documentation migrations, verify that active contracts, guides, agent instructions, and hardcoded consumers agree on each document's authority and location.
4. Separate **new drift**, **pre-existing drift**, and **unable to verify**. Absence of proof is not proof of correctness.
5. Assess impact-report accuracy. Emit `PASS` (grounded), `PARTIAL` (gaps/uncertainty), or `FAIL` (material inconsistency). Do not change files.

Return:

```text
Architecture Drift Review
- Code ↔ docs consistency: PASS | PARTIAL | FAIL
- Missing doc updates: none | paths
- Unnecessary doc updates: none | paths
- New drift: none | evidence
- Pre-existing drift: none | evidence
- Architecture Impact accuracy: correct | partial | incorrect
- Validation limitations: none | details
- Cleanup safe: yes | no | not applicable
```

Use `Cleanup safe` only when reviewing a disposable branch and ensure unrelated user changes will not be destroyed.

## AUDIT — read-only inventory

Inspect the current architecture documentation against code and critical flows. Rank findings by architectural impact: security, tenancy/authorization, data ownership, API contracts, integration and sync paths, application runtime, deployment, and domain invariants. Report the minimal file changes needed. Do not rewrite docs or code in AUDIT.

## Documentation style

- Prefer diagrams that fit an ordinary Obsidian viewport without horizontal scrolling. For long pipelines, use `flowchart TB` or split a diagram into overview and detailed sequence. Do not make text microscopic just to fit.
- Keep a page in this order when appropriate: 2–4 lines of context → main diagram → 3–6 critical invariants → links to relevant code.
- Use `[[wikilinks]]` to connect *existing knowledge notes* when supported by the vault. Use standard relative Markdown links for source code, SQL migrations, tests, or other files; do not create unresolved graph nodes to represent code files.
- Do not duplicate complete SQL schemas, API inventories, script lists, or dependency trees that authoritative tooling can generate. Link to the source of truth instead.
- An ADR is warranted for a durable choice with real alternatives and consequences, **not** for every implementation detail. Record accepted decisions without inventing historical rationale.
- C4/Structurizr is optional for complex systems when several views need a shared architecture model. Start with Markdown/Mermaid unless the repository already uses another formalism. Do not force a second modeling system.
- Avoid maintaining a hand-edited `last_reviewed` date as a proxy for correctness; use diffs and verification.

## Architecture Impact format

For non-trivial change tasks, finish with:

```text
Architecture Impact
- Boundaries: changed | unchanged
- Data model: changed | unchanged
- API/RPC: changed | unchanged
- Auth/authorization: changed | unchanged
- Integrations/jobs: changed | unchanged
- Application runtime: changed | unchanged
- Business rules: changed | unchanged
- Infrastructure/request routing: changed | unchanged
- Docs updated: paths | none
- ADR required: yes | no
- Validation: commands/tests run; unavailable checks
```

These are orthogonal dimensions: an infrastructure rewrite can affect ingress without changing application runtime; a provider credential mechanism can change external authentication without changing application user authorization. Explain only the non-obvious distinctions.

## Project-specific adoption

A globally installed skill does **not** guarantee every coding agent will automatically invoke it for every task. For projects that must enforce this as part of their Definition of Done, place the compact contract from [repo-contract.md](references/repo-contract.md) in that project's persistent agent instructions (`AGENTS.md`, and/or `CLAUDE.md` as appropriate). Keep stack-specific conventions in the project, not in this skill.