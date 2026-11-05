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

## 2026-11-03 (night) — Track 1 finish: first images, Flux, PROJ closeout

**Done**
- Pre-flight: Mac restarted (88% memory free reported), SSD mounted, Ollama/oMLX off.
- T-EVENT-05: SDXL base 1.0 at 512 (42.5 s cold) and 1024 (146.7 s; checkpoint and both
  prompt encodes cached). Workflow saved from server history as
  `workflows/sdxl-basic-1024.{ui,api}.json`; API file submitted via curl (new seed, 512
  override) → success, 34.2 s. Timings in `docs/setup.md` → Performance on the M4.
- T-PROJ-02: `gh` reports the repo PUBLIC. T-PROJ-01: operator confirmed the session is
  on `claude-personal` (skills/commands were missing there and had to be re-added).
- T-EVENT-06: downloaded FLUX.2 [klein] 4B distilled (7.75 GB), Qwen3 4B text encoder
  (8.04 GB) and FLUX.2 VAE (0.34 GB) from Comfy-Org/Flux-2-Klein-4B @ `5b3abca` to the
  SSD; all SHA-256 match. Registered (Apache 2.0). Generated via the Desktop template at
  1024: 84.3 s cold, 64.2 s warm via curl API submission. Workflow saved as
  `workflows/flux2-klein-4b-distilled-1024.{ui,api}.json`. Track 1 marked done.
- Refreshed `.atlas/overview.md` (2 [NEEDS]: success test; a shareable sample image).

**Decisions and reasoning**
- Flux choice: Klein 4B distilled over the planned quantised Flux 1 Schnell (GGUF Q4_K_S
  + T5 GGUF, ~10 GB, needs ComfyUI-GGUF). Klein is newer, 4B vs 12B, step-distilled
  (4 steps), Apache 2.0, native in ComfyUI. Option A (native bf16, ~16 GB download)
  picked over B (unsloth Q8_0 GGUF 4.3 GB + ComfyUI-GGUF) for no custom node; B remains
  the fallback, reusing the same encoder and VAE.
- Ruled out: the Qwen3 fp4 text encoder (fp4 unlikely to work on Metal); the BFL
  `ae.safetensors` (gated); the base Klein model (needs 20–50 steps + CFG).
- SDXL at 512 is below native; 1024 is the size for slides.

**Observed**
- Desktop's "49 s" readout on the 1024 SDXL run was mid-job; the job was still running
  (VAE decode). Use server history for timings.
- The Klein template has two subgraph pipelines; base active, distilled bypassed. Its
  missing-model prompt asked for `flux-2-klein-base-4b.safetensors` until swapped.
- Swap: ~4.2 GB after SDXL 1024; ~12.8 GB after the first Klein run, back to ~3.2 GB
  after the warm run.

**Commits**
- `0f9905a` T-EVENT-05; `6b0fd2e` Klein download + T-PROJ-01/02; `2e05369` T-EVENT-06 +
  Track 1 done; plus this housekeeping commit.

## 2026-11-04 — Event-day generation session

**Done**
- Recalled the tested models (SDXL base 1.0; FLUX.2 [klein] 4B distilled) from `docs/models.md`.
- Pre-flight: SSD mounted (1.4 TB free), all 4 model files present, Ollama/oMLX not
  running, Desktop up on `:8188` (core 0.44.0, PyTorch 2.12.1, MPS), 7.2 GB free.
- Five 1024 images via curl API submission (outputs in `~/ComfyUI-Shared/output/`, not
  committed): Klein hedgehog test (95 s, first after launch), Klein analytic-cubist
  "Picasso style" woman with instrument (102 s), Klein cubist Fort Worth cowboy (94 s),
  Klein photoreal celebrity-in-eggshell (104 s), SDXL politician-at-desk ×2 (154 s each).

**Decisions and reasoning**
- Model fit: Klein 4B renders photoreal scenes well but has little artist-style or
  celebrity-likeness knowledge — cubism prompts produced intact realistic figures with
  facet lines; a named celebrity came out generic. SDXL produced a recognisable public
  figure. Working rule: Klein for speed/photoreal, SDXL for styles/likeness.
- SDXL prompt length: a long prompt (≈75+ tokens, object-heavy, "from behind his
  shoulder") produced an empty desk. A short prompt with the subject first and weighted
  `(name:1.3)` fixed it. Fine details (tie colour, Mac mini, node-graph screen) still
  missed; on-screen UI is better composited from a real screenshot.
- Called `POST /free` before switching Klein → SDXL; swap fell 12.8 → 5.6 GB.
- Real-person images flagged to the operator: label as AI, keep out of work use.

**Observed**
- API-submitted images don't show on the Desktop canvas; they appear in the
  Queue/Assets sidebar. Operator initially couldn't find the first image.
- The saved SDXL API workflow's negative prompt includes "color, colored" (from the
  monochrome statue test) — must be overridden for colour work.
- Klein cold first run at the event was 95–104 s (vs 84 s at home); swap hit 12.8 GB.

**Commits**
- This housekeeping commit only.
