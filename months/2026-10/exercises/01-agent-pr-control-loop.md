# Exercise 1 — Agent PR Control Loop

**Time:** 90–120 minutes  
**Target repo:** Prefer `nino96/home-server`.

## Goal

Take one small useful coding task through a repeatable flow:

```text
task contract -> isolated branch/worktree -> agent implementation
-> deterministic verification -> pull request -> human final review -> run record
```

The point is to practice the control loop, not to build a new orchestration framework.

## Recommended task

A useful candidate is a **read-only diagnostic script** for the home-server repo that reports whether expected agent configuration, OpenCode, the user service, and Tailscale access appear healthy. It must not change service state.

Choose acceptance checks before launching the agent. For example:

- the script parses successfully;
- static analysis passes when available;
- missing required inputs produce a clear nonzero result;
- the implementation contains no service-management mutations;
- documentation explains how to run it.

If a better small real task already exists when you do this exercise, use that instead.

## Step 1 — Write the contract first

Copy `../artifacts/agent-task.example.yaml` and fill it out.

The contract should name:

- objective;
- allowed files or modules;
- forbidden behavior;
- exact verification commands;
- risk level;
- whether human review is mandatory.

The agent may decide implementation details, but it does not get to redefine success after seeing the problem.

## Step 2 — Use isolated repository state

Create a dedicated Git branch and worktree for the writing agent.

Rules:

- one writing agent per worktree;
- no two agents own the same files unless the parent explicitly coordinates a handoff;
- research/review subagents may stay read-only.

## Step 3 — Give bounded context

Use whichever harness you currently prefer: Codex, Claude Code, OpenCode, or Copilot CLI.

Give it only:

1. the task contract;
2. relevant repository instructions/docs;
3. permission to inspect before editing;
4. the verification commands;
5. an instruction to stop when checks pass and summarize residual risk.

Avoid pasting large historical conversations.

## Step 4 — Make checks authoritative

Run verification outside the model as well. The check result, not the agent's claim, determines whether the gate passed.

The agent can diagnose failures and propose fixes, but it must not weaken or bypass a gate merely to make the task green.

## Step 5 — Open a PR and automate only the inner loop

Let the agent handle low-level remediation such as:

- test or lint failures;
- mechanical review comments;
- straightforward merge conflicts;
- documentation drift.

Keep final merge as a human decision.

Your review should focus on architecture, scope creep, permission changes, safety-check weakening, and behavior not covered by tests.

## Step 6 — Record the run

Copy `../artifacts/run-record.example.json` and record:

- base/head revision;
- harness/model if observable;
- wall time;
- checks and outcomes;
- retries;
- human review minutes;
- accepted/rejected result;
- one durable lesson.

Store the record under this month's artifacts or link to it from `notes/progress.md`.

## Definition of done

- [ ] Task contract existed before agent execution.
- [ ] Writing happened in an isolated branch/worktree.
- [ ] At least two deterministic checks ran.
- [ ] A real pull request was opened.
- [ ] Final merge remained a human decision.
- [ ] A machine-readable run record captures review time and outcome.

## Optional routing experiment

If HydraFusion is available in your Copilot Pro account, pick two matched microtasks and compare your existing manual routing strategy with HydraFusion.

Measure accepted result, wall time, human review minutes, retries, and usage if visible.

The goal is to evaluate the completed task, not which model sounded smarter.
