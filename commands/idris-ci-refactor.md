# Idris CI refactor

Orchestrate **continuous CI refactoring** for Idris2 under **idris-tdd**. Policy:
`~/.cursor/skills/idris-tdd/AGENTS.md` (mission: reduce build time, maximize
effectiveness). Do not invent a parallel CI policy.

**Trigger only:** an explicit CI failure, or a maintainer-pasted failed
`push-build` Spec compile / Idris step log. Ordinary type work stays `/idris-tdd`.

1. Read `~/.cursor/skills/idris-tdd/SKILL.md` (CI loop section) and `AGENTS.md`.
2. If the skill is missing:
   `git clone https://github.com/JMSBPP/idris-tdd.git ~/.cursor/skills/idris-tdd`
3. Capture failing job/step + log (`gh run view --log-failed` or paste). If the
   failure is primarily Plank, hand off to `/plank-ci-refactor` (or keep ownership
   here and call `plank-tdd` for sibling slices — one plan issue, failing-step owns).
4. **Always** invoke `/request-refactor-plan` first (mission from `AGENTS.md`). Do
   not jump to code from a red log. Specialize **Testing Decisions**: named CI
   jobs/steps; success = green run URL; no unpinned host Idris when the host pins
   Docker / `just idris`.
5. Execute approved plan slices with `/idris-tdd`. Speculative workflow YAML /
   GHCR cache / image-pin optimizations are their own plan slices: try = branch/PR
   + measure wall-clock from Actions; cite GitHub Actions docs; never silently
   change merge-gate required checks.
6. Push; watch CI. Exit when the triggering failure is green **and** open plan
   slices for that failure are done or deferred on the issue.
7. Any args after `/idris-ci-refactor` are failure context (run URL, log excerpt) —
   incorporate them after confirming the failing step.
