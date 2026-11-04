# Setup — ComfyUI on this machine

## Model storage (external SSD)
- Volume: `/Volumes/LLM-2TB` — APFS, 2 TB, PCI-Express (Thunderbolt) link. Shared with
  the `mlx/` and `ollama/` model stores.
- ComfyUI root on the drive: `/Volumes/LLM-2TB/comfyui/`
- Layout — model files only, nothing machine-specific, so the drive moves to another Mac
  as-is:
  ```
  comfyui/models/
    checkpoints/       # all-in-one checkpoints (SDXL)
    diffusion_models/  # standalone diffusion models (Flux, Wan)
    unet/              # legacy name some nodes/GGUF loaders still use
    text_encoders/  clip/  clip_vision/
    vae/  loras/  controlnet/  upscale_models/  embeddings/
  ```

## ComfyUI Desktop
- Desktop app 1.1.8, ComfyUI core 0.44.0, frontend 1.57.6 (Express install, 2026-11-03).
- API: `http://127.0.0.1:8188` (localhost only). Check with `curl -s :8188/system_stats`.
- Install (Python venv, code): `~/ComfyUI-Installs/ComfyUI/` — internal disk.
- Input/output: `~/ComfyUI-Shared/input`, `~/ComfyUI-Shared/output`.
- Model paths: Desktop generates
  `~/Library/Application Support/Comfy Desktop/instance-model-paths/inst-*.yaml`
  ("do not edit manually"); its base path follows the Shared Models setting below.
- Model folder → SSD: Desktop Settings → Storage → Shared Models set to `/Volumes/LLM-2TB/comfyui/models` (marked DOWNLOADS, so in-app downloads land on the SSD). The instance keeps "Include Shared Directories" on. Restart Desktop after changing it. The INSTANCE row (`~/ComfyUI-Installs/ComfyUI/ComfyUI/models`) is the install's private folder — unused.

## Performance on the M4 (16 GB)
SDXL base 1.0, 25 steps, dpmpp_2m/karras, measured 2026-11-03 after a fresh restart with
Ollama/oMLX off. Times are server-side (`execution_start` → `execution_success` in `/history`).

| Size | Time | Notes |
|---|---|---|
| 512×512 | 42.5 s | First run, includes model load |
| 512×512 | 34.2 s | Model already loaded (API submission via curl) |
| 1024×1024 | 146.7 s | Model and prompts cached; swap reached ~4.2 GB |

SDXL's native size is 1024×1024; use it for anything slide-facing. The workflow is in
`workflows/sdxl-basic-1024.{ui,api}.json`. Submit the API file as
`{"prompt": <file contents>}` to `POST :8188/prompt`, then poll `GET :8188/history/<prompt_id>`.
