# Engineer Agent — Management (Template)

Manager: Muse. The engineer agent never publishes on its own.
Muse briefs it, checks its verified result, and owns publishing
(GitHub push, deployment live check, project-tracker update).

Agent file: `agents/engineer.md`
Skills: Matt Pocock skills plugin (installed on Codex — record
your installed version in `agents/skills-installed.md`).

> This is a template. The log below holds one example entry only.
> Replace it with your own tasks as they happen; do not keep the
> example as a record of real work.

## How tasks are delegated

1. The user gives Muse a task in plain words.
2. Muse writes the task here (status: queued), with scope, files,
   and what must not break.
3. Muse briefs the engineer agent with the task, the agent file,
   and the relevant Matt Pocock skill(s) for that job.
4. The engineer works in its own pass and returns: understanding,
   exact changes, tests/verification, own code-review notes, handoff.
5. Muse verifies (local checks, then live check after publishing),
   publishes only approved changes, and updates this log.

## Task log

### 1. [Example] Code review of your project — review only
- Status: [example — completed, review only, no file changes]
- Scope: Review the main files in your project repo (e.g.
  `index.html`, `styles.css`, `script.js`). Use the Matt Pocock
  `code-review` skill. No file changes.
- Must not break: content, links, search, filters, copy, and
  accessibility fixes from the most recent design pass.
- Result: [example] Verdict: correct as it stands, no blocking
  issues. Findings (all low/minor): e.g. missing null guards,
  copy text extraction outside a try block, a control at the
  minimum usable width, JS-only controls visible/dead if JS
  fails, no committed tests. Recommended next step: implement
  the recommendations as one small hardening pass.

## Queue

- (empty — add new tasks here as the user gives them)
