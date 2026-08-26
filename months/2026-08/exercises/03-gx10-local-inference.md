# Exercise 3 — Turn GX10 inference into a reproducible service

**Time budget:** 60–90 minutes  
**Priority:** High

## Objective
Stop treating local model serving as a one-off shell command. Package the current GX10 setup as a stable, rebuildable OpenAI-compatible service.

## Target architecture

```text
clients
  ↓
OpenAI-compatible API
  ↓
vLLM (current baseline)
  ↓
local model on ASUS Ascent GX10
```

The application-facing contract should remain stable even when the model, quantization, or inference engine changes later.

## Capture configuration explicitly
Version-control all non-secret configuration needed to reproduce the service, including:

- container/image version
- exact model identifier
- quantization/precision
- context length
- vLLM startup flags
- GPU memory utilization
- exposed port
- restart policy
- model/cache volume locations

Use an `.env.example` for values that vary by machine. Do not commit credentials or tokens.

## Minimum service contract
Provide commands such as:

```bash
make local-ai-up
make local-ai-down
make local-ai-logs
make local-ai-smoke-test
```

The smoke test should:

1. query `/v1/models` or equivalent;
2. send one small inference request;
3. fail non-zero when the service is unavailable or returns an invalid response.

## Reliability
- Configure automatic restart where appropriate.
- Document model download/cache behavior.
- Document expected RAM/storage usage.
- Document how to change to another model without changing client configuration.
- Record any ARM64/GX10-specific container constraints discovered during setup.

## Optional extension
Put LiteLLM or another gateway in front only if it solves a real routing need now. Do not add a gateway merely for architecture aesthetics.

## Definition of done
- [ ] Service configuration is version controlled.
- [ ] A clean restart/redeploy works without reconstructing the command from shell history.
- [ ] `/v1/models` responds after deployment.
- [ ] A scripted inference smoke test passes.
- [ ] Model replacement procedure is documented.
- [ ] Secrets, if any, are external to Git.

## Artifact to link back here
Add the implementation repo/PR and the tested model/engine configuration to `../notes/progress.md` when complete.
