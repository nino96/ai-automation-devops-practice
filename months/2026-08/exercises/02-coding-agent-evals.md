# Exercise 2 — Build the first coding-agent eval set

**Time budget:** ~90 minutes  
**Priority:** High

## Objective
Replace subjective impressions like "agent A felt better" with a tiny but repeatable evaluation harness.

## Create five tasks
Use a small repository and define tasks such as:

1. Add validation to an endpoint.
2. Fix a deliberately seeded bug.
3. Add a small endpoint following an existing pattern.
4. Improve test coverage without changing behavior.
5. Diagnose and fix a deliberately broken CI workflow.

Each task should have explicit acceptance criteria, for example:

```yaml
id: fix-null-validation
acceptance:
  commands:
    - ./gradlew test
  assertions:
    - no changes outside src/ and tests/
    - existing public API remains compatible
  max_iterations: 10
```

## Compare two setups
Run the same five tasks with two coding-agent setups you already have access to, for example Claude Code and GitHub Copilot CLI/chat.

Capture at minimum:

```text
task success
acceptance checks passed
human corrections required
elapsed time
number of attempts/iterations
tokens/cost when exposed by the tool
notable failure mode
```

## Important rule
Do not optimize prompts after every individual task. Run a baseline first. Then make one harness/prompt/context change and rerun so the comparison means something.

## Turn failures into regression cases
If an agent makes an interesting mistake, preserve that case. The long-term goal is a growing regression suite rather than five disposable demos.

## Definition of done
- [ ] Five tasks exist with deterministic acceptance checks.
- [ ] Two agent setups have attempted the same task set.
- [ ] Results are stored in a version-controlled file (CSV/JSON/Markdown is fine).
- [ ] At least one failure is retained as a regression case.
- [ ] One concrete harness/context improvement is identified from the results.

## Artifact to link back here
Add the eval repo/path and results summary to `../notes/progress.md` when complete.
