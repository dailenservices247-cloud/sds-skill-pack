---
name: plan-mode-prep
description: Generate a structured Plan Mode brief before any code mutation. Use when about to write or edit code on anything larger than a single-file edit. Produces the read-only plan the user approves before mutations land.
---

# Plan Mode Prep

Produces the brief that Claude Code's Plan Mode operates on. Forces planning as a first-class step, not an afterthought.

## When to use

Trigger on any of:
- About to write or edit code across multiple files
- About to refactor existing code
- About to add a new feature
- About to integrate a new dependency
- Any change that could break existing tests or workflows

Do NOT trigger on: single-line fixes, comment changes, formatting-only edits, exploratory reads.

## What the brief contains

1. **Goal**, the named outcome in one sentence
2. **Constraints**, what cannot change (tested behavior, public APIs, brand register, etc.)
3. **Files to read**, list of paths the plan will consume as input (read-only)
4. **Files to mutate**, list of paths the plan will modify or create, with a one-line reason per file
5. **Order of operations**, sequence of changes, each tagged with reversibility (safe / risky / destructive)
6. **Verification**, how we'll know the change worked (test command, manual check, MCP self-verify call)
7. **Rollback plan**, how to undo if verification fails
8. **Banked side effects**, any "while we're here" items found during planning that are NOT in this change

## Process

1. Parse the user's request into the named outcome.
2. Read-only scan: list every file that needs to be understood before changes can start.
3. Identify the constraints (tests that must keep passing, APIs that must keep their shape, register that must not drift).
4. Draft the order of operations with reversibility tags.
5. Name the verification step explicitly.
6. Name the rollback path explicitly.
7. List banked side effects with the "Parked until X" rule applied.
8. Present the full brief and wait for user approval before any mutation.

## Output format

```
# Plan Mode Brief: <named outcome>

## Goal
<one sentence>

## Constraints
- <constraint 1>
...

## Read-only inputs
- <path>, <reason for reading>
...

## Mutations
1. <path>, <one-line reason> [safe | risky | destructive]
...

## Verification
- <command or check>

## Rollback
- <undo path>

## Banked (not in this change)
- <item>, unblock when: <X>
```

## Anti-patterns

- Do NOT proceed to mutation before user approves the brief.
- Do NOT include speculative file changes ("we might need to touch X"). Either it's in the plan or it's banked.
- Do NOT mark anything destructive as "safe", destructive = data loss possible, irreversible.
- Do NOT skip the rollback plan because the change "feels safe." Cheap insurance.

## Handoff

Run /scope-defend on the brief before presenting. After user approves, run /self-verify on the verification step if it's UI-touching.
