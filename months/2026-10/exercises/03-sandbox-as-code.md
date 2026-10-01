# Exercise 3 — Make the GX10 Agent Sandbox Rebuildable

**Time:** 75–90 minutes  
**Carry-forward:** this is the highest-value unfinished September item.

## Goal

Take one local agent sandbox on the GX10 and make its boundary **declarative, reviewable, and rebuildable**.

Use NemoClaw/OpenShell if it fits cleanly. If your existing setup already enforces equivalent boundaries another way, keep that implementation and produce the same artifacts.

## Required outputs

Keep:

- the pinned tool/runtime version;
- a secret-free exported or hand-authored configuration;
- explicit allowed/denied capabilities;
- three negative policy tests;
- rebuild/restore notes;
- one smoke test after rebuild.

Do not commit credentials.

## Step 1 — Pick one narrow agent

Use one agent you actually expect to keep using, not a demo-only sandbox.

Good target:

- coding/repository agent with one project directory;
- local model through the existing LiteLLM/OpenAI-compatible gateway;
- no unrestricted Docker socket;
- no access to unrelated home directories or secret files.

## Step 2 — Pin the implementation

Record:

```text
NemoClaw/OpenShell version
agent/runtime version
local model route or logical alias
mounts
network policy
MCP servers/tools
credential references
```

Avoid `latest` where an immutable version is available.

## Step 3 — Express the policy

Default to the smallest useful surface.

Examples of policy intent:

- workspace is read/write;
- system paths are read-only or unavailable;
- network is deny-by-default with only required endpoints allowed;
- unnecessary MCP tools are denied;
- credentials are referenced by name rather than embedded in committed config.

NemoClaw's September releases added managed MCP tool-denial rules and broader canonical configuration export for supported sandboxes. Use those capabilities if your chosen version supports them.

## Step 4 — Export and inspect

Export the sandbox configuration, or generate an equivalent canonical file from your chosen tool.

Before committing it:

- inspect for secrets;
- inspect for machine-specific paths you do not want to preserve;
- verify that the exported policy matches the intended live boundary;
- document any fields that must be supplied during restore.

Commit the sanitized configuration in the implementation repo, not in this curriculum repo unless it is only an example.

## Step 5 — Write three negative tests

Prove the boundary by attempting actions that should fail.

At minimum test three categories:

1. read a forbidden file/path;
2. write outside the allowed workspace;
3. use a denied tool or contact a disallowed network destination.

Record expected and observed outcomes.

A policy is not proven because the YAML looks restrictive.

## Step 6 — Rebuild once

Destroy/recreate or otherwise restore the sandbox from the versioned configuration according to the chosen tool's supported workflow.

After rebuild:

- run a positive smoke task;
- rerun the three negative checks;
- verify the local model route still works;
- compare exported state to the committed configuration where practical.

## Definition of done

- [ ] Tool/runtime versions are pinned.
- [ ] Secret-free configuration is version-controlled.
- [ ] Three negative tests fail as intended.
- [ ] One positive smoke task succeeds.
- [ ] The sandbox was rebuilt/restored from versioned configuration.
- [ ] Rebuild notes identify any unavoidable manual secret/bootstrap step.

## Do not expand scope

Do not add messaging channels, many MCP servers, or a general-purpose autonomous home agent during this exercise.

First earn confidence that one sandbox can be reproduced and that its denied capabilities remain denied after rebuild.
