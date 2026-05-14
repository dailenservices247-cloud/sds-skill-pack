---
name: icac
description: Tier-gate any external source ingest (Full / Light / No ICAC). Use when ingesting a video, article, podcast, scrape, transcript, or book. Determines compression depth, archive duration, and whether a canon companion gets authored.
---

# ICAC, Ingest, Compress, Archive, Cull

Tier-gated ingest discipline. Every external source classified at ingest time. Quarterly cull on Light tier nodes. Prevents two failure modes: over-formalization (full ICAC on disposable lookups wastes time) and zero-compounding (no ICAC at all means tmp dirs expire and nothing compounds).

## When to use

Trigger on any of:
- Watching a video for substance (not entertainment)
- Reading an article that informs strategy / craft / methodology
- Listening to a podcast for lifts
- Running a channel or person scrape
- Capturing a transcript
- Reading a book and wanting to bank its substance
- Receiving any document the user wants to keep referenceable

Do NOT trigger on: factual lookups answered in one search, entertainment consumption, throwaway research.

## The three tiers

### Full ICAC

**Triggers (any):**
- Source locks a decision (authoring of an atomic `decision` node directly references it)
- Source is or will be cited in `~/.claude/canon/<theme>/` for Doctrine Injection
- Source has been referenced more than once across existing atomic nodes

**Output:**
- 1 atomic-typed node (`context` or `inference`) under `AI Hub/Research/<source-type>/`
- 1 Doctrine Injection canon source under `~/.claude/canon/<theme>/<slug>.md`
- Raw artifact stored at `AI Hub/Research/<source-type>/<slug>/raw/` (kept forever)

### Light ICAC (default for most ingest)

**Triggers:**
- Single video, article, podcast, or scrape that doesn't yet meet Full ICAC criteria

**Output:**
- 1 atomic-inference or context node under `AI Hub/Research/<source-type>/`
- No canon source
- Raw artifact stored at `AI Hub/Research/<source-type>/<slug>/raw/` for 30 days then auto-culled to `AI Hub/Research/_archive/`

### No ICAC

**Triggers:**
- Off-the-cuff fact check, one-time lookup, throwaway query

**Output:**
- Chat summary only
- Tmp dir expires naturally; nothing persists

## Process

1. Identify the source. Get the URL or file path.
2. Classify the tier using the triggers above. If unsure, default to Light.
3. Confirm tier with user if the classification is non-obvious.
4. Run the ingest pipeline (e.g. `/watch` for video, web scrape for article).
5. Generate the atomic node frontmatter with the required `icac_tier` field set to `full`, `light`, or `none`.
6. Append entry to `AI Hub/Research/Research Log.md` rolling index.
7. For Full ICAC, also author the canon companion file.

## Cull rules (quarterly)

Every 90 days, run a cull pass on Light ICAC nodes:
- **Stale** = no `derived_from` link from any node authored in the last 6 months AND thesis didn't enter a locked decision
- Stale nodes move to `AI Hub/Research/_archive/` (cold storage, excluded from default retrieval)
- Stale raw artifacts deleted after archive

What's NEVER stale:
- Locked decisions or inferences (already promoted)
- Canon sources under `~/.claude/canon/`
- Sources cited in any retrieval contract

## Anti-patterns

- Do NOT default to Full ICAC. The compression overhead kills ingest velocity.
- Do NOT default to No ICAC. That's how tmp dirs expire and nothing compounds.
- Do NOT skip the Research Log entry. Without the index, the atomic node is orphaned.
- Do NOT cull aggressively before 90 days. Light tier needs time to either compound or stay stale.

## Handoff

After tier is set, run /atomize on the source to produce the actual atomic node. For Full ICAC also run /doctrine-inject preparation by extracting the source's techniques into the canon companion file.
