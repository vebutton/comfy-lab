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
