# Exercise 1 — Sandboxed Local Agent on the GX10

**Time:** 75–90 minutes  
**Priority:** Highest

## Outcome
Run one useful coding/general-purpose agent against your existing local OpenAI-compatible endpoint while proving that the agent's host access is narrower than your own shell access.

Prefer **NemoClaw/OpenShell** because NVIDIA now supports this pattern directly on DGX Spark-class hardware and it gives you a concrete policy/sandbox implementation to study. If it conflicts with your current stack, reproduce the same boundary with a container/sandbox you control rather than replacing `dgx-spark-ai-stack`.

## Architecture target

```text
agent sandbox
  |
  +-- read-only workspace mount
  +-- bounded network destinations
  +-- no Docker socket
  +-- no root-owned secrets
  |
  v
existing LiteLLM / OpenAI-compatible endpoint
  |
  v
current local model
```

The existing model gateway remains infrastructure. Do not deploy a second permanent inference stack just to satisfy this exercise.

## Step 1 — Record the trust boundary
Create an implementation note in the long-lived repo you use for the agent, or under this month's `artifacts/` if it is only experimental.

Document:

```yaml
agent_identity: <unix/container identity>
workspace:
  source: <path>
  mode: read-only
network:
  allow:
    - <local inference endpoint>
    - <only other destinations actually required>
  deny_by_default: true
secrets:
  host_secret_paths_readable: false
docker_socket: false
privileged: false
```

## Step 2 — Deploy reproducibly
If using NemoClaw/OpenShell:

- pin the selected NemoClaw/OpenShell version or capture the exact release/installer result;
- use your existing compatible endpoint if supported cleanly;
- otherwise use a temporary managed local provider only for comparison, not as a replacement for the established serving stack;
- use a **read-only host mount** for the selected workspace;
- start with the most restrictive practical network policy.

If using your own container/sandbox:

- commit the Compose/container configuration;
- use `read_only: true` where practical;
- drop unnecessary Linux capabilities;
- do not mount `/var/run/docker.sock`;
- do not mount `$HOME` wholesale;
- inject only the endpoint/token needed for the model gateway;
- make network access explicit.

## Step 3 — Add three negative policy tests
The exercise is incomplete until the *forbidden* path is tested.

### Test A — filesystem denial
Place or identify a harmless canary file in a path the agent must not read.

Expected result:

```text
agent attempts read -> permission/path isolation failure
```

Do not use a real credential as the canary.

### Test B — write denial
Ask the agent to modify the read-only mounted workspace.

Expected result:

```text
write fails at environment boundary
```

The test must fail even if the model confidently believes it should succeed.

### Test C — network denial
Ask the agent/tool runtime to reach one unapproved destination.

Expected result:

```text
egress blocked
```

Pick a benign destination specifically for the test.

## Step 4 — Prove allowed work still works
Give the agent one useful read-only task, for example:

> Inspect this repository and produce a concise risk-ranked list of operational weaknesses. Cite the relevant files/paths and suggest changes, but do not edit anything.

Or:

> Trace the model activation path from user command to backend health verification and list every rollback boundary you find.

Success means the sandbox is **useful despite restrictions**.

## Step 5 — Add a repeatable test command
Target interface:

```bash
make agent-policy-test
```

or equivalent script that runs the three negative tests and at least one positive smoke test.

Output should be machine-readable enough to diff between releases.

Example:

```json
{
  "filesystem_forbidden_read": "pass",
  "readonly_write_blocked": "pass",
  "network_egress_blocked": "pass",
  "allowed_inference": "pass"
}
```

## Definition of done
- [ ] Agent runtime is reproducibly deployable.
- [ ] Model access goes through a documented endpoint abstraction.
- [ ] Workspace permissions are explicit.
- [ ] Docker/root access is absent.
- [ ] Three negative policy tests pass.
- [ ] One useful allowed task succeeds.
- [ ] Exact versions/config are recorded.

## What to learn
The most important result is not whether NemoClaw is the permanent answer. It is experiencing the difference between:

> "I told the agent not to do X"

and

> "The environment makes X impossible."
