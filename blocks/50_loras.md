<p id="lora" align="center">◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆</p>

## ◉ LoRA &amp; adapters

Style, character, control, edit and fix adapters for the 5B DiT. Grouped by type, then roughly by size. Most cards say **SFT preferred** even when trained on Base; where both diffusers and ComfyUI formats exist, the repo root usually holds both. Sizes are the primary adapter file, not the whole repo. A dash in **Precision** means the card does not state a dtype.

| Name | Type | Precision | Size | Links | Notes |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **Edit-HQ** | ![Control][ltype-control] | ![bf16][badge-bf16] | 0.50 GB | [![][gh-akhaliq]](https://huggingface.co/akhaliq/MOSS-Image-Edit-HQ) | Instruction-based image editing; every experiment, checkpoint and side-by-side is published. **Take armV5** — v6 introduced colour casts and the card does not recommend it. ComfyUI-plugin and ai-toolkit formats. |
| **Transparent background** | ![Control][ltype-control] | ![bf16][badge-bf16] | 0.15 GB | [![][gh-genai--craft]](https://huggingface.co/genai-craft/transparent-lora-for-moss-image) | Outputs RGBA PNG cut-outs, logos and lettering directly, with no background-removal pass. Needs the [FLUX.2 Klein Alpha VAE](https://huggingface.co/trmz/flux2-klein-alpha) to decode four channels. |
| **Moss-Image-Edit** | ![Control][ltype-control] | ![bf16][badge-bf16] | 0.30 GB | [![][gh-chinoll]](https://huggingface.co/chinoll/Moss-Image-Edit) ┊ [![][gh-Comfy--Org]](https://huggingface.co/Comfy-Org/MOSS-Image/resolve/main/loras/moss_image_edit_lora_rank_64_bf16.safetensors) | The original rank-64 editing LoRA, also bundled in the Comfy-Org repack. Edit-HQ is the community effort to beat it. |
| **Color-Depth Control** | ![Control][ltype-control] | ![bf16][badge-bf16] | 0.31 GB | [![][gh-Comfy--Org]](https://huggingface.co/Comfy-Org/MOSS-Image/resolve/main/loras/moss_image_color_depth_control.safetensors) | Follows a colour-region map plus a depth map. Bundled in the Comfy-Org repack — the original darask0 repo is no longer public, so this file is the surviving source. A Civitai copy also sits in the [LoRA archive](#lora). |
| **Tile Control** | ![Control][ltype-control] | — | 0.07–0.27 GB | [![][gh-Xabsurd]](https://huggingface.co/Xabsurd/MOSS-Image-Tile-Control) | Structure-preserving tile control for detail regeneration after upscaling. **beta 2** is a 0.27 GB LoRA for **Base**, loaded through ComfyUI's built-in colour/depth control node with the reference image as `color_map`; **beta 3** is a 0.07 GB residual **model patch** for **SFT**, needing the bundled `ComfyUI-Moss-Tile-Control` nodes and `models/model_patches/`. Weights and strength values are not interchangeable. No license declared; evaluation is subjective, with no benchmark. |
|  |  |  |  |  |  |
| **CROWN Anime2Real 1:1** | ![Edit][ltype-edit] | — | 0.30 GB | [![][gh-S3ckn11g0r]](https://huggingface.co/S3ckn11g0r/CROWN-MOSS-Image-Edit-Anime2Real-1to1) | Image-to-image **edit** LoRA on the MOSS-Image Edit stack: turns 2D anime into 1:1 photorealistic portraits, rank 64 on 514 curated pairs. Trigger `crown_anime2real, A2R`; weight 0.90–1.00, 28–30 steps, CFG 3.2–3.5. Bespoke license. |
|  |  |  |  |  |  |
| **MAX ULTRA hires LoHa** | ![Fix][ltype-fix] | ![bf16][badge-bf16] | 0.13–0.18 GB | [![][gh-comfyanonymous]](https://huggingface.co/comfyanonymous/MOSS_image_MAX_ULTRA_hires_lora_test) | LoHa tested at 4–10 MP; seven versions v0.1 through v0.7 with 9 MP examples and a no-LoRA baseline. |
| **Natural faces** | ![Fix][ltype-fix] | ![bf16][badge-bf16] | 0.30–0.31 GB | [![][gh-deadman44]](https://huggingface.co/deadman44/MOSS_Image_LoRA) | Three checkpoints against the polished "AI face" look. Ships three ComfyUI T2I workflows. |
|  |  |  |  |  |  |
| **MOSS-Image LoRA archive** | ![Collection][ltype-collection] | ![bf16][badge-bf16] | 371.39 GB | ⚠️ [![][gh-mossloras]](https://huggingface.co/mossloras/moss-image-lora-archive) | Bulk **Civitai MOSS LoRAs dump**: 2,495 archived version IDs and 2,499 files, sorted into dated folders keyed by Civitai model/version ID. Uncurated, mixed quality and includes adult-only material; `manifest.json`, `skiplist.json` and `backfill_state.json` track an automated backfill. Not a curated set — this is a mirror to search, not a pack to download. |
| **MOSS Image LoRA pack** | ![Collection][ltype-collection] | ![bf16][badge-bf16] | 12.16 GB | [![][gh-pmczip]](https://huggingface.co/pmczip/MOSS_Image_LoRAs) | 83 celebrity-likeness adapters, ~0.15 GB each, each with an obfuscated trigger token. Bespoke license; check it before redistribution. |
| **Tendou Aris** | ![Character][ltype-character] | ![bf16][badge-bf16] | 0.20 GB | [![][gh-Te2douNin9]](https://huggingface.co/Te2douNin9/MOSS-Image-TendouAris) | Blue Archive character enhancer preserving face shape and halo; trigger `tendou_aris_moss`. |
| **Kei r4 / NL-1.3** | ![Character][ltype-character] | ![bf16][badge-bf16] | 0.20 / 0.15 / 0.40 / 0.30 GB | [![][gh-Te2douNin9]](https://huggingface.co/Te2douNin9/MOSS-Image-Kei) | Four checkpoints now: the r4 ComfyUI/LyCORIS pair at strength 0.9 and the newer **NL-1.3** `moss-kei-nlr3-1410-*` ComfyUI/native pair. Trigger `tendou_kei_moss`. r4 needs Tendou Aris stacked to fix the halo; NL-1.3 works alone and also fixes the ringed eyes. |
| **EndField** | ![Character][ltype-character] | ![bf16][badge-bf16] | 0.20 GB | [![][gh-ShiyumeMeguri]](https://huggingface.co/ShiyumeMeguri/MossImage-EndField) | Rank-32 Arknights: Endfield style; the examples pair it with a 4-step speed LoRA at strength 1.0. |
| **Mikayla Demaiter** | ![Character][ltype-character] | ![bf16][badge-bf16] | 0.15 / 0.07 GB | [![][gh-Playtime--AI]](https://huggingface.co/Playtime-AI/MOSS-Image-Mikayla_Demaiter) | v2 and v1 likeness checkpoints; trigger `Mikayla Demaiter`. |
|  |  |  |  |  |  |
| **Beauty LoRA** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.07 GB × 4 | [![][gh-axiomofmind]](https://huggingface.co/axiomofmind/Moss-image-beauty-lora) | Editorial beauty portraiture; rank 16, trigger `moss_beauty_style`. Root is the 2,000-step / 176-image checkpoint, `alternative-checkpoint-1750/` the narrower 149-image one. Both diffusers and ComfyUI formats. |
| **Beauty LoRA GGUF** | ![Style][ltype-style] | ![F16][badge-f16] | 0.07 GB × 2 | [![][gh-axiomofmind]](https://huggingface.co/axiomofmind/Moss-image-beauty-lora-GGUF) | F16 GGUF conversions of both Beauty checkpoints for the [SFT GGUF base](#quant-gguf); adapter strength stays adjustable. Adapter only, no DiT/TE/VAE inside. |
| **Soft-light 3D anime** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.30 GB | [![][gh-Helives]](https://huggingface.co/Helives/Soft-light-3D-Anime-style-For-MOSS) | V2 switches to DoRA at rank 64 for deeper cel-shaded lighting and fewer oversized heads. |
| **Kodachrome V2** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.04 GB | [![][gh-aoleg]](https://huggingface.co/aoleg/MOSS-Image-Kodachrome-V2) | LoKr reproducing 1940s 4x5 Kodachrome colour and contrast; V2 retrains with a low-noise timestep bias so composition stops leaking from the dataset. |
| **nin style** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.20 / 0.15 GB | [![][gh-Te2douNin9]](https://huggingface.co/Te2douNin9/MOSS-Image-nin) | Crisp Japanese anime illustration look, ComfyUI and LyCORIS builds, strength 1.0. |
| **PixelArt 16x16** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.15 GB | [![][gh-Angais]](https://huggingface.co/Angais/MOSS-Image-PixelArt-16x16) | Rank-32 adapter drawing one 16×16 pixel-art game icon per image on a flat magenta background that keys out cleanly; trigger `pixel art 16x16 game icon`. Trained on a synthetic sprite set, Apache-2.0. |
| **CROWN Douyin OOTD** | ![Style][ltype-style] | — | 0.30 GB | [![][gh-S3ckn11g0r]](https://huggingface.co/S3ckn11g0r/CROWN-MOSS-Image-Douyin-Babe-OOTD) | Portrait and fashion photorealism in two buckets: close-ups and OOTD body shots, rank 64 on 355 deduplicated images. Triggers `crown_douyin_babe` + `crown_babe_face` / `crown_babe_ootd`; weight 0.85–0.95, 28–30 steps, CFG 3.5. Bespoke license. |
| **BAStyle v1** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.15 GB | [![][gh-nekoshrimp]](https://huggingface.co/nekoshrimp/MOSS-Image-BAStyle-v1) | Blue Archive cel-shaded illustration style; rank 32, DiT-only with the text encoder frozen, 58 captioned images. Trigger `bastyle` at the start of a natural-language prompt; strength 0.90–0.95. Halos are not baked into the trigger. Ships a dependency-free ComfyUI workflow. CC-BY-NC-4.0. |
| **Anime screenscap** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.07 GB | [![][gh-RunningHubAI]](https://huggingface.co/RunningHubAI/rh-anime-screenscap-mossimage-v1.safetensors-lora) | Anime screenshot-style adapter from RunningHub. |
| **hh48288196 style** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.01 GB | [![][gh-RunningHubAI]](https://huggingface.co/RunningHubAI/rh-hh48288196-style-moss-image-v1-lora) | Compact 12 MB style adapter from RunningHub. |
|  |  |  |  |  |  |
| **NSFW uncensored** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 27.41 GB repo | ⚠️ [![][gh-Ansh92928219]](https://huggingface.co/Ansh92928219/Moss-image-nsfw-uncensored-lora?not-for-all-audiences=true) | Adult-only, gated on Hugging Face; trigger `moss_xvstyle`. The repo also carries a full merged diffusers model, which is why its byte total is large. |

> [!NOTE]
> The **Color-Depth Control** weights now live only in the Comfy-Org repack: the original `darask0` repo started returning 401 on 2026-11-10. The transparent-background LoRA is the only one here that needs a different VAE — [trmz/flux2-klein-alpha](https://huggingface.co/trmz/flux2-klein-alpha) decodes the fourth channel.

<p id="lora-control" align="center">· · · · · · · · · · · · · ·</p>

### ▣ Condition-map control LoRAs

Seven sibling repos from **thedeoxen**, all trained on **MOSS-Image-SFT**. Despite `controlnet` in their names, these are **ControlNet-style edit LoRAs**, not ControlNets and not the official MOSS colour/depth concat adapter: load one with a LoRA loader and pass a single control image through **Text Encode MOSS-Image Edit**. Each repo ships its own ComfyUI workflow.

| Condition | Checkpoint | Size | Links | Control image |
| :--- | :---: | :---: | :---: | :--- |
| **Pose** | step 4500 | 0.29 GB | [![][gh-thedeoxen]](https://huggingface.co/thedeoxen/MOSS-Image-pose-controlnet) | DWPose / OpenPose skeleton on black; the prompt sets character, clothing and scene. |
| **Depth** | step 5000 | 0.29 GB | [![][gh-thedeoxen]](https://huggingface.co/thedeoxen/MOSS-Image-depth-controlnet) | Grayscale depth map, near is white; follows distance and layout. |
| **Segmentation** | step 4500 | 0.29 GB | [![][gh-thedeoxen]](https://huggingface.co/thedeoxen/MOSS-Image-segmentation-controlnet) | Flat-colour region map; the prompt decides what each region becomes. |
| **Scribble** | step 5000 | 0.29 GB | [![][gh-thedeoxen]](https://huggingface.co/thedeoxen/MOSS-Image-scribble-controlnet) | Loose white strokes on black; texture and style stay free. |
| **Normal map** | step 5000 | 0.29 GB | [![][gh-thedeoxen]](https://huggingface.co/thedeoxen/MOSS-Image-normal-controlnet) | Colour normal map; follows surface direction and volume. |
| **Lineart** | step 5000 | 0.29 GB | [![][gh-thedeoxen]](https://huggingface.co/thedeoxen/MOSS-Image-lineart-controlnet) | Fine white line drawing on black; the prompt sets colour and material. |
| **Canny** | step 4500 | 0.29 GB | [![][gh-thedeoxen]](https://huggingface.co/thedeoxen/MOSS-Image-canny-controlnet) | White-on-black Canny edges; follows contours and framing. |

> [!NOTE]
> All seven were published **2026-11-10** and are the same 0.29 GB adapter shape, so pick by the map you can produce. The [Comfy-Org colour-depth adapter](#official-comfy) is a different, concatenated architecture — these rows do not replace it.

