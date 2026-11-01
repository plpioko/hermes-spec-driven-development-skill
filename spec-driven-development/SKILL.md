---
name: spec-driven-development
description: "Use for spec-driven development. Guide feature work."
version: 2.0.0
author: Hermes Agent
license: MIT
metadata:
  hermes:
    tags: [SDD, specifications, roadmap, feature planning, AI coding agents, testing, orchestration]
---

# Spec-Driven Development

Hermes is the SDD facilitator and delivery orchestrator: it keeps product intent, decisions, specs, tests, and roadmap aligned. User-selected coding agents do the delegated research, document drafting, implementation, and testing. Hermes independently checks their outputs and reports evidence; an agent's claim is not verification.

Guide one step at a time when the user is learning or asks to proceed incrementally. Do not dump the entire lifecycle unless requested.

## When to Use

Use for greenfield or brownfield product work organized around a mission, technology choices, roadmap, feature specs, implementation, and validation. Use when Hermes coordinates coding agents selected by the user.

## Non-negotiable rules

- The user owns product priorities and consequential trade-offs. Agents may analyze and recommend; obtain agreement on scope and material decisions before treating them as settled.
- At project kickoff, establish which coding agents the user wants, how each is invoked/reached (CLI, Hermes agent, tmux session, or other known route), and their intended roles. Prefer the user's Hermes/tmux workflow when available. Never assume an installed agent, command, session, or capability; verify readiness before delegation. If the user already supplied the roster and invocation, do not ask again.
- Hermes owns the process: identify the current SDD stage, keep handoffs scoped, preserve decisions, review agent outputs, reconcile conflicts, and verify acceptance criteria. Delegate implementation and document drafting only within agreed scope.
- First classify the project as greenfield or brownfield. For brownfield, inspect or delegate an explicit codebase assessment before proposing architecture or the first roadmap. For greenfield, locate/confirm the intended repository and workspace before drafting files; do not silently create, clone, or modify a repository.
- Define testing in the constitution: test framework, test locations/conventions, commands, and required checks. Every feature adds tests for new behavior; preserve earlier tests and run regression coverage. Do not rewrite old tests merely to make a change pass. Update affected tests only when an agreed behavior/contract changes, and explain why.
- Keep requirements, plan, and validation mutually traceable. No implementation task may add unapproved behavior.
- Aim for small, local, reviewable changes—not zero changes to existing code. Use dependency injection at real external-dependency or variation boundaries; do not inject every helper. Use typed shared definitions for repeated domain values; keep one-off stable literals near their use. Externalize UI copy when localization, reuse, or frequent editing warrants it. Avoid speculative abstractions and global string catalogs.
- Do not stage, commit, merge, push, create/delete branches, or create worktrees without the user's explicit permission. A request to implement or write specs is not Git-operation consent.
- Verify through repository state and actual command output. Report tests that were not run and remaining uncertainty plainly.

## Workflow

Follow [the workflow reference](references/workflow.md). Keep the active step explicit, and pause for the user's cue when proceeding step by step. Before any file-writing or Git action, respect the user's consent and the relevant stage gates.
