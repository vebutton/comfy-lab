# comfy-lab — Requirements

Tasks and open questions live in [TODO.md](../TODO.md), not here.

## Project-wide

### Repo contents
- **Committed:** workflow JSON files (`workflows/`), the model register with licences
  (`docs/models.md`), the custom-node register (`docs/custom-nodes.md`), setup notes
  (`docs/setup.md`), and any scripts that drive ComfyUI.
- **Never committed:** model weights (`*.safetensors`, `*.ckpt`, `*.pt`, `*.pth`,
  `*.bin`, `*.gguf` — guarded in `.gitignore`); generated images/clips (`output/`,
  gitignored); third-party presenter content until its licence allows (keep it in
  `collateral/`, gitignored).
- The repo is public. Anything personal or employer-related goes in `CLAUDE.local.md`.

### Portability
- Model files live on the external SSD (APFS), with ComfyUI's model folder pointed at it.
- Moving to another machine = install ComfyUI, plug in the SSD, re-point the model folder.
  The repo must hold everything else needed to rebuild: workflows, node list, model list.

### Workflows
- Save each workflow worth keeping as a JSON file in `workflows/`.
- Where Claude Code will submit it via the HTTP API, also save the **API-format** export
  alongside the UI-format one (`<name>.json` and `<name>.api.json`).
- ComfyUI embeds the workflow in every PNG it writes; a PNG from the presenters can be
  dropped into ComfyUI to recover its workflow.

### Licences
- Every model in `docs/models.md` records its licence and whether it permits work /
  commercial use. No model is used for work-facing output until that column is filled.
- Known: Flux Dev is non-commercial.

### Automation interface
- ComfyUI's local HTTP API: POST an API-format workflow to the prompt endpoint, then
  retrieve outputs. Port to confirm on the Desktop app (possibly 8000; standard 8188).
- comfy-cli: optional, for installs, custom nodes, model downloads and running workflows.

### Resource constraints
- 16 GB unified memory shared with Ollama and oMLX. Unload their models before generating.
- Expected on 16 GB: SDXL runs well; quantised Flux slow; video only with small models,
  short clips, long waits.

## Track 1 — Event-ready M4

**Objective:** before leaving for the event, ComfyUI Desktop runs on the M4 with its model
folder on the SSD, SDXL is downloaded (quantised Flux if time allows), and one test image
has been generated. Everything else is secondary.

**Done means:**
1. ComfyUI Desktop launches on the M4; version and API port recorded in `docs/setup.md`.
2. ComfyUI's model folder resolves to the SSD; the config is recorded in `docs/setup.md`.
3. An SDXL checkpoint sits on the SSD and is listed in `docs/models.md` with its licence.
4. One SDXL image generated; its workflow saved to `workflows/` (UI and API format).
5. *(Optional)* A quantised Flux model plus any required custom node, both registered.

**Constraints:**
- Downloads happen on home Wi-Fi tonight, not at the event.
- Do not let Track 1 depend on API automation, comfy-cli or the Studio — those are later.
