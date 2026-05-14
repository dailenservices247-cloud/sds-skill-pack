---
name: stress-test
description: Run multi-doc session discipline check before closing any session that produced 3 or more substantive documents. Catches cross-reference rot, terminology drift, secret leaks, file-existence breaks, and numeric inconsistency. Deferring this catch class = catching never.
---

# Stress Test

10-minute audit pass at end of session. Specifically built to catch the bug class that produced 21 silent failures in a v9.3 spec set on 2026-04-28. Banked as required for any session producing more than 3 substantive docs.

## When to use

Trigger on any of:
- Session produced 3 or more new substantive docs
- Spec / PRD / eval set about to be declared "locked"
- Any v-lock milestone on a doc set
- Before any commit that touches more than 5 markdown files

Do NOT trigger on: code-only sessions (use language linters and tests), trivial doc edits, sessions that produced fewer than 3 docs.

## Five checks

### 1. Cross-reference integrity
Find every node ID or doc path referenced in frontmatter or body. Verify each one resolves to a real file on disk. Flag broken references.

### 2. Terminology drift
Find every term the session named or renamed. Grep across all session-produced docs to verify usage is consistent. Catches "Super App" vs "Super App Platform" drift class.

### 3. Secret leak detection
Scan for API key patterns, OAuth tokens, password fields, EIN / SSN / TIN, .env file contents, private repo paths. Flag anything that should not ship in a markdown file.

### 4. File-existence breaks
Find every absolute or relative file path mentioned. Verify each exists. Catches paths referenced from CLAUDE.md or atomic nodes that were never created.

### 5. Numeric inconsistency
Find every named number (revenue target, price, percentage, count). Cross-check across all session docs. Catches "$5K/mo" in one doc and "$3K/mo" in another for the same offering.

## Process

1. Glob all session-produced or modified `.md` files.
2. Run each of the 5 checks in sequence.
3. For each finding, output: file, line, problem, suggested fix.
4. Group findings by severity:
   - **Blocker**, broken cross-ref to atomic node, secret leak, file-doesn't-exist
   - **Drift**, terminology inconsistency, numeric mismatch
   - **Polish**, minor formatting, suspected typo
5. Block session close on any Blocker. Drift gets resolved if possible, banked if not. Polish ignored unless under 10 minutes total.

## Companion command

If `~/.local/bin/check-spec-refs` exists, run it on the session doc set as an additional cross-ref pass:

```bash
~/.local/bin/check-spec-refs <doc-or-dir>
```

Exit 0 = clean. Exit 1 = fix and re-run.

## Anti-patterns

- Do NOT defer the stress test "until I have more time." Defer equals catch never.
- Do NOT mark Blockers as Polish to close the session faster.
- Do NOT skip terminology drift checks because "I'll fix it later." Already-written sections silently retain old terms.
- Do NOT scan only the most recently modified file. Multi-doc drift requires multi-doc scan.

## Handoff

After stress test passes, run /session-relay to write the closing entry. After relay, if any decisions locked this session, run /atomize on each so they enter the memory graph as atomic nodes.
