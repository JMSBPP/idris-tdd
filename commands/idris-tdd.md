# Idris TDD

Invoke the **idris-tdd** skill and follow it exactly for this conversation turn onward.

1. Read and obey `~/.cursor/skills/idris-tdd/SKILL.md` (plus `REFERENCE.md` / `EXAMPLES.md` as needed).
2. If the skill is missing, clone it:
   `git clone https://github.com/JMSBPP/idris-tdd.git ~/.cursor/skills/idris-tdd`
3. Start with mandatory AskQuestions (working directory default `.spec/`, then algebra) — one question at a time.
4. Do not invent the algebra; do not write function bodies before types/holes are agreed.
5. Any args after `/idris-tdd` are the user's starting task context — incorporate them after the working-directory question.
