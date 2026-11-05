# Exercise 1 — Audit agent capabilities and prove denials (90 min)

**Target:** `nino96/home-server` agent configuration; do not copy its secrets into this public curriculum repo.

1. In a disposable checkout, inventory `AGENTS.md`, agent-specific instruction/config files, skills, hooks, MCP servers and enabled tool grants. Record version/pin, executable path, source URL, write mounts, network access, credential source and approval mode. Do not print secret values.
2. Create a local `agent-capabilities.yaml` using the example in [../artifacts/capabilities.example.yaml](../artifacts/capabilities.example.yaml). Replace examples with actual observed values. **Do not claim this template is a live policy.**
3. Write three *negative* checks using the real runtime's supported sandbox controls: (a) denied secret-file read, (b) denied write to a read-only fixture, (c) denied outbound connection to a controlled disallowed destination. Use synthetic secrets and a disposable container/VM. Never run a deliberately unsafe test on the production host.
4. Record expected vs observed exit codes and logs in an ignored local evidence directory. A prompt saying “do not” is not a passing enforcement test.
5. Pin mutable dependencies where feasible, and document any unavoidable dynamic provider configuration.

**Pass:** all three operations are blocked by OS/container/runtime policy; independent checks can verify it. If a control is missing, mark FAIL and describe the minimal fix.

**Rebuild:** checkout at recorded commit, install pinned dependencies, apply config, repeat the three negative tests, compare effective capabilities. **Teardown:** destroy disposable workspace/containers; ensure no production secrets were mounted.
