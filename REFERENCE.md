# REFERENCE — idris-tdd

## Brady — Type-Driven Development with Idris

Search the machine (or your library) for Edwin Brady, *Type-Driven Development with Idris* (Manning, 2017) before inventing process — e.g. filenames matching `brady-type-driven-development*.pdf` / `*.md` extracts. Do not vendor copyrighted book text into this repo.

Core mantra (**Type, define, refine** — Brady §1.2.4):

1. Write input/output types (and data types) first
2. Define using structure of input types; incomplete defs OK via **holes**
3. Refine types and definitions together as the model clarifies

Types are a **plan**, not only a check. Prefer interactive hole-driven editing (`idris2-lsp`) over writing full bodies up front.

## Algebra-Driven Design (Maguire)

Treat the **algebra** (carriers, operations, laws) as the abstraction clients cannot escape. Code implements the algebra; the algebra is not reverse-engineered from code.

Reference: Sandy Maguire, *Algebra-Driven Design*. Use a local copy if you have one; do not paste book text into the skill repo.

Ask until carriers, operations, and laws are explicit. Invalid states should be unrepresentable in the type when dependent/indexed types allow.

## Pack + LSP

- **pack**: https://github.com/stefan-hoeck/idris2-pack — use project `pack.toml` / `.ipkg`; prefer `pack build`, `pack test`, `pack repl` over ad-hoc global installs
- **idris2-lsp**: https://github.com/dunhamsteve/idris2-lsp — hole info, case-split, type-driven edits
- Tutorial: https://idris2.readthedocs.io/en/latest/tutorial/typesfuns.html

If pack/LSP missing, ask the user how their environment is set up; do not silently switch package managers.

## Working directory

Default: **`.spec/`** under the project root.

Ask every session (or when switching modules). All new `.idr` / tests for this skill’s work go under that directory unless the user overrides (e.g. `idris/abi`).

## Type kinds (categorize before defining)

Record one primary kind in `types.toml`:

| Kind | Meaning |
|------|---------|
| `plain` | Non-parameterised data / simple alias |
| `generic` | Parameterised, non-dependent |
| `dependent` | Type depends on values |
| `indexed` | Family indexed by another type/value |
| `interface` | Idris interface / constrained ops |
| `view` | View / with-pattern decomposition |
| `other` | Explain in metadata `notes` |

## `types.toml` schema

Path: `{working_dir}/types.toml` (create if missing). Split by **domain** tables:

```toml
# {working_dir}/types.toml

[abi.Outcome]
module = "Abi.Outcome"
kind = "plain"
file = "Abi/Outcome.idr"
carriers = ["Returndata"]
operations = ["succeed", "revert", "statusOf", "returndataOf"]
laws = ["Success carries 32-byte returndata", "Revert has empty returndata"]
refined = false
notes = "EVM ABI observation sum"

[abi.U8]
module = "Abi.U8"
kind = "generic"
file = "Abi/U8.idr"
carriers = ["Byte"]
operations = ["packU8", "fromMaybeU8"]
laws = ["packU8 left-pads to 32 bytes", "Nothing maps to Revert"]
refined = false
notes = "No library U8 carrier; Byte is Bits8/Fin 256 at call site"
```

In the `.idr` file, immediately above the type (after LaTeX), add a short pointer:

```idris
-- types.toml: abi.Outcome
```

Update the same entry when refining (set `refined = true`, amend `laws`).

## Hybrid with behavioral TDD

From sibling `tdd` skill:

- Vertical slices only (one behavior → implement → next)
- Tests describe WHAT through the public API
- No mocking internals; no testing hole names or private helpers

Idris-specific RED: typechecks with holes failing totality/tests, or a test module asserting public results. GREEN: fill holes minimally for that behavior.

## Anti-patterns

- Writing bodies before algebra agreement
- Skipping LaTeX comments
- Bulk tests before any type exists
- Omitting `types.toml` registration
- Using hevm/oracle rules from other skills unless the issue says so
- Inventing a U8 (or other) carrier the user said belongs at the call site
