# Adaptive Coaching Aids

Use this file when running the proficiency probe, delivering a learning kit, or updating session memory. Keep responses concise so the main skill stays small.

## Proficiency tiers
- **Novice:** Cannot state project goal or core flow; guesses at invariants; needs guided resources.
- **Intermediate:** Knows goal and main flow but is fuzzy on invariants/dependencies; can attempt warm-up tasks with guidance.
- **Ready:** Can describe goal, main flow, and at least one invariant/dependency with evidence; proceed to full comprehension checks.

## Probe question starters (pick 3)
1) "What problem does this project solve and for whom?"
2) "Walk me through the happy path from entrypoint to persistence/response."
3) "Name one invariant or dependency that must hold for the happy path to work (feature flag, schema, service, env var)."
4) "If traffic doubles tomorrow, what code/config would you inspect first?"

## Learning-kit template (when proficiency < Ready)
- **Notes primer:** 10–15 bullets summarizing goal, primary user/job story, key components, data/contracts, invariants, and failure signals.
- **Articles/docs (1–2):** Prefer the project’s own ADR/README/design sections. Otherwise pick recent, reputable sources; include title + date + 1-line why it matters.
- **Video (1):** A concise talk or explainer; include duration and key takeaway.
- **Warm-up task:** One small exercise grounded in the codebase (trace a flow, add a guard+test, or write a log/metric). Provide files/paths to inspect.

## Memory file schema
Write newline-delimited JSON to the first writable path in this order:  
1) `${AGENT_STATE_HOME}/codebase-onboarding-check/state.jsonl` (if set)  
2) `${XDG_STATE_HOME}/codebase-onboarding-check/state.jsonl` (if set)  
3) `$HOME/.cursor/state/codebase-onboarding-check/state.jsonl` (Cursor)  
3) `$HOME/.claude/state/codebase-onboarding-check/state.jsonl` (Claude)  
4) `$HOME/.local/state/codebase-onboarding-check/state.jsonl` (generic default)  
Fallback: `memory/state.jsonl` beside the skill. `mkdir -p` parents before appending. Keep each record under 1 KB.

Fields:
```json
{
  "session_id": "2026-04-20T15:30Z-<rand>",
  "user": "<handle or email>",
  "topic": "<project/service name>",
  "proficiency": "Novice|Intermediate|Ready",
  "probe_answers": ["..."],
  "resources_shared": [{"type": "notes|article|video", "title": "...", "link": "...", "why": "..."}],
  "warmups": ["..."],
  "checks_pending": [2,4,6],
  "resume_pointer": "start-check-4" ,
  "gaps": ["need schema for <table>"]
}
```

## Resume protocol
- On a new session, load the latest record; restate the last proficiency score, pending checks, and warm-up tasks.
- If the last record is older than 14 days, rerun the probe quickly to confirm readiness.
- After finishing checks, append a new record with the updated proficiency and unanswered questions (likely empty) so history remains append-only.
