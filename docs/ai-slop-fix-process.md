# Fixing AI Slop — Process

A repeatable process for finding, fixing, and preventing AI slop:
code and writing that looks plausible, is verbose and generic, and
is wrong, unnecessary, or unmaintainable in ways a human would not
produce. Slop is not a style problem alone — it is a quality,
correctness, and trust problem.

This process is designed to run with the designer and engineer
agents in this workspace, but it works with any coding agent.

## What counts as slop

- **Verbose filler:** code, comments, or prose that restates the
  obvious, repeats itself, or pads a simple idea into paragraphs.
- **Generic design:** purple gradients, generic hero sections,
  emoji bullets, "delve", "in today's fast-paced world", and other
  AI tells — in code and in copy.
- **Over-engineering:** abstractions, helpers, and frameworks added
  for a problem that does not need them.
- **Unverified claims:** code that was never run, tests that were
  never executed, numbers with no source, features that do not
  actually work.
- **Copy-paste patterns:** the same pattern repeated instead of a
  shared helper, or a helper used once.
- **Dead weight:** unused code, unused imports, commented-out code,
  TODOs with no owner, and features nobody asked for.

If it would not survive a human code review, it is slop — even if
it runs.

## The process

### Stage 0 — Freeze the scope

Before fixing anything, write down:

- What is in scope (files, screens, sections) and what is not.
- What must not break (features, links, data, tests, deployment).
- What "fixed" means for this pass (see Stage 4 gates).

Do not start rewriting until scope is written. Scope creep is how
slop fixes become slop rewrites.

### Stage 1 — Detect (engineer + designer, review only)

No file changes in this stage.

1. **Read, don't skim.** Read the actual files, not a summary.
2. **Run it.** Build, serve, click through, run the tests. Note
   anything that does not actually work, whatever the code claims.
3. **Score each finding** by type (see list above) and severity:
   - **P0 — Wrong:** incorrect, broken, insecure, or misleading.
   - **P1 — Wasteful:** dead, duplicated, or over-engineered code.
   - **P2 — Generic:** AI tells in design or prose that erode trust.
   - **P3 — Polish:** naming, spacing, consistency, minor clarity.
4. **Produce a slop report:** findings tied to specific files and
   lines, with a before/after sketch for each P0 and P1. Do not
   rewrite yet.

Agents and skills for this stage:

- Engineer agent, Matt Pocock `code-review` skill.
- Designer agent, for design and prose slop (hierarchy, generic
  visuals, AI-tell copy).
- pstack `/unslop` skill, for writing: removes AI tells from prose.
- pstack `/interrogate` skill, for diffs: different models try to
  break the change, keeping only findings a run proves.

### Stage 2 — Diagnose (one cause per finding)

For each P0 and P1 finding, name the cause before fixing it:

- Was the prompt vague? (Fix the prompt, not just the output.)
- Was the model guessing? (Add evidence: run it, test it, check it.)
- Was there no style guide, token set, or spec? (Write one first.)
- Was the change too big to review? (Split it.)

A fix without a cause will recur. If the same slop type appears
three times, the cause is the process, not the model — fix the
process in Stage 5.

### Stage 3 — Fix (small, verified, in order)

Fix in severity order, smallest change first:

1. **P0 — Wrong:** delete, correct, or revert. Prove the fix with
   a run, a test, or a screenshot — never with an assertion.
2. **P1 — Wasteful:** delete dead code, deduplicate, simplify.
   Prefer deletion over refactoring; the best fix for slop is
   often less code, not better code.
3. **P2 — Generic:** replace AI tells with specific, human choices:
   real copy, real data, one clear hierarchy, design tokens
   instead of one-off styles.
4. **P3 — Polish:** naming, consistency, spacing — only after
   P0–P2 are done.

Rules for every fix:

- One finding per change; one change per commit.
- Keep existing content, links, data, and tests intact unless the
  task explicitly says otherwise.
- No new frameworks, dependencies, or build steps unless the task
  explicitly asks for them.
- Every fix gets a verification step in the same pass (Stage 4).

Agents and skills for this stage:

- Engineer agent, Matt Pocock `implement`, `tdd`,
  `diagnosing-bugs`, and `code-review` (reviewing its own diff).
- Designer agent, for P2 design and prose fixes.
- pstack `/poteto-mode` playbooks, for rigorous work: bug fix
  (repro first), refactoring (behaviour-preserving), feature
  (from a named data shape), and shipping (verify, then land).

### Stage 4 — Verify (gates, not vibes)

A fix is not done until it passes every applicable gate:

- [ ] **Runs:** the app builds, serves, and the changed feature
  works by clicking through it — not just by reading the code.
- [ ] **Tests pass:** existing tests pass, and new behaviour has
  a test (or a committed harness check, for static sites).
- [ ] **No regressions:** search, filters, navigation, copy,
  forms, and mobile layout still work exactly as before.
- [ ] **Review passed:** a second agent (or the engineer agent
  reviewing its own diff with `code-review`) found no P0/P1 left.
- [ ] **Slop check:** re-run the Stage 1 detection on the changed
  files only. New slop introduced by the fix counts as a failure.
- [ ] **Live check:** after publishing, verify the live site or
  app, not just the repo.

If any gate fails, the fix goes back to Stage 3. Do not publish
around a failed gate.

### Stage 5 — Prevent (so it does not come back)

Slop fixed once will return unless the inputs change. After each
pass, update at least one of these:

1. **The brief:** make prompts specific — audience, primary
   action, constraints, what must not break, and what "done"
   means. Vague briefs are the top cause of slop.
2. **The agent files:** add the recurring slop type to the
   designer or engineer agent file as a rule, with an example
   of the bad pattern and the good pattern.
3. **The skills:** give the agent the skill that prevents that
   slop type (for example, `tdd` for unverified code,
   `code-review` for unreviewed diffs, `unslop` for AI-tell prose).
4. **The checks:** add a committed harness, linter rule, or
   checklist item that catches that slop type automatically.
   For static sites, a small Node harness (e.g. `test/check.js`)
   is enough.
5. **The log:** record the slop type, its cause, and the fix in
   the project's task log, so the next pass starts from evidence
   instead of memory.

## Running it on a project

| Step | Who | Output |
| ---- | --- | ------ |
| 0. Scope | Muse (manager) | Scope note, gates |
| 1. Detect | Engineer + Designer, review only | Slop report, P0–P3 |
| 2. Diagnose | Engineer | Cause per P0/P1 |
| 3. Fix | Engineer (+ Designer for P2) | Small verified changes |
| 4. Verify | Engineer + Muse | Gate checklist, live check |
| 5. Prevent | Muse | Updated brief, agent file, checks, log |

## Example: first run (generic)

Scope: a small static site, no content or link changes.
Detect: engineer code review + designer audit — P0 contrast and
touch targets, P1 structural fixes, plus code-review hardening
findings.
Fix: designer P0+P1+P2 pass, then engineer hardening pass.
Verify: local harness passes + live checks (search, filters,
clear-search, expand all, copy).
Prevent: designer and engineer agent files, Matt Pocock skills,
and the committed harness now catch the next round of slop
before it ships.
