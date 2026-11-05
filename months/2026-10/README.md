# October 2026 — Close the Loop

## Goal

Spend roughly **4–6 focused hours** turning the agent tools you already have into **operated workflows**: isolated workspaces, explicit acceptance checks, durable run records, human review points, and reproducible policy/configuration.

September's intended theme was bounded autonomy. Repository evidence shows useful progress in that direction outside this curriculum repo: `nino96/home-server` now has portable Codex/OpenCode/Claude/Copilot agent configuration, explicit worker-routing guidance, an OpenCode Web systemd service, and Tailscale-friendly remote access. What is **not** yet evidenced is the September policy-test suite, a measured single-vs-parallel coding experiment, or the read-only Ops triage agent.

So October should not add another agent framework. It should connect what already exists into a measurable control loop.

## Why this month matters

Several September practitioner signals converge on the same point.

- GitHub's September 16 write-up on porting the Copilot runtime to Rust describes an agent-heavy rewrite that ended at more than 800,000 lines of production Rust across 128 pull requests. The useful lesson is the operating model: incremental slices, isolated branches/worktrees, heavy mechanical verification, automated PR cleanup, and human attention focused on architecture and risky exceptions rather than line-by-line generation.
- GitHub's September 2 efficiency analysis found that optimizing a local metric such as tool-output token count can make the whole task **slower and more expensive** if the agent has to reread or rerun work. They evaluated end-to-end outcomes and added behavioral regression tests when prompt changes altered orchestration.
- OpenAI's September 10 Agents API launch makes durable sessions, context compaction, tool discovery, subagents, and recovery explicit parts of the **harness layer**. You do not need to buy API usage this month; treat the architecture as a reference for what your local control plane should own.
- OpenAI's September workflow research argues that the valuable transition is from one-off AI use to recurring, measurable workflows. A separate study of more than 1.5 million work-related messages found evidence that people repeatedly use AI to take on tasks outside their normal occupational boundary.
- NVIDIA's September NemoClaw releases moved sandbox configuration closer to infrastructure-as-code: verified secret-free configuration export, deny-by-default networking, and explicit MCP tool-denial rules.

The month therefore focuses on one idea:

> **The unit of AI leverage is a closed, observable workflow — not a clever prompt or an impressive single run.**

## What to learn this month

### 1. Distinguish subagents from isolated worker sessions

Use **subagents** for bounded research/review that should return a compact answer into a parent session. Use a **separate session/worktree/branch** for work that produces an independent diff.

A practical rule:

```text
needs only information?
    -> subagent

needs to modify overlapping repository state?
    -> one writer

needs an independent diff that can be merged later?
    -> isolated worktree/session
```

GitHub's Rust migration is useful here because it documents both patterns and the coordination cost that appears when multiple writing sessions touch shared hubs.

### 2. Automate the PR inner loop, not the final judgment

Aim for:

```text
task contract
  -> isolated branch/worktree
  -> agent implementation
  -> deterministic checks
  -> review feedback / CI
  -> agent remediation
  -> green PR
  -> human architecture/risk review
  -> merge
```

The machine should spend time on test failures, lint, straightforward review feedback, and merge conflicts. Your review time should move upward toward architecture, behavior, security, and whether an apparent waiver is actually justified.

### 3. Keep a stable behavioral oracle

For migrations and refactors, do not rewrite the test oracle at the same time as the implementation unless the behavior genuinely changes.

GitHub's migration post is unusually explicit: insufficient end-to-end coverage caused many regressions, and stable E2E tests were critical to proving that the port preserved behavior.

For your own agent tasks, acceptance checks should exist **before** the agent starts.

### 4. Optimize completed work, not token snippets

Do not optimize because a log became shorter or a cheaper model answered more calls.

Track at least:

```text
accepted outcome
wall-clock time
human review minutes
retries / repeated exploration
test success
premium/API usage when observable
```

A cheap first attempt that causes three retries is not cheap.

This also gives you a better way to evaluate your existing Codex/OpenCode worker-routing configuration.

### 5. Treat agent configuration and policy as infrastructure

The useful part of NemoClaw's September changes is not another agent brand. It is the direction:

```text
live sandbox
   -> verified secret-free export
   -> version control
   -> policy tests
   -> restore/rebuild
   -> compare observed state
```

Agent state that cannot be exported, diffed, rebuilt, and negatively tested is still a pet environment.

