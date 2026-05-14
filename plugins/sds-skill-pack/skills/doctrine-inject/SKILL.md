---
name: doctrine-inject
description: Run a 2-prompt canon-dissect-then-generate sequence on any craft task (copy, ads, headlines, microcopy, voice, proposals, contract review). Use when the user wants polished output that matches a specific source register. Banks the dissection step that base models skip.
---

# Doctrine Inject

The 2-prompt sequence that turns generic Claude output into source-register-matched craft. Without the dissect step, the model misses the right passages even when canon is in context.

## When to use

Trigger this skill on any craft task:
- Sales / marketing copy generation
- Ad headlines, hooks, microcopy
- Brand voice work, register matching
- Proposal writing, sales scripts
- Contract review / negotiation prep
- Any output that needs to sound like a specific author / company / register

Do NOT trigger on: code, generic explanation, factual lookups, summarization.

## The 2-prompt pattern

### Prompt 1, Dissection

Load 2 to 4 canonical sources from `~/.claude/canon/<craft>/` into context (or have the user point to specific source files / URLs).

Ask the model to extract:
- The 5 most-used rhetorical techniques in the source
- The 5 most-used structural patterns (opening hooks, transitions, closes)
- The vocabulary register (formal / colloquial / technical / aspirational)
- The cadence (sentence length distribution, paragraph density)
- The implicit values / assumptions the source treats as obvious

Output: a structured dissection document the user can review before generation begins.

### Prompt 2, Generation

Using the dissection as explicit instructions, generate the requested output. The model now has technique + structure + register + cadence + values as explicit constraints, not implicit pattern matching.

## Canon library structure

Canon sources live under `~/.claude/canon/<craft-area>/`:
- `~/.claude/canon/copywriting/` for sales copy, ad creative, headlines
- `~/.claude/canon/voice/` for brand voice anchors, taste profile
- `~/.claude/canon/proposals/` for proposal templates and exemplars
- `~/.claude/canon/contracts/` for contract review canon

Each canon source is one `.md` file with the actual source text plus frontmatter (`author`, `year`, `register`, `notes`).

## Anti-patterns

- Do NOT skip the dissection prompt. The base model "knows" the source vaguely without dissection but misses the actual techniques.
- Do NOT load more than 4 canon sources at once. Token cost grows quadratically with cross-reference; dissection quality drops.
- Do NOT use this skill for output that needs to be original. Doctrine Injection produces register-matched output, not novel-voice output.

## Output handoff

After generation, run /brand-voice-check on the output to gate against banned-words and drop-overclaim before delivery.
