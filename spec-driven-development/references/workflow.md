# Spec-Driven Development Workflow

Hermes facilitates the method; user-selected agents may draft documents, assess code, implement, and test. Hermes retains responsibility for coherence, scope, verification, and clear handoffs.

## 0. Kickoff and agent setup

1. Ask which coding agents the user wants, how to invoke/reach each (CLI, Hermes agent, tmux session, or another route), and the intended roles. Ask once in a compact grouped set. If already specified, record rather than repeat it. Prefer the user's Hermes/tmux workflow when available; verify the actual setup and do not invent commands.
2. Check each chosen agent/route is usable before relying on it. Load its relevant skill/docs if available. Distinguish Hermes' own delegation from the user's external coding agents.
3. Agree on boundaries: which agent may inspect, draft, implement, test, or review; whether it may modify files; and how results will be handed back. Never delegate secrets or broad unrelated tasks. Do not run concurrent writers in one working tree. Parallelize read-only work or use isolated workspaces only with user permission.
4. Identify the project root/workspace and whether Git operations are permitted. Branches, worktrees, staging, commits, merges, and pushes require explicit user consent.

Completion: known agent roster/invocation (or user-approved no-agent path), verified access where possible, project workspace identified, and operating boundaries clear.

## 1. Classify and understand the project

Ask/inspect read-only to classify as:

- Greenfield: no meaningful implementation exists, or the user explicitly treats it as a new product.
- Brownfield: an existing application, codebase, or established system is being extended.

Do not infer greenfield just because the repository is sparse. Clarify ambiguous cases.

### Brownfield

Inspect structure, stack, entry points, data boundaries, existing tests and commands, project guidance, current roadmap/specs, and relevant Git state (read-only unless otherwise authorized). Delegate a focused codebase assessment when useful. Ask the assessment agent to report evidence and unknowns: architecture, conventions, test health/coverage gaps, risks, existing contracts, and recommendations—not to refactor. Hermes checks the report against the repository and synthesizes it. Preserve working patterns unless there's a reason to change.

### Greenfield

Locate and confirm the intended repository/workspace (including any existing starter repository). If none exists, ask where the user wants it and whether they want repository creation/cloning; do not create or clone unasked. Gather stakeholder input, product goal, intended users, constraints, and success outcomes.

Completion: project type, current state, source of truth, and known constraints are recorded; brownfield assessment or greenfield discovery is sufficient to proceed.

## 2. Create/review the project constitution

Ask focused questions before drafting. The user owns decisions; agent-generated recommendations remain proposals until approved. Keep project-level documents concise:

- `mission.md`: product purpose, users, problems/outcomes, boundaries, and success signals.
- `tech-stack.md`: chosen technologies and rationale, constraints, conventions, and the testing contract.
- `roadmap.md`: small ordered user-visible increments toward the mission, with completion state and important dependencies.

The constitution MUST make testing operational, not aspirational. Record:

- test framework and relevant versions/pinning policy if the project requires it;
- where unit, integration, contract, and end-to-end tests live (only categories the project needs);
- canonical commands for focused tests and full regression suite, plus typecheck/lint/build or other required checks;
- the rule that new behavior gains tests and all prior relevant tests remain part of regression validation;
- how test data, external services, and nondeterministic dependencies are handled.

Avoid selecting a test stack before inspecting brownfield conventions. On greenfield, recommend a minimal fit and get approval. Define how an individual feature's `validation.md` points to or adds to this shared test strategy.

Changeability principles belong in tech-stack or engineering principles: prefer cohesive modules and small local changes; inject only real external/variable dependencies; use typed canonical definitions for repeated domain values; keep stable one-off literals local; externalize user-facing copy only when localization/reuse/edit frequency calls for it. No blanket ban on editing existing files or hardcoded literals.

Completion: user reviewed/approved material choices, tests and commands are explicit and discoverable, and assumptions are labeled. Do not write files until the user authorizes/document-drafting is clearly requested.

## 3. Shape and review the roadmap

Derive increments from mission, user/stakeholder needs, constraints, brownfield findings, dependencies, risk, learning value, and the intended MVP. Ask the user to establish the MVP objective and priority trade-offs if unclear. Agent proposes a sequence and explains why; user decides.

A roadmap item states an outcome and boundary, not a list of technical chores or every conceivable feature. Keep phases independently demonstrable/reviewable where practical. Show dependencies and assumptions. Do not claim the agent knows business priorities that were not supplied. Keep the roadmap revisable as evidence changes.

Completion: user accepts an initial sequence or explicitly asks for a provisional proposal; unresolved assumptions are visible.

## 4. Select one feature and resolve material questions

