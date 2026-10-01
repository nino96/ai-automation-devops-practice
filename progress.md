# Progress

| Month | Theme | Status | Core deliverables |
|---|---|---|---|
| 2026-08 | Build the harness, not another demo | In progress | GX10 serving is evidenced in `nino96/dgx-spark-ai-stack`; the separate ai-lab and coding-agent eval work is not confirmed complete |
| 2026-09 | Bounded autonomy | In progress | `nino96/home-server` evidences portable agent configuration, worker routing, and OpenCode Web setup; the remaining September exercises are not confirmed complete |
| 2026-10 | Close the loop | Proposed | agent-to-PR control loop, one reusable recurring AI workflow, and a rebuildable local-agent sandbox |

## Evidence notes

August: `nino96/dgx-spark-ai-stack` provides a version-controlled local inference platform with gateway, model lifecycle, health checks, recovery guidance, monitoring, and eval scaffolding.

September: `nino96/home-server` now contains portable Codex, OpenCode, Claude Code, and Copilot configuration, worker-routing guidance, and a reproducible OpenCode Web service. This is useful progress, but it does not demonstrate completion of every September exercise.

October carries forward the local sandbox/rebuild objective and uses the existing agent setup for a real pull-request workflow rather than adding another agent interface.

## Status meanings

- **Proposed** — curriculum added, work not yet confirmed complete
- **In progress** — some core exercises completed or independently evidenced
- **Completed** — monthly definition of done satisfied
- **Deferred** — intentionally carried forward
- **Superseded** — replaced by a better approach
