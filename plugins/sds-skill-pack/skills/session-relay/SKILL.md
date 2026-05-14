---
name: session-relay
description: Produce a relay.md entry in the banked format at end of any significant session. Use at session close to capture what was done, files changed, decisions locked, lessons surfaced, and status. Powers cross-session memory consolidation.
---

# Session Relay

End-of-session ritual. Writes one structured entry to `~/.claude/relay.md` (or equivalent activity feed) so future sessions and dream-protocol consolidation can see what happened.

## When to use

Trigger at the end of any session that:
- Produced commits or PRs
- Authored 3 or more new docs
- Locked a decision or buried one
- Discovered a gotcha worth banking
- Changed direction on a project mid-flight

Do NOT trigger on: short Q&A sessions, exploratory reads, sessions that produced no artifacts.

## Required format

```
## [YYYY-MM-DD HH:MM], <Session Type>, <Project>
- **What:** One-line summary of what was done
- **Files changed:** Key files modified (not exhaustive; the important ones)
- **Decisions:** Architecture / tool / strategy decisions made
- **Lessons:** Anything that broke or surprised you
- **Status:** What's done, what's pending
---
```

Session types: `CLI`, `Code-dispatch`, `Code-standalone`, `Cowork`

## Process

1. At session close, scan the conversation for:
   - Files written or edited (consult tool-call log)
   - Decisions that locked (atomic decision nodes authored, or "let's go with X" outcomes)
   - Lessons learned (gotchas, false starts, unexpected behavior)
   - Status of pending work (what's done, what's banked, what's blocked)
2. Compose the entry in the banked format above.
3. Append (never overwrite) to `~/.claude/relay.md`.
4. Verify: read the last entry back to confirm correct format and complete content.

## Anti-patterns

- Do NOT overwrite existing entries. Always append at the bottom.
- Do NOT skip the entry on a "small" session. If anything compounded, the entry compounds it further.
- Do NOT bundle multiple sessions into one entry. One entry per session, even if multiple sessions happened the same day.
- Do NOT bury decisions in the "What" field. Decisions go in the Decisions field so they're greppable later.
- Do NOT use em-dashes (use commas, colons, semicolons, or parens per banned-character rule).

## Handoff

After the relay entry lands, run /atomize on any decision that locked during the session. The atomic decision node lives forever; the relay entry is the activity feed.

If the session produced more than 3 substantive docs, also run /stress-test before closing per multi-doc session discipline rule.
