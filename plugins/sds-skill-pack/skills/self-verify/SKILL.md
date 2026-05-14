---
name: self-verify
description: Run Chrome DevTools MCP or iOS Simulator MCP e2e verification on any UI-touching change before marking it complete. Catches happy-path regressions agents miss when they "report done" without verifying. Closes the agent-write to bug-find loop.
---

# Self Verify

The one-line CLAUDE.md rule that drops debug cycles from 30 to 60 minutes per feature down to under 15. Without this skill, the loop is: agent writes, user tests, finds bug, agent rewrites, repeat. With this skill, the agent clicks through the happy path itself before reporting done.

## When to use

Trigger on any of:
- About to mark a UI-touching feature complete
- About to commit a UI-touching change
- After a refactor that could affect user-facing flows
- After dependency upgrades that touch the rendering pipeline
- Any change that should "still work" after the change

Do NOT trigger on: pure CSS adjustments below the fold, copy edits that don't change behavior, internal docs.

## Two MCPs (install per-repo CLAUDE.md)

### Web (Chrome DevTools MCP)

Used for any web app. Drives a real Chrome instance:
- Navigates to the affected page
- Clicks through the user happy path
- Reads the console for errors
- Reads network logs for failed requests
- Verifies the visible state matches expected

### Mobile (iOS Simulator MCP)

Used for iOS or PWA-on-iOS work. Drives the iOS Simulator:
- Launches the app on the simulator
- Taps through the user happy path
- Reads the device console for crashes or warnings
- Verifies visible state matches expected

## The CLAUDE.md directive

Add this line to the affected repo's `CLAUDE.md`:

```
After any UI-touching change, before reporting the task complete, run /self-verify:
- For web routes: chrome-devtools-mcp navigates to the affected route, clicks the happy path, reads console + network.
- For mobile screens: ios-simulator-mcp launches the app, taps the happy path, reads device console.
- If any error or unexpected state, fix it and re-verify. Do not report done with errors in console.
```

## Process

1. Identify the affected routes / screens from the change set.
2. Determine target MCP (web vs mobile).
3. For each route or screen, run the MCP through the happy path.
4. Read console and network logs.
5. If any error / warning / unexpected state, do NOT report complete. Fix the issue and re-verify.
6. Only after a clean verification, mark the task complete in the session relay.

## Anti-patterns

- Do NOT report a UI change complete without running self-verify, even if "the code looks right."
- Do NOT silence console warnings to make the check pass. Warnings often surface real regressions.
- Do NOT replace self-verify with manual user testing. The whole point is closing the agent loop.
- Do NOT skip mobile self-verify on iOS-targeted features because "web tested fine." Different runtime, different bugs.

## Handoff

After self-verify passes, the change is ready for commit and session-relay entry. If self-verify catches a real bug, /atomize the bug + fix as a `test`-type atomic node so the pattern compounds.
