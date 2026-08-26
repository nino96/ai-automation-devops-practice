# AGENTS.md

## Purpose
This repository is the durable curriculum and progress tracker for monthly AI, automation, agentic-coding, DevOps, and home-lab practice.

## Operating rules
- Read `README.md`, `ROADMAP.md`, `progress.md`, and prior month folders before adding a new month.
- Never overwrite completed historical months unless fixing a factual error or broken link.
- Prefer continuing high-value unfinished work over endlessly adding new projects.
- Keep each month realistic: roughly 4–6 hours of core work, with optional stretch tasks.
- Prefer primary sources, strong technical write-ups, credible evals, and useful repositories.
- Avoid generic AI news roundups, shallow launch summaries, tool-list spam, and hype without evidence.
- Reproducibility is mandatory: prefer version-controlled configs, containers, scripts, IaC, tests/evals, observability, backup/restore, and rebuild instructions.
- For larger implementation projects, this repo should remain the curriculum/control plane and link to the dedicated implementation repo or PR.

## Monthly structure
Use:

```text
months/YYYY-MM/
  README.md
  resources.md
  exercises/
  notes/
  artifacts/
```

Each monthly `README.md` should include:
- theme
- why this month matters
- 3–5 high-value concepts/workflows
- focused agentic-coding skill
- general-purpose automation pattern
- DevOps/reproducibility improvement
- home-lab/self-hosted direction when useful
- skip-this-month section
- 4–6 hour suggested schedule
- definition of done

`resources.md` should list only the sources worth consuming, with estimated time and a one-line learning goal.

`exercises/` should contain concrete, reproducible instructions and expected outputs.

## Change workflow
Prefer creating a branch and pull request for each monthly update. The PR description should summarize what was added, what continues from prior months, and any deferred items.
