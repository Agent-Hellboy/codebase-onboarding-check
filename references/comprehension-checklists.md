# Comprehension Checklists & Question Templates

Use these to construct coverage-balanced questions and to spot missing context quickly. Keep the main skill brief; pull this in when drafting checks or assessing drift.

## Question Templates (pick 6–10 total)
- **Concept recall:** What is the primary goal of this project? What is explicitly out-of-scope right now?  
- **Flow mapping:** Trace the happy path from input → core processing → storage/outputs. Which file/function starts the flow?  
- **Data/contract:** What data shapes or API contracts must be honored? Where are they validated?  
- **Failure/edge:** How do we recover if the external dependency `<X>` is down or slow? What metrics/alerts fire?  
- **Dependency/infra:** Which services/queues/cron jobs must be running? Any feature flags or migrations gating this?  
- **Change scenario:** If we add `<new feature>` or double traffic, what code paths and configs need updating?  
- **Code comprehension:** Why does `<module/function>` exist? What invariant does it maintain? Point to the specific guard/condition.  
- **Testing/verification:** Which tests or fixtures cover the critical flow? Which gaps remain untested?

## Coverage Rubric
- Hit at least one question in each bucket: goals/scope, data/contracts, flows, failure/observability, dependencies/infra, code-level detail, change-readiness.  
- Provide concise answer keys citing files/lines when possible; mark unknowns plainly instead of guessing.  
- Prefer scenario questions over trivia; they reveal whether the mental model is usable.

## Drift & Gap Checklist
- Doc claims vs. code reality: feature flags, legacy paths, stubs, TODO/FIXME clusters, config defaults that differ from doc.  
- Missing artifacts: sequence diagrams, schema definitions, env/setup steps, rollback plan, SLOs/alerts.  
- Ownership and review: who approves changes, which areas are “hot”; recent PRs that changed core behavior.  
- Risks before contribution: unclear invariants, unknown data ranges, migration status, shadow data models across services.
