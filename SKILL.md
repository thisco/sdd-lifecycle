---
name: sdd-lifecycle
description: >-
  Orchestrates the complete Spec-Driven Development (SDD) lifecycle for features and bugs.
  Uses powerful models for planning/review and efficient models for implementation.
  Ensures strict adherence to AGENTS.md, checkpoints, and TDD practices.
  AI-agent agnostic: works with Claude, Gemini, Copilot, or any LLM-based agent.
---

# SDD Lifecycle Orchestrator

## Overview
This skill implements the "Gold Standard" Spec-Driven Development (SDD) workflow. It orchestrates a multi-agent process to resolve bugs and implement features, ensuring that all code changes are preceded by formal specifications, test plans, isolated environments, and rigorous code reviews, strictly adhering to the repository's `AGENTS.md` (or `CLAUDE.md` / `GEMINI.md`) guidelines.

**AI-agnostic**: all model references in this skill use capability descriptors ("fast/cheap model", "powerful reasoning model") rather than specific product names. Adapt them to the AI platform you are using.

## Dependencies

| Skill | Purpose |
|---|---|
| `brainstorming` | Explore codebase and define technical approach |
| `writing-plans` | Structure the task-by-task execution plan |
| `using-git-worktrees` | Workspace isolation via branches or worktrees |
| `subagent-driven-development` | Delegate coding tasks to specialized agents |
| `test-driven-development` | Ensure test coverage before production code |
| `verification-before-completion` | Local test suite and linter validation |
| `requesting-code-review` | Automated senior-level code review |
| `finishing-a-development-branch` | Standardize commits and merge strategy |

## Quick Start

**Feature**: "Let's use `sdd-lifecycle` to implement the new multi-region export feature."
**Bug fix**: "Use `sdd-lifecycle` to fix the timezone drift in the scheduling daemon."

---

## Workflow

### Phase 1 — Context & Brainstorming

- **Action**: Look for a project conventions file (`AGENTS.md`, `CLAUDE.md`, `GEMINI.md`, or similar). If found, read it to internalize project rules. If none exists, proceed using the conventions from the root README or ask the user for the preferred conventions.
- **Action**: Read the user's bug report or feature request.
- **Action**: Invoke the `brainstorming` skill to explore the codebase and propose an architectural solution.
- **Action**: Draft or update the formal specification document inside the `docs/specs/` directory.

### Phase 2 — Architecture & Breaking Changes Check (GATEKEEPER)

- **Action**: Scan the spec manually for destructive changes: API contract breaks, DB schema changes (column removal, type changes, index drops), or removal of public interfaces.
- **Action**: If destructive changes are found, **block the flow**: invoke `documentation-and-adrs` to create an ADR documenting the trade-offs, and **await explicit human approval** before proceeding to Phase 3.
- **Action**: If no breaking changes are found, proceed immediately to Phase 3.

> **Why this gate exists**: irreversible changes (dropped columns, removed endpoints) cost disproportionately more to undo than to prevent. Catching them before planning saves re-work.

### Phase 3 — Human Spec Review (CHECKPOINT)

- **Action**: Present the generated specification to the user.
- **Action**: **PAUSE EXECUTION.** Explicitly ask the user: *"Do you approve this specification, or are there any business/technical rules we should adjust before planning?"*
- *Do not proceed to Phase 4 until the user explicitly approves.*

### Phase 4 — Writing Plans

- **Action**: Once the spec is approved, invoke the `writing-plans` skill (or `planning-and-task-breakdown` if available).
- **Action**: Generate a detailed implementation plan in `docs/plans/YYYY-MM-DD-nome-curto.md`. The plan must include:
  - Exact file paths and function/method signatures to be created or modified.
  - Test coverage requirements per task.
  - Markdown checkboxes (`- [ ]`) for each task, enabling progress tracking across sessions.

### Phase 5 — Workspace Isolation

- **Action**: Invoke `using-git-worktrees` or use native git commands to create an isolated branch:
  - Features: `feat/YYYY-MM-DD-nome-curto`
  - Bug fixes: `fix/YYYY-MM-DD-nome-curto`
- **Action**: Confirm the branch was created and is clean before delegating to the implementation subagent.

### Phase 6 — Subagent-Driven Implementation (TDD Focus)

- **Action**: Invoke a subagent using `subagent-driven-development`. Use a **fast, efficient model** on your AI platform (optimized for speed and cost, not maximum reasoning power) for the implementation subagent.
- **Action**: The subagent **must** invoke the `test-driven-development` skill and write failing tests *before* modifying production code.
  - **If the project has no test suite configured**: surface this to the user before starting. Propose a minimal test setup or document the exception explicitly — do not silently skip tests.
