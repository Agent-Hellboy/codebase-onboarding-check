# codebase-onboarding-check

Vendor-neutral **agent skill** for onboarding onto a real codebase: read docs and code, summarize the mental model, then run **question-driven checks** (short proficiency probe plus a larger comprehension set with answer keys). Optional pieces in `references/` cover question templates, adaptive coaching, and a small session-memory format if your agent implements it.

Only `SKILL.md` and `references/` ship here so you can copy or symlink the folder into whatever tool you use.

## What it helps with

- Build a concise picture: goals, main flows, invariants, contracts, glossary.
- Surface gaps and doc-vs-code drift before you ship changes.
- Leave you with a short **readiness plan** (what to read next, a few safe starter tasks).


## Using it in Cursor

**Personal (all workspaces):** copy or clone this folder to:

`~/.cursor/skills/codebase-onboarding-check/`

so it contains `SKILL.md` and `references/` next to each other.

**One repo only:** same structure under:

`.cursor/skills/codebase-onboarding-check/`

Restart or start a new agent chat after installing so the skill list refreshes.

Optional persistence (if you follow `SKILL.md`): session lines can go under `$HOME/.cursor/state/codebase-onboarding-check/state.jsonl` or the fallbacks described in the skill. No separate server or binary is required.


## License

MIT. See `LICENSE`.
