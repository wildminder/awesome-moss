<p id="lora" align="center">◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆</p>

## ◉ LoRA &amp; adapters

Style, character, control and fix adapters for the 5B DiT. Grouped by type, then roughly by size. Most cards say **SFT preferred** even when trained on Base; where both diffusers and ComfyUI formats exist, the repo root usually holds both. Sizes are the primary adapter file, not the whole repo.

| Name | Type | Precision | Size | Links | Notes |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **Edit-HQ** | ![Control][ltype-control] | ![bf16][badge-bf16] | 0.50 GB | [![][gh-akhaliq]](https://huggingface.co/akhaliq/MOSS-Image-Edit-HQ) | Instruction-based image editing; every experiment, checkpoint and side-by-side is published. **Take armV5** — v6 introduced colour casts and the card does not recommend it. ComfyUI-plugin and ai-toolkit formats. |
| **Transparent background** | ![Control][ltype-control] | ![bf16][badge-bf16] | 0.15 GB | [![][gh-genai--craft]](https://huggingface.co/genai-craft/transparent-lora-for-moss-image) | Outputs RGBA PNG cut-outs, logos and lettering directly, with no background-removal pass. Needs the [FLUX.2 Klein Alpha VAE](https://huggingface.co/trmz/flux2-klein-alpha) to decode four channels. |
| **Moss-Image-Edit** | ![Control][ltype-control] | ![bf16][badge-bf16] | 0.30 GB | [![][gh-chinoll]](https://huggingface.co/chinoll/Moss-Image-Edit) ┊ [![][gh-Comfy--Org]](https://huggingface.co/Comfy-Org/MOSS-Image/resolve/main/loras/moss_image_edit_lora_rank_64_bf16.safetensors) | The original rank-64 editing LoRA, also bundled in the Comfy-Org repack. Edit-HQ is the community effort to beat it. |
| **Color-Depth Control** | ![Control][ltype-control] | ![bf16][badge-bf16] | 0.31 GB | [![][gh-darask0]](https://huggingface.co/darask0/MOSS-Image-color-depth-control) ┊ [![][gh-Comfy--Org]](https://huggingface.co/Comfy-Org/MOSS-Image/resolve/main/loras/moss_image_color_depth_control.safetensors) | Follows a colour-region map plus a depth map; four checkpoints at 2500-step intervals. Bundled in the Comfy-Org repack. |
|  |  |  |  |  |  |
| **MAX ULTRA hires LoHa** | ![Fix][ltype-fix] | ![bf16][badge-bf16] | 0.13–0.18 GB | [![][gh-comfyanonymous]](https://huggingface.co/comfyanonymous/MOSS_image_MAX_ULTRA_hires_lora_test) | LoHa tested at 4–10 MP; seven versions v0.1 through v0.7 with 9 MP examples and a no-LoRA baseline. |
| **Natural faces** | ![Fix][ltype-fix] | ![bf16][badge-bf16] | 0.30–0.31 GB | [![][gh-deadman44]](https://huggingface.co/deadman44/MOSS_Image_LoRA) | Three checkpoints against the polished "AI face" look. Ships three ComfyUI T2I workflows. |
|  |  |  |  |  |  |
| **MOSS Image LoRA pack** | ![Collection][ltype-collection] | ![bf16][badge-bf16] | 12.16 GB | [![][gh-pmczip]](https://huggingface.co/pmczip/MOSS_Image_LoRAs) | 83 celebrity-likeness adapters, ~0.15 GB each, each with an obfuscated trigger token. Bespoke license; check it before redistribution. |
| **Tendou Aris** | ![Character][ltype-character] | ![bf16][badge-bf16] | 0.20 GB | [![][gh-Te2douNin9]](https://huggingface.co/Te2douNin9/MOSS-Image-TendouAris) | Blue Archive character enhancer preserving face shape and halo; trigger `tendou_aris_moss`. |
| **Kei r4** | ![Character][ltype-character] | ![bf16][badge-bf16] | 0.20 / 0.15 GB | [![][gh-Te2douNin9]](https://huggingface.co/Te2douNin9/MOSS-Image-Kei) | ComfyUI and LyCORIS builds; strength 0.9. Trigger `tendou_kei_moss`, and the card recommends stacking Tendou Aris to fix the halo. |
| **EndField** | ![Character][ltype-character] | ![bf16][badge-bf16] | 0.20 GB | [![][gh-ShiyumeMeguri]](https://huggingface.co/ShiyumeMeguri/MossImage-EndField) | Rank-32 Arknights: Endfield style; the examples pair it with a 4-step speed LoRA at strength 1.0. |
| **Mikayla Demaiter** | ![Character][ltype-character] | ![bf16][badge-bf16] | 0.15 / 0.07 GB | [![][gh-Playtime--AI]](https://huggingface.co/Playtime-AI/MOSS-Image-Mikayla_Demaiter) | v2 and v1 likeness checkpoints; trigger `Mikayla Demaiter`. |
|  |  |  |  |  |  |
| **Beauty LoRA** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.07 GB × 4 | [![][gh-axiomofmind]](https://huggingface.co/axiomofmind/Moss-image-beauty-lora) | Editorial beauty portraiture; rank 16, trigger `moss_beauty_style`. Root is the 2,000-step / 176-image checkpoint, `alternative-checkpoint-1750/` the narrower 149-image one. Both diffusers and ComfyUI formats. |
| **Beauty LoRA GGUF** | ![Style][ltype-style] | ![F16][badge-f16] | 0.07 GB × 2 | [![][gh-axiomofmind]](https://huggingface.co/axiomofmind/Moss-image-beauty-lora-GGUF) | F16 GGUF conversions of both Beauty checkpoints for the [SFT GGUF base](#quant-gguf); adapter strength stays adjustable. Adapter only, no DiT/TE/VAE inside. |
| **Soft-light 3D anime** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.30 GB | [![][gh-Helives]](https://huggingface.co/Helives/Soft-light-3D-Anime-style-For-MOSS) | V2 switches to DoRA at rank 64 for deeper cel-shaded lighting and fewer oversized heads. |
| **Kodachrome V2** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.04 GB | [![][gh-aoleg]](https://huggingface.co/aoleg/MOSS-Image-Kodachrome-V2) | LoKr reproducing 1940s 4x5 Kodachrome colour and contrast; V2 retrains with a low-noise timestep bias so composition stops leaking from the dataset. |
| **nin style** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.20 / 0.15 GB | [![][gh-Te2douNin9]](https://huggingface.co/Te2douNin9/MOSS-Image-nin) | Crisp Japanese anime illustration look, ComfyUI and LyCORIS builds, strength 1.0. |
| **Anime screenscap** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.07 GB | [![][gh-RunningHubAI]](https://huggingface.co/RunningHubAI/rh-anime-screenscap-mossimage-v1.safetensors-lora) | Anime screenshot-style adapter from RunningHub. |
| **hh48288196 style** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.01 GB | [![][gh-RunningHubAI]](https://huggingface.co/RunningHubAI/rh-hh48288196-style-moss-image-v1-lora) | Compact 12 MB style adapter from RunningHub. |
|  |  |  |  |  |  |
| **NSFW uncensored** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 27.41 GB repo | ⚠️ [![][gh-Ansh92928219]](https://huggingface.co/Ansh92928219/Moss-image-nsfw-uncensored-lora?not-for-all-audiences=true) | Adult-only, gated on Hugging Face; trigger `moss_xvstyle`. The repo also carries a full merged diffusers model, which is why its byte total is large. |

> [!NOTE]
> The two **control** rows bundled into the Comfy-Org repack are linked twice on purpose: the original repos carry the training cards and extra checkpoints, while the repack files are ready to drop into `models/loras/`. The transparent-background LoRA is the only one here that needs a different VAE — [trmz/flux2-klein-alpha](https://huggingface.co/trmz/flux2-klein-alpha) decodes the fourth channel.

