# Session State — Archaeology

> Backward-looking detail: thread-by-thread work, decisions and reasoning, ruled-out
> paths, commits, memories banked. The forward-looking brief lives in CLAUDE.md ->
> Session State. Consult this file on demand when depth is needed.

## 2026-11-03 (evening) — Track 1 install session

**Done**
- T-EVENT-01: the SSD (`LLM-2TB`) was already APFS on a PCI-Express link, 1.4 TB free,
  alongside the existing `mlx/` and `ollama/` stores. Created `comfyui/models/` with all
  28 folder types from Desktop's generated paths file (11 at first, the remaining 17
  added after comparing against the YAML).
- T-EVENT-02: Desktop 1.1.8 installed with Express defaults. ComfyUI core 0.44.0. The
  macOS "removable volume" access prompt was allowed. API is on `127.0.0.1:8188`, which
  resolves the 8000-vs-8188 open question.
- T-EVENT-03: Desktop Settings → Storage → Shared Models changed from
  `~/ComfyUI-Shared/models` to the SSD path; it carries the DOWNLOADS badge. The
  change took effect after a restart (regenerated YAML `base_path` verified).
- T-EVENT-04: `sd_xl_base_1.0.safetensors` was downloaded through Desktop's
  missing-model prompt into `checkpoints/`. Its SHA-256 matches the official release.
  Registered in `docs/models.md` (CreativeML Open RAIL++-M).
- New `docs/setup.md` records install paths, port, versions and storage config.

**Decisions and reasoning**
- Kept the install (venv, code) on the internal disk; only models go on the SSD, so
  the drive stays machine-agnostic for the Studio move.
- Ruled out: symlinking `~/ComfyUI-Shared/models` → SSD, and hand-editing Desktop's
  `instance-model-paths/*.yaml` ("do not edit manually"; Desktop regenerates it). The
  Desktop Settings shared-models setting is the supported route.
- Instance vs shared: an *instance* is one ComfyUI install (its own venv, custom nodes,
  private `models/`); *shared* directories are Desktop-level and used by every instance
  with "Include Shared Directories" on. Shared is set once in Desktop Settings.
- The "2 setup issues" in the Workflow Overview panel were Desktop's default Z-Image
  Turbo template missing its models (bf16 diffusion 11.46 GB + Qwen3-4B text encoder
  7.49 GB). Deliberately not downloaded: too big for 16 GB. Parked in TODO.md §5.
- Log warnings judged harmless on Apple Silicon: AIMDO "assuming Nvidia", no
  cuda/triton backends, Manager's matrix-nio missing.

**Observed**
- Free RAM was ~3 GB with other apps running. The operator restarted the Mac to clear
  memory before the first generation.

**Commits**
- Session housekeeping commit (setup doc, model register, TODO and CLAUDE.md updates).

## 2026-11-03 (late evening) — Atlas overview session

**Done**
- Created `.atlas/overview.md` via `/atlas-overview` (create mode): slug `comfy-lab`,
  title "ComfyUI Lab", visibility public, role_fit innovation-lead, host `mac-m4-pro`.
  `.atlas/` was already gitignored. Two [NEEDS] markers remain (success test; first
  image link), both resolvable only after the event / T-EVENT-05.
- Added a Standing operation in TODO.md §4 for refreshing the overview.

**Decisions and reasoning**
- Motivation clarified by the operator: continuous learning, self-initiated. No work use
  is planned, but output could feed work presentations later, so licence tracking stays.
  Overview wording reflects this; CLAUDE.md/README's "possibly for work" already matches.
- Event organiser: recorded in the overview only (internal file).

**Observed**
- Before restart: ~7.1 GB usable (1.9 free + 1.6 speculative + 3.6 inactive), 4.9 GB
  active, 2.3 GB wired; memory pressure normal; Ollama/oMLX not running; Desktop closed.
  Tight for SDXL, so the restart stays a pre-flight step.

**Commits**
- Session housekeeping commit (CLAUDE.md brief, TODO.md §4, this entry).
