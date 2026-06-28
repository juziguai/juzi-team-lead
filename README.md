# Juzi Team Lead Skill

Tool-agnostic PM + sub-agent workflow for multi-file implementation work.

## What this is

This repository defines a shared workflow for splitting non-trivial tasks into
bounded pieces, assigning ownership, and verifying results with actual files.
The host adapter decides how that workflow maps to concrete tools.

## When to use it

Use this skill when the work involves:

- 3 or more files
- multiple independent changes
- parallelizable work
- module boundaries that can be separated cleanly

Do not use it for:

- a single small edit
- pure research or exploration
- an urgent hotfix that should be applied directly

## Read order

1. `SKILL.md` for the skill entry point.
2. `references/core-workflow.md` for the shared PM/worker workflow.
3. `references/adapters/codex.md` when the host is Codex.
4. `references/adapters/claude-code.md` when the host is Claude Code.
5. `references/adapters/fallback.md` when sub-agents are unavailable.
6. `references/examples.md` when you need task templates or examples.

## Repository layout

```text
juzi-team-lead/
├── README.md
├── SKILL.md
└── references/
    ├── core-workflow.md
    ├── examples.md
    └── adapters/
        ├── codex.md
        ├── claude-code.md
        └── fallback.md
```

## Core rule

- Follow the shared workflow first.
- Use the active host adapter second.
- Do not mix host-specific tool names into the core workflow.
