# comfy-lab — ToDo

> The single durable task register. Committed, so every line stays generic: people's
> names, hosts, internal product names, credentials and customer-tied dates live in
> `CLAUDE.local.md` and are referenced, never copied here. Build and resolution dates
> are fine.

## Rules
- **ID:** `T-<SLUG>-<nn>`. Permanent, never reused, never renumbered. Numbered per track.
- **Slugs** come from CLAUDE.md → Tracks, plus `PROJ` for project/repo plumbing that
  belongs to no track. A new track is added to CLAUDE.md first, by agreement, never
  invented here.
- **An item is work that finishes.** Anything that recurs, or would never close, is a
  Standing operation (section 4): no ID, no checkbox.
- **Track and deliverable are fixed at creation.** If the deliverable changes, close the
  item with a pointer and open a new one that references the old ID. Don't retitle.
- **New items** append at the end of their track. Propose each one with a proposed ID
  and wait for agreement.
- **Closed items** stay in place, ticked: `(resolved YYYY-MM-DD: outcome)`. Move them to
  Archive only when the resolution no longer informs current work.
- **Item detail is current state only:** what's left, what it waits on, priority,
  dependencies, pointers. What happened and when goes in `docs/session-state.md`.
- **Open questions live only here** (section 3). Requirements and design docs point to
  this section instead of keeping their own lists.
- **This is the only durable task list.** The in-session todo tool is scratch. At end of
  session, reconcile it into this file, and give every open or next-step line written to
  `docs/session-state.md` an ID or an Unscheduled line, or drop it deliberately.
- **Refer to items by ID everywhere** (CLAUDE.md, docs, commit messages), never by
  position ("pickup item 3"). A commit that does an item's work names its ID.

## 1. Items

### PROJ — Project and repo plumbing
- [ ] T-PROJ-01 Set up the `claude-personal` wrapper and run comfy-lab sessions on the personal subscription
- [ ] T-PROJ-02 Create the public GitHub repo under the personal account, committing with the personal git identity

### EVENT — Track 1: Event-ready M4
- [x] T-EVENT-01 Prepare the external SSD for models (APFS, fast port, ComfyUI model folder layout) (resolved 2026-11-03: already APFS on a PCIe link; `comfyui/models/` created — see `docs/setup.md`)
- [x] T-EVENT-02 Install ComfyUI Desktop on the M4 and confirm it launches; record version and API port (resolved 2026-11-03: Desktop 1.1.8 / core 0.44.0, API on 127.0.0.1:8188 — see `docs/setup.md`)
- [x] T-EVENT-03 Point ComfyUI's model folder at the SSD and record the config in `docs/setup.md` (resolved 2026-11-03: Shared Models → SSD via Desktop Settings; verified in the regenerated paths file)
- [x] T-EVENT-04 Download an SDXL checkpoint to the SSD over home Wi-Fi; add it to `docs/models.md` with licence (resolved 2026-11-03: SDXL base 1.0 on the SSD, hash verified, registered)
- [ ] T-EVENT-05 Generate one SDXL test image; save the workflow (UI + API format) to `workflows/`
- [ ] T-EVENT-06 (if time) Download a quantised Flux model plus any required custom node; register both

## 2. Item detail

### PROJ
- **T-PROJ-01** — Looks done: the 2026-11-03 evening session ran with
  `CLAUDE_CONFIG_DIR=~/.claude-personal`. Left to close: confirm in `/status` that the
  login is the personal account, then tick. The policy sanity check (§3) stays open
  separately as accepted risk.
- **T-PROJ-02** — Looks done: `origin` is the personal-account repo and the first commit
  carries the personal email. Left to close: confirm the repo is public, then tick.

### EVENT
- **T-EVENT-05** — Doubles as the smoke test for the memory budget on 16 GB.
- **T-EVENT-06** — Expect slow generation on 16 GB. Flux Dev is non-commercial; record
  the exact licence of whichever variant is downloaded.

## 3. Open questions
Each one names the item that will resolve it, or is marked accepted risk.

- **What counts as success?** One concrete test, e.g. three slide-ready images faster
  than a hosted tool. Unresolved; decide after the event (§5 evaluation).
- **Which model licences allow work use?** Flux Dev is non-commercial. Resolved per model
  as each lands in `docs/models.md` (T-EVENT-04, T-EVENT-06).
- **Which custom nodes and models will the presenters need?** Answered at the event.
- ~~**Desktop app API port** — 8000 or 8188?~~ Resolved by T-EVENT-02: 8188.
- **Can presenter content be committed to a public repo?** Depends on what they share and
  under what licence. Until known, keep it in `collateral/` (gitignored).
- **Is running personal tooling on this machine fine with the employer?** Quick policy
  sanity check; the repo isn't work content. Accepted risk until checked (T-PROJ-01).
- **Should the project move to the Studio, and when?** Open; depends on the success test.

## 4. Standing operations
Recurring work: no IDs, no checkboxes, tracked by last-run date.

| Operation | Cadence / trigger | Last run | Notes |
|---|---|---|---|
| Unload Ollama and oMLX models | Before every ComfyUI generation session | — | All three share 16 GB; ComfyUI crawls or crashes otherwise |
| Register new models and custom nodes | Whenever one is downloaded | 2026-11-03 | `docs/models.md` (with licence), `docs/custom-nodes.md` |
| Refresh `.atlas/overview.md` | End of any session that moved the project | 2026-11-03 | Gitignored internal snapshot; refresh via `/atlas-overview` |

## 5. Unscheduled
Ideas that came up but aren't committed work. No ID until promoted into a track.

- Capture the presenters' shared workflows and notes after the event (see §3 on licence).
- Drive ComfyUI end to end from Claude Code via the HTTP API with API-format workflows.
- Try comfy-cli alongside the Desktop app (custom nodes, model downloads, running workflows).
- Try a community ComfyUI MCP server.
- Try Z-Image Turbo (the Desktop default template). bf16 needs 19 GB, too much for 16 GB; look for a smaller fp8/GGUF variant and check its licence.
- Short video clips with a small model (e.g. the lighter Wan variant).
- Evaluate against the success test and decide whether to keep ComfyUI.
- Move the project to the Mac Studio: plug in the SSD, re-point the model folder.

## 6. Archive
Closed items whose resolution no longer informs current work.
