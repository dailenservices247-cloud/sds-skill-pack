---
name: atomize
description: Convert long-form notes, transcripts, or rolling docs into atomic-node frontmatter-typed .md files. Use when the user has captured something in prose form that needs to enter the memory graph as a discrete decision, inference, context, or test.
---

# Atomize

Takes prose input and produces a properly typed atomic node ready to land in `AI Hub/Decisions/`, `AI Hub/Inferences/`, `AI Hub/Contexts/`, or `AI Hub/Tests/`. Required for the atomic-node memory format banked in CLAUDE.md.

## When to use

Trigger on any of:
- Long-form decision write-up that needs to enter the Decisions Log
- Session insight that should compound across future work
- Captured context from a meeting, video, or scrape
- Test result or eval outcome that informs future builds
- Any prose document longer than 200 words that contains a single discrete claim

Do NOT trigger on: code, rolling logs, indexes, raw transcripts before compression.

## Atom types

| Type | Use for | Body header |
|---|---|---|
| `decision` | A locked choice, alternatives considered, status | "Decision" |
| `inference` | A claim or pattern derived from sources | "Inference" |
| `context` | A snapshot of state at a moment in time | "Snapshot" |
| `test` | A validation, eval, or experiment with results | "Test" |

## Required frontmatter

```yaml
---
id: <type>-YYYY-MM-DD-<slug>
type: decision | inference | context | test
date: YYYY-MM-DD
summary: <one-sentence, max 140 chars>
authored_by: <name>
supports: []          # node IDs this strengthens
derived_from: []      # node IDs or sources this came from
contradicts: []       # node IDs this disagrees with
related_to: []        # node IDs at the same depth
---
```

Edge types are deliberately narrow (4 lifted from AI Impact Infinite Brain pattern). Resist adding new edge types unless a pattern repeats 3+ times.

## Required body sections

1. **Context**, what was true / what was happening before this node
2. **Decision / Inference / Snapshot / Test**, the substance (atomic, one claim per node)
3. **Why this over alternatives**, paths considered and rejected
4. **Status**, locked / pilot / draft / superseded (link replacement if superseded)

## Process

1. Read the input prose.
2. Identify the single discrete claim. If multiple claims, ask user whether to split into multiple atomic nodes.
3. Pick the atom type from the 4 options.
4. Generate frontmatter:
   - `id` from type + date + 3-5 word slug
   - `summary` is the one-sentence thesis (must be at most 140 chars)
   - Edges left empty by default; user fills in known relations
5. Write the 4 body sections from the prose, kept tight.
6. Save to `AI Hub/<Type-plural>/<id>.md` (e.g. `AI Hub/Decisions/decision-2026-05-14-atomic-node-going-forward.md`).
7. Append a one-line entry to `AI Hub/Decisions Log.md` (or the appropriate rolling index) linking to the new node.

## Anti-patterns

- Do NOT bundle multiple claims in one node. Atomic = one claim per file.
- Do NOT use prose summary fields longer than 140 chars. The summary is the AI's cheap scan layer; bloat breaks the retrieval contract.
- Do NOT invent new edge types. Stick to supports / derived_from / contradicts / related_to until a repeating pattern justifies expansion.
- Do NOT mass-migrate legacy long-form docs. Rule is atomic-going-forward, not retroactive rewrite.

## Handoff

If the new node represents a high-value pattern, run /icac to assign it a tier. If it locks a decision, append entry to Decisions Log.md rolling index.
