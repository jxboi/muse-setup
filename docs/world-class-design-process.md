# World-Class Design Process — for any site that needs to converge, not iterate forever

## Why the back-and-forth happens

Vibe feedback ("feels AI", "too flat", "still too flat") has no target.
Each pass optimises one feeling and disturbs another, because there is no
agreed definition of good, no reference set, and no finish line. Apple does
not converge by being right first time — Apple converges because every
iteration is judged against principles, prototypes, and a checklist, not
against a feeling in the moment.

This process replaces vibe iteration with a fixed sequence. No step is
skipped because it is slow; skipping a step is what creates the 100 rounds.

## The process

### Phase 0 — Intent and principles (one page, agreed before any design work)
Write, and get the user to correct once, not per round:
- Audience: who is this for, in one sentence.
- Job: the one thing a visitor must be able to do in 30 seconds.
- Feeling: three words the site must feel like, and three words it must
  never feel like.
- Principles, adapted from Apple's own (Clarity, Deference, Depth):
  - Clarity: hierarchy through size, weight, and space — never colour alone.
  - Deference: the interface gets out of the way of the content; no
    decoration that does not help reading or choosing.
  - Depth: layers and hierarchy give a sense of place; flatness and
    clutter are both failures, in opposite directions.
- Non-goals: what the site will not try to be (e.g. not a SaaS dashboard,
  not a marketing landing page, not a dark-mode showcase).
- Definition of done: the checklist in Phase 5, signed off once.

Example draft (replace with your own — this is a placeholder, not a spec):
- Audience: people choosing a tool for real work.
- Job: find an option worth trying, understand what it does, and try it
  — in under a minute.
- Must feel: trusted, editorial, precise. Must never feel: generated,
  templated, salesy.
- Non-goals: not a dashboard, not a landing page, not a showcase of
  effects.

### Phase 1 — Benchmarks (study, never copy)
Pick 3–5 world-class references matched to the job, not to fashion.
For a field guide, candidates are Apple's product/documentation pages
(clarity, type, restraint), Stripe docs (information density done well),
and one print-editorial reference (a real magazine or manual, for
hierarchy and texture). For each, record in writing: what it does that
ours does not, in element-level terms (hero, index, entry, rule, type
scale) — never "it feels better". No logos, proprietary fonts, or
trademarked assets are copied; only patterns and values are learned.

### Phase 2 — Design system spec (written before building)
A versioned spec file covering, with exact values:
tokens (colour, type, spacing, radii, rules), type scale and pairing,
grid and container, every component and every state (default, hover,
focus, active, empty, error, loading), responsive behaviour at 320px,
768px, and 1200px, motion rules (preferably none, or one restrained
purpose), and accessibility targets (WCAG AA contrast, 44px targets,
keyboard order, reduced motion). Building without a spec is how
one-off tweaks accumulate into incoherence.

### Phase 3 — Directions, compared side by side (not one after another)
Build 2–3 complete directions in isolated folders from the same spec
brief — not sequential tweaks to the live site. Each direction is judged
on the same scorecard: the Phase 0 principles, the Phase 1 benchmarks,
readability, mobile behaviour, and accessibility. The user picks one
direction, once, against the scorecard. This is the single most
important change: it replaces "change it, then change it back" with
"choose between finished options".

#### Experimenting with agents, models, and thinking effort
Directions are also an experiment, not just a choice. Vary the builder
across directions and record what each produced, so the next project
starts from evidence, not habit:
- Builders: Codex, Claude Code, and agy — at least two different
  builders across the directions, each in its own isolated folder with
  disjoint files, briefed by pointing at the spec (never by pasting it).
- Models and effort: vary reasoning effort where the tool allows it
  (e.g. Codex default vs a higher-effort run; Claude Sonnet vs Opus
  where available; agy Gemini Flash vs a Claude model) and record
  model + effort + tokens/time for each direction.
- Comparison: judge directions blind to builder where possible — the
  scorecard scores the result, not the agent that made it. Record which
  builder/effort produced the winning direction and why, in the task
  log, so the orchestration improves with evidence.
- Cost control: experimentation is bounded by Phase 3 itself — 2–3
  directions, one brief, one scorecard. It is not a licence for
  unlimited parallel passes; token/time per direction is logged and
  compared, per the delegation-token-optimisation rule.

