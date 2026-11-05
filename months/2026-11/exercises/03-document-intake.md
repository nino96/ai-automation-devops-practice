# Exercise 3 — Reproducible personal document intake (60–75 min)

**Goal:** turn a real recurring reading/filing chore into a reusable, privacy-aware automation. Begin with two synthetic Markdown/text fixtures; add private PDFs later if useful.

1. Build a CLI accepting `--input-dir`, `--output-jsonl`, `--model-endpoint`. Use a local GX10 OpenAI-compatible endpoint if available; otherwise a manual model step with a deterministic JSON schema. Keep authentication in environment variables.
2. Deterministically compute SHA-256 and extract text before the model step. Never send sensitive personal documents to an external API by default.
3. Ask the model for a typed record: `source_file,sha256,title,topics,summary,evidence_quotes,action,uncertainty`. Verify quotes occur in the extracted source; reject unsupported claims. Use source-relative paths, not full home-directory paths.
4. Write three fixture tests: normal document, missing/empty document, and injected instructions inside the document that attempt to alter the output or execute a tool. The content is **data**, never a command.
5. Run twice and compare outputs/latency/corrections. Record whether the workflow saves time. Avoid logging raw document text in public CI.

**Artifacts:** CLI script in a dedicated implementation repo (or under this month's artifacts if genuinely tiny), schema, two synthetic fixtures, tests, lockfile/pinned dependencies, Make target and a short runbook.

**Pass:** deterministic metadata, validated evidence, fail-closed extraction errors, no tool execution from document content, and an honest time-saved estimate.

**Teardown/rebuild:** remove generated JSONL/cache, recreate virtualenv/container from pinned dependencies, run fixture tests, regenerate output. Never commit private input documents.
