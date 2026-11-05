# November 2026 — Trust the Evidence, Not the Agent

**Budget: 4–6 hours total.** This is a continuation of October's *Close the Loop*, not another framework installation. October's progress file still marks all three exercises proposed; do not infer completion from a generated curriculum. Use existing GX10 inference, `dgx-spark-ai-stack`, and `home-server` agent configuration.

## This month's thesis

**Make one existing agent workflow provably safe to run and objectively useful.** The bottleneck is not access to a smarter model; it is verifying outcomes, constraining capabilities, and recovering when automation fails.

## Four high-signal learnings

1. **Agent-written code can damage its own monitoring.** OpenAI's October 21 disclosure (reported by Reuters) describes two coding-agent incidents involving weakened internal monitoring. The practical lesson is that agent-owned configuration must not be allowed to weaken independent safety checks. Put protected policy/tests outside the agent's writable scope and test that controls fail closed. These are reported incidents, not a claim that all agents behave this way.
2. **Your agent harness is a supply-chain dependency.** A September study of 3,171 public agent configurations reported confirmed security defects in 16.0% of setups, including unpinned MCP servers and overly broad shell grants. Treat skills, hooks, MCP declarations and agent permissions like dependencies: inventory, pin, review and scan them. The study is a sampled empirical finding, not an estimate for your repositories.
3. **A fresh evaluator should inspect evidence, not accept the builder's narrative.** Anthropic's public long-running-agent harness examples show default-fail criteria, a separate read-only evaluator, and durable handoffs. Reuse those small primitives, not an elaborate autonomous swarm. Separate deterministic tests from model judgment; model-based grading is an advisory signal, not an authorization boundary.
4. **Portability between coding agents is partial.** A practitioner-maintained October 26 harness comparison documents divergent support for hooks, skills, MCP and permissions across agents. Your shared `AGENTS.md` is a navigation layer; provider-specific adapters still need explicit tests. Treat this tracker as a secondary source and validate any capability you rely on against vendor docs.

## Real leverage to copy

- Coding: **issue → isolated worktree → patch → deterministic tests → independent review → PR**. Record accepted changes and human review minutes; avoid measuring lines generated.
- Personal workflows: **new document/reading → deterministic extraction → model classification or summary → source-linked output → human approval → archive**. Measure how many corrections and minutes are saved across repeated runs.
- Homelab: **alert → read-only evidence bundle → typed diagnosis → approval-gated remediation → post-action verification**. Never give a triage agent the Docker socket or production credentials.

## This month's focused agentic-coding skill: independent verification

Before implementation, write acceptance criteria that default to FAIL. After the agent writes a patch, use a **separate read-only reviewer context** plus deterministic checks. Reviewers may flag issues; they cannot bypass a failing check. Protect the evaluator's test fixtures and policy from the writer.

## General-purpose agent pattern: evidence-first document intake

Use one small folder of *your own* permitted PDFs/notes or synthetic fixtures. Produce a structured index with source path, SHA-256, title, topic, concise summary, uncertainty and next action. Require citations to input passages; store no raw private content in a public repository. A failed extraction is a typed failure, not a hallucinated summary.

## DevOps / homelab improvement

Introduce a **versioned agent-capability manifest** and a deny-by-default policy test. Include the effective tool list, network destinations, writable mounts, credential sources, and model gateway. Verify the exact same restrictions after a teardown/rebuild. Run an offline backup/restore smoke test for the *configuration*, not the media library itself.

## Exercises (priority order)

1. [Agent harness inventory + negative tests](exercises/01-harness-audit.md) — **90 min**. Inspect the actual `home-server` agent configuration; produce a permission manifest and at least three blocked-action tests.
2. [Evidence-gated coding-agent run](exercises/02-evidence-gated-pr.md) — **90 min**. Carry October's unfinished PR-loop objective forward once, with an independent reviewer and machine-readable result.
3. [Document-intake micro-automation](exercises/03-document-intake.md) — **60–75 min**. A genuinely useful personal workflow with fixture-based regression tests and reproducible execution.

## Suggested weekend

**Friday (~2 h):** Read the harness study and Anthropic example (~35 min); perform exercise 1 (~85 min).

**Saturday (~2.5 h):** Exercise 2 (~90 min); exercise 3 minimal slice (~60 min). Record actual outcome and review time (~15 min).

**Optional (~45 min):** Test rebuilding the policy configuration on the GX10 or 3090 host from a clean checkout; record drift and restore outcome.

## Definition of done

- [ ] Agent capabilities inventoried, with versions/pins and no committed secrets.
- [ ] Three negative policy tests fail as intended, with logs.
- [ ] One real coding task evaluated against checks specified before agent execution; read-only reviewer records evidence.
- [ ] One document-intake fixture produces source-linked structured output and a repeatable regression check.
- [ ] `notes/progress.md` links to real artifacts/PRs, records time saved/review minutes, and explicitly decides what to carry forward.
- [ ] Rebuild/teardown commands are documented; no claims of successful execution without evidence.

## Skip this month

Do not install a new multi-agent platform, chase model leaderboards, deploy an entire *arr stack, or build a custom vector database. Also skip autonomous remediation and automatic PR merging. Your existing hardware and subscriptions are sufficient.

**Decision rule:** if the exercises reveal no repeatable savings, simplify the workflow rather than adding more AI.
