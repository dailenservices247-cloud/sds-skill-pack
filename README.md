# SDS Skill Pack v0

Twelve productized Claude Code skills authored by [Synapse Dynamics Segmented](https://github.com/dailenservices247-cloud). Free.

Built for the **Peer Operator** audience: solo founders, multi-product builders, and operators-who-build. Not AI bros. Not futurist hype.

Each skill is one tight pattern, derived from real production discipline. Auto-triggers when Claude sees a matching context (no slash command memorization required).

## Install

### From this marketplace

```
/plugin marketplace add dailenservices247-cloud/sds-skill-pack
/plugin install sds-skill-pack@sds-skill-pack
/reload-plugins
```

Pick **"Install for you (user scope)"** when prompted.

### Requires

Claude Code v2.1+ with the plugin system enabled.

## The 12 skills

### Session start

| Skill | What it does |
|---|---|
| `/gap-analyze` | Runs the Safety Net Workflow 12-dimension gap analysis on any feature request. Catches what the user didn't think to ask. |
| `/plan-mode-prep` | Generates a structured Plan Mode brief before any code mutation. Read-only plan the user approves before mutations land. |
| `/scope-defend` | Applies Default-to-Minimum-Viable + Surgical-Edits rules to incoming requests. Names the smallest thing that delivers the outcome and banks the rest. |

### Craft

| Skill | What it does |
|---|---|
| `/doctrine-inject` | Runs the 2-prompt canon-dissect-then-generate sequence on craft tasks (copy, ads, headlines, proposals, contract review). |
| `/brand-voice-check` | Lints any markdown copy against a brand voice register before publish. Banned phrases, drop-overclaim, em-dashes, filler. |
| `/atomize` | Converts long-form notes into atomic-node frontmatter-typed .md files for graph-native memory. |
| `/retrieval-contract` | Authors a RETRIEVAL.md for a project before any vector DB or memory infrastructure pick. Layer 0 of the agent stack. |

### Ingest

| Skill | What it does |
|---|---|
| `/icac` | Tier-gates any external source ingest (Full / Light / No). Quarterly cull on Light tier. |

### Milestone

| Skill | What it does |
|---|---|
| `/stress-test` | Runs the multi-doc session discipline check before closing any session that produced 3+ substantive docs. Cross-refs, terminology drift, secret leaks, file-existence breaks, numeric inconsistency. |
| `/self-verify` | Runs Chrome DevTools MCP or iOS Simulator MCP e2e verification on UI-touching changes before marking complete. |

### Session end

| Skill | What it does |
|---|---|
| `/session-relay` | Produces a relay.md entry in the banked format at end of any significant session. |
| `/dream` | Runs the Dream Protocol memory consolidation cycle. Reviews recent sessions, updates stale memories, extracts patterns. |

## Why these twelve

Every skill captures one rule that, applied consistently, removes a known failure mode:

- `/gap-analyze` catches "the user didn't think to specify X"
- `/plan-mode-prep` catches "we mutated before thinking"
- `/scope-defend` catches "while we're here, let's also..."
- `/doctrine-inject` catches "base model wrote generic output"
- `/brand-voice-check` catches "we shipped overclaim copy"
- `/atomize` catches "the insight stayed in chat and disappeared"
- `/retrieval-contract` catches "we picked infrastructure before knowing what we need"
- `/icac` catches "the source expired and we lost the substance"
- `/stress-test` catches "the locked spec set had 21 silent failures"
- `/self-verify` catches "the agent reported done while the UI was broken"
- `/session-relay` catches "the next session has no idea what happened yesterday"
- `/dream` catches "memories went stale without anyone noticing"

## How they chain

Most skills hand off to other skills in this pack. A typical session looks like:

```
new feature request
  /gap-analyze
    user resolves gaps
  /scope-defend
    in-scope items only
  /plan-mode-prep
    user approves brief
  build
  /self-verify
    e2e clean
  /session-relay
    relay entry written
```

For craft work:

```
copy generation request
  /doctrine-inject
    dissection complete
    generation complete
  /brand-voice-check
    register clean
  ship
```

For knowledge work:

```
external source to ingest
  /icac
    tier set (Full / Light / No)
  /atomize
    atomic node authored
```

## License

MIT. Use freely. Attribution appreciated.

## Author

Built by Synapse Dynamics Segmented. Contact: [SDS website](https://synapsedynamicssegmented.com) (under construction), GitHub: [@dailenservices247-cloud](https://github.com/dailenservices247-cloud).

## Status

v0, first public release. Twelve skills. Production-tested in SDS internal work. Issues + improvements welcome via the [issues page](https://github.com/dailenservices247-cloud/sds-skill-pack/issues).

Banked for v0.2:
- `/before-after` (demonstrate via 2 outputs, not argument)
- `/agent-deploy-5cmd` (5 Commandments check before client agent deploy)
- `/three-questions` (persistent memory / editable artifacts / context compounds)
- `/tier-frame` (apply "unlimited everything = upper tier only" rule)
- `/promote-brief-to-prd` (brief to PRD without rewrite)
- `/eval-write` (eval doc generator with severity ratings)
