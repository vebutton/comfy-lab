# comfy-lab

A small lab for trying out [ComfyUI](https://github.com/comfyanonymous/ComfyUI) on Apple
Silicon: generating images (and maybe short video clips) locally, recording which models
and custom nodes were used and under what licences, and deciding whether the tool earns a
place in the workflow. Model weights live on an external drive, not in this repo; what's
here is the portable part — workflows, registers and setup notes — so the setup can be
rebuilt on another machine.

## Pointers
- [docs/requirements.md](docs/requirements.md) — what the repo holds and the rules it follows
- [TODO.md](TODO.md) — task register and open questions
- [docs/models.md](docs/models.md) — models in use, with licences
- [docs/custom-nodes.md](docs/custom-nodes.md) — custom nodes installed
- [workflows/](workflows/) — saved ComfyUI workflows (UI and API format)
- [CLAUDE.md](CLAUDE.md) — context for Claude Code sessions
