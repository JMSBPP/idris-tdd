---
name: idris-tdd
description: Hybrid Idris2 type-driven development and behavioral TDD with heavy questioning, LaTeX algebra comments, holes-first refine, and types.toml metadata. Use when writing or refining .idr/.ipkg code, designing Idris types, running pack/idris2-lsp workflows, or when the user mentions Idris, type-driven development, holes, or algebra-driven design in Idris.
---

# Idris TDD (type-driven × behavioral TDD)

Hybrid of Brady **type, define, refine** and Cursor **tdd** vertical slices. Algebra and types lead; tests assert observable behavior through public interfaces. **Heavy AskQuestions** — do not invent the algebra.

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

## Details

- Schema, type kinds, Brady/Maguire pointers: [REFERENCE.md](REFERENCE.md)
- LaTeX / file / test templates: [EXAMPLES.md](EXAMPLES.md)
- Behavioral TDD rules: sibling skill `tdd` (vertical slices, no implementation-detail tests)
