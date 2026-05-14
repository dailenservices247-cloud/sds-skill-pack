---
name: brand-voice-check
description: Lint any markdown copy against a brand voice register before publish. Catches banned phrases, drop-overclaim violations, em-dashes, and filler words. Use before any client-facing copy ships.
---

# Brand Voice Check

Enforces the SDS voice register on any copy before it leaves the building. Runs the same rule pack as `~/.local/bin/brand-voice-lint` (if installed locally) plus an authoring-time review pass.

## When to use

Trigger on any of:
- Generated marketing copy
- Sales scripts, hooks, headlines
- Email drafts
- Web copy (hero, body, CTA)
- Proposal drafts
- Any text that will be read by a customer, prospect, or partner

Do NOT trigger on: internal docs, code comments, atomic-node frontmatter, session relay logs.

## Default rule pack (overridable per project)

### Banned phrases (overclaim register)
- "is dying" / "is dead" / "revolutionary" / "revolutionize"
- "10x your X" / "100x" / "billion-dollar idea"
- "skyrocket" / "game-changing" / "game changer"
- "disrupt" / "disruptor" / "next-gen" / "next generation"
- "cutting-edge" / "bleeding-edge" / "world-class"
- "transform your" / "elevate your" / "supercharge your"

### Banned phrases (AI-bro register)
- "leverage AI" / "AI-powered" / "harness the power" / "unlock the power"
- "the future is now" / "in this digital age"

### Banned phrases (drop-overclaim)
- "save you hours" / "save hours" / "save time" / "time saved"
- "replace your team" / "fire your X"

### Filler (warnings, not failures)
- "very" / "really" / "just" / "basically" / "actually" / "simply"

### Banned characters
- Em-dash (, ), use comma, colon, semicolon, period, or parens instead
- Double-hyphen substitute (--), same rule

## Process

1. Run the rule pack against the input text. Flag every match with line + reason.
2. For each banned phrase, propose a register-matched replacement.
3. For em-dashes, replace with the right grammatical alternative based on context.
4. For filler warnings, mark as "polish opportunity", don't fail the check on these.
5. Output a clean version + a violations summary so the user can verify each substitution.

## Override mechanism

Project-specific rules live at `<project>/.brand-voice-rules.json`:

```json
{
  "additional_banned": ["specific-phrase-1", "specific-phrase-2"],
  "exempt_phrases": ["industry-term-that-matches-a-rule"],
  "allow_em_dash": false,
  "fail_on_filler": false
}
```

Load the override file on every check. Project rules add to (or exempt from) the default pack, never replace it.

## Anti-patterns

- Do NOT skip this check on "internal" copy that becomes external later. Run it on anything that might ever ship.
- Do NOT auto-rewrite without showing the user the original. Substitutions need user review.
- Do NOT add a banned phrase to "exempt_phrases" without a written reason in the override file's comments.