- **Action**: The subagent must run the project's test suite (e.g., `pytest`, `jest`, `go test`, `cargo test`, `mvn test`) and linters after each task and mark the corresponding checkbox in the plan.
- **Action**: Instruct the subagent to update `CHANGELOG.md` upon completion.
- **Action**: **STRICT FALLBACK** — if the subagent hits a structural technical blocker where the spec proves unviable, it is **forbidden** from improvising workarounds. It must:
  1. Abort execution immediately.
  2. Return control to the orchestrator with a clear description of the blocker.
  3. The orchestrator then triggers a **Return to Phase 1, Step 4** (update the spec document) and re-triggers the Phase 3 checkpoint.

### Phase 7 — Verification & Code Review

- **Action**: Invoke `verification-before-completion` to run the full project test suite and linters locally. Do not claim success until all checks pass.
- **Action**: Invoke `requesting-code-review` to spawn a **"Code Reviewer" subagent** using a **powerful reasoning model** (highest capability available on your AI platform). The reviewer must check the diff against:
  - The original implementation plan (`docs/plans/`).
  - The project conventions file (`AGENTS.md` / `CLAUDE.md` / etc.).
- **Action**: Address review feedback. If critical issues are found, send them back to the implementation subagent (Phase 6). If architectural issues are found, escalate to Phase 1.

### Phase 8 — Finishing & Cleanup

- **Action**: When all checks and reviews pass, invoke `finishing-a-development-branch`.
- **Action**: Ensure commit messages follow the Conventional Commits specification (e.g., `feat(module): description`, `fix(scope): description`).
- **Action**: Decide on integration strategy:
  - **Create a PR** if working on a shared/team repository, if branch policy requires review, or if CI must pass before merge.
  - **Merge directly to `main`** only for solo projects where CI has already passed locally and no remote review is required.
- **Action**: Delete the feature branch (or worktree) after successful merge.

---

## Resuming a Paused Lifecycle

Large features may span multiple sessions. To resume:

1. Read `docs/plans/YYYY-MM-DD-nome-curto.md` and identify the last checked (`- [x]`) task.
2. Check `git log --oneline` to confirm which commits already landed.
3. Resume from the first unchecked task in the plan, on the correct feature branch.
4. If the spec or plan was updated since the last session, re-read `docs/specs/` before resuming.

---

## Common Mistakes

- **Skipping the Checkpoint (Phase 3)**: Proceeding to write code or plans without explicit human approval of the `docs/specs/` document.
- **Not reading the conventions file first**: Skipping `AGENTS.md` / `CLAUDE.md` in Phase 1 causes the subagent to violate project rules (language, naming, test strategy).
- **Lack of Traceability**: Failing to update the checkboxes in `docs/plans/` as the subagent progresses — makes multi-session resumption impossible.
- **Silent TDD Skip**: Letting the subagent skip tests because "there's no test setup" without surfacing the gap to the user.
- **Silent Failures**: Letting the implementation subagent loop indefinitely on failing tests instead of escalating to the orchestrator.
- **Wrong Branch Naming**: Not following `feat/` or `fix/` + date + short slug convention — breaks traceability in `git log`.

---

## Supplementary Skills Reference

These skills enhance specific phases when available in your agent's plugin ecosystem. All are optional unless marked.

| Phase | Skill | Purpose |
|---|---|---|
| 1 | `idea-refine` | Stress-tests initial ideas before committing to an approach |
| 1 | `spec-driven-development` | Formally authors the PRD/Spec in `docs/specs/` |
| 1–2 | `api-and-interface-design` | Essential when the spec defines new contracts or endpoints |
| 4 | `planning-and-task-breakdown` | Augments `writing-plans` with incremental, testable tasks |
| 4 | `documentation-and-adrs` | Records architectural decisions as ADRs |
| 6 | `test-driven-development` | **(Required)** Enforces tests-before-code |
| 6 | `incremental-implementation` | Prevents context overflow on large plans |
| 6 | `source-driven-development` | Grounds implementation in official docs, not model memory |
| 6 | `frontend-ui-engineering` | Frontend-specific implementation guidance |
| 6 | `security-and-hardening` | Security review during implementation |
| 6 | `performance-optimization` | Performance-critical path guidance |
| 7 | `debugging-and-error-recovery` | Root-cause analysis when tests fail in CI or locally |
| 7 | `code-review-and-quality` | Multi-axis review against plan and conventions |
| 7 | `browser-testing-with-devtools` | Dynamic frontend validation in a real browser |
| 8 | `shipping-and-launch` | Launch checklists and rollback planning |
| 8 | `deprecation-and-migration` | Handles legacy code removal safely |
