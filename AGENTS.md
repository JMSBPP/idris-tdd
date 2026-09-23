# idris-tdd — agent guide

## Mission statement

Continuously refactor Idris2 work so CI stays green with shorter wall-clock and higher
signal — every refactor slice is planned via `/request-refactor-plan`, executed under
`/idris-tdd` (and `/idris-ci-refactor` when the trigger is a CI failure), and verified
only by the host project's existing push/gate patterns (`compile.toml`, domain
manifests, pinned GHCR `spec-tools` image via `just idris` / `just spec-compile`).

**Goals (ordered):**

1. **Reduce build time** — shrink the critical path in-repo *and* in workflows: prefer
   image pull over rebuild, lean `compile.toml` / domain scope, no host Idris escape
   hatch when CI pins Docker, plus proactive workflow optimizations (GHCR image cache,
   job shape, parallelization where the runner model allows). Prefer applying or
   trying changes toward that goal rather than only documenting them. When touching
   Actions YAML, cite current GitHub docs (workflow syntax, cache, reusable workflows,
   container jobs) so proposals stay grounded.
2. **Maximize effectiveness** — fail early on `push-build` Spec compile; keep merge
   gates as authority; never silence or skip gates to go green.
3. **Small batches** — tiny commits that leave the tree working; CI failure → agent
   loop → plan → fix → push → re-watch.

**GitHub Actions pointers** (refresh from docs when optimizing):

- Workflows: https://docs.github.com/en/actions/using-workflows
- Caching: https://docs.github.com/en/actions/using-workflows/caching-dependencies-to-speed-up-workflows
- Reusable workflows: https://docs.github.com/en/actions/using-workflows/reusing-workflows
- Container / service containers: https://docs.github.com/en/actions/using-jobs/running-jobs-in-a-container

## CI loop (with `/request-refactor-plan`)

When CI fails on an Idris-related step (or the maintainer asks for continuous CI
refactoring):

1. Capture the failing job/step and log excerpt (e.g. `gh run view --log-failed`).
2. Invoke `/request-refactor-plan` for a scoped plan issue (mission above).
3. Execute slices with `/idris-tdd` (or `/idris-ci-refactor` as the orchestrator).
4. Push; treat GitHub Actions / `just spec-compile` in CI as verification — do not
   substitute an unpinned host Idris when the host repo pins Docker.

**Speculative CI optimizations:** always their own plan slice. “Try” = branch/PR + measure
wall-clock from Actions. Do not merge or silently change merge-gate required checks
without maintainer approve on that slice.

**Loop exit:** stop when the triggering run is green **and** open plan slices for that
failure are done or explicitly deferred in the issue. Do not hunt unrelated build-time
wins in the same invocation unless already listed on the plan.

## Artifacts (this skill)

Policy lives in this file. Implementation also ships:

- `commands/idris-ci-refactor.md` — orchestrator slash command
- CI loop section in `SKILL.md` (trigger → plan → slices → push → watch → exit)
- README install / symlink lines for the new command

## Testing Decisions (for `/request-refactor-plan` issues)

When the plan is opened from a CI-refactor trigger:

- Verification = named CI jobs/steps (`push-build` Spec compile / forge / merge gate).
- Success = green run URL or log excerpt for those steps.
- Do **not** list “verify with unpinned host Idris” when the host repo pins Docker /
  `just idris`.
- Still list Idris tests / property checks when the fix is type *behavior*; CI remains
  the authority gate.
