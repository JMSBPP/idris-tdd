# idris-tdd

Cursor / agent **skill** for Idris2 development: hybrid of Brady-style **type, define, refine** and behavioral **TDD**, with heavy questioning, LaTeX algebra comments, holes-first definitions, and `types.toml` metadata.

## Install (Cursor)

```bash
mkdir -p ~/.cursor/skills
git clone https://github.com/JMSBPP/idris-tdd.git ~/.cursor/skills/idris-tdd
```

Or add as a git submodule under your skills directory. The agent loads `SKILL.md` when Idris / type-driven work is requested.

## Layout

| File | Role |
|------|------|
| [SKILL.md](SKILL.md) | Main workflow (required) |
| [REFERENCE.md](REFERENCE.md) | Pack, LSP, type kinds, `types.toml` |
| [EXAMPLES.md](EXAMPLES.md) | LaTeX / holes / test templates |

## Tooling pointers

- Pack: https://github.com/stefan-hoeck/idris2-pack
- LSP: https://github.com/dunhamsteve/idris2-lsp
- Tutorial: https://idris2.readthedocs.io/en/latest/tutorial/typesfuns.html

Brady (*Type-Driven Development with Idris*) and Maguire (*Algebra-Driven Design*) are cited as method sources — obtain lawfully; this repo does not redistribute them.

## License

MIT — see [LICENSE](LICENSE)
