# Exercise 2 — Parallel Coding-Agent Migration Eval

**Time:** 75–90 minutes  
**Priority:** High

## Outcome
Measure whether decomposing one verifiable migration across multiple coding agents creates real leverage **after accounting for review/merge effort**.

This is inspired by Asana's August 2026 Enzyme-removal case, but deliberately scaled to a personal experiment.

## Pick the right task
Choose a real or fixture migration with a binary-ish end state. Examples:

- replace one deprecated API across several independent modules;
- migrate a test library or assertion style;
- replace a logging/configuration pattern;
- update several independent service modules to a new helper/interface;
- remove a legacy dependency and convert its call sites.

Avoid subjective refactors.

A good task can be expressed like:

```yaml
objective: Remove deprecated Foo API
acceptance:
  - forbidden_symbol_count == 0
  - tests_pass == true
  - build_passes == true
  - no_public_api_break == true
```

## Experimental design
Run two approaches against equivalent clean starting points.

### A — Single-agent execution
One agent receives the full migration objective and acceptance contract.

Record:

```text
agent runtime
human prompt/setup time
human review time
number of corrections
checks passed first try
final accepted? yes/no
```

### B — Parallel/decomposed execution
Split only on natural boundaries, for example:

```text
agent 1 -> module A/B
agent 2 -> module C/D
agent 3 -> tests/docs or module E/F
```

Each agent gets:

- the same global objective;
- its explicit scope;
- the same relevant constraints;
- deterministic checks for its scope;
- an instruction not to edit outside its ownership unless it reports why.

Use separate branches/worktrees/cloud tasks so agents do not trample each other.

## Planning rule
For both A and B, require a short plan before edits.

The plan should identify:

1. expected files/modules;
2. migration pattern;
3. likely edge cases;
4. validation commands.

Do not spend time making the prompt elaborate. Treat it like a good GitHub issue.

## Minimal result schema
Save results as YAML/JSON/CSV under this month's `artifacts/` or your long-lived eval repo.

Example:

```yaml
experiment: deprecated-api-migration
single_agent:
  wall_clock_minutes: 42
  human_setup_minutes: 5
  human_review_minutes: 14
  corrections: 2
  acceptance_passed: true
parallel_agents:
  agents: 3
  wall_clock_minutes: 24
  human_setup_minutes: 9
  human_review_minutes: 27
  merge_conflicts: 1
  corrections: 3
  acceptance_passed: true
```

## Metrics that matter
Do **not** optimize only for wall-clock agent time.

Calculate at least:

```text
human_minutes = setup + review + corrections/merge work
accepted_per_human_hour = accepted_result / human_minutes
```

Also note:

- duplicate work;
- conflicting architectural choices;
- changes outside scope;
- whether parallel execution made review cognitively harder.

## Optional model/tool comparison
If convenient, use the tools you already pay for rather than adding subscriptions. Examples:

- Claude Code / Claude Pro
- Codex through ChatGPT Plus
- GitHub Copilot coding agent / CLI where your plan supports it
- local agent through the GX10 endpoint

Do **not** turn this into a leaderboard. The primary variable is single vs parallel/decomposed execution.

## Definition of done
- [ ] One migration has deterministic acceptance checks.
- [ ] Single-agent baseline is recorded.
- [ ] Parallel/decomposed run is recorded.
- [ ] Human review time is measured for both.
- [ ] Merge/conflict/correction overhead is recorded.
- [ ] You write one paragraph answering: **When did parallelism help, and when did it merely multiply review work?**

## What to learn
The frontier workflow is not "launch as many agents as possible."

It is:

> Find work with a crisp end state, partition it along real boundaries, let agents execute independently, and preserve a cheap verification/merge path.
