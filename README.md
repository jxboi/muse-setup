# Muse Setup

A reusable Muse setup — skills, agents, workflow docs, and
templates — so you can replicate a working Muse orchestration
setup in another Muse account, instead of rebuilding it from
scratch.

## Who it is for

Muse users who orchestrate builders — Codex, Claude Code, and
agy — rather than building directly. Muse owns briefing,
verification, publishing, and tracking; builders do the
implementation.

## What is inside

```
skills/       Reusable skills (e.g. design hill climb, setup-builders)
agents/       Reusable agent files (designer, engineer, researcher,
              reviewer) and their task logs / skills inventory
docs/         Workflow docs — the processes the agents follow
evals/        Design rubric and evaluation material
templates/    Starting templates for Muse's home files
```

- `docs/development-workflow.md` — the canonical 8-stage workflow:
  intake, spec/tickets, delegate, build/TDD, review/slop gate,
  verify, publish, track/retro, plus the full-run checklist.
- `docs/builder-routing.md` — which builder does what, model
  defaults, and standing rules.
- `docs/world-class-design-process.md` — converging on a design
  (Phases 0–6) instead of iterating on vibes.
- `docs/ai-slop-fix-process.md` — detect, diagnose, fix, verify,
  prevent AI slop, with severity levels P0–P3.
- `docs/agent-self-improvement-plan.md` — the capture → review →
  update → test → measure loop for the agents.
- `docs/engineer-tasks.md` — template for the engineer task log.
- `evals/design-rubric.md` — the project-agnostic design rubric
  (criteria fixed; evidence anchors supplied per project).

## Install into a fresh Muse account

Copy each folder to its home in your Muse workspace:

| In this repo | Copy to |
| --- | --- |
| `skills/` | `~/workspace/custom-skills/` |
| `agents/` | `~/workspace/custom-agents/` |
| `docs/` | `~/workspace/custom-agents/` (or a docs folder of your choice — update cross-references if you move them) |
| `evals/design-rubric.md` | `~/workspace/evals/` |
| `templates/` | `~` as `AGENTS.md` / `SOUL.md` / `IDENTITY.md` / `TOOLS.md` |

## Adapt it

- **Model defaults:** set your own in `docs/builder-routing.md`
  (the defaults in this repo are gpt-6.1-sol for implementation
  and Opus 5.5 for review — change them if you use different
  models).
- **Machine-specific paths:** files carry an "Adapt this" note
  where they assume the author's machine (tool install paths,
  worktree locations, timezone-specific times). Replace them
  with your own.
- **Installed skills:** fill in `agents/skills-installed.md`
  with the skills actually installed on your builders — the
  agent files reference skills that may not be installed.
- **Placeholders:** replace `[Your Name]` placeholders with
  your name, and the generic project examples with your own
  projects.

## Note

This is a starting template, not a finished setup. It captures
one working orchestration — the workflows, roles, and gates that
proved useful — so you can start from something real and adapt
it. Expect to change it as your own experience accumulates;
that is what the self-improvement plan in `docs/` is for.
