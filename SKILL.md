---
name: codebase-onboarding-check
description: Onboard quickly by reading design docs, plans, READMEs, or the codebase to extract key concepts, surface gaps, and generate comprehension checks with answer keys. Use when a user wants help understanding a project, validating their grasp of the architecture, or creating a readiness plan before contributing code.
---

# Codebase Onboarding Check

## Overview
Help a user prove they understand a project before contributing. Read design docs, plans, READMEs, and relevant code paths; extract the mental model, vocabulary, invariants, and data contracts; highlight gaps or drift; and produce comprehension checks with answer keys plus a short onboarding plan.

## Quick Start
- Collect artifacts: paths to design docs/PRDs/ADR/README, key modules, and recent architecture diagrams if available.
- Skim docs first, then anchor in code (entrypoints, services, critical modules). Use `rg` for fast discovery (e.g., `rg --files -g"*design*"`, `rg "<term>" src`).
- Build a one-page context: goals, constraints, primary flows, schemas/contracts, external dependencies, and invariants.
- Generate comprehension checks (use templates in `references/comprehension-checklists.md`) and answer them from the sources. Flag anything you cannot answer as knowledge gaps.
- Deliver a readiness report: what’s understood, open questions, suspected drift between doc and code, and 2–3 starter tasks/tests to build confidence.

## Workflow
### 1) Collect inputs
- Ask for or locate: design/plan docs, ADRs, README/setup, key service/module paths, and any recent changes (PRs, migration notes).
- If artifacts are missing, state assumptions and list what would reduce risk.

### 2) Digest the doc
- Extract: purpose/goal, scope & non-goals, success metrics, constraints, data/contracts, sequencing, and failure/rollback strategy.
- Build a glossary of domain terms and component names. Note unresolved questions or TODOs the doc mentions.

### 3) Map doc to code
- Identify runtime entrypoints (CLI, HTTP handlers, jobs). Map each major doc concept to concrete files/functions.
- Check drift: is behavior implemented? Any divergences, feature flags, or legacy paths? Record evidence (file+line references).
- Trace one happy path + one failure path end-to-end (inputs → output, persistence, external calls).

### 4) Comprehension checks
- Create 6–10 questions spanning: concept recall, data/flow mapping, edge/failure handling, dependency/infra, change-scenario (“if we add X, what breaks?”), and code-level understanding.
- Provide brief answer keys using doc/code citations; mark unknowns explicitly. Use the templates/rubric in `references/comprehension-checklists.md` to ensure coverage.

### 5) Readiness plan
- Recommend a reading order (doc sections → code modules → tests).
- Propose 2–3 safe starter tasks (small refactor, add log/metric, add a guard/test) tied to the concepts above.
- List verification steps (tests to run, fixtures/data needed). Encourage the user to attempt the questions first, then compare to the answer key.

## Outputs to return
- Concise project summary (goal, constraints, primary flow, invariants).
- Glossary of terms/components.
- Gap list: unanswered questions, suspected doc/code drift, missing diagrams/contracts.
- Comprehension check set with answer key and what is still unknown.
- Onboarding plan with next reads and 2–3 practice tasks/tests.

## References
- Load `references/comprehension-checklists.md` for ready-made question stems, coverage rubric, and quick drift/gap checklist when crafting the comprehension checks.
