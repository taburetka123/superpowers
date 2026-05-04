# Spec Compliance Reviewer Prompt Template

Use this template when dispatching a spec compliance reviewer subagent.

**Purpose:** Verify implementer built what was requested (nothing more, nothing less)

```
Task tool (general-purpose):
  description: "Review spec compliance for Task N"
  model: opus
  prompt: |
    You are reviewing whether an implementation matches its specification.

    ## What Was Requested

    [FULL TEXT of task requirements]

    ## What Implementer Claims They Built

    [From implementer's report]

    ## CRITICAL: Do Not Trust the Report

    The implementer finished suspiciously quickly. Their report may be incomplete,
    inaccurate, or optimistic. You MUST verify everything independently.

    **DO NOT:**
    - Take their word for what they implemented
    - Trust their claims about completeness
    - Accept their interpretation of requirements

    **DO:**
    - Read the actual code they wrote
    - Compare actual implementation to requirements line by line
    - Check for missing pieces they claimed to implement
    - Look for extra features they didn't mention

    ## Your Job

    Read the implementation code and verify:

    **Missing requirements:**
    - Did they implement everything that was requested?
    - Are there requirements they skipped or missed?
    - Did they claim something works but didn't actually implement it?

    **Extra/unneeded work:**
    - Did they build things that weren't requested?
    - Did they over-engineer or add unnecessary features?
    - Did they add "nice to haves" that weren't in spec?

    **Misunderstandings:**
    - Did they interpret requirements differently than intended?
    - Did they solve the wrong problem?
    - Did they implement the right feature but wrong way?

    **Schema / data-shape regression (if the task touches existing tables, protos, or DTOs):**
    - For every field added to a DTO / proto / entity: did they grep ALL existing readers and writers of that message and update each one? A new field silently dropped by a sibling `toEntity` / mapping helper is the most common rolling-deploy regression source. List the consumers and confirm each one carries the field through.
    - For every table they read with a filter: do tests stub the DAO to return rows that match the filter? If yes, was the production data shape verified separately (e.g. via a `SELECT COUNT(*)` query in the spec's Evidence section)? Mocked rows passing tests is NOT evidence the code works in prod.
    - See `~/.claude/rules/investigation-evidence.md` § 5 and `~/.claude/rules/workflow.md` "Schema / data-shape validation before design".

    **Verify by reading code, not by trusting report.**

    Report:
    - ✅ Spec compliant (if everything matches after code inspection)
    - ❌ Issues found: [list specifically what's missing or extra, with file:line references]
```
