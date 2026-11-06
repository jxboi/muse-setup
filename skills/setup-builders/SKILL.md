# setup-builders

> Adapt this: the builder binary paths below (`~/.local/npm/bin/codex`,
> `~/.local/npm/bin/claude`, `~/.local/bin/agy`) and the model names
> (gpt-6.1-sol, Sonnet 5.5, Gemini 3.8 Flash, Claude Opus 4.6 Thinking)
> are the author's setup. Replace them with the paths and models on your
> machine, and record your own defaults in `docs/builder-routing.md`.

Configure which builder Muse uses per role and at what reasoning budget.
Modelled on pstack's `setup-pstack` skill (cursor/plugins, `setup-pstack/SKILL.md`),
adapted from Cursor subagent model slugs to Muse's three builder tools:
Codex, Claude Code, and agy (Antigravity CLI).

Use for: "setup builders", "configure builders", "which builder for what",
"builder budget", or changing the builder choices in
`docs/builder-routing.md`.

## Setup builders

Write `docs/builder-routing.md`, an always-applied
rule that sets the builder per role. Re-running this skill updates it
(idempotent: overwrite the whole rule file).

## Steps

### 1. Detect available builders

Enumerate the builders that actually work in this session. That is the
dependable source — never write a builder or model you have not
confirmed is available.

- Codex: `~/.local/npm/bin/codex --version` (NOT the system binary,
  which cannot execute tools). Known working model from past sessions:
  gpt-6.1-sol. Codex `exec` reports tokens; record them per pass.
- Claude Code: `~/.local/npm/bin/claude --version`. Known working
  model from the workflow test: Sonnet 5.5 (session model id
  claude-sonnet-5-5). Non-interactive form:
  `claude --allowedTools "Write,Bash" -p "..."`;
  `--dangerously-skip-permissions` is blocked when running as root.
- agy: `~/.local/bin/agy --version` and `~/.local/bin/agy models`.
  Non-interactive file work needs
  `agy --dangerously-skip-permissions -p "..."`.
  (`--mode accept-edits` fails headless.)
- Muse subagents (designer/engineer/researcher files in
  `agents/`): audit and review roles, not builders.
  They do not report token counts; never invent one.

If a builder cannot be detected, mark its roles as needing a choice
and ask the user — do not guess a model for it.

### 2. Load current state

If `docs/builder-routing.md` already exists, read
it and treat its `# budget` line and its role values as the current
choices. Otherwise start from the defaults in step 5. A line whose
role is not in step 5 is from a retired role — drop it, and list
every dropped line in step 3.

### 3. Budget, map, and confirm

**(a) Ask for a budget.** Offer these four options with these exact
labels, and name the current budget when the rule records one. With
no rule, say that `large` matches the defaults.

- `unlimited — max reasoning`
- `large — high reasoning (defaults)`
- `medium — medium reasoning`
- `small — low reasoning, fast`

**(b) Apply it.** Build the working table from the defaults, and on a
re-run keep any role the user changed by builder or model. The budget
sets each builder's effort: Codex reasoning effort, Claude Code's
model/effort where settable, and agy's model variant
(Gemini 3.8 Flash High / Medium / Low, or a thinking model for
unlimited). `unlimited` escalates hardest/judgment roles to the
strongest available thinking model (agy's Claude Opus 4.6 Thinking);
`small` drops every role one effort step and prefers the fastest
variant. If the resulting model is not a detected one, use the same
builder's closest detected model at or below the target, else mark
the role as needing a choice.

**(c) Show the roles and confirm.** Show every role with its builder
and model, marking any undetected builder/model as needing a choice.
Also list each line step 2 dropped. Ask whether to accept as-is or
change specific roles, offering the detected builders and models as
the options. For arena/panel roles (competing directions), the value
is a list, and one builder runs per entry — list length sets the
count. A builder never judges its own direction; the judge is a
different builder (or Muse) from the builders in the panel.

### 4. Validate

Every builder/model written must be in the detected set. If a chosen
builder or model is not available, stop and ask again. Never write a
model slug you have not confirmed.

### 5. Write the rule

Write `docs/builder-routing.md` with a `# budget`
line and one line per role, using the same labels the workflow uses.
Overwrite the whole file so re-runs stay idempotent. Shape:

```
# builder routing. One line per role. Delete a line to fall back to the default.
# budget: large (high)

# Builders: codex (gpt-6.1-sol), claude-code (Sonnet 5.5),
# agy (Gemini 3.8 Flash High), muse-subagent (designer/engineer files)

mechanical builds, small sequential edits, tdd implementation: codex
prose, wording, design judgment, second-builder direction: claude-code
research, google-backed search, fast exploration: agy
design audit, engineering review, critique: muse-subagent
competing directions (arena runners): codex, claude-code, agy
hardest tasks, cross-builder judgment: claude-code, agy (opus thinking)
publishing, verification, notion, memory: muse (orchestrator only, never a builder)
```

Rules that always hold, whatever the table says:

- Website/coding work goes through a builder, never built directly
  by Muse (standing rule).
- The orchestrator (Muse) owns publishing, verification, Notion, and
  memory. Builders never publish on their own.
- Small sequential fixes default to a single builder (Codex) — three
  builders on one tiny change means merging three versions for
  little benefit. Parallel builders are for competing directions,
  genuinely parallel work, or explicit user requests.
- Token optimisation applies: split by concern with disjoint file
  sets, brief by pointing at files, cap outputs to findings.

### 6. Confirm

Tell the user the rule was written and that it applies to new work.
Re-running this skill updates it.

### 7. Offer a verification skill (optional)

Check whether the project has a way to drive the real app for proof
(a project-local verification harness, e.g. `test/check.js`, or a
`verify-*` skill). If not, offer once to generate a project-local
verification skill. On yes, build it; on no, move on without pushing.

### 8. Offer a formal verification skill (optional)

Check whether the project already has a formal verification skill
(a model checker, proof, or other verifier against the code). If
not, offer once to generate one. On yes, build it; on no, move on
without pushing.
