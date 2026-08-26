# August 2026 — Build the Harness, Not Another Demo

## Goal
Spend roughly **4–6 focused hours** improving the system around AI rather than chasing another model or framework. This month establishes the reusable foundations for everything that follows: repository context, agent evals, reproducible local inference, and deterministic guardrails around agentic work.

## Why this month matters
The most durable practitioner lesson is that capable coding agents increasingly behave like nondeterministic production software. Their usefulness depends heavily on the surrounding environment: durable repository context, deterministic tooling, isolated execution, tests/evals, traces, and CI gates.

That maps directly onto a DevOps mindset:

```text
prompt / issue
    ↓
agent reasoning
    ↓
tools + environment
    ↓
change / outcome
    ↓
tests + evals
    ↓
CI gate
    ↓
telemetry
    ↓
observed failure
    ↓
new regression test
```

## High-value ideas to learn

### 1. Repository design is part of agent performance
Treat `AGENTS.md` as a concise map, not an ever-growing memory dump. Put detailed architecture, decisions, runbooks, and plans in structured docs that agents can discover when relevant.

Practice the pattern:

```text
repo/
├── AGENTS.md
├── ARCHITECTURE.md
├── README.md
├── docs/
│   ├── index.md
│   ├── decisions/
│   ├── runbooks/
│   └── plans/
├── scripts/
├── evals/
└── .github/workflows/
```

### 2. Agent evals are the equivalent of automated tests
Do not judge a coding agent by "felt good" or by final prose alone. Start capturing task success, tests passed, human corrections, elapsed time, and tool/trajectory behavior where available.

The target loop is:

```text
bad run
  ↓
review failure
  ↓
turn failure into eval
  ↓
regression suite
  ↓
change prompt/model/harness
  ↓
rerun
```

### 3. The GX10 should be a service, not a one-off experiment box
Create one stable OpenAI-compatible local inference endpoint and make downstream experiments depend on that interface rather than a specific model.

Desired abstraction:

```text
clients
  ↓
OpenAI-compatible endpoint
  ↓
vLLM / llama.cpp / other engine
  ↓
local model on GX10
```

Model changes should become configuration changes, not application rewrites.

### 4. Developer → orchestrator
Use LLMs for ambiguous reasoning; use ordinary software for guarantees.

```text
LLM SHOULD DECIDE                  SOFTWARE SHOULD GUARANTEE
Likely cause of CI failure         execution timeout
Which docs need updating           secret boundaries
What implementation plan to try    mandatory tests
Which tool is relevant              allowed filesystem/network
```

The durable skill is designing the system through which agent-generated work is proposed, validated, reviewed, and shipped.

## Focused agentic-coding skill
### Context engineering through repository design

For each task, practice:

1. Read `AGENTS.md`.
2. Locate only the architecture/docs relevant to the task.
3. Inspect an analogous implementation.
4. Produce a plan before editing.
5. Execute narrowly.
6. Run deterministic checks.
7. Review the diff.
8. Record only durable knowledge that should remain true.

## General-purpose automation pattern
### Event → reason → propose → approve → execute → verify

```text
EVENT
  ↓
collect deterministic context
  ↓
LLM reasons
  ↓
structured proposed action
  ├─ low risk ─→ execute
  └─ high risk ─→ human approval ─→ execute
                                      ↓
                                    verify
                                      ↓
                                   telemetry
```

Use this later for CI remediation, document processing, server alerts, personal-content workflows, and home automation.

## DevOps improvement of the month
Establish a common project contract:

```bash
make bootstrap
make up
make down
make test
make eval
make lint
make backup
make restore-test
```

A future clean Linux host + Git clone + documented secret injection should be enough to recreate the environment.

## Home-lab direction
Do **not** build the entire media/content stack this month. Establish a structure it can later live within:

```text
homelab/
├── inventory/
├── ansible/
├── compose/
│   ├── observability/
│   ├── ai/
│   ├── content/
│   └── automation/
├── config/
├── backups/
└── docs/
```

The aim is to avoid accumulating unrelated Compose files without a recovery or operating model.

## Core exercises
1. [`exercises/01-ai-lab-repository.md`](exercises/01-ai-lab-repository.md) — create the reusable AI lab skeleton.
2. [`exercises/02-coding-agent-evals.md`](exercises/02-coding-agent-evals.md) — build the first five-task coding-agent eval set.
3. [`exercises/03-gx10-local-inference.md`](exercises/03-gx10-local-inference.md) — turn local model serving into a reproducible service.

## Suggested 4–6 hour schedule

### Friday — ~2 hours
- Read the two highest-priority harness/coding-agent sources in `resources.md`.
- Build the `ai-lab` repository baseline and agent-facing docs.

### Saturday — ~2.5 hours
- Build five coding-agent eval tasks and run an initial comparison.
- Package the current GX10 inference setup as a reproducible service.

### Optional stretch — ~1 hour
Instrument either the eval runner or local inference service with OpenTelemetry and connect it to an existing or minimal observability stack.

## Definition of done
August is complete when:

- [ ] A reusable `ai-lab` repository or equivalent project skeleton exists.
- [ ] At least five coding-agent tasks have explicit acceptance checks.
- [ ] At least one agent/model comparison has been recorded with objective outcomes.
- [ ] The GX10 inference endpoint can be rebuilt from version-controlled configuration.
- [ ] A smoke test verifies the local inference service after restart/redeploy.
- [ ] Unfinished items are explicitly marked for carry-forward rather than silently abandoned.

## Skip this month
- Large "multi-agent company" demos without failure data or reproducible evals.
- Hunting for the perfect local model before the serving abstraction exists.
- Building a giant MCP/tool collection without a concrete workflow need.
- Prompt-library collecting as a substitute for context, tests, tools, and feedback loops.
