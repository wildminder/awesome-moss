<p id="contributing" align="center">◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆</p>

## ✎ Contributing

This list is a **set of Markdown files, not one**. `README.md` is generated and
should never be edited directly, since your change will be overwritten on the next
build. Each section lives in its own file under [`blocks/`](blocks/):

| To change… | Edit this file |
| :--- | :--- |
| Title, badges, table of contents, quick start | [`blocks/00_header.md`](blocks/00_header.md) |
| Base, SFT or the Comfy-Org file table | [`blocks/10_official_checkpoints.md`](blocks/10_official_checkpoints.md) |
| Full fine-tunes and all-in-one checkpoints | [`blocks/20_finetunes.md`](blocks/20_finetunes.md) |
| GGUF, SDNQ, Nunchaku, FP8 / INT8 ConvRot | [`blocks/30_quantizations.md`](blocks/30_quantizations.md) |
| Distilled adapters and merged few-step DiTs | [`blocks/40_turbo.md`](blocks/40_turbo.md) |
| LoRAs and adapters | [`blocks/50_loras.md`](blocks/50_loras.md) |
| Text encoders and prompt engines | [`blocks/60_text_encoders_prompt_engines.md`](blocks/60_text_encoders_prompt_engines.md) |
| Workflow collections and custom nodes | [`blocks/70_workflows.md`](blocks/70_workflows.md) |
| Modular Diffusers, runtimes, gotchas | [`blocks/80_tools.md`](blocks/80_tools.md) |
| Badge and link definitions | [`blocks/99_footer.md`](blocks/99_footer.md) |

The file prefix controls the order: the build concatenates `blocks/*.md` in
filename order, so `10_` always lands before `20_`. A new section just needs a
new number.
