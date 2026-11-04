# Models

Every model ComfyUI uses. Weights live on the external SSD, never in the repo.
Fill the licence columns before using a model for anything work-facing.

| Model | Type | File | SSD path (relative to model root) | Size | Source | Licence | Work use OK? |
|---|---|---|---|---|---|---|---|
| SDXL base 1.0 | Checkpoint (all-in-one) | `sd_xl_base_1.0.safetensors` | `checkpoints/` | 6.94 GB | stabilityai/stable-diffusion-xl-base-1.0 (Hugging Face), via ComfyUI Desktop's missing-model download. SHA-256 `31e35c80fc4829d14f90153f4c74cd59c90b779f6afe05a74cd6120b893f7e5b` | CreativeML Open RAIL++-M | Yes, with conditions: commercial use is allowed subject to the licence's use-based restrictions |
| FLUX.2 [klein] 4B (distilled) | Diffusion model | `flux-2-klein-4b.safetensors` | `diffusion_models/` | 7.75 GB | Comfy-Org/Flux-2-Klein-4B (Hugging Face) @ `5b3abca`, repackaged from black-forest-labs/FLUX.2-klein-4B. SHA-256 `ec3d4e733a771f61c052fb4856c48b336c55eaf2c65487c2a1faeb9bbda7a343` | Apache 2.0 | Yes |
| Qwen3 4B (Klein text encoder) | Text encoder | `qwen_3_4b.safetensors` | `text_encoders/` | 8.04 GB | Comfy-Org/Flux-2-Klein-4B @ `5b3abca`. SHA-256 `6c671498573ac2f7a5501502ccce8d2b08ea6ca2f661c458e708f36b36edfc5a` | Apache 2.0 | Yes |
| FLUX.2 VAE | VAE | `flux2-vae.safetensors` | `vae/` | 0.34 GB | Comfy-Org/Flux-2-Klein-4B @ `5b3abca`. SHA-256 `868fe7b343cc8f3a19dbcfcafbc3d5f888802be3f89bd81b65b3621a066ce8f3` | Apache 2.0 | Yes |
