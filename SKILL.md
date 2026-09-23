---
name: idris-tdd
description: Hybrid Idris2 type-driven development and behavioral TDD with heavy questioning, LaTeX algebra comments, holes-first refine, types.toml metadata, and CI-failure continuous refactor via /idris-ci-refactor + /request-refactor-plan. Use when writing or refining .idr/.ipkg code, designing Idris types, fixing Idris-related CI / Spec compile failures, running pack/idris2-lsp workflows, or when the user mentions Idris, type-driven development, holes, algebra-driven design in Idris, or /idris-ci-refactor.
---

# Idris TDD (type-driven × behavioral TDD)

Hybrid of Brady **type, define, refine** and Cursor **tdd** vertical slices. Algebra and types lead; tests assert observable behavior through public interfaces. **Heavy AskQuestions** — do not invent the algebra.

Slash command for routine work: `/idris-tdd`. CI failure → `/idris-ci-refactor`
(mission/policy: [AGENTS.md](AGENTS.md)).

## Quick start

1. Ask: working directory? (default **`.spec/`** relative to project root)
2. Ask algebra of the next type (one question at a time) before any body
3. Create/open `.idr` under that dir; write **LaTeX math comments**, then type + holes
4. Register type in **`types.toml`** (domain section) + short pointer comment in the `.idr`
5. Vertical TDD: one behavioral test (with LaTeX semantics) → minimal fill of holes → repeat

## Iron laws

- No function bodies before the **algebra** is agreed with the user
- Holes first; never horizontal “all tests then all code”
- Tests use **public interface only**; LaTeX behavioral semantics above each test
- Prefer `pack` + `idris2-lsp`; do not invent a parallel toolchain

## AskQuestions (mandatory)

Ask **one** clarifying question at a time (interactive UI if available; else numbered options). Cover at least:

- Working directory (default `.spec/`)
- Domain / `types.toml` section name
- Algebra: carriers, operations, laws, invalid states
- Type kind: plain | generic | dependent | indexed | interface/constraint | view | other
- Next behavior to prove with a test

Do not skip algebra to “save time.”

## Type workflow (type → define → refine)

For each type/function:

1. **Setup** — `.idr` under working dir; module header
2. **Algebra** — grill user; draft LaTeX block in comments (see [EXAMPLES.md](EXAMPLES.md))
3. **Kind** — categorize; record in `types.toml` + `--- types.toml: domain.name ---` pointer above the type
4. **Type** — write the type signature / data declaration
5. **Define** — holes (`?name`) guided by the type; interact with LSP/`pack repl` as needed
6. **Refine** — tighten types/laws; update LaTeX + `types.toml` metadata
7. **Behavior** — one RED→GREEN test slice with matching LaTeX semantics

## Tooling

| Role | Source |
|------|--------|
| Package manager | https://github.com/stefan-hoeck/idris2-pack |
| LSP | https://github.com/dunhamsteve/idris2-lsp |
| Language tutorial | https://idris2.readthedocs.io/en/latest/tutorial/typesfuns.html |
| Type-driven method | Local Brady PDF — see [REFERENCE.md](REFERENCE.md) |
| Algebra design | Sandy Maguire, *Algebra-Driven Design* — see [REFERENCE.md](REFERENCE.md) |

## CI loop (`/idris-ci-refactor`)

Mission and full policy: [AGENTS.md](AGENTS.md). Summary:

1. **Trigger only** on an explicit CI failure / pasted failed log (not routine type work).
2. Capture failing job/step (`gh run view --log-failed` or paste). Failing-step owns;
   if cross-cutting with Plank, keep one plan issue and may call `plank-tdd` for sibling
   slices (or hand off to `/plank-ci-refactor` when the primary failure is Plank).
3. **Always** open/update a plan via `/request-refactor-plan` before code. Testing
   Decisions = named CI jobs/steps; success = green run URL; no unpinned host Idris when
   the host pins Docker / `just idris`.
4. Execute slices with `/idris-tdd`.
5. Workflow YAML / GHCR cache / image-pin optimizations are in scope: cite GitHub Actions
   docs, try on a branch/PR, measure wall-clock; never silently change merge-gate
   required checks. Speculative opts are their own plan slices.
6. **Exit** when the triggering run is green and plan slices for that failure are done
   or deferred — no drive-by build-time hunting beyond the plan.

## Details

- Mission / CI policy: [AGENTS.md](AGENTS.md)
- Schema, type kinds, Brady/Maguire pointers: [REFERENCE.md](REFERENCE.md)
- LaTeX / file / test templates: [EXAMPLES.md](EXAMPLES.md)
- Behavioral TDD rules: sibling skill `tdd` (vertical slices, no implementation-detail tests)
- Sibling: `plank-tdd` (`/plank-ci-refactor`), `request-refactor-plan`
- Slash commands: `/idris-tdd`, `/idris-ci-refactor` → install via [commands/](commands/) into `~/.cursor/commands/`
