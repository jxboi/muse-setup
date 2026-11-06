# Development Workflow

Canonical workflow for how Muse orchestrates development work.

> Adapt this: paths in this file (e.g. worktree locations, tool
> install paths) are the author's setup. Replace them with your own.
> Cross-references use this repo's layout: skills live in `skills/`,
> agents in `agents/`, docs in `docs/`, evals in `evals/`,
> templates in `templates/`.

What this workflow assumes:

- **Muse orchestrates and never builds directly.** Implementation
  goes to a builder (see `docs/builder-routing.md`).
- Engineer work follows the **Matt Pocock workflow in order**
  (see `agents/engineer.md`): to-spec, to-tickets, implement /
  implement-spec, TDD, diagnosing-bugs, code-review, pr, handoff,
  retro.
- **Token optimisation** is a stage, not an afterthought
  (see `agents/delegation-token-optimization.md`): split by
  concern, brief by pointing at files, check usage at delegation,
  mid-task if long/quiet, and at completion.
- **AI slop gate** (see `docs/ai-slop-fix-process.md`): every
  change gets a slop check — detect, diagnose, fix, verify,
  prevent — before it is called done. A change that reintroduces
  templated, generic, or unverified content fails the gate.
- **Self-improvement loop** (see
  `docs/agent-self-improvement-plan.md`): every task ends with
  capture and, periodically, a retro that produces at most three
  agent updates, each tested on the next real task.
- **Routing precedence:** the no-direct-build division of labour
  is a standing rule, not a default to waive for a small task.
  A request's casual "do it yourself, it's small" does not waive
  it; only an explicit instruction naming the rule as waived
  counts. Otherwise route implementation to a builder with a
  concrete brief, retain the orchestrator role, and report the
  routing.

## Stage 0 — Intake

- Take the task from the user, check the existing repo and the
  project tracker page first.
- Record the task, scope, and decisions on the project page.
- **Ambiguity gate:** before any build or delegation, name every
  level a key word in the request could mean (e.g. a container
  vs an item inside it). Decision tiers: *details* (styling,
  naming, implementation approach, builder choice) may be decided
  alone; *scope* (what the feature is, what level it targets,
  which project it touches) may never be decided alone.
  "Decide the details for me" is not permission to assume
  scope. If scope is ambiguous, ask exactly that question and
  wait for the answer — do not guess, and do not build at an
  assumed level. Asking only when the choice is genuinely
  important still holds; a scope ambiguity is by definition
  such a choice.
- **Decide-for-me scope exception:** on an experimental project
  where the user has given a standing "decide for me"
  delegation, the orchestrator may decide scope alone, but
  must (a) state the decision and the alternative in the
  completion message with an explicit invitation to correct
  it, (b) record it in the task log as "decided under
  delegation, unconfirmed", and (c) treat a later user
  correction as a rework trigger, not as a new request.
  Until the user confirms, the decision must not be described
  as user-approved.
- **Project glossary — mandatory:** every project maintains a
  glossary, updated in the same pass as the work that touches
  it, not afterwards. Location: `GLOSSARY.md` in the project
  workspace; if a project has no workspace folder, a Glossary
  section in its task log / spec file. Each entry: term,
  one-sentence definition, source (doc, repo file, or dated
  decision), and what it is NOT (the confusable term, e.g. a
  container is not an item). A grill opens by defining the
  contested term against its docs, and that definition is
  written into the glossary before building. A pass is not
  complete if it introduced, redefined, or resolved an
  ambiguous term without recording it. Existing projects gain
  their glossary on their next pass; no separate retroactive
  glossary project is needed. This is the project's internal
  glossary for agents, separate from any user-facing site
  glossary.
- **Gate-conflict check:** before building, check whether any
  file in scope is pinned by a hash, gate, or fixture. If so,
  name the conflict up front — the feature will break the pin —
  and agree the path before editing that file: stop and hand
  back the diff for orchestrator review (default), or an
  undo-helper plan (Stage 5). Do not edit the pinned file
  first and sort the gate out afterwards.
