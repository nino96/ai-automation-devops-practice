# September 2026 — Bounded Autonomy

## Goal
Spend roughly **4–6 focused hours** moving from a reproducible AI serving stack to reproducible **agent systems that can be trusted with useful work**.

August established the right foundation: repository context, evals, deterministic guardrails, and a stable local-model abstraction. Repository evidence now shows that the GX10 serving portion is substantially real in [`nino96/dgx-spark-ai-stack`](https://github.com/nino96/dgx-spark-ai-stack): authenticated OpenAI-compatible access, LiteLLM routing, transactional model swaps, root-owned secrets, recovery docs, sanity probes, and benchmark hooks already exist. Do **not** rebuild that layer.

September's question is:

> How do I safely delegate larger chunks of work to agents while keeping outcomes measurable, permissions narrow, and the whole system reproducible?

## Why this month matters
Several recent practitioner signals point in the same direction.

- On **August 18**, Asana reported using up to four Codex agents in parallel to remove Enzyme from its codebase in about two weeks, with engineers reviewing proposed changes. The important lesson is not the headline speedup; it is that large, previously uneconomic migration work becomes a good agent target when the desired end state is mechanically checkable.
- OpenAI's current internal Codex guidance emphasizes planning first for larger changes, treating prompts like well-written GitHub issues, improving the agent development environment iteratively, and using agents as a task queue for work that can proceed while the engineer does something else.
- On **August 25**, OpenAI described an internal IT agent that triages and actions support requests and, at reporting time, resolved roughly 45% of ticket volume. The reusable pattern is **context + policy + supported actions + escalation**, not an unrestricted chatbot.
- Anthropic's **August 31** security update is a useful warning: capable agents given live access can take unintended actions when the environment or controls are wrong. Earlier Anthropic containment work makes the engineering response explicit: filesystem boundaries, egress controls, sandboxing, and approval policy matter because model intent alone is not a sufficient safety boundary.
- NVIDIA's August NemoClaw releases added practical DGX Spark features directly relevant here: managed vLLM profiles, policy tiers, stronger lifecycle/readiness checks, authenticated local endpoints, and read-only host mounts that persist across sandbox rebuilds.

The durable skill is therefore **bounded autonomy**: let the agent reason broadly while the environment constrains what it can actually do.

## What to learn this month

### 1. Select work by verifiability, not by size
Agents are increasingly useful on large migrations and maintenance tasks when success can be expressed as deterministic checks.

Good agent work:

```text
replace deprecated framework
  + all tests pass
  + forbidden dependency count = 0
  + build succeeds
  + diff constrained to expected modules
```

Poor agent work:

```text
"improve the architecture"
"clean up the repo"
"make this production ready"
```

The key shift is from **task decomposition first** to **acceptance criteria first**.

### 2. Parallel agents are valuable only when merge/review cost stays bounded
The Asana case is useful because several agents worked in separate codebase copies while a human reviewed proposed changes. Parallelism is not automatically leverage: four agents can generate four times the review burden.

Practice parallelism only when tasks are separable and each branch has a machine-checkable contract.

```text
migration objective
      |
      +--> agent A: module group 1 --+
      +--> agent B: module group 2 --+--> deterministic validation --> human review
      +--> agent C: module group 3 --+
```

Track **accepted output per unit of review time**, not number of agents launched.

### 3. Sandbox and policy are part of the agent architecture
Treat an agent like an untrusted-but-useful operator.

A useful local pattern is:

```text
                        GX10 host

  root-owned secrets / Docker socket / system config
               X             X
               |             |
      +--------------------------------+
      | sandboxed agent                |
      |                                |
      | read-only mounted workspace    |
      | bounded network policy         |
      | no host secrets                |
      | explicit allowed tools         |
      +---------------+----------------+
                      |
                      v
             LiteLLM / local model
```

This complements the security model already present in `dgx-spark-ai-stack`, where agents can edit a repo but cannot directly read root-owned secrets or freely operate Docker.

### 4. Real automation is context + reasoning + policy + deterministic execution
OpenAI's IT example is a useful blueprint for personal automation. The valuable agent is not one that can "do anything". It is one that can:

1. gather approved context;
2. classify/diagnose the situation;
3. choose from a small action vocabulary;
4. execute low-risk actions or request approval;
5. verify the result;
6. preserve an audit trail.

For your home systems, start read-only.

```text
alert / request
      |
      v
collect health + logs + config metadata
      |
      v
LLM diagnosis
      |
      v
structured recommendation
      |
      +--> safe/read-only follow-up
      |
      +--> mutation requested -> human approval
```

### 5. The agent harness should survive model replacement
Your existing LiteLLM/OpenAI-compatible abstraction is exactly the right base. September exercises should call the gateway contract, not hard-code Qwen/Nemotron/Claude/OpenAI assumptions.

A model change should trigger:

```text
model change
   -> sanity tests
   -> agent eval suite
   -> compare success / latency / cost
   -> promote or roll back
```

not application rewrites.

## Focused agentic-coding skill
### Acceptance-driven delegation

Before asking a coding agent to implement anything, write:

```yaml
objective: Remove deprecated API X from module Y
constraints:
  - no public API change
  - no dependency additions
checks:
  - ./gradlew test
  - ./gradlew build
  - grep for forbidden symbol returns zero matches
review:
  - summarize architectural changes
  - list files changed outside expected scope
```

Then let the agent plan and execute.

The practice is not "write better prompts". It is **turn intent into an executable contract**.

## General-purpose automation pattern
### Observe → Diagnose → Propose → Approve → Act → Verify

Use one typed schema between reasoning and action:

```json
{
  "diagnosis": "...",
  "confidence": 0.0,
  "evidence": ["..."],
  "proposed_action": "none|restart_service|open_issue|...",
  "risk": "read_only|low|high",
  "verification": ["..."]
}
```

The executor should reject unknown actions. The model does not get arbitrary shell access just because it can produce shell commands.

## DevOps improvement of the month
### Add policy + eval gates around agents

The reusable deployment contract should evolve from August's service-level commands to:

```bash
make agent-up
make agent-smoke
make agent-eval
make agent-policy-test
make agent-down
```

At minimum, policy tests should prove:

- the agent cannot read a known forbidden secret path;
- a read-only mount really is read-only;
- disallowed network destinations fail;
- unsupported action names are rejected;
- every run emits a machine-readable result/log.

## Home-lab direction
Do **not** build the full *arr/content stack merely because it is popular. Use September to build the **operating pattern** that any future stack can reuse: read-only diagnostics, structured recommendations, explicit mutation boundaries, and auditability.

Once that works, the same agent can later understand Jellyfin/*arr errors, disk pressure, backup failures, failed containers, or media-library inconsistencies without being granted unrestricted root/Docker access.

## Core exercises
1. [`exercises/01-sandboxed-local-agent.md`](exercises/01-sandboxed-local-agent.md) — put a useful local agent behind real filesystem/network boundaries on the GX10.
2. [`exercises/02-parallel-migration-eval.md`](exercises/02-parallel-migration-eval.md) — test whether parallel coding agents create measurable leverage on a verifiable migration.
3. [`exercises/03-personal-ops-triage-agent.md`](exercises/03-personal-ops-triage-agent.md) — build the first read-only personal/home-server operations agent pattern.

## Suggested 4–6 hour schedule

### Friday — ~2 hours
- Read the Asana migration case and Anthropic containment article.
- Complete Exercise 1 through the first policy tests.

### Saturday — ~2.5 hours
- Spend ~75–90 minutes on the parallel migration experiment.
- Spend ~60 minutes building the read-only Ops triage prototype.
- Spend ~20 minutes recording outcomes in `notes/progress.md`.

### Optional stretch — ~1 hour
Route agent traces/structured run records into your existing monitoring stack, or add a small regression job that runs the agent eval suite whenever the local model catalog/pin changes.

## Definition of done
September is complete when:

- [ ] One local agent runs on the GX10 with a documented sandbox/policy boundary.
- [ ] At least three negative policy tests prove forbidden actions actually fail.
- [ ] One coding task has been attempted with both single-agent and parallel/decomposed execution using identical acceptance checks.
- [ ] Review time and accepted outcome are recorded, not just agent runtime.
- [ ] One read-only Ops workflow produces a typed diagnosis/recommendation from real or fixture health data.
- [ ] No agent requires unrestricted Docker/root access or secrets stored in the practice repository.
- [ ] Results are recorded in `notes/progress.md` and any long-lived implementation is linked rather than copied here.

## Skip this month
- **Model leaderboard hopping.** Your serving abstraction already makes model swaps cheap; improve the eval that decides whether a swap is worthwhile.
- **Unbounded multi-agent frameworks.** Parallelism without acceptance checks usually multiplies review work.
- **A giant MCP catalog.** Every connector is another capability and trust boundary. Add one only for a real workflow.
- **Autonomous infrastructure remediation with root/Docker access.** Earn write privileges incrementally after the read-only diagnostic loop is reliable.
- **Rebuilding your existing GX10 serving stack.** It is already materially beyond the August baseline; use it as infrastructure.
