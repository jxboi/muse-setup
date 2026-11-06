# AGENTS.md

> Template: adapt this file to your own workspace. It is your operating
> manual — the durable lessons and conventions you pick up as you work,
> the kind of thing you'd want a future session to know.

Your operating manual for this workspace, written by you. Your main instructions cover how you work in general. This file is where you keep the specific, durable lessons and conventions you pick up as you work, the kind of thing you'd want a future session to know. It's not about the user (that goes in `USER.md` and your memory) or your personality (that's `SOUL.md`); it's about how you get work done here.

## Conventions
Add an entry whenever you work something out worth keeping, for example:
- a convention you've settled on ("keep data exports in `workspace/exports/` and clean them up monthly")
- a tool or site quirk worth remembering ("site X hides its form behind a cookie banner; dismiss it first")
- a workflow that worked, or a mistake not to repeat

It starts empty and is meant to grow slowly. Don't pad it; a short, accurate file beats a long, stale one.

## Conventions

- **Delegation and token use:**
  when delegating to different agents, occasionally check their
  usage and split work accordingly to optimise token usage. Full
  rules: `agents/delegation-token-optimization.md`.
  In short: split by concern (designer = design/prose, engineer =
  code, Codex = mechanical edits, browser = live checks) with
  disjoint file sets; brief by pointing at files, never pasting
  them; cap outputs to findings, not file dumps; check
  `subagent.list` at delegation, mid-task if long/quiet, and at
  completion (running time, last activity, tokens if reported —
  Codex reports them, Muse subagents do not, so use proxies and
  never invent a token number); record agent/task/time/tokens/
  waste in the task log. Parallelising saves time, narrowing
  scope saves tokens — do not confuse them.

- **Artifact repair scope:**
  when authorising an `artifact.inspect` repair, the
  `verbatim_request` must carry the full authorised scope (the
  defects as described and approved), not just the user's short
  tap text — a narrow verbatim gets read literally and the
  repair does only that part, leaving the rest undone.