- **Gate-conflict check before approval:** before the user
  approves a change, inventory the pinned surface including
  dependent pins; the approval must name the gates and their
  values. A pin added or changed after approval needs a fresh
  approval — it is not covered by the earlier one.

## Stage 1 — Spec and tickets (Matt Pocock)

For anything non-trivial:

1. **to-spec.** Synthesise a spec from what is already known, do
   not interview. Sections: Problem Statement, Solution, a long
   numbered list of user stories ("As an <actor>, I want a
   <feature>, so that <benefit>"), Implementation Decisions
   (modules, interfaces, contracts, architecture — no file paths
   or code snippets, unless a prototype snippet encodes a decision
   better), Testing Decisions.
2. **Agree test seams up front.** The public boundaries where
   behaviour will be observed, preferring existing seams and the
   highest seam possible. No test at an unconfirmed seam.
3. **to-tickets.** Break the spec into tracer-bullet vertical
   slices, each narrow but complete through every layer, demoable
   or verifiable on its own, sized for one fresh context window,
   each declaring its blocking edges. Present title, blocked by,
   and what it delivers for approval; iterate until approved.
   Prefactoring first. Wide refactors go expand-contract.
   Ticket approval: on experimental projects, the orchestrator
   may approve the breakdown; on any other project, the user
   approves it before building.
4. **Adaptation (recorded, not hidden):** no issue tracker is
   configured, so spec and tickets live in the task log / spec
   file, not in a tracker. (If you do use a tracker, say so
   here instead.)
5. **Density budget:** every content pass states, in the spec,
   a maximum visible (collapsed) word count and a maximum
   expanded word count per unit (per card, per section, per
   page — whichever unit the pass touches), plus a site-total
   cap. Collapsed-by-default does NOT count as removal: the
   expanded budget must be met by actual cuts, not by hiding
   words behind a toggle. Budgets here are measured
   discipline, not blocking ceilings: state the measured
   visible and expanded totals in the spec, consider a cut
   or compression when a pass grows, and never treat a
   stated figure as a hard ceiling that blocks a pass by
   itself unless the user explicitly ordered that figure as
   a ceiling for that pass.
6. **Foundational-thinking gate:** before tickets are approved,
   the spec must name its core types / data structures, the
   scaffold-vs-feature order, and what concurrent actors share.
   If those are unsettled, the spec goes back — it is not ready
   for building, however complete its user stories look.
7. **Spec-claim source check:** any absolute claim in a spec —
   "never", "none", "not a", "appears nowhere", "only" — must
   quote its source line, or be rewritten as a scoped identifier
   rule (what was checked, where, and what would falsify it).
   A spec carrying an unsourced absolute is not ready, however
   complete its user stories look.
8. **Baseline-quote check:** every string, markup pattern, or
   count a spec asserts about the baseline must quote its
   baseline source line. The orchestrator checks the spec's
   baseline claims against the fetched baseline tree before
   briefing the builder — summary text, row markup, list
   element, pinned counts — not only the input data. A spec
   whose baseline claims are unquoted is not ready, however
   complete its user stories look.

## Stage 2 — Delegate, split for token efficiency

- Split by concern with disjoint file sets: designer = design /
  prose, engineer = code / review, Codex = mechanical edits and
  builds, browser = live checks only, Claude Code = independent
  review or second build, agy = parallel build of a separate
  piece.
- Brief by pointing at files, never pasting them. Name scope,
  must-not-break, and one deliverable per agent. Explicitly say
  what to ignore from unrelated work.
- **Claude Code brief form for edits:** a brief for edits to
  existing files must include Edit — the form is
  `claude --model claude-opus-5-5 --effort medium
  --allowedTools "Write,Bash,Read,Glob,Grep,Edit" -p "..."`.
  Write alone covers new files only; a brief for new files
  only may stay Write,Bash.
- **Review briefs:** a review brief ships the diff + the spec +
  the symbol list + the harness result line — and nothing else:
  never the build transcript, never the builder's own report.
  Review output is severity-graded. Test and gate changes are
  audited as P0: any change to a gate, fixture, hash, or
  threshold needs the same explicit approval as a gate change.
  The brief leads with the harness result; a review of
  unverified work is not a review, and review never substitutes
  for verification.
- Keep sequential work together (spec → tickets → frontier
  implementation). Parallelise only independent concerns.
- Check usage at delegation, mid-task if long or quiet, and at
  completion. Record agent, time, tokens if reported, and waste
  in the task log.

## Stage 3 — Build in isolation, with TDD

- Each builder works in its own folder or on the agreed files,
  so no one overwrites another. Muse compares outputs, keeps the
  best, and merges into the main version.
- **Git worktrees for parallel builders:** parallel builders work
  in git worktrees — one tree per builder, never two builders in
  one tree. A worktree is created from the verified main head;
  work merges back through the normal publish path, never by
  copying between trees.
- **Scratch-tree isolation and shared-input pinning:** each
  builder works in its own scratch tree, built from a verified
  baseline, not from another builder's in-progress tree. Any
  input shared across trees (a baseline file, a fixture, a
  harness input) is pinned per tree — its identity recorded —
  or reconstructed in that tree before building, so a tree
  never silently inherits a changed shared input. If trees
  diverge on a shared input, stop and reconcile the input
  before merging, rather than merging outputs built on
  different inputs.
- **TDD loop for every behaviour change and bug fix:** red
  before green, one seam / one test / one minimal implementation
  per cycle, tests at public interfaces only, no implementation-
  coupled or tautological tests, refactoring in review, not in
  the loop.
- **Bugs:** `diagnosing-bugs` first — build a tight feedback loop
  that goes red on this bug, tighten it, then fix with TDD. No
  loop, no hypothesised fix. Redact secrets from all outputs.
- **Attack-the-premise (pstack principle — diagnosing bugs):**
  if two fixes fail the same gate, stop and test the premise
  both fixes shared directly against the evidence before
  writing another fix. A third fix on an untested premise is
  not allowed. Explain the premise, the evidence, and the
  conclusion in a user-facing message or report (not private
  thinking alone) — the explanation need not precede the
  fix, but the test itself must. The final report carries
  three labelled lines — Premise, Evidence, Conclusion —
  so the diagnosis is never left in thinking alone. This is
  the diagnostic counterpart to the foundational-thinking
  gate in Stage 1: that gate settles the foundation up front;
  this rule fires mid-work when the foundation itself is what
  keeps failing.
- Small, verified steps over big unverified rewrites, unless a
  full rebuild is requested. One change per commit, one concern
  per change. No new frameworks, build steps, or dependencies
  unless the task explicitly asks.

## Stage 4 — Review (two axes) and slop gate

- **code-review** against a pinned fixed point, on Standards
  (repo standards plus the Fowler smell baseline, as judgement
  calls, overridden by documented repo standards; mechanical
  violations become deterministic checks) and Spec (does the diff
  do what the spec / tickets asked?).
- **Hill-climb iteration review:** after every hill-climb
  iteration, an independent agent — one that did not produce
  the work — reviews it, report-only, against the iteration's
  own claims. The iteration is not done until the review's
  findings are dispositioned (fixed, or recorded as known).
  A review never edits the work it reviews; fixes go back
  to the producer.
- **Slop gate:** run the AI slop process on the change. P0
  (wrong / misleading) and P1 (wasteful / duplicated) must be
  fixed before publishing; P2 (generic) is fixed when it can be
  fixed without inventing claims; P3 (polish) is recorded, not
  silently dropped. Do not invent install commands, star counts,
  links, or repo identities — verify each against its source.
- **Density gate:** the review checks the spec's visible and
  expanded budgets against the shipped output. "Accurate,
  sourced, but far too much" is a review failure, not a pass:
  a change that adds words above the expanded budget without
  its removal/compression ticket does not publish, however
  clean its tests are.
- **Composition by principle, not by count:** as much as
  possible, do not hardcode a specific gate (no permanent
  max-blocks or max-elements number). The decision to keep,
  move, or remove a visible element comes from a design
  principle, applied element by element and recorded in the
  spec: does it serve the screen's one primary action? Does
  another element already do this job (duplicate → combine
  or drop)? Does it add hierarchy or noise for a reader
  deciding in under a minute? An element that fails those
  questions moves or goes, and the spec states the principle
  reason for each one. A number reached in one pass (e.g.
  16 elements down to 8) is that pass's outcome, never a
  standing gate copied into the workflow or harness for
  future passes — the next pass re-derives its list from the
  principles and may land on a different number.
  Tightenings, all part of the same rule:
  - A principle reason counts only per element, stated for
    that element — a bulk list ("these seven moved") and a
    count justification ("seven had to go", "to hit 8") are
    not principle reasons, and calling the number an outcome
    does not excuse a decision whose reason is the number.
  - A disclosure is not a hiding place for duplicates: moving
    a duplicated element inside a disclosure leaves the
    duplication unresolved — a duplicate is dropped or
    combined into one element (one action, not two
    destinations in different places), and a move inside is
    only for an element that is genuinely secondary detail.
  - One symbol, one meaning: a glyph that means one thing
    (e.g. a star for favourite) must not also label a
    different datum (e.g. a star count) — resolve it to plain
    text or move the datum, with its own reason.
  - Hiding an element (`hidden`, display:none) to reach a
    number is not a principle decision: it is a removal, it
    needs a removal reason, and hiding a functional component
    (a disclosure, a favourite control) is removing a
    feature, not trimming.
  - When a requested number conflicts with principle
    decisions, the principle governs the method — decide
    every element by principle, implement those decisions
    through the normal route, and report the resulting
    number and the conflict plainly. A clear request that
    conflicts with a principle is not a scope ambiguity and
    is not handed back as a question; asking is for when
    the request's meaning or scope is unclear. Analysis
    without implementation is not compliance: per-element
    reasons must appear in the artifact, not only in prose,
    and "every element" includes newly requested ones —
    lumping ("these stay") is not a reason for any of them.
  - Duplicate means the same job AND the same content or
    destination, not merely related or overlapping wording.
    A description (what something is) is not a duplicate of
    a Use-when line (when to use it), and core elements —
    title, description, Use when, Inside list, Favourite —
    are never duplicates of each other. When an element is
    secondary but not a true duplicate, move it inside a
    disclosure rather than deleting it; deletion is only
    for a true duplicate or an element whose own principle
    reason says it goes. When in doubt between move and
    delete, move.
  - Composition is judged across instances of a repeated
    component, not only within one instance. An element
    earns its place by what it adds beyond its context and
    beyond its siblings. Wording the container already
    states — e.g. a label inside a card that restates what
    the card is — is not information, however true it is,
    and identical wording repeated across every instance
    adds nothing the second time it appears. Past approval
    does not exempt wording from this judgement: a label
    approved for one quality (honesty) is still judged for
    another (redundancy). Laying all instances side by side
    is one way to see it; the check itself is the principle,
    not the procedure.
- **Housekeeping before you add (subtract-before-you-add —
  polish and change passes):** every pass starts with
  housekeeping and lint on the area being touched — identify
  dead weight and redundant copy, lint/format issues, dead
  CSS/tokens, stub references — and tidy what can be tidied
  within the user's constraints BEFORE adding anything, so
  additions build on a tidier base. This is a habit, not a
  stop gate: if a constraint (e.g. keep all copy) blocks a
  specific cut, do the housekeeping that is still allowed,
  say plainly what could not be cut and why, then proceed —
  do not block the requested work, and do not use the
  constraint as an excuse to skip housekeeping entirely. An
  addition inside a polish pass must be paid for by a cut in
  the same pass, unless the user explicitly ordered the
  addition (in which case its measured word cost is stated
  instead). Editorial voice (e.g. hero slogans, chapter
  ledes) is not dead weight and is not removed under this
  rule. Sequence that makes this checkable: (1) explain, in
  a user-facing message or report (not private thinking
  alone), what dead weight you found, what editorial voice
  you preserved rather than treating as dead weight, and
  what cuts were blocked and why — the explanation need not
  precede the first edit, but the housekeeping edit itself
  must precede additions, and the final report carries three
  labelled lines — Dead weight found, Editorial voice
  preserved, Cuts blocked and why; (2) make a
  housekeeping-only edit first, with no additions in it, and
  verify the tidy base (diff or grep) before proceeding —
  bundling housekeeping and additions into one write is not
  housekeeping-first, whatever the report says; (3) then
  make the additions edit; (4) measure the added words from
  the diff of added lines by script and state the exact
  integer with no hedging (never "about", "roughly", or "~").
- Fix review and slop findings in a single follow-up pass, then
  re-review if the fix was non-trivial. **Block re-review rule:**
  a Block verdict's fix round must be re-reviewed by an
  independent agent — one that did not produce the fix —
  before the artifact is treated as done or used as a brief.
  Direct reading/grep verification alone closes Minor and
  Approve-with-issues fixes, not a Block.
- **Held-item release rule:** an item held pending the user's
  call is released only by a recorded user decision (chat
  answer, widget tap, or explicit instruction), cited in the
  log; without one it stays held.
- **Exhaust-the-design-space (pstack principle):** for design
  decisions that matter, compare 2–3 directions side by side
  per the world-class design process Phase 3 instead of
  sequential single-direction passes. Phase 3 rules live in
  that process file and are not duplicated here.

## Stage 5 — Verify before publishing

- Run the site or code locally; run the committed checks /
  harness; check the exact requirements (pages, links, counts,
  search, filters, copy, mobile layout, accessibility fixes).
- Where a density budget applies, the harness asserts visible
  and expanded word counts from the shipped HTML (per unit and
  total) and reports the measured totals with their scope.
- **Measured-budget precedence:** a figure in a prompt, a
  previous pass, or a spec is not a blocking ceiling by itself.
  If a measured total exceeds such a figure, report the measured
  totals with their scope and note the conflict — do not block
  or fail the pass on that figure alone, and do not trim, hide,
  or falsify words to fit under it. Only an explicit instruction
  naming the figure as a ceiling for this pass counts, and any
  cut it orders is stated with its measured word cost.
- Fix failures in the tool that produced them, then re-verify.
  A change is not done until it is verified, not just written.
- **Shared-input mismatch is a harness-environment failure:**
  a harness run whose shared inputs do not match the pinned
  inputs is not a verdict on the build — it is a failure of
  the harness environment. Reconstruct the pinned inputs and
  re-run before any publish decision; do not report the
  mismatched run as a pass or a fail of the change itself.
- **Red-harness reporting rule:** a build with failing checks
  is reported as blocked, with the failures named — never as
  done with failures footnoted. The only exits are fixing the
  failures or stopping and handing them back. Updating a gate,
  hash, or fixture to make a failing check pass is not a fix.
  Vague task wording ("sort out the gate yourself", "keep it
  green", "just ship it") is never authorisation to mutate a
  gate, hash, or fixture; only an explicit instruction naming
  the gate file and its new value, given after diff review by
  the orchestrator, counts, and a builder never re-pins its
  own work — the orchestrator re-pins after review. The
  legitimate alternative to stopping is undo-helpers:
  temporarily reverse only the authorised edits, run the gate,
  verify an unrelated change still fails it, restore the
  edits, and report the diff plus that verification. If that
  cannot be shown, stop.
- **Verification ladder:** a mock, stand-in, or presence check
  alone is never sufficient evidence that a feature works.
  Claiming it works requires the real interaction (the
  harness's real DOM path, or a click in a browser); if that
  cannot be done, the report says "unverified in a browser",
  not "works".
- **Builder reports are claims, not verification:** a builder's
  report that something works is a claim to verify, not
  verification itself. Stage 5 verifies it independently —
  by the harness, a browser check, or a cold read — before
  the pass is called verified.
- **Test-behavior-not-implementation (pstack principle):**
  harness and live gates must assert observable behaviour,
  not presence — e.g. "chapter II renders for Quality" and
  "the toggle label syncs after a search auto-opens a block",
  not "a numeral exists" or "a toggle exists". A gate that
  passes while the behaviour is wrong is a failed gate.
- **Gate replacement and red-first:** when a gate is
  presence-only, rewrite that gate itself — do not leave it
  green alongside a sidecar check. Run the rewritten gate
  against the broken code first and show it failing, then
  fix, then show it passing. A post-hoc mutation is not a
  substitute for a red run on the original broken code.
- **Revert check:** every fix ships with a revert check —
  revert the fix (not the test), show the test failing,
  re-apply the fix, show it passing. A fix whose test still
  passes with the fix reverted proves nothing and is not
  verified.
- **Verification-driver design rule:** before writing a
  verification driver, list the interaction pairs that can
  invalidate each other (e.g. reset × delayed popup, magnet
  radius × pickup radius, dense-cluster incidental
  collection); a driver must assert collections by identity
  (storage / live ids), never by attributing a collection to
  the leg that happened to be running, and must settle (poll
  live position) before asserting a collection at a distance.
  A driver that breaks on any collection, or dead-reckons
  time under a capped dt, is not ready.
- **Visual verification of changed elements:** after the
  behaviour checks, visually confirm the primary elements
  the pass changed — type, layout, and states — on the live
  page before calling the pass live-verified. A verification
  that checked behaviour only is reported as
  "behaviour-verified", not "live-verified".
- **Saved verification artifact for checks that bypass the
  harness or the browser task:** when a verification cannot
  run as a harness gate or a browser task (e.g. the checking
  browser cannot resize viewports), the pass folder must
  contain the re-runnable check (script) and its measured
  outputs (viewport widths, overflow counts, overlap counts),
  not only a narrative claim. A pass whose only evidence is a
  chat or memory note is reported as "behaviour-claimed", not
  "verified".
- **Before/after screenshots for visible UI changes:** every
  visible UI change ships two screenshots of the same view —
  before, from the pre-change build, and after, from the new
  build — captured in the browser harness, and both are shown
  with the change report. A UI-change report without the pair
  is incomplete.
- **Measured accessibility for accessibility-themed passes:**
  an accessibility-themed pass must report measured numbers —
  contrast ratios (WCAG AA), touch-target sizes, keyboard
  order — before it is called verified. A colours-only review
  is not sufficient evidence for a high-contrast or
  accessibility claim.
- **Read before verified:** before a pass is called verified
  or live-verified, a cold read of the shipped text, whole
  page, in order, blind to spec and history, must have
  happened, and its findings must be dispositioned — fixed,
  or recorded as known. Gates, diffs, and visual checks
  verify; they do not read. A pass with no cold read is
  reported as "unread", not "verified", however green its
  gates are.
- Never remove a feature, test, or check to make a change pass.

## Stage 6 — Publish

- Push to GitHub (your GitHub account, main branch). If the
  GitHub push fails, use the GitHub website upload as fallback
  (note: the web flow may split root files and subdirectories
  into separate commits — that is acceptable, record both
  hashes).
- Confirm the deployment is live at the project URL, and check
  the live site — not just the repo — visually confirming the
  primary elements the pass changed (see Stage 5 visual
  verification). Leave the live page in a clean state after a
  check.
- Engineer agents never push, publish, or deploy on their own;
  Muse owns publishing, after verification.
- **Live-check fallback and publish-status vocabulary:** when
  the sandbox browser cannot reach the live URL, a browser-task
  live check must be attempted and its result recorded; if no
  route can reach the live site, the publish is reported as
  "byte-verified, live-visual unconfirmed" (never as
  "live-verified"), and the user's confirmation is tracked as
  an open item in the task log until it arrives, not left as a
  caveat in one chat message.
- **Push-launch rule:** launch a push only when the user is
  awake — in practice, in the author's timezone, 07:30–08:00
  in the morning, never 23:00–07:00 unless explicitly
  coordinated (adjust these times to your timezone); check
  for pending approvals before relaunching (a live approval
  card may still be pending); run the push via background exec
  only; if the approval card expires, record the expiry rather
  than retrying. Stage 5 (verify + stage) and Stage 6 (push
  launch) are decoupled: verify and stage overnight, launch in
  the morning.

## Stage 7 — Track, hand off, retro

- Update the project tracker page summary and progress; file
  new pages under your project tracker (e.g. Notion — the
  author's choice); keep the workspace grouped; no duplicate
  pages for projects that already have one.
- **Mirror-sync:** when this workflow file is amended, update
  the corresponding page in your project tracker in the same
  pass; after syncing, verify by re-fetching the page and
  checking the amended term is present in its new wording. If
  the re-fetch does not show it, record the miss as drift in
  that day's retro — a sync that was sent but not seen on the
  page does not count as synced. If the sync cannot be done in
  the same pass, record the drift in that day's retro.
- **Parked-item return:** when a version closes and the next
  is parked — or at any natural break — return each parked
  user instruction with a single concrete surface proposal,
  not silence and not a fresh open question. A parked
  instruction is carried explicitly until the user names,
  confirms, or withdraws its surface; it is never treated as
  superseded by intervening progress.
- **handoff:** what changed (exact files), what was verified,
  what remains, next step. Ship notes use the `pr` template:
  Summary, Evidence (Before / After), Merge Danger (door,
  blast radius).
- **retro:** after the task, propose environment improvements
  (navigation pointers, automated checks, reviewer rules,
  wasted tool calls, missing information) in severity order.
  Turn at most three into agent updates, each tested on the
  next real task, under the self-improvement plan. A user
  correction is the highest-value log entry: it always
  produces an update, not just an apology. Daily retro runs
  in the early morning for the previous day, reported later
  that morning (the author's schedule: 03:00, reported
  08:00 — adapt to your timezone); per-task retros remain
  optional for small passes.
- Never paste secrets, codes, or keys in chat, except the
  one-time sign-in code a tool is actively waiting for, exactly
  as shown.

## Full-run checklist (use on every full run)

- [ ] Spec written, seams agreed, tickets approved, ambiguity gate cleared (levels named, scope question asked and answered if ambiguous)
- [ ] Glossary checked and updated (new, redefined, or resolved terms recorded with source)
- [ ] Foundational-thinking gate: core types, scaffold-vs-feature order, shared actors settled
- [ ] Spec-claim source check: every absolute claim quotes its source line or is rewritten as a scoped identifier rule
- [ ] Baseline-quote check: every baseline claim in the spec quotes its source line
- [ ] Work split by concern, briefs point at files
- [ ] TDD: failing test first, then minimal implementation
- [ ] Code review: Standards + Spec, fixed point pinned
- [ ] Slop gate: P0/P1 fixed, P2/P3 recorded
- [ ] Polish passes: housekeeping/lint done before any addition (subtract-before-you-add, as a habit not a stop gate)
- [ ] Local verification: checks + harness pass, gates assert behaviour not presence, no failing check reported as done, real interaction verified (not mock or presence alone)
- [ ] Verification driver: interaction pairs listed, collections asserted by identity, driver settles before asserting (where a driver is used)
- [ ] Design decisions that matter compared side by side (world-class process Phase 3), not sequentially
- [ ] Published: GitHub commit(s) recorded
- [ ] Live verification: deployment check passed, page left clean — or reported as "byte-verified, live-visual unconfirmed" with the user's confirmation tracked as an open item
- [ ] Project tracker updated, task logs updated, retro done
