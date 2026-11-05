# Exercise 2 — One evidence-gated coding PR (90 min)

Carry forward October's agent-to-PR loop. Choose a **small, real, reversible** change in `home-server` or a toy repository. No production credentials.

1. Write a task contract before invoking any agent: base commit SHA, permitted paths, acceptance tests, forbidden edits, and review rubric.
2. `git worktree add ../task-worktree -b practice/nov-evidence <base-ref>` and run the coding agent **only inside that worktree**.
3. Run deterministic tests (lint/unit/contract tests). Save command, exit code, duration, and a diff summary. Do not modify the test oracle to make failures disappear.
4. Use a *fresh* reviewer context without write tools. Give it the contract, diff, and test evidence. Require evidence references and an explicit PASS/FAIL/UNCERTAIN per criterion; start every criterion FAIL.
5. Manually inspect risky changes, open a PR only if checks pass, and leave merging to the human. Store a run record conforming to [../artifacts/run-record.example.json](../artifacts/run-record.example.json).

**Metrics:** accepted change (yes/no), human review minutes, wall time, retry count, check failures, and any observable token cost. Compare against a manual baseline later; do not invent time savings.

**Cleanup:** `git worktree remove ../task-worktree` after the work is preserved in a branch/PR; do not delete uncommitted changes.

**Pass:** evidence supports every criterion; the reviewer cannot edit tests or code; a failed criterion blocks promotion.
