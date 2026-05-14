---
name: retrieval-contract
description: Author a RETRIEVAL.md for a project before any vector DB, graph store, or MCP infrastructure pick. Use when starting agent work on a new project or when an existing project is wasting tokens on rediscovery. Layer 0 of the agent stack.
---

# Retrieval Contract

The contract every project authors before picking memory infrastructure. Names what the agent needs to know, in what shape, with what refresh cadence, and what rediscovery patterns are banned. Derived from Nate B Jones "memory wars" lift.

## When to use

Trigger on any of:
- Starting agent work on a new project (always required)
- Existing project where session token spend is high and unexplained
- Before installing any vector DB, graph store, RAG system, or memory MCP
- Before adopting an orchestration framework that bundles its own memory (e.g. RuFlo, claude-flow)
- Quarterly review on any active retrieval contract

Do NOT trigger on: throwaway scripts, one-shot agent tasks, exploratory questions.

## Required structure

The contract is a markdown file at `AI Hub/Retrieval Contracts/<project>.md` with this shape:

```yaml
---
project: <project-name>
agent_purpose: <one sentence on what the agent does for this project>
authored_by: <name>
date: YYYY-MM-DD
status: draft | piloted | locked | superseded
---
```

### Section 1, What the agent needs to know

Table with three columns: fact type, shape (where stored, what format), refresh cadence.

| Fact type | Shape | Refresh cadence |
|---|---|---|
| Brand voice register | Markdown file at `~/.claude/canon/voice/` | Weekly |
| Active page state | Component file at `app/.../page.tsx` | Per session |
| ... | ... | ... |

### Section 2, Retrieval triggers

Table with three columns: when (the trigger condition), what gets pulled, how filtered.

| When | What gets pulled | How filtered |
|---|---|---|
| User asks for copy generation | Voice canon + banned-words list | Doctrine Injection 2-prompt sequence |
| ... | ... | ... |

### Section 3, Token budget per turn

- Max context loaded: <e.g. 12000 tokens>
- Max retrieval calls: <e.g. 4 per turn>
- Fallback when over: <e.g. summary-only mode + user re-prompt>

### Section 4, Anti-patterns banned

Explicit list of rediscovery patterns to refuse:
- Re-reading handoff from scratch every turn
- Re-asking questions answered in CLAUDE.md
- Searching for the catalog when it's in this contract
- Loading entire canon directory at once

## Process

1. Interview the user (or read existing project docs) to fill section 1.
2. Identify the top 3 to 5 retrieval triggers and document them in section 2.
3. Set conservative token budgets in section 3 (lower than current actual; tighten over time).
4. List concrete anti-patterns in section 4 based on observed rediscovery waste.
5. Save the contract with `status: draft`.
6. After 5 sessions using the contract, review and promote to `status: piloted`.
7. After 30 days of stable use, promote to `status: locked`.

## Anti-patterns (for authoring the contract itself)

- Do NOT pick infrastructure (Pinecone, Cloudflare Memory, Microsoft Graph) before the contract is authored. The contract dictates the infra fit, not the reverse.
- Do NOT inherit a template contract from another project verbatim. Each project's retrieval shape differs.
- Do NOT set unrealistic token budgets (e.g. "max context 4000"). Set what current sessions actually need, then tighten.
- Do NOT skip section 4. The banned anti-patterns section is the load-bearing part; without it the contract is a wishlist.

## Output handoff

Once the contract is drafted, run a session against it for one week. Track every time the agent violates the banned list. After one week, revise the contract based on observed violations. Then choose infrastructure (or stay with what works).
