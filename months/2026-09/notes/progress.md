# September 2026 Progress

## Starting point

August remains only partially evidenced from the practice repository itself. However, the linked [`nino96/dgx-spark-ai-stack`](https://github.com/nino96/dgx-spark-ai-stack) demonstrates that the **reproducible GX10 serving** objective is substantially satisfied outside this curriculum repository:

- authenticated OpenAI-compatible access over Tailscale;
- LiteLLM gateway abstraction;
- version-controlled model catalog and activation flow;
- transactional model swaps with health/sanity checks and rollback;
- root-owned secret handling separated from agent-writable repository content;
- operations, recovery, upgrade, security, compatibility and monitoring documentation;
- sanity and benchmark-eval scaffolding.

The August `ai-lab` repository baseline and five-task coding-agent eval set are **not yet confirmed complete** by repository evidence, so September deliberately incorporates the valuable part of those objectives rather than marking all of August complete.

## September status

**Status:** Proposed

| Exercise | Status | Durable output target | Evidence/link |
|---|---|---|---|
| Sandboxed local agent | Proposed | pinned sandbox/policy config + negative policy tests | |
| Parallel migration eval | Proposed | machine-readable single-vs-parallel result | |
| Read-only Ops triage agent | Proposed | collector + schema + three fixtures/evals | |

## Results to record

### Exercise 1 — sandboxed local agent
- implementation used:
- pinned version:
- model endpoint:
- positive smoke test:
- forbidden-read test:
- read-only-write test:
- blocked-egress test:
- implementation repo/PR:

### Exercise 2 — parallel migration eval
- task:
- single-agent tool/model:
- parallel tool/model:
- single-agent human minutes:
- parallel human minutes:
- single accepted?:
- parallel accepted?:
- key lesson:
- artifact link:

### Exercise 3 — read-only Ops triage
- incident class:
- collector path:
- fixture count:
- model/provider tested:
- schema validation:
- useful diagnosis?:
- implementation repo/PR:

## End-of-month decision
At the end of the month, explicitly mark each item **Completed**, **Deferred**, or **Superseded**. October should carry forward at most one unfinished high-value item before introducing new infrastructure.