## Focused agentic-coding skill

### PR lifecycle orchestration

Practice taking **one small real change** all the way from an executable task contract to a green PR while minimizing manual intervention before the final review.

Do not practice "write a larger prompt". Practice the control loop around the agent.

## General-purpose automation pattern

### Trigger → context → contract → work → evidence → decision → record

For a recurring non-coding workflow, define:

```yaml
trigger: when does this run?
inputs: what arrives?
context: what sources may be read?
tools: what capabilities are allowed?
output_contract: what must be produced?
evidence: what must support the result?
human_decision: where must a person decide?
metric: how will value be measured?
retention: what state should survive the run?
```

This is the bridge from "I sometimes use ChatGPT for this" to a workflow you can improve over time.

## DevOps improvement of the month

### Add a run manifest to agent work

Every non-trivial agent run should leave a small machine-readable record.

At minimum:

```json
{
  "task_id": "doctor-script",
  "repo": "nino96/home-server",
  "base_sha": "...",
  "agent": "codex|claude-code|opencode|copilot",
  "model": "if observable",
  "started_at": "...",
  "checks": [],
  "result": "accepted|rejected|partial",
  "human_review_minutes": 0,
  "notes": ""
}
```

This becomes the seed of your eval dataset instead of relying on memory.

## Home-lab direction

You already have an always-on remote agent surface through OpenCode Web and Tailscale-oriented setup in `home-server`. **Do not add another UI this month.**

Improve the operating layer around it:

- one reproducible task runner;
- isolated worktrees for writes;
- explicit secret boundaries;
- logs/run records;
- sandbox policy expressed as configuration;
- rebuild/restore instructions.

The next useful home-lab capability is not "more services". It is the confidence to let an agent touch an existing service repository without turning the GX10 into an unreviewable pet machine.

## Core exercises

1. [`exercises/01-agent-pr-control-loop.md`](exercises/01-agent-pr-control-loop.md) — build and use a minimal issue/task → worktree → agent → verify → PR control loop.
2. [`exercises/02-productize-one-recurring-workflow.md`](exercises/02-productize-one-recurring-workflow.md) — turn one recurring personal or knowledge-work task into a reusable, measurable workflow contract.
3. [`exercises/03-sandbox-as-code.md`](exercises/03-sandbox-as-code.md) — carry September's sandbox objective forward using policy-as-code, configuration export, and a rebuild test.

## Suggested 4–6 hour schedule

### Friday — ~2 hours

- Read the selected sections of GitHub's Rust migration case and the agent-efficiency article.
- Complete Exercise 1 through one real green PR.
- Record the run rather than relying on memory.

### Saturday — ~2.5 hours

- Spend ~60 minutes on Exercise 2.
- Spend ~75–90 minutes on Exercise 3.
- Spend ~20 minutes updating `notes/progress.md` with actual outcomes.

### Optional stretch — ~45 minutes

Run one matched coding task with your normal routing strategy and with GitHub HydraFusion if it is available in your Copilot Pro account. Compare **accepted result + review time**, not just apparent model quality.

## Definition of done

October is complete when:

- [ ] One real coding task has gone through an isolated branch/worktree, deterministic verification, and a PR.
- [ ] The run has a machine-readable record including human review time.
- [ ] One recurring non-coding workflow has a written trigger/input/output/evidence/metric contract and has been run at least once.
- [ ] One local-agent sandbox has versioned, secret-free configuration or an equivalent declarative specification.
- [ ] At least three negative policy checks prove a forbidden capability is actually blocked.
- [ ] The sandbox can be rebuilt or restored from the committed configuration.
- [ ] Unfinished September items are explicitly marked Completed, Deferred, or Superseded rather than silently accumulating.

## Skip this month

- **Model-release tourism.** September had another wave of frontier-model releases. Your bigger bottleneck is the operating loop around models.
- **Building a custom multi-agent framework.** Study the harness boundary; do not recreate context compaction, session recovery, scheduling, and tool discovery unless a real requirement forces you to.
- **Token shaving without task-level measurement.** Local savings can produce more retries and higher end-to-end cost.
- **Unbounded parallel coding agents.** Parallelism is useful only when ownership boundaries and merge/review cost stay controlled.
- **Full *arr/content-stack expansion.** Get restore, secrets, policy, and agent-run hygiene right before increasing the number of stateful services.