### Phase 4 — Bounded refinement (max 3 rounds, each against the spec)
Each round has a written critique, element by element, against the spec
and the principles — not a feeling. A round changes only what the
critique names. If a round's feedback is a new feeling with no element
attached, the first job of that round is to translate it into an element
and a principle before any code changes. Rounds stop at 3 or at the
definition of done, whichever comes first; a 4th round requires a spec
change, not another tweak.

### Phase 5 — Engineering and polish audit (every element, meticulously)
A full pass over every element, in order, with a checklist that must
all pass before shipping: header, hero, search, filters, index,
chapters, entries, guides, prompts, copy states, footer, built-with,
empty state, no-JS state, focus order, keyboard use, 320px layout,
768px layout, 1200px layout, contrast ratios, touch targets, and
performance (no new dependencies, no render-blocking additions).
This is the "every element well thought out" step — it is a checklist,
not a feeling, so it can actually be completed.

### Phase 6 — Freeze and ship
Ship the chosen direction, verify live, and freeze the spec. Later
changes go through the same process at small scale: a spec amendment
first, then a bounded round. No more live-site vibe tweaks.

## Feedback protocol (so one round does not become 100)

- Feedback names an element, the principle it breaks, and the feeling
  wanted — or the agent translates it into those three things at the
  start of the round, before touching code.
- One round, one critique document. No new directions mid-round.
- "Better" is judged against the Phase 0 words and the Phase 5
  checklist, not against the previous version's feeling.
- The user is the final arbiter, but only at the gates: principles
  (Phase 0), direction choice (Phase 3), and sign-off (Phase 6).
  Between gates, the agents work without asking.

## Roles

- Designer agent: principles, benchmarks, spec, directions, critique.
- Engineer agent: spec conformance, accessibility, performance, tests,
  and the Phase 5 audit (Standards + Spec axes, per the Matt Pocock
  workflow).
- Codex: builds, in isolated folders for directions, then the chosen
  direction only.
- Muse (orchestrator): owns the gates, the scorecard, publishing, and
  the rule that a round never starts without a written critique.

## What this deliberately does not promise

- One pass to perfection. Apple ships after many internal rounds too;
  the difference is that theirs are structured and convergent.
- A Figma file, a user-testing panel, or a real device lab — we do not
  have those. The substitutes are: isolated direction builds, live
  browser checks, a real phone glance from the user at the gates, and
  the Phase 5 checklist.
- That taste becomes objective. It does not. It becomes *arguable*:
  every choice can be traced to a principle, a benchmark, or a
  checklist item, so disagreements are about named things, not vibes.

## Evidence from research

What world-class teams actually do, in the parts that transfer to a
small code-built site:
- **Apple, 10 → 3 → 1:** ten genuinely different directions, three
  refined, one chosen — explicitly not seven straw men built to make
  three look good. Weekly demos where leaders act as arbiters of
  taste and decide on the spot, plus paired meetings kept separate:
  one to brainstorm without constraints, one to nail down production.
  Convergence needs a decider and a deadline, not another round.
- **Critique format that converges:** brief shared ≥24h before
  (goal, user, specific questions, constraints, fidelity level);
  silent written review first (what works / what is unclear /
  questions); walkthrough in 5 minutes, not a sales pitch; discussion
  in brief-question order, forcing specificity ("which element?");
  problems before solutions; feedback linked to goals, not taste
  ("I notice / I wonder / what if"); actions read back aloud as
  must-fix / should-fix / explore-later, with a named owner, kept in
  a searchable critique log. Critique is not approval and not
  flaw-hunting. Next session starts by checking the previous revision
  brief — including what was deliberately *not* changed and why.
- **Design systems as decision hierarchies:** principles first, then
  primitive tokens → semantic tokens (never used directly in
  components) → components (≤3–4 variants, every state specified) →
  patterns. A single `:root` token block *is* the system for a static
  site, provided nothing hardcodes a value elsewhere — lint for drift.
  Spacing from one base (4/8px, few values); type roles with size +
  weight + line-height + tracking; tabular figures for numbers; mono,
  small, tracked, uppercase only for labels/metadata.
