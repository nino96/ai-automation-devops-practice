# Exercise 2 — Productize One Recurring Workflow

**Time:** 45–60 minutes

## Goal

Turn one repeated use of AI into a reusable workflow instead of another ad-hoc chat.

Choose a task you already do at least monthly, such as research triage, comparing options against fixed criteria, converting notes into a decision memo, extracting actions from documents, or turning an incident into a draft RCA.

## Step 1 — Write a workflow card

Copy `../artifacts/workflow-card.example.yaml` and define:

- trigger;
- inputs;
- allowed context;
- output contract;
- evidence requirement;
- human decision point;
- value metric;
- retained state.

The most important parts are the output contract, evidence requirement, and where a person still makes the decision.

## Step 2 — Create one fixture

Use one low-risk historical example.

Suggested structure:

```text
workflows/<name>/
  workflow.yaml
  fixtures/
    001-input.md
  expected/
    001-checklist.md
```

The checklist should describe what a useful result must contain without prescribing exact wording.

Examples:

- important claims are supported by evidence;
- rejected alternatives are named;
- uncertainty is explicit;
- the output is short enough to act on.

## Step 3 — Run it with a tool you already have

Use ChatGPT Plus or Claude Pro. Do not add API spend for this exercise.

Run from the written contract and fixture rather than improvising a fresh prompt.

Record:

- human time spent;
- corrections required;
- whether the checklist passed;
- whether the output was useful enough to act on.

## Step 4 — Make one controlled improvement

Change only one variable based on the observed failure: context, output schema, evidence rule, stop condition, instruction, or review point.

Run the same fixture again.

## Step 5 — Decide whether it deserves automation

Automate only if the workflow is recurring, valuable, stable enough to specify, and safe to run with bounded permissions.

If it is not worth repeating, archive the workflow card. Discovering that something should stay manual is a valid outcome.

## Definition of done

- [ ] One recurring workflow has a written contract.
- [ ] One fixture and quality checklist exist.
- [ ] It has been run at least once from the contract.
- [ ] One correction or failure has been recorded.
- [ ] One controlled improvement has been tested.
- [ ] You made an explicit automate / keep-manual / discard decision.

## Why this matters

Successful AI use compounds when the workflow can be refreshed, checked, improved, and handed to a different model or tool without depending on remembered chat history.
