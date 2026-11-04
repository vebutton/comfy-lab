# comfy-lab — Project Context for Claude Code

## User
The operator — a technical user who runs local AI tooling (LLM engines, coding
assistants) on Apple Silicon and is new to ComfyUI. Committed files never name the user
or their employer: both live in `CLAUDE.local.md`, so this file reads correctly in any
teammate's session.

## This Project
A quick-and-dirty lab for trying out ComfyUI after attending a ComfyUI event: generate
images (and maybe short videos), possibly for work presentations, and decide whether the
tool is worth keeping. The repo tracks the to-dos and holds the small, portable pieces:
workflow JSON files, the custom-node list, the model list with licences, and setup notes.
Large model files never live in the repo.

- **Full requirements:** [docs/requirements.md](docs/requirements.md)
- **Task register:** [TODO.md](TODO.md) — the only durable task list. Read it before
  planning any work.
- **Model and node registers:** [docs/models.md](docs/models.md),
  [docs/custom-nodes.md](docs/custom-nodes.md)
- **Internal details:** `CLAUDE.local.md` — gitignored, read it every session. (Holds the
  user's name and employer, accounts and identities, the event date and location.)

## Input / Output
- **Input:** ComfyUI workflows (JSON files, or embedded in generated PNGs), presenter
  content shared at the event, model downloads.
- **Output:** Generated images/clips in `output/` (gitignored); workflows saved to
  `workflows/` (committed); registers in `docs/` updated as models and nodes are added.
- **`temp/`** (gitignored) is the operator's personal scratch space. Never read or write it.

## Interface
Claude Code CLI. Claude Code drives ComfyUI two ways:
- **Local HTTP API** — submit workflows saved in *API format* to the server's prompt
  endpoint and fetch the results (curl or small scripts). The Desktop app listens on
  `127.0.0.1:8188` (see `docs/setup.md`).
- **comfy-cli** — the official CLI for installs, custom nodes, model downloads and running
  workflows. Optional alongside the Desktop app.

## Key Integrations
- ComfyUI Desktop (macOS, PyTorch on Apple Metal)
- Later, maybe: a community ComfyUI MCP server (TODO.md §5)

## Tech / Tooling
- **ComfyUI install route:** the ComfyUI Desktop app (.dmg) on the M4 laptop.
- **Repo stack:** no language toolchain yet. The repo holds docs, workflow JSON and, if
  needed, shell/curl scripts. If Python scripts become worthwhile, use **pyenv** + **uv**
  (commit `pyproject.toml`, `.python-version`, `uv.lock`); no venv, Poetry, Conda or Pipenv.
- **Hardware:** an M4 laptop with 16 GB unified memory, shared with Ollama and oMLX. A
  Mac Studio may take over later.
- **Model storage:** the 2 TB external SSD (APFS, fast USB/Thunderbolt port). ComfyUI's
  model folder points at it, so moving machines means plugging the drive in and
  re-pointing — no re-downloading.
- **Memory discipline:** ComfyUI is its own engine; it does not use Ollama or oMLX, but
  all three share the 16 GB. Unload Ollama/oMLX models before generating.
- **Realistic expectations on 16 GB:** SDXL runs well; quantised Flux runs slowly; video
  only with small models (e.g. the lighter Wan variant), short clips, long waits. Heavy
  work waits for the Studio.

## How to Work With This User
- Quick and dirty is the point — favour getting something running over completeness.
- Record every model's licence before it's used for anything work-facing.
- Don't let tinkering run open-ended: tie work back to the success test (TODO.md §3).

## Environment
- First started: CLI
- Date: 2026-11-03
- Account: comfy-lab runs on the personal Claude subscription via the `claude-personal`
  wrapper (`CLAUDE_CONFIG_DIR=~/.claude-personal`), not the default work login. Skills
  and commands live in that config separately — missing ones mean the wrong account.

---

## Tracks

> A **track** is a distinct workstream with its own objective. Tracks say what the project
> is trying to do; the work items live in `TODO.md`, prefixed by the track's slug:
> `T-<SLUG>-<nn>`. Slugs are short, uppercase and permanent.

### Track 1 — Event-ready M4 (EVENT)
- **Objective:** Before leaving for the event: ComfyUI Desktop runs on the M4, its model
  folder points at the external SSD, SDXL (plus a quantised Flux if time allows) is
  downloaded over home Wi-Fi, and one test image has been generated. Everything else is
  secondary.
- **Status:** Done (2026-11-03). T-EVENT-01 → T-EVENT-06 closed: SDXL and FLUX.2 [klein] 4B
  both generate at 1024 on the M4, with workflows in `workflows/`.
- **Depends on:** Nothing — this is the first track.

**Further tracks:** None yet. Add each as `### Track N — [Name] ([SLUG])` with the same
three fields and a matching section in `TODO.md`, stating whether it **may depend on an
existing track** or **must stand alone**.

---

## Session State

> Forward-looking operating context only. Archaeology lives in `docs/session-state.md`.
> Task state is not kept here: open items, progress and recurring work live in `TODO.md`.

**Last (2026-11-03 late evening):** Short housekeeping session. Created the Atlas
overview `.atlas/overview.md` (gitignored). Checked memory: ~7 GB usable, no Ollama/oMLX,
Desktop closed — the planned restart had not happened yet. Install state is unchanged:
Desktop on 8188, Shared Models on the SSD, SDXL base 1.0 registered, no image generated
yet. The event is tomorrow (`CLAUDE.local.md`).

**Next session — pick up here:**
1. Cold start — re-read CLAUDE.md and CLAUDE.local.md. Consult docs/session-state.md on demand.
2. Pre-flight: restart the Mac first if it hasn't been (~7 GB usable is tight for the
   ~7 GB SDXL checkpoint). SSD mounted; Ollama/oMLX not running; Desktop running, then
   `curl -s :8188/system_stats` → check `ram_free`.
3. T-EVENT-05: open the SDXL template in Desktop (not the default Z-Image Turbo template
   — its 19 GB bf16 models don't fit in 16 GB), generate one image, export the workflow
   in both UI and API format to `workflows/`, submit the API version via curl as a
   check.
4. T-EVENT-06 (quantised Flux) only if time remains.
5. Close T-PROJ-01 (`/status` shows personal login?) and T-PROJ-02 (repo public?) once
   confirmed — see TODO.md §2.

**Continuity reminders:**
- Git identity: commits must use the personal email (in `CLAUDE.local.md`), not the
  global default. Set it repo-local after any `git init` and verify before committing.
- Model files (`*.safetensors`, `*.ckpt`, `*.gguf`, …) never enter the repo — `.gitignore`
  guards against it.
- The repo will be public: no third-party presenter content gets committed until its
  licence is known (TODO.md §3).
- Setup facts (paths, port, versions, Desktop's instance vs shared storage) live in
  `docs/setup.md`. Never hand-edit Desktop's generated `instance-model-paths/*.yaml`;
  change Desktop Settings → Storage and restart instead.
- The operator shares screenshots by dropping them in `collateral/` (gitignored) — look
  there, newest first.

**Session continuity:** Next session is a cold start — re-read CLAUDE.md before doing
anything. Sessions are never re-entered. End-of-session updates cover three layers: this
brief, `docs/session-state.md`, and `TODO.md` — plus a refresh of `.atlas/overview.md`
(gitignored) when the session moved the project.
Full archaeology: `docs/session-state.md` — consult when prior-thread depth, decision
reasoning, or ruled-out paths are needed.