- **Shared grammar of Stripe / Linear / Vercel / Apple:** aggressively
  high contrast, generous whitespace (double what feels like enough),
  monochrome + one rationed accent, sharp geometric type, hairline
  rules + large gaps as separators (not shadows or background shifts),
  left-aligned prose, one message per section. Copy the discipline,
  not their exact values (Stripe indigo, Vercel greyscale, whisper
  weight 300) — cloning values produces pastiche.
- **Small-team approximation:** no design-to-engineering handoff —
  iterate in code, the real medium. Private preview (self + 2–3
  readers) long before public; polish in many small cycles; dogfood
  daily (browse it on a phone, keyboard-only, slow connection).
  Fidelity rises monotonically: early critiques judge structure,
  late critiques judge polish only — re-litigating structure at
  polish stage is a process failure.
- **Definition of done, kept short enough to follow:** tokens only;
  all states present (hover/focus/visited, empty, missing); responsive
  at 320/768/1280px and 200% zoom; contrast ≥4.5:1 normal / 3:1 large,
  body ≥16px, line-height 1.4–1.6 for UI/chrome text and 1.65–1.8
  for serif prose and display headings (amended after a polish
  audit on the author's project: body 1.65, prose 1.7, hero 1.8 are
  the measured editorial values for that kind of page, and reflowing
  the site to fit the old 1.4–1.6 band would trade readability for
  a number — the band was wrong for that kind of page, not the
  site; measure your own values rather than copying these),
  line length 45–75ch; keyboard +
  screen-reader pass; critique log with must-fix closed; copy proofed
  in place; smoke-tested on the preview URL. Automate what can be
  automated (token grep, contrast/axe scans); a DoD nobody follows is
  worse than a shorter one everybody does.
- **What not to copy:** Apple's org chart, secrecy, and scale; taste
  as a job title without weekly involvement; exact brand values;
  a big design system for a small site (Linear's 98 → 3 theme
  collapse is the warning); minimalism without foundations (removing
  decoration before hierarchy is specified just looks unfinished);
  critique as voting or redesign by committee; A/B-testing your way
  out of judgment at small scale; motion/shadows/gradients/second
  accents "for polish".

Caveats from the research itself: Apple 10-3-1 and internal-process
details rest on a 2008 talk, a 2018 book, and secondary reporting —
mechanics corroborated, exact current practice unverified. Stripe /
Vercel token values are secondary reverse-engineering, not official
documentation — treat as observed patterns to adapt, not specs to
clone. The current HIG frames the same ideas as foundations/patterns/
components; Clarity/Deference/Depth are long-stated themes, not a
quoted current official list.

## Comparison: a second, independent research pass

A second research pass corroborated the findings above almost exactly —
same loop, same Apple mechanics, same critique format, same token
hierarchy, same DoD gates. That convergence across two independent
research engines raises confidence in the process mechanics. Distinct
additions worth keeping (not contradictions):
- **Shape Up tools for the diverge phase:** breadboarding (places /
  affordances / connections, text-only flows) and fat-marker sketches
  (thick strokes only, no micro-detail) before code — use these to
  keep the 3 directions genuinely structural, not colour tweaks.
- **Critique source named:** Connor & Irizarry, *Discussing Design*
  (O'Reilly) — anchor critiques to objectives and criteria, not
  reactions. Same 45-minute protocol as above.
- **Heading gestalt:** more space above a heading (32–40px) than below
  it (12–16px), so headings belong to the content below them;
  `text-wrap: balance` on headings to avoid orphan/widow wraps.
- **Index geometry spelled out:** fixed columns (number / name /
  category / summary / affordance), tabular numerals, 1px peer rules,
  2px document boundaries only — and never a third divider weight.
- **Token example caution:** a sample `:root` using a blue accent
  (#0055ff) and sans display type is a generic example, not a
  recommendation for your project, whose accent and type identity
  are already decided in its spec. Adopt the architecture, not the
  example values. Same caution as above: Stripe/Vercel values are
  reverse-engineered, not official specs.
- **Visited-link state** explicitly added to state coverage for a
  catalogue (scanned items must remain clear) — include it in the
  Phase 5 audit, where the first pass also named it.

No material contradiction between the two reports. The process above
stands; the second pass's additions are folded into Phases 2, 3, and 5
as noted.
