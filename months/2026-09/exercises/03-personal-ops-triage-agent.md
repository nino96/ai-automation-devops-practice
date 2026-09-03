# Exercise 3 — Read-Only Personal Ops Triage Agent

**Time:** 60–75 minutes  
**Priority:** Medium-high

## Outcome
Build a reusable pattern for an agent that can understand home-server/local-AI operational problems while remaining strictly read-only.

```text
request or alert
  -> deterministic evidence collection
  -> LLM diagnosis
  -> typed recommendation
  -> human decision
```

The goal is diagnosis quality, not autonomous remediation.

## Step 1 — Choose one narrow incident class
Start with a safe, observable condition such as:

- model endpoint unhealthy;
- service health check failing;
- disk-space warning;
- application returning 5xx;
- backup status reporting failure;
- expected Tailscale-published path not responding.

Prefer your existing local-AI stack because it already has health, sanity, and operational status commands.

## Step 2 — Separate evidence collection from reasoning
Create a deterministic collector such as:

```bash
bin/collect-ops-context <incident-type>
```

It should output JSON and run read-only inspection commands only.

Example shape:

```json
{
  "timestamp": "...",
  "incident_type": "local_ai_unhealthy",
  "host": {
    "memory_available_gib": 0,
    "disk_free_gib": 0
  },
  "services": [],
  "health": {},
  "recent_logs": [],
  "config_metadata": {}
}
```

Redact secrets before the model sees the payload.

## Step 3 — Define the model output contract
Require structured output such as:

```json
{
  "summary": "...",
  "likely_causes": [
    {"cause": "...", "confidence": 0.0, "evidence": ["..."]}
  ],
  "recommended_checks": ["..."],
  "recommended_next_step": "...",
  "requires_human_action": true
}
```

For September, the agent does not execute changes.

## Step 4 — Build three fixtures
Do not wait for real failures. Save sanitized fixture payloads:

1. healthy baseline;
2. obvious failure;
3. ambiguous/noisy failure.

Example assertions:

```yaml
healthy:
  requires_human_action: false
obvious_memory_pressure:
  must_reference: memory
ambiguous:
  max_confidence: 0.75
  requires_followup_checks: true
```

These become the beginning of your Ops-agent eval set.

## Step 5 — Route through your model abstraction
The reasoning call should target your stable LiteLLM/OpenAI-compatible interface, not a specific model implementation.

Later you can compare local and cloud models using exactly the same fixtures and output schema.

## Step 6 — Preserve an audit artifact
Each run should save or emit:

```text
input fixture/context hash
model/provider identifier
prompt/policy version
structured result
validation result
latency
```

Do not store secrets or large raw logs by default.

## Definition of done
- [ ] One incident class is supported.
- [ ] Evidence collection is deterministic and read-only.
- [ ] Model output is schema-constrained and validated.
- [ ] Three sanitized fixtures exist.
- [ ] At least one assertion exists per fixture.
- [ ] The agent performs no mutations.
- [ ] The workflow can be pointed at local or cloud models through configuration.

## Future promotion path
Only after repeated evaluation shows the diagnostic loop is useful should a later month explore narrowly scoped, separately implemented actions with explicit approval and post-action verification.
