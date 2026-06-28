# Core Workflow

## Table of Contents

- Overview
- Roles
- When to Use
- Task Sizing
- Pre-flight
- Planning
- Dispatch
- Verification
- Accept or Reject
- Composite Modes
- Recovery and Rollback
- Context Sharing
- Budget and Observability
- Escalation

## Overview

Use a PM + worker workflow for non-trivial implementation work. Plan first, dispatch only after the task is split cleanly, verify with actual files, and accept or reject explicitly.

Never let a worker verify its own work.

The host adapter owns the actual tool mapping. This file only defines the shared workflow.

## Roles

- PM: break work apart, assign ownership, verify, and decide pass/fail.
- worker: implement a bounded write task.
- reviewer: perform read-only verification or cross-checks.
- planner: shape decomposition or architecture before implementation.

## When to Use

Use this workflow when the task involves:

- 3 or more files
- multiple independent changes
- parallelizable work
- module boundaries that can be separated cleanly

Do not use it for:

- a single small edit
- pure research or exploration
- an urgent hotfix that should be applied directly

## Task Sizing

Guideline:

- 1 file or under 10 lines: do it directly
- 1-2 files: usually one worker is enough
- 3-5 files: split by ownership and parallelize if possible
- 6+ files or multiple modules: group by module

If a task would cause overlapping writes, split it further.

## Pre-flight

Before planning, do three checks:

1. Inspect workspace state and record any pre-existing user edits.
2. List the files in scope and mark file conflicts.
3. Define acceptance criteria and the exact verification commands or checks.

Never overwrite user changes without acknowledging them.

## Planning

- Break the work into self-contained tasks.
- Give each task a single owner and a narrow write scope.
- Define dependencies explicitly.
- Prefer independent tasks that can run in parallel.
- If the task cannot be decomposed safely, keep it on the main thread.

## Dispatch

- Dispatch independent workers in parallel when the write scopes do not overlap.
- Keep task descriptions concrete: file paths, expected behavior, and acceptance criteria.
- If the host cannot run workers in the background, degrade to synchronous execution and say so.
- Do not send overlapping tasks to different workers.

## Verification

- Read the actual changed files.
- Check each acceptance criterion directly.
- Use a separate reviewer when cross-checking matters.
- Do not trust a worker's self-report alone.

## Accept or Reject

- PASS: mark the task complete and report it clearly.
- FAIL: describe the exact problem and send the task back to a new worker.
- After repeated failures, reduce scope or escalate to the user.

## Composite Modes

- Pipeline: use when the work needs stepwise refinement or is on a sensitive path.
- Swarm: use when multiple valid solutions need to compete.
- Hierarchical: use when the task spans many files or several modules with clear boundaries.

## Recovery and Rollback

- Prefer repair over rollback.
- If rollback is needed, preserve user edits first.
- Use the smallest safe reversal possible.
- If the task already touched a user-edited file, do not use an overwrite-style rollback.

## Context Sharing

- PM is the only routing point.
- Workers should not message each other directly.
- Pass only the context needed for the next task.
- Reuse shared read-only context instead of duplicating work.

## Budget and Observability

- Track completion, retries, and worker turnaround.
- Keep task descriptions tight.
- Merge tiny tasks instead of spawning extra workers.
- Report when parallelism is unavailable so the user sees the downgrade.

## Escalation

Escalate to the user when:

- the request is ambiguous in a way that changes the implementation
- the work touches a core or risky path that needs confirmation
- delegation is unavailable and the task is too large to do safely in one pass
