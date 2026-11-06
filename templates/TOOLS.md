# TOOLS.md - Local Notes

> Template: adapt this to your own setup. The paths, flags, and browser
> builds below are the author's setup — replace them with yours, and
> record your own usage-check commands here instead of dated snapshots.

Short, durable notes that make external tools work reliably in this
particular setup: device nicknames, host aliases, preferred voices, and
environment-specific quirks. Skills describe how tools work in general; this
file holds only what is unique here. Leave it empty until there is something
worth recording.

- **agy quota:** agy has no `usage`/`quota` subcommand; check quota with `agy -p "/quota"` (or `/quota` in an interactive session). Quotas are pooled per model family (Gemini vs Claude/GPT) over 5-hour and weekly rolling windows.
- **Claude Code usage:** `claude -p "/usage"` reports current session % + reset and current week (all models) % + reset.
- **Codex usage:** no quota/usage subcommand; `codex login status` only confirms the login. `/status` (interactive slash command, also works as an exec prompt) reports session status only — workdir, model, provider, tokens used in that session — not remaining quota or reset time. Per-run token counts come from exec footers only; remaining allowance lives on OpenAI's side.
- **Narrow-width render:** the checking browser cannot resize its viewport, so do narrow renders with local Playwright instead: launch a local Chromium with `--no-sandbox --disable-dev-shm-usage --disable-gpu` plus the WebGL domain policy flags (the author's setup used `--hatch-webgl-allowed-domains-file=<path>` and its sha256 as `--hatch-webgl-allowed-domains-sha256`; without those, Chromium exits with "Hatch WebGL domain policy is required"). Localhost navigation is blocked by local network access checks, so serve files by page.route interception to a `.local` host (e.g. `http://phone.local/`) instead of an http server. Measure scrollWidth vs viewport, overflow elements, and star/name bounding boxes at 320px and 360px.
- **Testing browser pin:** pin a known-good Chromium build for Testing runs and record its version, revision, and install path here (the author's setup hand-unpacked one after the standard `playwright install chromium` failed). Use the pinned build for Testing runs until re-pinned.
