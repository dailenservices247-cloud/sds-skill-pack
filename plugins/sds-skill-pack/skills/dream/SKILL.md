---
name: dream
description: Run the Dream Protocol memory consolidation cycle on a vault or memory directory. Use when the user types "dream" or when periodic consolidation is due. Reviews recent sessions, updates stale memories, extracts patterns, refreshes indexes. Like sleep for the brain.
---

# Dream

Memory consolidation cycle. Modeled on how the brain consolidates during sleep. Triggered on demand (user types "dream") or on a cadence (typically weekly).

## When to use

Trigger on any of:
- User explicitly types "dream"
- Weekly cadence on an active memory vault
- After 5+ significant sessions without a consolidation pass
- Before any major project transition or milestone
- When recent decisions feel scattered and need synthesis

Do NOT trigger on: every session close (too aggressive), inactive vaults (nothing to consolidate).

> **AUTHORITY NOTE (added 2026-07-05):** when Dailen says "dream", the authoritative spec is the **10-step Dream Protocol in `~/.claude/CLAUDE.md`** — this skill is the working checklist for it, not a substitute. Two steps below (9, 10) were previously missing here; running an 8-step subset was the exact partial-consolidation failure banked in Lessons Learned 2026-05-22.

## The ten-step cycle

### 1. Review recent activity
Read recent handoff files, last 3-5 session logs, and the last week of relay entries. Build a mental model of what happened.

### 2. Update stale memories
Walk every file in the memory directory. For each, ask: is this still accurate? If something contradicts a later decision, mark the older entry as superseded and link the replacement.

### 3. Add new memories
Extract significant learnings from the recent activity that aren't yet captured. New gotchas, new patterns, new decisions. Author as atomic nodes via /atomize.

### 4. Update Lessons Learned
New gotchas, failed approaches, workarounds. One line each in `AI Hub/Lessons Learned.md`.

### 5. Update Decisions Log
New architecture decisions, tool choices, strategic pivots. One line each in `AI Hub/Decisions Log.md` with link to the atomic decision node.

### 6. Extract patterns
What workflows work? What keeps failing? What does the user keep asking for? Patterns that repeat 3+ times get promoted to atomic-inference nodes.

### 7. Update indexes
Refresh `Research Log.md`, `Decisions Log.md`, and any other rolling indexes so they accurately list current atomic nodes.

### 8. Update global operating docs if needed
CLAUDE.md, `~/.claude/current-focus.md`, roadmap, and per-project handoffs (`~/.claude/handoffs/*.md`) — anything the consolidation revealed as stale. Also refresh the memory-directory `MEMORY.md`, `mempalace.yaml`, and `entities.json` indexes per the CLAUDE.md protocol.

### 9. Write the dream-cycle session log
Log the dream cycle itself as a session entry. Records what was consolidated, what was promoted, what was archived, and what was left open.

### 10. Archive and reset the relay
Copy the processed `~/.claude/relay.md` to a dated archive, then reset `relay.md` to its clean template. A relay that never resets stops being an activity feed and becomes a second vault.

## Goal

Make the next session smarter than this one. The user should be able to start tomorrow's work without re-explaining yesterday's context.

## Process notes

- This is a heavy operation. Expect 20-45 minutes of focused work depending on vault size.
- Run it when the vault is at rest (not mid-build). Otherwise consolidation conflicts with active edits.
- If the dream cycle surfaces a pattern that contradicts a banked rule, name the conflict explicitly. Don't silently override.

## Anti-patterns

- Do NOT run dream on every session close. Too aggressive; doesn't let patterns surface naturally.
- Do NOT skip step 2 (update stale memories). Stale memories quietly mislead future sessions.
- Do NOT promote a "pattern" after 1-2 occurrences. Patterns need 3+ repetitions to be real.
- Do NOT delete superseded entries. Mark as superseded with a link to the replacement so the history stays traceable.

## Handoff

After dream completes, run /stress-test on the updated indexes to catch any cross-reference rot. Then /session-relay to log the dream cycle itself.
