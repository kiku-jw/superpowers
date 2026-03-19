# Review and Delegation Lanes

Use this file only when the main skill decides a side lane is worth the extra complexity.

## 1. Terminal Codex Worker

Use a terminal Codex worker when the task is bounded and one of these is true:

- a fresh-context read-only pass would be useful
- a cheap first-pass review is enough before the main lane verifies locally
- a narrow implementation packet can be isolated cleanly

### Packet Shape

Include:

- goal
- scope boundaries
- exact files or directories in scope
- expected output format
- forbidden actions

### Example Packet

```text
Review only these files for regression risk and missing tests:
- path/to/file-a.ts
- path/to/file-b.test.ts

Do not edit files.
Return:
1. findings ordered by severity
2. missing tests
3. open questions
```

## 2. AGX Review Lane

Use AGX only when:

- the diff is meaningful enough to benefit from an outside skeptical pass
- a contradiction check or cheap bounded helper lane is worth the setup cost
- the main Codex lane is still doing final judgment and local verification

### Packet Shape

Include:

- task summary
- changed files or diff summary
- review goal
- severity expectations
- explicit instruction to prefer findings over praise

### Example Packet

```text
Review this change skeptically.
Focus on:
- behavioral regressions
- missing verification
- risky assumptions
- tests that should exist but do not

Return findings first, ordered by severity.
```

## 3. Conflict Handling

If a side lane disagrees with the main lane:

1. do not accept or reject it blindly
2. verify the claim with repo evidence or commands
3. update the plan or status if the disagreement changes scope

## 4. Anti-Patterns

- Do not send the entire task away just because a helper lane exists.
- Do not treat AGX as the default planner.
- Do not skip local verification because a reviewer sounded confident.
