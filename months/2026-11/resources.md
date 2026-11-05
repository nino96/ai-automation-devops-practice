# November 2026 — Curated resources

Read in order; **~60–80 minutes total**. Dates refer to publication/disclosure where known. Some research predates October but is included because it directly supports this month's exercises.

1. **Scanning the Harness** — empirical study of 3,171 public coding-agent configurations (September 7, 2026), **20 min**. Extract three concrete configuration checks to apply to `home-server`. https://arxiv.org/abs/2609.07360
2. **Anthropic: Harness Primitives for Long-Running Claude Agents** — primary-source code examples, **15 min**. Understand default-fail criteria, independent evaluator, and handoff. These are examples, not a maintained production package. https://github.com/anthropics/cwc-long-running-agents
3. **Reuters: OpenAI agents weakened internal monitoring** (October 21, 2026), **10 min**. Identify a guardrail that must remain outside an agent's write access. https://www.reuters.com/legal/litigation/openai-says-its-ai-agents-weakened-internal-monitoring-system-2026-10-21/
4. **Agent Harness Tracker** (reviewed October 26, 2026), **10 min**. Identify portability assumptions in Codex/Claude/Copilot/OpenCode configuration; verify critical ones with official docs. https://paulochang.com/harness/
5. **ActGuard: Pre-execution Action Auditing** (September 14, 2026), **optional 20 min**. Research pattern: compare proposed actions to expected tool behavior before execution. This is research evidence, not a substitute for hard allowlists. https://arxiv.org/abs/2609.14987

## Evidence hierarchy

The study is empirical but sample-limited. The Anthropic repository is a transparent example of mechanisms. Reuters reports specific disclosed incidents. The comparison tracker is community-maintained. The ActGuard paper is a research proposal, not a production security guarantee.

## Useful official implementation references

- GitHub Actions security hardening: https://docs.github.com/en/actions/security-for-github-actions/security-guides/security-hardening-for-github-actions
- Docker Compose specification: https://docs.docker.com/compose/compose-file/
- Git worktree reference: https://git-scm.com/docs/git-worktree
- OpenTelemetry documentation: https://opentelemetry.io/docs/
