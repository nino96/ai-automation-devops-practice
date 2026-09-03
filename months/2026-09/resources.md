# September 2026 Resources

Keep total reading/watching to roughly **90 minutes**. The goal is to extract patterns you can immediately implement, not to stay current on every AI release.

| Priority | Resource | Time | Why it is worth consuming | Learning goal |
|---|---|---:|---|---|
| 1 | [Asana cleared 5 years of engineering work in 2 weeks with Codex — OpenAI, Aug 18 2026](https://openai.com/index/asana/) | 15 min | Concrete large-scale migration case: up to four coding agents in parallel, separate codebase copies, human review, explicit cost comparison. Treat the vendor-authored case study as evidence of one implementation, not a universal productivity guarantee. | Identify what made this work *delegatable*: repetitive migration objective, separable work, reviewable patches, and clear end state. |
| 2 | [How we contain Claude across products — Anthropic](https://www.anthropic.com/engineering/how-we-contain-claude) | 25 min | Strong engineering discussion of sandboxing, filesystem/network boundaries, permissions, and why model-layer intent checks are insufficient. | Translate containment concepts into three enforceable policy tests on your GX10 agent. |
| 3 | [Improving our alignment and security practices — Anthropic, Aug 31 2026](https://www.anthropic.com/news/improving-alignment-security-efforts) | 10 min | Recent reminder that real tool access plus environment/configuration mistakes can produce unintended real-world actions. | Internalize that the environment is part of agent correctness, not merely a deployment detail. |
| 4 | [Run NemoClaw with a Local LLM — NVIDIA DGX Spark](https://build.nvidia.com/spark/nemoclaw/instructions) plus [Aug 12 release notes](https://docs.nvidia.com/nemoclaw/latest/user-guide/deepagents/release-notes/2026/8/12) | 20–25 min | Directly matches the GX10. Current instructions cover local vLLM, OpenShell sandboxing and supported agents; Aug 12 added persistent read-only host mounts. | Decide whether to use NemoClaw/OpenShell as the sandbox implementation or reproduce the same boundary with your own container policy. |
| 5 | [How OpenAI uses Codex](https://openai.com/business/guides-and-resources/how-openai-uses-codex/) | 15–20 min skim | Practical internal workflows: plan-first on larger changes, issue-like task specs, environment tuning, background task queues, persistent repository context. | Pick one work item you normally postpone and rewrite it as an acceptance-driven delegated task. |
| 6 | [Introducing the Admin plugin for ChatGPT Work and Codex — OpenAI, Aug 25 2026](https://openai.com/index/introducing-admin-plugin/) | 10 min optional | The useful part is the internal IT pattern: collect context, apply existing permissions/policies, perform supported actions, escalate exceptions; OpenAI reported ~45% of ticket volume resolved by deployed workflows at the time of reporting. | Steal the architecture for a personal/home-server Ops agent without needing the product itself. |

## Recent but lower-priority awareness

### GitHub Copilot / VS Code August release
[GitHub Copilot in VS Code, August 2026 releases](https://github.blog/changelog/2026-08-31-github-copilot-in-vs-code-august-2026-releases/) improves agent-session organization, review/navigation, integrated browser workflows, and related agent-host ergonomics.

**Why it is not core study:** useful interface improvements, but they do not change the engineering principles you should practice this month.

### NVIDIA NemoClaw August iteration
NemoClaw shipped frequently through August. Relevant additions included DGX Spark managed vLLM choices, authenticated local provider handling, stronger readiness/recovery, read-only host mounts, policy improvements, and experimental new model profiles.

Useful release notes:
- [Aug 4](https://docs.nvidia.com/nemoclaw/user-guide/deepagents/release-notes/2026/8/4)
- [Aug 6](https://docs.nvidia.com/nemoclaw/user-guide/deepagents/release-notes/2026/8/6)
- [Aug 10](https://docs.nvidia.com/nemoclaw/user-guide/deepagents/release-notes/2026/8/10)
- [Aug 12](https://docs.nvidia.com/nemoclaw/latest/user-guide/deepagents/release-notes/2026/8/12)
- [Aug 17](https://docs.nvidia.com/nemoclaw/user-guide/deepagents/release-notes/2026/8/17)

**Rule:** pin one known-good release if you adopt it. Do not turn this month into continuous NemoClaw upgrade testing.

## Practitioner evidence to remember

### What looks durable
- Large, mechanically verifiable migrations are becoming plausible agent work even when their *human* estimate is very large.
- Human judgment moves toward specifying outcomes, reviewing plans/diffs, and deciding what is safe to delegate.
- Capability without containment is not a mature agent deployment pattern.
- Local agents are most useful when the model endpoint, sandbox, permissions, evals, and logs are separately replaceable components.

### What remains anecdotal/vendor-reported
- Exact productivity multipliers and dollar savings in vendor case studies.
- Claims that a particular model/framework is universally better for autonomous work.
- Multi-agent scaling claims that do not report human review/merge overhead.

Use these sources to design experiments; do not copy their headline numbers into expectations for your own work.
