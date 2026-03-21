# claude-code-tdd

Fully automated TDD pipeline for Claude Code. Approve the plan once — come back to a ready-to-merge PR with green CI.

Tester, coder, reviewer — three roles separated so the agent can't cheat.

```
you
 └─ reviewer (orchestrates everything)
      ├─ tester (writes tests BEFORE code exists)
      └─ coder (writes code to pass tests it can't modify)
```

## Why

Claude Code writes tests that validate its own bugs. It rewrites existing tests to match broken code. It hallucinates API responses. It skips its own review step when you let it orchestrate.

All tests green. Code broken. Agent insisting it's your fault.

These roles make cheating structurally impossible through file system boundaries, not instructions.

## Quick start

```bash
# Copy into your project
cp -r roles/ yourproject/roles/
cp scripts/run-step.sh yourproject/scripts/

# Launch the reviewer — it runs everything else
claude --dangerously-skip-permissions --system-prompt "$(cat roles/reviewer.md)"

# Describe your feature. Approve the plan. Walk away.
# Come back to a ready-to-merge PR with green CI.
```

## How it works

1. You describe a feature or bug to the **reviewer**
2. You prepare all the required API keys in .env file
3. Reviewer writes prompts, shows you the plan, **waits for your OK**
4. Reviewer launches **tester** → tests written (code doesn't exist yet, tests will fail)
5. Reviewer launches **coder** → code written to make tests pass
6. Reviewer verifies boundaries, runs checks, fixes failures **autonomously**
7. You come back to a ready-to-merge PR

The reviewer splits work into **functional prompts** (need your approval) and **operational prompts** (handles autonomously during a cycle). You approve once, agent does the rest.

## Key rules

| Rule | Why |
|---|---|
| Tester writes only in `test/` | Can't adjust code to make bad tests pass |
| Coder writes only in `src/` | Can't "fix" tests instead of fixing code |
| Existing tests are read-only | Prevents silent regression rewrites |
| Mocks only at HTTP boundary | Internal logic runs for real in tests |
| Every mock verified against real API | Agent hallucinates response structures |
| `git show --name-only` on every commit | Boundary violations = auto-reject |

## Files

```
roles/
  reviewer.md      # orchestrator
  tester.md         # tests only
  coder.md          # code only
  acceptance.md     # acceptance checklist
scripts/
  run-step.sh       # agent launcher
```

## Full story

**[How I stopped Claude Code from lying to my face](https://dev.to/elasticlove1/how-i-stopped-claude-code-from-lying-to-my-face-5ani)** — 6 things I caught the agent doing, and the system that fixed each one.

## License

MIT