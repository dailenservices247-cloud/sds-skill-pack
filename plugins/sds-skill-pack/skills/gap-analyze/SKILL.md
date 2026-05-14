---
name: gap-analyze
description: Run the Safety Net Workflow 12-dimension gap analysis on any feature request before building. Catches what the user didn't think to ask. Use before writing any code or shipping any deliverable.
---

# Gap Analyze

A pre-build audit that surfaces dimensions the user didn't name. Built for builders who aren't sure what to scope, who need a safety net before the first commit lands.

## When to use

Trigger on any of:
- New feature request
- New product / service / offering proposal
- New automation / integration / pipeline build
- New script or tool authoring
- Any deliverable above a 30-minute scope

Do NOT trigger on: bug fixes, trivial edits, exploratory questions.

## The 12 dimensions

For each dimension, ask: "Did the user name this? If not, surface it as a gap."

1. **Target users / personas**, who actually uses this?
2. **Platforms**, web, mobile, PWA, desktop, CLI?
3. **Auth / permissions**, who can access it? guest vs authed vs role-gated?
4. **Data model / storage**, what data, where stored, schema?
5. **Error handling / edge cases**, what can go wrong, how does it fail?
6. **Accessibility / i18n**, keyboard nav, screen readers, RTL, locale strings?
7. **Performance / scale**, expected load, latency budget, cost ceiling?
8. **Monetization / billing**, does Stripe touch this? subscription? one-time?
9. **Analytics / tracking**, what to measure, what events fire?
10. **Security / privacy**, input validation, OWASP, PII handling?
11. **Dependencies / integrations**, what external services, what APIs?
12. **Migration / rollback**, how to deploy safely, how to undo?

## Process

1. Parse the user's request into the named scope.
2. Run each of the 12 dimensions against the scope. For each, output one of:
   - **Named**, user covered this
   - **Implicit**, user didn't say but a reasonable default exists
   - **Gap**, user didn't name AND no reasonable default exists; needs an answer
3. For each Gap, present 3 options:
   - **Option A**, full implementation
   - **Option B**, simpler / partial implementation
   - **Option C**, skip for now (MVP)
4. Wait for the user to pick per dimension before generating a PRD or starting work.

## Output format

```
# Gap Analysis: <feature name>

## Named scope
<user's stated request>

## Dimensions
1. Target users, [Named | Implicit: <default> | Gap: <options A/B/C>]
2. Platforms, [...]
...

## Gaps requiring user input
<list of dimensions still in Gap state with their A/B/C options>
```

## Anti-patterns

- Do NOT skip dimensions because they "obviously don't apply." Note as Implicit with the assumed default so the user can override.
- Do NOT proceed to build while any Gap is unresolved. Bank the build until gaps close.
- Do NOT bundle multiple gaps into one decision. Each dimension gets its own pick.

## Handoff

Once gaps close, run /scope-defend on the resolved scope to strip premature features. Then generate a PRD using the resolved scope as the spine.
