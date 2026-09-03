# September Artifacts

Store only small, durable evidence here. Put substantial implementation code in its natural long-lived repository and link it from `../notes/progress.md`.

Recommended artifacts:

```text
policy-test-results.json
parallel-migration-results.yaml
ops-fixture-healthy.json
ops-fixture-failure.json
ops-fixture-ambiguous.json
ops-eval-results.json
```

Do not commit:

- secrets, tokens, cookies, credentials or unredacted environment files;
- raw personal data;
- large logs/model outputs that add no durable learning value;
- model weights or generated caches.

For each experiment, capture enough metadata to reproduce the result:

```yaml
date: YYYY-MM-DD
implementation_repo: owner/repo
commit: <sha>
model_or_provider: <identifier>
prompt_or_policy_version: <identifier>
acceptance_or_eval_version: <identifier>
result: <summary>
```
