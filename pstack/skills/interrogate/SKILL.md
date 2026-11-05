---
name: interrogate
description: "Use for \"interrogate\", \"adversarial review\", \"challenge this\", \"stress test this code\", \"find blind spots\", or \"tear this apart\". Perform a rigorous adversarial review yourself."
disable-model-invocation: true
---

# Interrogate

Perform an adversarial code review yourself. Do **not** delegate, spawn subagents, run background reviewer processes, or use multiple models. The deliverable is your own pragmatic verdict. Do **not** auto-apply changes.

The scope is what the user names. Otherwise it is the branch diff from its merge base with the base branch, through the working tree. State the intent with the **in-your-own-words** skill, from the user's message, the commits, and the PR description. Ask the user when the intent is unclear.

## Fan out

- If the user points at specific files or a diff, use that.
- If on a feature branch, run `git diff main...HEAD` (or the appropriate base branch) for the full changeset.
- If the user's message references recent work, gather the relevant files.

Read the diff or file contents and the surrounding callers, callees, types, tests, configuration, and deployment context necessary to understand the change.

## Verify

State the code's intent explicitly. Derive it from:

- The user's message
- Commit messages
- PR description, if one exists
- The code itself

Write one clear paragraph. Challenge whether the implementation achieves the intent well, not whether the intent itself is correct. If the intent is genuinely ambiguous, ask the user before proceeding.

## Step 3, Review

Review the change through the relevant lenses below. Do not force irrelevant lenses or pad the review with nits.

- **Correctness:** trace happy and failure paths, edge cases, idempotency, error handling, and state transitions.
- **Concurrency:** for shared mutable state, verify structural serialization, ownership, lease expiry, retry behavior, and recovery from partial failure.
- **Root cause and structure:** identify symptom-level workarounds, duplicated paths, misplaced logic, and avoidable coupling.
- **Verification:** confirm behavior is tested at the appropriate unit, integration, and boundary layers. A bug fix needs a regression test.
- **Complexity:** prefer direct, maintainable code and justified abstractions. Flag complexity only when it creates a concrete cost or risk.
- **Security:** flag only vulnerabilities with a traceable untrusted-input-to-dangerous-sink path.

For every potential issue, trace a reachable execution path and distinguish a real defect from a preference or hypothetical concern.

## Step 4, Lead Judgment

Act as a pragmatic senior engineer, not a neutral aggregator. Categorize each finding:

- **Act on:** a real correctness, security, or maintainability issue that should block the PR.
- **Consider:** a legitimate concern whose value may not outweigh the cost of fixing it now.
- **Noted:** technically valid but low-impact or not presently actionable.
- **Dismissed:** incorrect, speculative, stylistic-only, or contradicted by repository context.

Keep the Act On list focused. Explain the concrete execution path and why the priority is appropriate.

## Output Format

Present the verdict in this structure:

### Intent
> [One-paragraph stated intent]

### Act On
[Each finding: severity, file and line/function, concrete failure path, and practical fix.]

### Consider
[Legitimate but non-blocking concerns and their tradeoffs.]

### Noted
[Valid but low-priority observations.]

### Dismissed
[Rejected concerns and a concise reason.]

### Verification
[Commands/checks reviewed or run, their results, and meaningful coverage gaps.]
