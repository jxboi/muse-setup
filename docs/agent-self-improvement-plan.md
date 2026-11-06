# Agent Self-Improvement Plan

How the designer, engineer, and orchestrator (Muse) agents improve
over time and experience — instead of repeating the same mistakes
on every project.

Principle: an agent does not improve by being told to "be better".
It improves when experience is captured, turned into a rule, tested,
and kept only if it works. Everything else is a slogan.

## The loop

Every task, no matter how small, goes through the same loop:

```
Do the task → Capture what happened → Review it → Update the
agent → Test the update → Keep it, or revert it
```

If a step is skipped, the loop is broken and the agent will repeat
the mistake. The loop is cheap for small tasks (a few lines in a
log) and thorough for large ones (a full retrospective).

## Stage 1 — Capture experience (after every task)

Immediately after a task finishes — while it is still fresh —
record, in the agent's task log:

- **Task:** what was asked, in one line.
- **Outcome:** what was delivered, and whether it was verified
  (locally, live, or not at all).
- **What worked:** decisions, patterns, or skills that helped.
- **What failed:** errors, rework, corrections from the user,
  findings from a review that should have been caught earlier.
- **Surprises:** anything the agent did not expect — a tool limit,
  a site behaviour, a user preference.
- **Time and rework:** how many passes it took, and what caused
  the extra passes.

Do not record opinions ("I did well"). Record evidence: files
changed, checks run, user corrections quoted, review findings
cited. A log entry with no evidence cannot teach anything.

Logs live here:

- Designer tasks: `agents/designer-tasks.md`
  (create it the first time the designer is used, following the
  engineer log's format — it does not exist yet).
- Engineer tasks: `agents/engineer-tasks.md`
- Orchestration (Muse): your orchestrator's notes/memory and the
  project page in your project tracker (e.g. Notion), for
  decisions, preferences, and commitments.

## Stage 2 — Review (weekly, or after every 5 tasks, whichever first)

Muse runs a review of the recent log entries and asks:

1. **What repeated?** Any failure, correction, or review finding
   that appeared twice or more is a pattern, not bad luck.
2. **What was the cause?** For each pattern, name one cause:
   a missing rule, a wrong skill, a vague brief, a missing check,
   or a tool limit.
3. **What would have caught it earlier?** A rule, a skill, a
   checklist item, a test, or a different agent assignment.
4. **What should we stop doing?** A pattern, skill, or habit that
   consistently costs more than it helps.

Use the Matt Pocock `retro` skill for this review: capture the
recipe, not just the feeling. The output is a short list of
proposed updates — no more than three per review, so updates
actually get tested instead of piling up.

## Stage 3 — Update the agent (small, one change at a time)

Turn each proposed update into exactly one of these:

- **A rule in the agent file** (`designer.md` / `engineer.md`):
  a concrete do/don't, with a bad example and a good example.
  Example: "Do not stack competing aesthetic skills as equals;
  pick one lead skill and consult the others for a specific job."
- **A skill change:** add a skill that prevents a slop type
  (for example, `tdd` for unverified code, `code-review` for
  unreviewed diffs, `unslop` for AI-tell prose), or remove a
  skill that consistently pulls in the wrong direction.
- **A check:** a committed harness, linter rule, or checklist
  item that catches that failure automatically (for example,
  a small Node harness for search/filter/guide behaviour).
- **A brief template change:** a question Muse must answer
  before delegating (audience, primary action, what must not
  break, what "done" means), if vague briefs caused the failure.
- **A role change:** if a task type consistently needs a
  different agent (for example, design reviews need the designer,
  not the engineer), change the assignment rule, not the agent.

Rules for updates:

- One update at a time, so its effect can be measured.
- Write the rule in the agent's own words, as an instruction
  it can follow, not as a note to a human.
- Date the update and link it to the log entries that caused it.
- Never add a rule that contradicts an existing rule without
  removing or rewriting the old one.

## Stage 4 — Test the update (on the next real task)

Do not test an update in isolation. Test it on the next real
task of the same type, and record:

- Did the failure it was meant to prevent occur again?
- Did it cause a new problem, slow the task, or conflict with
  another rule?
- Would the task have been better without it?

If the answer to the first question is "no" and the second is
"no", keep the update. If either is "yes", revert it and record
why — a reverted update is also learning, and it stops the next
review from proposing the same idea again.

## Stage 5 — Measure (monthly, lightly)

Once a month, Muse looks at the logs and counts, roughly:

- **Rework rate:** tasks that needed a second pass, and why.
- **Review catch rate:** findings from reviews that the builder
  should have caught itself.
- **User corrections:** how often the user had to correct scope,
  a repo, a name, or a decision — and which corrections repeated.
- **Verification rate:** tasks verified live, not just locally.
- **Agent file growth:** lines added vs lines removed. An agent
  file that only grows is accumulating rules nobody follows;
  prune ruthlessly.

No dashboards, no scores for their own sake. The point of
measuring is one question: are the same mistakes happening less
often than last month? If yes, the loop is working. If no, the
loop is being skipped somewhere — usually Stage 1 (capture) or
Stage 4 (test).

## What improves, in order

1. **First, the briefs.** Most repeated failures start with a
   vague brief, not a weak agent. Fixing briefs pays off fastest.
2. **Second, the checks.** A committed harness or checklist
   catches failures without relying on memory.
3. **Third, the agent rules.** Add rules only for patterns that
   checks cannot catch.
4. **Last, the skills.** Change skills only when a rule and a
   check cannot fix the pattern — skills are the most expensive
   change, because they change how the agent thinks, not just
   what it checks.

## What does NOT count as improvement

- Adding more rules without removing any.
- Adding more skills without testing whether they help.
- A retrospective with no update attached to it.
- A log entry that says "went well" with no evidence.
- Changing an agent file during a task, for that task only,
  without recording it in the log.

## Roles

- **Designer agent:** improves its eye — hierarchy, contrast,
  usability, and prose — by logging which audit findings were
  real, which were false positives, and which fixes actually
  shipped.
- **Engineer agent:** improves its rigour — correctness, tests,
  verification — by logging which review findings were live bugs
  vs robustness, and which hardening steps paid off.
- **Muse (orchestrator):** improves its management — briefs,
  agent assignment, verification, and publishing — by logging
  which delegations worked, which needed rework, and which
  user corrections repeated.
- **The user:** the only source of truth for preferences and
  priorities. A user correction is the highest-value log entry
  there is — it should always produce an update, not just an
  apology.

## Starting point

This plan starts from real experience, not theory. A first
project will already have produced a few patterns worth a rule —
for example:

- Verify live, not just in the repo.
- Do not invent install commands or star counts.
- Confirm which repo a name refers to before building.
- Stop retrying a blocked sign-in instead of hammering it.

The AI slop process (`docs/ai-slop-fix-process.md`) is Stage 5
prevention for content and code quality: detect, diagnose,
fix, verify, prevent.

Run the first review after the next 5 tasks, or in one week,
whichever comes first — and keep it to three updates maximum.
