# Skills inventory — template

> **This is a template, not a finished inventory.** Fill in the
> `[Your skills here]` sections with the skills actually installed
> on your machine, in each host tool you use. Keep this file as the
> single source of truth: every reusable agent
> (designer, engineer, researcher, reviewer) must read this file
> before claiming a skill is available. Do not claim a skill
> exists in the host tool unless it is listed here as installed
> for that host, or the brief explicitly supplies it.
>
> Update this file in the same pass whenever you install, remove,
> or upgrade a skill. Agents never edit this file to grant
> themselves a skill.

## The distinction that matters

- **Installed** — on disk in the host tool, callable by name.
  Use it directly.
- **Referenced / catalogue** — known by name and URL, or listed
  in a skills catalogue, but NOT installed in the host
  tool. An agent may consult it as reference, or ask the
  orchestrator to install it — it must never pretend it
  already has it.
- **Orchestrator-only** — Muse's own product skills (shopping,
  scheduling, messaging, and similar). Those belong to Muse
  the orchestrator, not to agents running in Codex, Claude
  Code, or agy, and are not listed here.

## Installed — [Your skills here]

List each installed skill or skill plugin once, under the host
tool(s) it is installed in, with its version and install path
as of the date you last verified it. Versions change — re-check
yours rather than trusting any version number in an example.

Template per entry:

```text
### <Skill or plugin name> — <Host tool(s)>

Installed as `<package or plugin id>`, version <version>,
verified <date>. Install path: <path>.
What it is for: <one line>.
Notes / cautions: <anything an agent must know before using it>.
```

### [Your skills here]

- [ ] Skill/plugin name, host(s), version, install path, purpose
- [ ] Skill/plugin name, host(s), version, install path, purpose

## Example — the author's setup (as of writing; check yours)

The entries below are an example of a filled-in inventory, from
the author's machine. They show the shape a completed inventory
takes; do not copy them as your own — verify what is actually
installed on your machine and replace this section with your
own list.

### Matt Pocock skills plugin — Codex only (example)

Installed on Codex as `mattpocock-skills@mattpocock`
(version 1.3.2 as of writing; check yours).

Engineering: ask-matt, code-review, codebase-design,
diagnosing-bugs, domain-modeling, grill-with-docs, implement,
implement-spec, improve-codebase-architecture, pr, prototype,
research, retro, setup-matt-pocock-skills, tdd, to-spec,
to-tickets, triage, wayfinder, wizard.

Productivity: handoff, teach, to-questionnaire, wait-what,
writing-for-agents. (grill-me / grilling are installed in
the plugin but DO NOT USE — grill-with-docs already covers
grilling, including glossary definitions.)

### Impeccable — all three builders (example)

Version 4.8.0 as of writing; check yours. Installed globally
on Claude Code, Codex, and agy/Antigravity. Use it for
frontend design, critique, audit, polish, and quality checks,
and for its detector and craft-floor reference. It does not
replace UI/UX Pro Max as design lead, and it is not a backend
skill.

### Matt Pocock spine à la carte — Claude Code + agy (example)

Copied from the Codex plugin cache as plain SKILL.md folders
into the Claude Code and agy skills directories:
to-spec, to-tickets, implement, implement-spec, tdd,
code-review, diagnosing-bugs, grill-with-docs, handoff, retro.
(grill-me / grilling deliberately NOT copied — DO NOT USE,
same as on Codex.)

### UI/UX Pro Max — Claude Code (example)

Installed via the Claude Code plugin CLI, version 2.15.0 as of
writing; check yours. Installed as design lead. Paired
evaluation on existing surfaces returned hold — it did not
find materially more actionable issues than Impeccable alone,
and the new-direction role is untested. Impeccable remains
the audit layer.

### skill-security-auditor — Claude Code (example)

Scans a skill for malicious execution patterns, subprocess
calls, network exfiltration, and prompt injection, returning
PASS / WARN / FAIL. Run it over any third-party skill before
installing or using it.

## Referenced, not installed — fetch or install on demand

These are named in `agents/designer.md` and are NOT on disk in any
host tool (in the author's setup; check yours). An agent that needs one says so in its report
and uses the closest installed skill meanwhile, or asks
the orchestrator to install it — never silently substitutes
training-data recall for the skill.

- Anthropic frontend-design (inside
  https://github.com/anthropics/skills)
- Refactoring UI skill —
  https://github.com/edisonmbli/refactoring-ui-skill
- Design systems / tokens / accessibility —
  https://github.com/plugin87/ux-ui-agent-skills
- Frontend design audit —
  https://github.com/mistyhx/frontend-design-audit
- Exploring directions —
  https://github.com/oil-oil/oil-ui
- Known-brand visual vocabulary (`popular-web-designs`,
  MIT v1.0.0, 54 templates) — pick exactly one template,
  adapt its tokens, never copy logos or proprietary fonts.
- Mobile / apps (Apple HIG reviewer) —
  https://github.com/dickwu/apple-design-skill

[Your referenced skills here — add any others you consult by name
but have not installed.]

## Catalogue only — consult, do not invoke

A skills catalogue (a site or repo listing skills with their
repos and descriptions) is a catalogue for choosing skills, not
installed agent skills. Those entries are for choosing, not
invoking. An agent may cite a catalogue entry
as a recommendation; invoking it requires installing it
first.

[Your catalogue here — name the catalogue you use, if any, and
its URL. Do not list catalogue entries as installed skills.]

## Research engine (not a skill)

agy (Antigravity CLI) is the researcher agent's default
engine for Google-backed web research. It is a tool, not
a skill file, and it is also the independent judge engine
for evals. The judge role is defined in
`evals/judge.md`, not by a skill.

## Rules for every agent

1. Read this file at the start of a task that needs a
   skill, and name the skills you will use from it.
2. If a skill you need is referenced but not installed,
   say so in your first report — do not proceed as if
   you had it.
3. If a skill is installed in one host but not the host
   you are running in, say so; do not assume Codex-only
   skills exist in Claude Code or agy, or vice versa.
4. When a new skill is installed, the orchestrator updates
   this file in the same pass. Agents never edit this
   file to grant themselves a skill.
