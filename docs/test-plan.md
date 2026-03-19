# Test Plan

## Source
- Task: Add a Codex-first autonomous delivery lane to Superpowers, with AGX as optional review.
- Plan file: `docs/plans/2026-03-18-codex-autonomous-delivery.md`
- Status file: `docs/status.md`
- Repo context: `superpowers`
- Last updated: 2026-03-18

## Validation Scope
- In scope: new Codex-first skill surface, Codex-facing documentation, AGX review positioning, trigger phrasing consistency.
- Out of scope: full live end-to-end orchestration runtime, Claude integration behavior, marketplace packaging changes.

## Environment / Fixtures
- Data fixtures: none
- External dependencies: none required for the structural slice
- Setup assumptions: repo is locally editable; validation can use shell tools such as `test`, `rg`, and `git`

## Test Levels

### Unit
- Not applicable for this slice; no new executable library code is planned.

### Integration
- Cross-file consistency between the new skill, `README.md`, and `docs/README.codex.md`.
- Consistent framing of AGX as optional review rather than mandatory planner.

### End-to-End / Smoke
- A Codex user can find the new lane from the Codex docs and a prompt example.
- The new skill folder is present and contains the required files.

## Negative / Edge Cases
- Docs overclaim autonomy beyond what the skill actually instructs.
- The new skill silently reintroduces Claude-specific assumptions.
- AGX wording accidentally makes it sound required.

## Acceptance Gates
- [x] `test -f docs/plans/2026-03-18-codex-autonomous-delivery.md`
- [x] `test -f docs/status.md`
- [x] `test -f docs/test-plan.md`
- [x] `test -f skills/codex-autonomous-delivery/SKILL.md`
- [x] `rg -n "codex-autonomous-delivery|AGX|autonomous" README.md docs/README.codex.md skills/codex-autonomous-delivery`

## Release / Demo Readiness
- [x] New Codex-first lane is documented
- [x] Core terminology is consistent across skill and docs
- [x] No blocker-level contradiction remains
- [x] Future Codex run can resume from `docs/status.md`

## Command Matrix
```sh
test -f docs/plans/2026-03-18-codex-autonomous-delivery.md
test -f docs/status.md
test -f docs/test-plan.md
test -f skills/codex-autonomous-delivery/SKILL.md
rg -n "codex-autonomous-delivery|AGX|autonomous" README.md docs/README.codex.md skills/codex-autonomous-delivery
git diff --stat
```

## Open Risks
- Real trigger quality in Codex may still need a live session smoke later.
- The first slice may still be too documentation-heavy if the skill instructions are not explicit enough.

## Deferred Coverage
- Live Codex trigger smoke test against the installed skill.
- Optional AGX-assisted review example or helper script if the docs alone are not enough.
