# August 2026 Resources

Keep reading secondary to practice. Core reading/watch time should stay under roughly 2 hours.

| Priority | Resource | Est. time | Learning goal |
|---|---|---:|---|
| 1 | OpenAI — How OpenAI uses Codex: https://openai.com/business/guides-and-resources/how-openai-uses-codex/ | 20 min | Extract concrete production coding-agent workflow patterns: issue quality, planning, repository context, migrations, testing, and incident support. |
| 2 | OpenAI — Harness engineering: https://openai.com/index/harness-engineering/ | 25 min | Understand why repository structure, environment design, CI, observability, and durable context can matter as much as the underlying model. |
| 3 | Anthropic — Claude Code advanced patterns: https://www.anthropic.com/webinars/claude-code-advanced-patterns | 30–45 min | Learn the separate roles of subagents, hooks, and MCP instead of treating every extension mechanism as interchangeable. |
| 4 | LangChain — Agent evals: https://www.langchain.com/resources/agent-evals | 15 min | Understand outcome, trajectory, and state evaluation rather than grading only the final answer. |
| 5 | NVIDIA DGX Spark playbooks: https://build.nvidia.com/spark | 15 min browsing | Identify a reproducible local-serving pattern suitable for the GX10 and compare it with the current vLLM setup. |

## Practitioner takeaways to retain

### Harness > giant prompt
A concise repository navigation layer plus structured documentation is more maintainable than continuously appending instructions to one giant context file.

### Evals must become regression tests
When an agent fails in an interesting way, capture the failure as a reusable test case before changing the model, prompt, or harness.

### Local inference should be an API contract
Keep downstream tools coupled to a stable API, not to a specific local model or inference engine.

### Deterministic controls stay deterministic
Use hooks, CI checks, permissions, timeouts, schemas, and policy code for guarantees; reserve LLM judgment for ambiguity.