Choose the next incomplete increment. State its user-visible outcome and confirm that it is a coherent, testable slice. Before drafting a feature spec, interview the user on decisions that materially affect scope, data, behavior, UX, constraints, or acceptance. Ask focused questions, preferably grouped; do not ask about details already settled by the constitution or existing patterns. Record unanswered non-blocking details as explicit assumptions; block on consequential ambiguity.

A coding agent may inspect adjacent code/specs or propose questions, but Hermes ensures the user has agreed to material scope.

Completion: feature outcome, boundary, and material choices are clear enough to specify.

## 5. Draft and cross-check the feature specification

Read the mission, tech stack, roadmap, relevant code, adjacent specs, and test conventions first. Use a dated descriptive folder such as `specs/YYYY-MM-DD-feature-name/` if it matches repository conventions. A selected agent may draft the documents, but Hermes checks them and the user resolves material decisions.

Create:

- `requirements.md`: goal, scope/non-goals, observable behaviors and data rules, agreed decisions, constraints, context, and labeled assumptions.
- `plan.md`: ordered small task groups; each task traces to a requirement or necessary quality attribute and avoids unapproved scope.
- `validation.md`: concrete pass/fail acceptance checks, test cases or test locations to add, commands, required previous regression suite, and any manual checks with observable results.

Keep executable tests in the project's shared test suite, not duplicated inside each feature spec. Validation names the tests to add/run and the project-wide checks to run.

Cross-check both directions: every requirement has implementation coverage and a verification method; every plan item is justified; validation covers the requirements; test commands match the constitution and repository. Fix inconsistency before implementation.

Completion: the user accepts scope and the three documents form a consistent, testable contract.

## 6. Implement in small slices with agent handoffs

Delegate bounded work to the user's selected coding agent(s), matching role and task. Provide project root, relevant approved documents, exact task boundary, test strategy/commands, and report format. Ask agents not to alter out-of-scope files, weaken/delete tests, or perform unauthorized Git operations. Do not have multiple agents edit the same files simultaneously.

Implement in small vertical slices. Follow the project's chosen testing method (TDD if specified/adopted); test behavior, not private implementation details. For each slice, run focused tests and inspect output/diff. Preserve all prior tests. For changes to an earlier contract, get agreement, update the affected requirements/spec and tests deliberately, and explain the compatibility impact. Do not modify earlier tests solely to accommodate a defect or scope creep.

Hermes reviews the actual diff against requirements and the plan, checks for unrelated changes and overengineering, and validates agent claims. DI and abstraction should solve a real seam; shared domain values should be type-safe where duplication risks drift; no automatic centralization of every string.

Completion: all planned requirements implemented, changes are in scope and reviewed, and focused checks pass (or failures are reported with a concrete next action).

## 7. Validate feature and regressions

Treat `validation.md` as acceptance criteria, not as a separate test suite. Run:

1. focused tests for the new/changed behavior during implementation;
2. the complete available regression suite, including previous feature tests;
3. constitution-required typecheck/lint/build/security or other checks;
4. manual checks only where automated validation is insufficient.

Read real command results and inspect failures. Do not claim a check passed if it was skipped, mocked beyond usefulness, or not actually run. If validation fails, fix implementation or revisit an agreed requirement; never make tests pass by deleting/weakening prior coverage. If a requirement changes, update spec and test in a traceable way.

Completion: every acceptance criterion has evidence; outstanding failures/risks are explicit.

## 8. Close the phase and learn

Only after validation succeeds, update the roadmap phase to complete in the repository's established format. Summarize delivered outcome, files/change areas, tests/checks and results, and any deviations or open issues. Git branch, commit, merge, push, or cleanup remains a separate action requiring explicit permission.

Compare discoveries with the spec and roadmap. Update decision records where needed; do not retroactively represent an assumption as user-approved. Replan downstream phases when dependencies, risk, or user priorities changed. Keep a completed phase complete when later optional enhancements arise: capture new work as a new roadmap item/feature. Reopen the phase only if its accepted outcome was not actually met or a contract change requires it.

Completion: delivered phase is accurately marked, follow-up work is captured, and the user receives an evidence-based handoff.

## 9. Agent orchestration and verification rules

- Prefer the invocation route the user specified (for example a known Hermes-managed agent via tmux); consult that agent's installed skill for exact commands. Do not improvise a CLI or assume a particular agent exists.
- Use one writer per shared worktree. Parallelize independent code assessment/review or separate isolated tasks only where files and interfaces do not conflict. Hermes integrates results and resolves disagreements against approved requirements.
- Have agents report: task performed, files changed, commands executed with outcomes, assumptions, blockers, and tests not run. Hermes independently checks repository state/diffs and reruns the necessary checks.
- Writing documents or running tests does not authorize unrelated code edits or Git state changes. Follow explicit user consent for all persistent or externally visible operations.
- When the user asks to learn step-by-step, explain the current step only and wait for the cue before moving on.
