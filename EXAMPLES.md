# EXAMPLES — idris-tdd

## LaTeX above a type

```idris
||| ABI observation: success returndata vs bare revert.
|||
||| $$
||| \mathrm{Outcome} ::= \mathrm{Success}(w) \mid \mathrm{Revert}
||| $$
||| with $|w|=32$, and $\mathrm{Revert}$ carrying $\varepsilon$ (empty returndata).
|||
||| types.toml: abi.Outcome
module Abi.Outcome

public export
data Outcome : Type where
  Success : (returndata : Returndata) -> Outcome
  Revert  : Outcome
```

Idris doc-comments (`|||`) are preferred so LSP/`pack` show the math. Plain `--` LaTeX blocks are OK if the project style demands it.

## LaTeX above an implementation (holes first)

```idris
||| Encode a byte as ABI uint8 word:
||| $$
||| \mathrm{packU8}(v) = 0^{31} \mathbin{\!+\!\!+} [v]
||| $$
||| types.toml: abi.U8
public export
packU8 : Bits8 -> Returndata
packU8 v = ?packU8_rhs
```

Agree algebra → write type → leave `?packU8_rhs` → refine with LSP case-split / search.

## Test file with behavioral semantics

```idris
||| Behavioral: packing 255 yields a 32-byte success word with low byte 0xFF.
||| $$
||| \mathrm{status}(\mathrm{Success}(\mathrm{packU8}(255))) = \mathsf{ok}
||| \land \mathrm{returndata} = 0^{31}\mathbin{\!+\!\!+}[255]
||| $$
packU8_255_matches_fixture : packU8 255 = expectedWord255
packU8_255_matches_fixture = Refl  -- or fixture equality helper
```

Name tests after **behavior**, not after hole names. One semantic claim per test when practical.

## Session skeleton (AskQuestions)

1. “Working directory for this Idris work? `[.spec/]`”
2. “Domain key for `types.toml`? (e.g. `abi`)”
3. “What are the carriers of this algebra?”
4. “What operations, and which laws must hold?”
5. “Type kind: plain / generic / dependent / indexed / interface / view?”
6. “Which single behavior should the first test lock?”

Only after (3)–(5): create files and holes.
