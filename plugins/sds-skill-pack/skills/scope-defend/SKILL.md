---
name: scope-defend
description: Apply Default-to-Minimum-Viable plus Surgical-Edits rules to incoming feature requests, refactoring proposals, or "while we're here" expansions. Use when the user asks for anything beyond the original named outcome. Names the smallest thing that delivers the outcome and banks the rest.
---

# Scope Defend

Two banked rules combined into one skill:
- **Default to the minimum viable.** Build the smallest thing that delivers the named outcome. Speculative features, premature abstractions, and "while we're here" expansions all require an explicit named justification.
- **Surgical edits unless authorized otherwise.** Edit only what the task requires. Preserve existing style, naming, and structure even when you'd write it differently.

## When to use

Trigger on any of:
- User proposes a feature with multiple sub-features bundled together
- Mid-build, an adjacent improvement looks tempting
- A refactor would "clean up" something not in the original task
- A new abstraction would future-proof something the user hasn't asked for
- Style / formatting / naming differs from your preference

Do NOT trigger on: explicit user request to expand scope, named refactoring tasks, sessions where the user authorized "while we're here" work.

## Process

1. Parse the user's request into the **named outcome** (the one specific deliverable they asked for).
2. List every item in the build plan or proposed change set.
3. For each item, classify:
   - **In scope**, directly serves the named outcome
   - **Out of scope, banked**, would be useful but not required for the named outcome; gets a banked note in the relay log
   - **Out of scope, declined**, speculative, premature, or stylistic; not banked, just refused
4. If any "Out of scope, banked" item has high value, present it as an option for the next sprint with named X (unblock condition). Otherwise it stays banked.
5. Generate the minimum-viable plan from in-scope items only.

## Anti-patterns

- Do NOT "improve" adjacent code while editing one file for an unrelated reason. That's a separate task needing its own greenlight.
- Do NOT introduce a new abstraction "for future flexibility." Build for the named outcome, refactor when the abstraction becomes necessary.
- Do NOT normalize style across a file you're touching for one reason. The user can't review style changes mixed with feature changes.

## Output format

```
# Scope Decision: <feature name>

## In scope (named outcome)
- <item>, directly serves <named outcome>
...

## Banked for next sprint
- <item>, value: <X>, unblock when: <Y>
...

## Declined
- <item>, reason: speculative / premature / stylistic
...
```

After scope is locked, run /plan-mode-prep on the in-scope items only.
