---
name: codebase-onboarding-check
description: Onboard quickly by reading design docs, plans, READMEs, or the codebase to extract key concepts, surface gaps, and generate comprehension checks with answer keys. Includes an adaptive proficiency probe with learning resources and persistent session memory so the user can resume onboarding where they left off. Use when a user wants help understanding a project, validating their grasp of the architecture, or creating a readiness plan before contributing code.
license: MIT
metadata:
  author: agent-hellboy
  version: "1.0.0"
---

# Codebase Onboarding Check

## Overview
Help a user prove they understand a project before contributing. Read design docs, plans, READMEs, and relevant code paths; extract the mental model, vocabulary, invariants, and data contracts; highlight gaps or drift; and produce comprehension checks with answer keys plus a short onboarding plan.

Add an adaptive gate up front: probe the user’s proficiency, and if they are not ready, deliver a lightweight learning kit (notes, 1–2 articles, 1 video) before advancing to deeper checks. Persist progress so the next session can resume the test where it left off.

## Quick Start
- Collect artifacts: paths to design docs/PRDs/ADR/README, key modules, and recent architecture diagrams if available.
- Run a 3-question proficiency probe about the project/domain (scope, primary flow, key invariant). If answers are thin, jump to the learning kit step below before deeper checks.
- Skim docs first, then anchor in code (entrypoints, services, critical modules). Use `rg` for fast discovery (e.g., `rg --files -g"*design*"`, `rg "<term>" src`).
- Build a one-page context: goals, constraints, primary flows, schemas/contracts, external dependencies, and invariants.
- Generate comprehension checks (use templates in `references/comprehension-checklists.md`) and answer them from the sources. Flag anything you cannot answer as knowledge gaps.
- Deliver a readiness report: what’s understood, open questions, suspected drift between doc and code, and 2–3 starter tasks/tests to build confidence.
- Log the session to the memory file so the next session can pick up at the last unanswered check. Path selection (first writable wins):  
  1) `${AGENT_STATE_HOME}/codebase-onboarding-check/state.jsonl` if `AGENT_STATE_HOME` is set.  
  2) `${XDG_STATE_HOME}/codebase-onboarding-check/state.jsonl` if set.  
  3) `$HOME/.cursor/state/codebase-onboarding-check/state.jsonl` (Cursor)  
  4) `$HOME/.claude/state/codebase-onboarding-check/state.jsonl` (Claude)  
  5) `$HOME/.local/state/codebase-onboarding-check/state.jsonl` (generic default)  
  Fallback: `memory/state.jsonl` next to this skill. `mkdir -p` the parent directory before appending.

## Workflow
### 1) Collect inputs
- Ask for or locate: design/plan docs, ADRs, README/setup, key service/module paths, and any recent changes (PRs, migration notes).
- If artifacts are missing, state assumptions and list what would reduce risk.
- Load the latest memory record from the first existing file in the priority list above; if none exist, fall back to `memory/state.jsonl` beside the skill (create `memory/` if needed).

### 1a) Proficiency probe
- Ask 3 tight questions to gauge current understanding: project goal, main user flow, and one invariant or dependency that must hold.
- Score qualitatively (Novice/Intermediate/Ready). If Novice, pause the full check and hand over a learning kit (see below). If Intermediate, give the kit and a single warm-up exercise before proceeding. If Ready, continue.
- Note the score in the memory file before moving on.

### 2) Digest the doc
- Extract: purpose/goal, scope & non-goals, success metrics, constraints, data/contracts, sequencing, and failure/rollback strategy.
- Build a glossary of domain terms and component names. Note unresolved questions or TODOs the doc mentions.

### 3) Map doc to code
- Identify runtime entrypoints (CLI, HTTP handlers, jobs). Map each major doc concept to concrete files/functions.
- Check drift: is behavior implemented? Any divergences, feature flags, or legacy paths? Record evidence (file+line references).
- Trace one happy path + one failure path end-to-end (inputs → output, persistence, external calls).

### 3a) Provide a learning kit when needed
- If proficiency < Ready, supply: (1) a 10–15 bullet “notes” primer, (2) links to 1–2 concise articles or docs sections, (3) one solid explainer video or conference talk. Use the project’s own docs first; otherwise pick reputable, recent sources and state the date.
- Include one small practice task (e.g., “trace the login flow from handler to DB” or “add a guard and test for missing header”).
- After the user signals completion, resume the comprehension checks at the last saved point.

### 4) Comprehension checks
- Create 6–10 questions spanning: concept recall, data/flow mapping, edge/failure handling, dependency/infra, change-scenario (“if we add X, what breaks?”), and code-level understanding.
- Provide brief answer keys using doc/code citations; mark unknowns explicitly. Use the templates/rubric in `references/comprehension-checklists.md` to ensure coverage.

### 5) Readiness plan
- Recommend a reading order (doc sections → code modules → tests).
- Propose 2–3 safe starter tasks (small refactor, add log/metric, add a guard/test) tied to the concepts above.
- List verification steps (tests to run, fixtures/data needed). Encourage the user to attempt the questions first, then compare to the answer key.
- Update memory with: latest score, unanswered questions, gaps, assigned warm-up tasks, and a pointer to where to resume (append a JSON line to the chosen state file; use the fallback only if the preferred paths cannot be written).

## Outputs to return
- Concise project summary (goal, constraints, primary flow, invariants).
- Glossary of terms/components.
- Gap list: unanswered questions, suspected doc/code drift, missing diagrams/contracts.
- Comprehension check set with answer key and what is still unknown.
- Onboarding plan with next reads and 2–3 practice tasks/tests.
- Memory entry written to the selected state file (priority list above); fallback `memory/state.jsonl` beside SKILL.md. Include session id, user handle, proficiency score, links to resources shared, unanswered question numbers, and next-step pointer.

## References
- Load `references/comprehension-checklists.md` for ready-made question stems, coverage rubric, and quick drift/gap checklist when crafting the comprehension checks.
- Load `references/adaptive-coaching.md` for proficiency scoring, learning-kit templates, and the memory file schema.
