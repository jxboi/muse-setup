# Design Hill Climb

> Adapt this: the Codex and Claude Code commands below use the author's
> binary paths (`~/.local/npm/bin/codex`, `~/.local/npm/bin/claude`).
> Replace them with the paths on your machine. The model defaults
> (Codex gpt-6.1-sol, Claude Code Opus 5.5) are the author's defaults;
> set your own in `docs/builder-routing.md`.

Build a design page from a reference screenshot and climb its score until every rubric category is at least 8.5. Use it when the user supplies a reference screenshot and asks to build it, to run a hill climb, or to reach 8.5 in every category.

## Roles

- Muse orchestrates only and never builds the page directly. The artifact builder is not an acceptable implementer because it is Muse's own agent.

- Codex implements every version. Use gpt-6.1-sol with high reasoning effort. Do not use Astra unless the user explicitly says so.

- Claude Code scores every version. Use Opus 5.5 with medium effort, blind to prior scores and to the implementer. The scorer never implements what it scores and the implementer never scores its own work.

- Muse verifies each build from the files, captures screenshots, publishes only when asked, and keeps the climb log current.

## Setup

- Create a work directory at `evals/<project>-climb/` with a `v1-codex` subdirectory and an `annotated` subdirectory for arrowed screenshots.

- Copy the reference screenshot into the work directory as `reference.jpg` and keep it there as the single source of truth for the target design.

- Write a rubric instance at `evals/<project>-design-rubric.md` from the project-agnostic master at `evals/design-rubric.md`. Keep the ten criteria unchanged and supply only the evidence anchors for this project, with exact values, names, counts, and labels from the reference.

- Create a user goal for the climb with `user_goal.create` and write the setup notes into its `GOAL.md`, including the reference path, the rubric path, the work directory, and the known constraints.

- When the reference shows portrait photos, generate illustrated portrait stand-ins for each named person with `media.generate_image` and save them under the climb work directory. Disclose them as generated stand-ins in the page and in the log. A stand-in caps the Imagery criterion at 5, so real portrait files from the user are the only way to reach 8.5 there. Say this to the user up front.

## Build version one

- Brief Codex to build a single self-contained HTML file that recreates the reference as faithfully as possible. Give the exact layout, every data value, every label, the interactions that must work, the responsive behavior at desktop and phone widths, and the requirement of zero external requests.

- Attach the reference image to the Codex call. Pass the prompt through stdin with a trailing `-` because the argument form fails when an image is attached. Run it as `~/.local/npm/bin/codex -m gpt-6.1-sol -c 'model_reasoning_effort="high"' exec -C <work-dir> -s workspace-write --skip-git-repo-check -i <reference.jpg> - < <brief-file>`.

- Tell Codex to embed supplied portraits as small WebP data URIs, each with name alt text, a tooltip disclosing a generated illustrated portrait, and an initials fallback.

- Tell Codex to report the file size, sha256, counts of data image and data font URIs, confirmation of zero external URLs, and the result of `node --check` on the inline script.

## Verify each version

- Check the built file yourself. Confirm the size and sha256, count `data:image` and `data:font` occurrences, confirm zero external `http` URLs in sources and styles, and run `node --check` on the extracted inline script.

- Spot-check the data against the reference. Confirm every number, name, date, and label. A wrong value or a missing item is a real gap, so record it instead of trusting the builder's report.

- Render the page at 1440 by 900 and at 390 by 844 with your own screenshot harness. Command shape: `node <your-shot-script> <html> <width> <height> <out.png>`. Remove the harness profile directory between runs and never use `pkill -f` in the same command string.

- Read both screenshots yourself and compare them with the reference. Note what differs before you send the version for scoring.

- Capture a before and after screenshot for every visible UI change and show both with the change report.

## Score each version

- Brief Claude Code to score the built page against the reference using the rubric instance. Give it the reference image, the built file, the rubric, and the rendered screenshots in a dedicated score directory.

- Ban prior score files and environment listings in the scoring brief so the score stays blind. Require per-criterion scores with one concrete evidence sentence each, the overall mean to one decimal, the single biggest gap, and what would raise the score most.

- Run it as `~/.local/npm/bin/claude --model claude-opus-5-5 --effort medium --allowedTools "Write,Edit,Bash,Read" -p "<prompt>"` after changing into the score directory. Ask it to write the report to a score file there and to report the scores in its final message.

- Treat the score as a report, not a verdict to argue with. A known stand-in is a real gap and scores as one.

## Iterate to 8.5

- Brief Codex to fix the gaps the score named, starting with the biggest gap. Keep the scope to the scored findings plus any regression the fix would introduce.

- Re-verify the new version from the files and re-render both screenshots. Then send it for a fresh blind re-score.

- Repeat until every criterion is at least 8.5. The overall mean does not count. Every single category must reach 8.5.

- Stop after three consecutive regressions that fail to beat the best build. Pause the climb, keep the best build as the current best, and report the plateau to the user. Do not launch further iterations without the user's direction.

- Record the score progression and the binding constraint in the goal and in memory so the next conversation starts from the right facts.

## Log the climb

- Keep a climb log in a Google Doc. Create it as a local document artifact, upload it with the Drive update call, and verify it by reading it back. Record the character count and the inline object count after each update.

- Log every version with its score, the biggest gap, the changes made, and the before and after screenshots. Add numbered red arrows to the after images that point at the changes, with an arrow key underneath, except where a viewport shows no visible change.

- Keep the log current through the latest scored version. A version counts as logged only after the read-back confirms it.

## Publish when asked

- Publish only when the user asks. Never publish a regression. Never publish overnight.

- Push the landing page and the publishable versions in one consolidated push with a single approval card for the push itself. Verify the result through the live site with `curl`, not through GitHub reads, so the push needs only that one card.

- Confirm the live page is byte-identical to the staged files and that every new URL returns 200. Append the sync entry to the push snapshot JSON with an atomic replace.

## Report

- Report each version to the user with its score, the biggest gap, and the next fix. Report the final result with the best build, its per-category scores, the live links when published, and the climb log link.

- Report where each piece of work is implemented and how it is delegated, naming the builder, the model, the surface, and how the work is split.

## Available connectors

- [GitHub](/opt/hatch/connectors/github/CONNECTOR.md) for publishing the built versions to a repository.

- [Google Docs](/opt/hatch/connectors/google_docs/CONNECTOR.md) for reading and verifying the climb log.

- [Google Drive](/opt/hatch/connectors/google_drive/CONNECTOR.md) for uploading the climb log document.

- [Vercel](/opt/hatch/connectors/vercel/CONNECTOR.md) for hosting the published versions.

- Use `muse.connector_search` to find other connectors and `muse.skill_search` to find related skills.

- If nothing here fits, look on the web for a maintained API or MCP server and set it up with [Custom Connectors and MCPs](/opt/muse/skills/custom-connector-setup/SKILL.md), or build a custom connector.
