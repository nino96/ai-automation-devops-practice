# Exercise 1 — Build the reusable `ai-lab` repository

**Time budget:** ~90 minutes  
**Priority:** Highest

## Objective
Create a reusable project skeleton that future AI experiments can inherit instead of starting from an empty folder every month.

## Suggested structure

```text
ai-lab/
├── AGENTS.md
├── ARCHITECTURE.md
├── README.md
├── compose.yaml
├── .env.example
├── Makefile
├── docs/
│   ├── index.md
│   ├── ADR/
│   └── runbooks/
├── infrastructure/
├── services/
├── evals/
├── experiments/
└── .github/workflows/
```

## Steps
1. Create the repository and initial directory structure.
2. Keep `AGENTS.md` short: build/test commands, definition of done, safety constraints, and pointers to deeper docs.
3. Add `ARCHITECTURE.md` describing the intended local/cloud model boundary and how experiments should consume model APIs.
4. Add a `Makefile` with placeholders or working commands for:
   - `make bootstrap`
   - `make up`
   - `make down`
   - `make test`
   - `make eval`
   - `make lint`
5. Add `.env.example`; do not commit real secrets.
6. Add a minimal CI workflow that at least validates Markdown/YAML or runs a smoke test.
7. Document rebuild assumptions in `README.md`.

## Reproducibility contract
A clean Linux host should be able to clone the repo, inject documented secrets/config, and reach the same working state without relying on undocumented GUI actions.

## Definition of done
- [ ] Repository exists in GitHub.
- [ ] `AGENTS.md` is concise and links to deeper context.
- [ ] Standard Make targets exist.
- [ ] Secret/config expectations are documented.
- [ ] A CI workflow runs successfully.
- [ ] Rebuild steps are documented.

## Artifact to link back here
Add the implementation repo/PR URL to `../notes/progress.md` when complete.
