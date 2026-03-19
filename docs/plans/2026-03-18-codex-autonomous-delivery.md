# Codex Autonomous Delivery Implementation Plan

## Source
- Task: Add a Codex-first autonomous delivery lane to Superpowers, with AGX used only as an optional review lane.
- Canonical input: Nick's request on 2026-03-18 to make the workflow "pure Codex", more autonomous, and less dependent on manual subagent choreography.
- Repo context: `superpowers` Codex docs and skills.
- Last updated: 2026-03-18

## Assumptions
- The new lane should live alongside existing Claude-oriented skills, not replace them.
- The first slice is docs + skill instructions, not a full executable orchestration framework.
- Codex should remain the primary planner, implementer, verifier, and final judge.
- AGX is optional and should only be framed as a bounded review or cheap side-lane, never as the main control plane.

## Validation Assumptions
- This slice is documentation and skill-surface work, so validation is structural: file presence, cross-file references, and trigger text consistency.

## Milestone Order
| ID | Title | Depends on | Status |
| --- | --- | --- | --- |
| M1 | Define the Codex autonomous lane and durable execution docs | - | [x] |
| M2 | Add a new Codex-first skill with autonomous execution rules | M1 | [x] |
| M3 | Wire the new lane into Codex-facing documentation and AGX review guidance | M2 | [x] |
| M4 | Run structural verification and record handoff state | M3 | [x] |

## M1. Define the Codex autonomous lane and durable execution docs `[x]`
### Goal
- The repo has a written source of truth for the new Codex-first lane before changing skill surfaces.

### Tasks
- [ ] Create a plan file for the Codex autonomous delivery lane.
- [ ] Create or update a live status file with current phase, decisions, and next steps.
- [ ] Create or update a test plan that matches the new docs/skill scope.

### Definition of Done
- The plan, status, and test plan exist and agree on scope.
- The docs clearly frame Codex as the main control plane and AGX as optional review.

### Validation
```sh
test -f docs/plans/2026-03-18-codex-autonomous-delivery.md
test -f docs/status.md
test -f docs/test-plan.md
```

### Known Risks
- The plan could drift into abstract workflow philosophy instead of repo changes.

### Stop-and-Fix Rule
- If the execution docs disagree on scope or lane ownership, fix them before editing skills.

## M2. Add a new Codex-first skill with autonomous execution rules `[x]`
### Goal
- Superpowers exposes a single Codex-first skill that can drive planning, execution, verification, and optional AGX review with minimal user babysitting.

### Tasks
- [ ] Add `skills/codex-autonomous-delivery/SKILL.md`.
- [ ] Add one or more reference files only if they materially reduce ambiguity.
- [ ] Add `agents/openai.yaml` for the new skill if the repo uses agent metadata for skill surfacing.
- [ ] Encode the intended behavior: durable artifacts first, autonomous execution by default, terminal Codex worker preference when delegation helps, AGX only for review or bounded cheap lanes, and explicit stop conditions for irreversible risk.

### Definition of Done
- The skill is concise, Codex-specific, and more autonomous than the default Superpowers flow.
- The skill does not depend on Claude-specific tools or marketplace behavior.

### Validation
```sh
test -f skills/codex-autonomous-delivery/SKILL.md
rg -n "Codex|AGX|autonomous|verification" skills/codex-autonomous-delivery
```

### Known Risks
- Overfitting to Nick's local environment could make the skill brittle or too private.

### Stop-and-Fix Rule
- If the skill reads like a Nick-only ops note instead of a reusable Codex workflow, rewrite before wiring docs.

## M3. Wire the new lane into Codex-facing documentation and AGX review guidance `[x]`
### Goal
- Codex users can discover the new lane from the existing install and usage docs without reading the whole repo.

### Tasks
- [ ] Update `docs/README.codex.md` to explain the Codex autonomous delivery lane.
- [ ] Update the root `README.md` where the Codex platform path and workflow overview are described.
- [ ] Add a concrete example prompt that triggers the new skill.
- [ ] Explain that AGX is optional and primarily useful as a skeptical review lane or cheap bounded helper.

### Definition of Done
- A Codex user can install Superpowers and understand how to invoke the new lane.
- The docs do not imply that AGX is required.

### Validation
```sh
rg -n "codex-autonomous-delivery|AGX|autonomous" README.md docs/README.codex.md
```

### Known Risks
- The docs could promise automation that the repo does not yet actually encode.

### Stop-and-Fix Rule
- If documentation claims executable automation that only exists as an idea, narrow the wording.

## M4. Run structural verification and record handoff state `[x]`
### Goal
- The new lane is discoverable, internally consistent, and resumable by a future Codex run.

### Tasks
- [ ] Re-read the new skill and docs for conflicting claims.
- [ ] Run the structural command matrix from the test plan.
- [ ] Update `docs/status.md` with decisions, current state, and next work.

### Definition of Done
- Structural checks pass.
- `docs/status.md` and `docs/test-plan.md` reflect the final slice honestly.

### Validation
```sh
rg -n "codex-autonomous-delivery" README.md docs/README.codex.md skills/codex-autonomous-delivery
git diff --stat
```

### Known Risks
- Trigger phrasing may still need live validation in a real Codex session later.

### Stop-and-Fix Rule
- If structural checks fail or docs disagree, fix before closing the slice.
