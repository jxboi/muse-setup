# Design evaluation rubric (project-agnostic)

Scores any built design page against its reference screenshot. The criteria never
change between projects; only the evidence anchors do — exact values, names,
counts, and labels are supplied per evaluation as a companion sheet (for
example, the my-project instance at `my-project-design-rubric.md`).

## Scale and rules

- Each criterion scores **0–10**. 10 means indistinguishable from the reference, or
  fully meeting the bar as stated.
- **Overall = mean of all 10 criteria, to 1 decimal.**
- Every score needs **one concrete evidence sentence** (what was observed, where).
  A criterion that could not be observed scores **0** with "not observed" — never
  guess, and never re-score the builder's own audit claims without observing them.
- Do not inflate: a known stand-in is a real gap and scores as one.

## Criteria

1. **Layout fidelity** — Structure, order, and proportions of the page's regions
   match the reference at desktop width. 10 = same structure and proportions.
2. **Visual fidelity** — Colors, typography (sizes and weights), spacing, corner
   radii, and shadows match the reference. 10 = no visible difference at a glance.
3. **Content accuracy** — Every text label, number, name, and value exact and in
   the reference order. One wrong value caps this at 7; a missing item caps it
   at 5.
4. **Component accuracy** — Every UI element in the reference (inputs, buttons,
   tabs, cards, tables, badges, navigation) present and rendered as shown.
   10 = every component present and rendered as in the reference.
5. **Imagery & media treatment** — Photos, portraits, avatars, icons, and
   illustrations match the reference. Stand-ins (initials, placeholders, generic
   icons) are real gaps: if stand-ins replace reference imagery, this scores
   **at most 5**, with the gap named.
6. **Interaction correctness** — Every interaction specified for the page works
   (search/filter, tabs, toggles, navigation, collapse, forms). Each broken
   interaction deducts; 10 = all work as specified.
7. **Responsive behavior** — No horizontal overflow or clipped/unreachable
   controls at desktop (1440px) and phone (390px) widths. 10 = clean at both.
8. **Accessibility** — The page is usable by people with different abilities:
   body text meets a 4.5:1 contrast ratio, interactive elements have visible
   focus states and touch targets of at least 24×24px, images carry meaningful
   alt text, the page uses semantic landmarks and a logical heading order, and
   motion respects reduced-motion preferences. 10 = meets the bar across the
   page; each gap deducts.
9. **Design quality (taste)** — Hierarchy (the most important info is apparent),
   restraint (no decoration beyond the reference), cohesion (one visual system),
   no clutter. 10 = reads as a production page, not a demo.
10. **Completeness** — Every element in the reference is present; nothing added
    contradicts the reference (extra chrome must be disclosed, not hidden).
    10 = nothing missing, nothing contradictory.

## Report

Per-criterion score (0–10) with one evidence sentence each, the overall mean to
1 decimal, the single biggest gap, and what would raise the score most.
