---
name: codex-autonomous-delivery
description: "Use when working in Codex and you want a mostly autonomous end-to-end delivery loop: shape the task, write durable execution docs, implement in the same Codex run, verify with real checks, and optionally route skeptical review or cheap bounded helper work through AGX without making AGX the main control plane."
---

# Codex Autonomous Delivery

Use this when the user wants to stay inside Codex for the whole build lane: requirements shaping, durable plan, implementation, verification, and final handoff.

The default behavior is to keep moving autonomously after assumptions are explicit. Do not stop for routine approvals between planning, coding, and verification.

## Use When

- the user wants "just do it" autonomy inside Codex
- the work is substantial enough to benefit from durable docs before or during implementation
- the user wants Codex to remain the main planner, implementer, verifier, and finisher
- optional outside review or cheap helper lanes may help, but should not become the primary workflow

## Do Not Use When

- the task is a trivial one-shot edit
- the work is still pure ideation with no delivery surface yet
- destructive operations, production migrations, or legal/security ambiguity require explicit human confirmation first

## Default Lane

### 1. Normalize the outcome

- Turn the ask into a one-line task statement, scope, assumptions, risks, and stop conditions.
- Ask at most one narrow blocking question. Otherwise make assumptions explicit and proceed.

### 2. Treat the PRD as product truth, not execution input

- If a PRD exists, treat it as a living product document.
- Update it when scope, constraints, UX, contracts, or rollout truth changes.
- Do not treat a raw PRD as an implementation packet for the coding lane.

### 3. Write durable execution docs before meaningful code changes

- For non-trivial work, use `issue-control-loop sequence` first when the next question is still "what should happen after what."
- If GitHub is available, prefer `issue-control-loop sequence --github-mode available --emit artifacts --write-artifacts`.
- Treat the GitHub issue as the primary execution surface to look at.
- Treat plan, status, and test-plan files as durable companion artifacts under version control.

### 4. Convert PRD or spec into an execution surface

- Before meaningful coding, turn the PRD, raw spec, or rough request into an implementation plan.
- When GitHub is in the loop, create or update the relevant issue or issue breakdown first and execute from the current issue.
- If GitHub is unavailable, use the checked-in plan/status files as the temporary execution lane.

### 5. Research only as much as needed

- Use repo truth first.
- Use web or external docs only when recency or source accuracy materially matters.
- Record links or file references in the durable docs when they affect implementation decisions.

### 6. Implement autonomously in small reversible slices

- Start with the smallest slice that can produce evidence.
- Run validation after each meaningful slice.
- Keep moving until the work is actually done or a real blocker appears.

### 7. Delegate only when delegation pays for itself

- Prefer terminal Codex CLI workers for bounded side work, fresh-context read-only passes, or first-pass review.
- Keep the main Codex thread as orchestrator and final judge.
- Do not offload the whole task just because subagents exist.

### 8. Use AGX only as an optional lane

- If AGX is available locally and the diff is meaningful, use it for skeptical review, contradiction checks, or cheap bounded helper work.
- AGX is not the main planner.
- If AGX is unavailable, do an in-session skeptical review instead.

### 9. Verify before claiming success

- Run the commands or UI checks that prove the result.
- For browser or chat-app flows, use the real tool surface when practical.
- No completion claims without fresh verification evidence.

### 10. Close with a reusable handoff

- Update status docs with what changed, what was verified, and what remains.
- Point to exact files and commands.

## Autonomy Rules

- Default to continuing from plan -> implementation -> verification without waiting for extra approval.
- Stop only for irreversible risk, conflicting repo changes, missing secrets or access, or unresolved product ambiguity that would materially change the build.
- Prefer explicit assumptions over conversational drift.
- Prefer real artifacts under git over "here is what I would do".
- If a task touches UI or user-visible behavior, include a smoke path, not just unit-level edits.

## Delegation and Review

For ready-made packet shapes, read `references/review-lanes.md`.

- CLI worker default:
  - use a terminal Codex worker for bounded read-only analysis, first-pass review, or isolated implementation packets
  - prefer cheaper models first for these packets
- AGX default:
  - use AGX for skeptical review, contradiction checks, or cheap bounded helper work
  - if AGX review disagrees with the main lane, treat that as a signal to verify deeper, not as an automatic override

## Output Requirements

- current GitHub issue or issue-ready execution surface for substantial work
- plan, status, and test-plan file paths for non-trivial work
- concise note about the current phase and the next step
- exact validation commands or UI checks used
- honest blockers if autonomy had to stop
