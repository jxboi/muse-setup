# Delegation and Token Optimisation

When Muse delegates work to designer, engineer, Codex, Claude Code,
agy, or browser agents, token usage is a real cost — and the way
work is split decides it. This file is the standing rule: when delegating, occasionally check usage and
split work accordingly to optimise token usage.

## The honest limit first

`subagent.list` does not report token counts for Muse subagents.
It reports status, how long an agent has been running, and how
long since its last activity. Codex `exec` does report tokens
used. So:

- For Codex, use its reported token count.
- For Muse subagents, use proxies: running time, time since last
  activity, output length, and how many files it re-read.
- For every agent, require it to report what it read and what it
  produced, so the next check has something to compare.

Do not claim a token number you did not see. Record the proxy
you actually used.

## Why splitting saves tokens (and what it does not)

Splitting work across agents does **not** save total tokens by
itself — two agents reading the same files cost more than one.
It saves tokens when it does these things:

1. **Narrows context.** Each agent reads only its own file set.
   A designer reading only `index.html` and `styles.css` costs
   less than one agent reading all five files plus the test
   harness plus the README. A single mega-agent that does detect
   + fix + verify + publish re-reads everything at every stage.
2. **Prevents rework.** A brief that names the exact files, the
   exact scope, and what must not break stops an agent wandering
   the whole repo, re-checking already-verified work, or
   re-deriving decisions already made.
3. **Matches the cheapest capable agent to the job.** Mechanical
   edits go to Codex; review-only reports go to a designer or
   engineer subagent; live verification goes to the browser task.
   Do not send Codex to redo a review a subagent already did, and
   do not send a review agent to do a mechanical edit.
4. **Makes outputs mergeable.** Parallel agents with disjoint
   outputs (designer slop report + engineer slop report) can be
   merged by Muse in one pass, instead of one agent producing a
   single giant report that then has to be re-read and split.

Parallelising saves wall-clock time, not tokens. Narrowing scope
saves tokens. Do both, but do not confuse them.

## How to split

- **By concern, not by file count.** Design/prose to the designer,
  code/robustness to the engineer, mechanical edits to Codex,
  live checks to the browser. File sets should be as disjoint as
  the task allows.
- **By stage, not all at once.** Detect (review only) first, then
  fix, then verify. A fix agent that also has to detect wastes
  its context re-doing detection, and a detection report that is
  never used is pure waste — so do not spawn a detector unless
  the fix stage is actually planned.
- **Keep sequential work together.** Work with real dependencies
  (a spec, then tickets, then implementation in frontier order)
  stays in one chain; splitting it across parallel agents creates
  merge work and duplicated reads that cost more than it saves.
  Exception: itemised research batches are
  parallelised and seeded from prior research — see
  `agents/researcher.md` "Speed" section. For research, time was chosen
  over tokens; for other work, this file's default still holds.
- **One agent, one deliverable.** A brief should end with one
  output: a report, a set of file changes, or a verification
  result. "Do everything" briefs are the biggest token waste
  there is.

## How to brief cheaply

- Point at files (`~/workspace/.../index.html`), do not paste
  their contents into the brief. The agent can read them.
- Name only the context the agent needs: scope, files, what must
  not break, and the output format. Subagents inherit the parent
  conversation, so explicitly say what to ignore ("ignore the X
  bookmarks work; it is unrelated").
- Cap the output: findings with file and line, before/after
  sketches for P0/P1 only, no full file dumps, no restating the
  brief. A report that re-quotes the whole codebase costs as
  much as reading it again.
- For Codex, pass a single precise prompt with a numbered task
  list and a "do not change" list, and close stdin, so it does
  not hang waiting for input.

## When to check usage (the "occasionally" rule)

Check at three points, and only those, unless something looks
wrong:

1. **At delegation:** is this task splittable into disjoint
   concerns? If yes, split now — it is cheapest to split before
   an agent has read anything.
2. **Mid-task, if it runs long or goes quiet:** check
   `subagent.list` for running time and last activity. If an
   agent has been running far longer than its task size warrants,
   or quiet for a long time, check what it is doing: it may be
   re-reading, stuck on a tool, or expanding scope. Narrow it,
   split off a stuck sub-task, or close and re-brief it — do not
   let it run to a giant context on autopilot.
3. **At completion:** record in the task log: agent, task, running
   time, output size, tokens if reported, and whether any work
   was re-done or duplicated. That record is what the next
   delegation decision is based on.

Do not poll in a loop. A check is a decision point, not a watch.

## Token-heavy traps to avoid

- Asking an agent to re-verify something already verified (live
  check, harness pass) without a reason.
- Giving two agents the same full file set and the same question.
- Briefs that include the whole conversation history, the whole
  repo, or the whole previous report "for context".
- Verbose reports: summaries, not transcripts. If a report is
  longer than the change it describes, the brief was wrong.
- Re-running a full test suite or full site check after every
  tiny change; batch changes, then verify once, then re-verify
  only what changed.
- Spawning a coordinator to spawn subagents for a task one agent
  could do in a single pass. Nesting costs context at every level.

## Recording usage

After each delegated task, add one line to the relevant task log
(`engineer-tasks.md`, `designer-tasks.md`, or the project log):

`Agent | task | running time | tokens (if reported) | output | waste noted`

Example: designer and engineer ran
in parallel, disjoint concerns (design/prose vs code), review
only, no file changes — cheap detection, and the fix pass then
reused their reports instead of re-detecting. A Codex fix pass
reported its token count, changed a handful of files, and the
harness passed, with the site's content
unchanged apart from shared SVG reuse.

If the same waste appears twice, it becomes a rule in the
self-improvement plan (`agent-self-improvement-plan.md`), not
just a note.
