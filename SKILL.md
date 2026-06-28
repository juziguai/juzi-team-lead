---
name: juzi-team-lead
description: "Tool-agnostic PM + sub-agent workflow for multi-file implementation tasks. Use when the user asks for delegation, parallel work, team coordination, or any task that benefits from split ownership."
metadata:
  trigger: Multi-file implementation, refactoring, feature development with 3+ files
  author: juzi
---

# Juzi Team Lead Skill

## Read Order

1. Read `references/core-workflow.md` for the shared workflow.
2. Read `references/adapters/codex.md` when the host is Codex.
3. Read `references/adapters/claude-code.md` when the host is Claude Code.
4. Read `references/adapters/antigravity.md` when the host is Antigravity.
5. Read `references/adapters/fallback.md` when sub-agents are unavailable.
6. Read `references/examples.md` only when you need task templates or examples.

## Rule

- Follow the core workflow first.
- Use the active host adapter second.
- Do not mix host-specific tool names into the core workflow.
