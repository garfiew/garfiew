# Handoff — garfiew (Skinless Foundation learning project)
_2026-07-13_

## Why This Exists

The user is going OOO and wants any future session (Cowork chat or Claude Code) to resume this project without a manual re-brief. This doc is a point-in-time snapshot — it does not replace `TASKS.md`, which is live and should always be re-read for current status.

## Current State

- Course is a learning-by-doing setup: every lesson applies directly to one real client brief, Skinless Foundation (see `MISSION.md` for full context, `CAMPAIGN-PRD.md` for the finished strategy).
- Lesson 1 (Brief Decoding) is complete — see `learning-records/0001-session-start.md`.
- The full Skinless Foundation campaign PRD is drafted and considered a finished deliverable, including a detailed budget rationale added afterward — not something to redraft from scratch.
- Lesson 2 (Content Pillars) has not started yet. It's the next teaching step.

## How to Resume

**Cowork chat:** open a new chat inside this project (not a blank chat) so `CLAUDE.md` auto-loads, then say: "Read handoff_garfiew_2026-07-13.md, then continue from TASKS.md." It will re-read `TASKS.md` live (this doc is only a snapshot) and propose next actions. It will still confirm before drafting anything into `CAMPAIGN-PRD.md`.

**Claude Code:** `cd` into this project folder and run `claude`. `CLAUDE.md` loads automatically; say: "Read handoff_garfiew_2026-07-13.md for OOO context, then check TASKS.md." Same confirm-before-drafting rule applies.

## Suggested Skills

- None of the repo's installed skills are required just to resume teaching — the flow above (`CLAUDE.md` + `TASKS.md`) is sufficient.
- If a new lesson's output should itself be turned into a structured deliverable (like `CAMPAIGN-PRD.md` was), the `to-prd` skill in `.claude/skills/to-prd/` is already installed and fits that pattern.
- If wrapping up a future session, use the `handoff` skill (`.claude/skills/handoff/`) to generate the next snapshot doc the same way this one was produced — save it to the project folder, not just the OS temp dir, so both Cowork and Claude Code can see it.

## Not Duplicated Here

Strategy content, budget numbers, and KPIs are not repeated in this doc — they live in `CAMPAIGN-PRD.md`. Teaching preferences and audience context live in `MISSION.md` / `NOTES.md`. This doc only covers what's needed to resume the handoff process itself.
