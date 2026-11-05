# Awesome MOSS-Image

A curated list of checkpoints, quants, step-distilled builds, LoRAs, text encoders, workflows, and tooling for **MOSS-Image**, OpenMOSS's 5B text-to-image model with native layout control.

<div align="center">

```text
  ┌──────────────────────────────────────────────────┐
  │   A W E S O M E   ·   M O S S - I M A G E        │
  │   base / quants / turbo / loras / tools          │
  └──────────────────────────────────────────────────┘
```

[![Hugging Face][hf-shield]][hf-url]
[![License][lic-shield]][lic-url]
[![Last Commit][commit-shield]][commit-url]
[![PRs Welcome][prs-shield]][prs-url]

</div>

> [!NOTE]
> **MOSS-Image** was announced **2026-10-16** with a base model and a curated-data SFT fine-tune. It pairs a **5B DiT** with a **Qwen3.5 9B** text encoder and the **FLUX.2 small VAE**, accepts prompts up to **4K tokens**, and renders English and Chinese text. Beyond plain prompts it takes **JSON layout specifications** — objects, rendered text and bounding boxes — through the same interface. Weights are **Apache-2.0**; pipeline code and third-party components keep their own licenses.

<details>
<summary><b>Table of Contents</b></summary>

* [Quick start](#quick-start)
* [Official checkpoints](#official)
  * [Base model](#official-base)
  * [SFT model](#official-sft)
  * [ComfyUI repack](#official-comfy)
* [Fine-tunes &amp; merged checkpoints](#finetune)
* [Quantizations](#quant)
  * [GGUF](#quant-gguf)
  * [SDNQ (4/8-bit)](#quant-sdnq)
  * [Nunchaku Lite (SVDQuant)](#quant-nunchaku)
  * [FP8 &amp; INT8 ConvRot](#quant-lowbit)
* [Turbo &amp; step distillation](#turbo)
  * [The lineage](#turbo-lineage)
  * [Distilled adapters](#turbo-adapters)
  * [Merged few-step DiTs](#turbo-merged)
* [LoRA &amp; adapters](#lora)
* [Text encoders &amp; prompt engines](#encoders)
* [Workflows &amp; custom nodes](#workflow)
* [Tools &amp; integrations](#tools)
  * [Modular Diffusers](#tools-diffusers)
  * [Runtimes &amp; apps](#tools-runtimes)
  * [Gotchas](#tools-gotchas)
* [Contributing](#contributing)

</details>

---

<a id="quick-start"></a>

## ⌬ Quick start

Pick your entry point based on the runtime you already have.

| I want to… | Use | Why |
| :--- | :--- | :--- |
| Run the reference model in Diffusers | **[MOSS-Image-Base](https://huggingface.co/OpenMOSS-Team/MOSS-Image-Base)** | Official BF16 shards with the bundled `pipeline_moss_image` custom pipeline |
| Get cleaner text grouping and layouts | **[MOSS-Image-SFT](https://huggingface.co/OpenMOSS-Team/MOSS-Image-SFT)** | Curated-data fine-tune, same architecture and sampler |
| Use it in ComfyUI | **[Comfy-Org/MOSS-Image](https://huggingface.co/Comfy-Org/MOSS-Image)** | Pre-split BF16 and INT8 ConvRot files in the right folders |
| Fit the DiT in 8–12 GB VRAM | **[SFT INT8 ConvRot](https://huggingface.co/Comfy-Org/MOSS-Image/resolve/main/diffusion_models/moss_image_sft_int8_convrot.safetensors)** | One 5.41 GB single-file DiT, no LoRA needed |
| Fit the whole stack in Diffusers | **[SFT SDNQ INT4](https://huggingface.co/OzzyGT/MOSS_Image_SFT_sdnq_dynamic_4bit)** | 10.03 GB total including the quantized text encoder |
| Generate in 2–8 steps | **[IDM 2 steps](https://huggingface.co/akhaliq/MOSS-Image-IDM-2-Steps)** · **[Turbo MFD](https://huggingface.co/congruency/MOSS-Image-Turbo-MFD)** | CFG-free distilled adapters on the SFT base |
| Run on a phone or CPU | **[4-step merged Q4_0 GGUF](https://huggingface.co/mischiefaudio/MOSS-Image-SFT-4step-merged-GGUF)** | 3.33 GB for stable-diffusion.cpp, distillation baked in |
| On a Mac (Apple Silicon) | **[MLX Q8-G64 text encoder](https://huggingface.co/Ouinche/moss-image-qwen3.5-te-mlx-q8g64)** | 10 GB peak encoding instead of 19 GB, for DiffusionBear |
| Place text and objects exactly | **[User guide · layout prompts](https://github.com/OpenMOSS/MOSS-Image/blob/main/USERGUIDE.md#layout-prompts-experimental)** | Pass a JSON layout as the prompt, with centered bounding boxes |

**Official resources**

* [Project page](https://openmoss.ai/MOSS-Image/): showcases, layout examples, model table
* [GitHub · OpenMOSS/MOSS-Image](https://github.com/OpenMOSS/MOSS-Image): inference code, Gradio app, ComfyUI plugin, user guides, prompt-enhancer skill
* [Hugging Face Space demo](https://huggingface.co/spaces/OpenMOSS-Team/MOSS-Image): official Gradio demo
* [Base model card](https://huggingface.co/OpenMOSS-Team/MOSS-Image-Base) · [SFT model card](https://huggingface.co/OpenMOSS-Team/MOSS-Image-SFT): weights, license, sampling defaults
* [ModelScope · Base](https://modelscope.cn/models/openmoss/MOSS-Image-Base) · [ModelScope · SFT](https://modelscope.cn/models/openmoss/MOSS-Image-SFT): official CN mirrors
* [User guide (EN)](https://github.com/OpenMOSS/MOSS-Image/blob/main/USERGUIDE.md) · [用户指南 (ZH)](https://github.com/OpenMOSS/MOSS-Image/blob/main/USERGUIDE_ZH.md): prompts, resolutions, LoRA training
* [ComfyUI plugin guide](https://github.com/OpenMOSS/MOSS-Image/blob/main/comfyui/README.md) · [example workflow](https://github.com/OpenMOSS/MOSS-Image/blob/main/comfyui/moss-image/example_workflows/moss-image.json)
* [Prompt Enhancer skill](https://github.com/OpenMOSS/MOSS-Image/tree/main/skills/moss-image-prompt-enhancer): agent skill for captions and JSON layout drafts
* [Technical report (PDF)](https://openmoss.ai/MOSS-Image/paper/MOSS_Image_Technical_Report.pdf) · [arXiv:2610.23374](https://arxiv.org/abs/2610.23374)

**Model facts worth knowing**

* **5B parameters** in the DiT, **Qwen3.5 9B** as the text encoder, **FLUX.2 small decoder** as the VAE. The text encoder is the largest component.
* One official repo is **27.23 GB**: **10.16 GB** DiT (3 shards), **16.79 GB** text encoder (4 shards), **0.25 GB** VAE.
* Prompts can be natural language, tag sequences, or a **JSON layout** with `summary.caption` and `items`. Boxes are `[y_min, x_min, y_max, x_max]`, `yxyx`, measured from the canvas center with `bbox_coord_space: "abs_centralized"`.
* Sampling defaults: **Flow-matching Euler, 50 steps, CFG 3, shift 5**, negative-branch layer skipping at index **22**, empty negative prompt. Guidance costs two transformer forwards per step.
* The transformer and text encoder run in **bf16**; the VAE and initial noise run in **fp32**. Python-API dimensions must be **positive multiples of 16**.
* **SFT** is fine-tuned from Base on curated data; the official comparison shows clearer text grouping and a more organized whiteboard layout. Both use the same sampling interface.
* ComfyUI support ships as a **native plugin** installed through ComfyUI Manager; integration into the upstream ComfyUI repository is in progress.

<p id="official" align="center">◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆</p>

## ▓ Official checkpoints

The reference weights. Start here before touching any community conversion.

<p id="official-base" align="center">· · · · · · · · · · · · · ·</p>

### ▣ Base model

| Name | Precision | Layout | DiT | Text encoder | VAE | Links |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **MOSS-Image-Base** | ![bf16][badge-bf16] | diffusers | 10.16 GB | 16.79 GB (Qwen3.5 9B) | 0.25 GB | [![][gh-OpenMOSS--Team]](https://huggingface.co/OpenMOSS-Team/MOSS-Image-Base) |

The reference release. Load it with `DiffusionPipeline.from_pretrained(..., custom_pipeline="pipeline_moss_image", trust_remote_code=True)`; the pipeline code and scheduler config ship inside the repo, so pull the whole repository rather than individual shards. Python 3.10+, PyTorch 2.9+, `diffusers>=0.38` and `transformers>=5.3`.

<p id="official-sft" align="center">· · · · · · · · · · · · · ·</p>

### ▣ SFT model

| Name | Precision | Layout | DiT | Text encoder | VAE | Links |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **MOSS-Image-SFT** | ![bf16][badge-bf16] | diffusers | 10.16 GB | 16.79 GB (Qwen3.5 9B) | 0.25 GB | [![][gh-OpenMOSS--Team]](https://huggingface.co/OpenMOSS-Team/MOSS-Image-SFT) |

Same architecture and sampling as Base, fine-tuned on curated data. It is the parent of most community LoRAs and distillations on this list; when a card says "SFT preferred", this is the checkpoint it means.

<p id="official-comfy" align="center">· · · · · · · · · · · · · ·</p>

### ▣ ComfyUI repack

**[Comfy-Org/MOSS-Image](https://huggingface.co/Comfy-Org/MOSS-Image)** is the day-0 ComfyUI repackage. It splits both official checkpoints into single-file DiTs and ships the text encoder at three precisions, two VAEs, and two community control LoRAs. Files land directly in `models/` with no renaming.

| Name | Precision | Size | Links |
| :--- | :---: | :---: | :---: |
| **Image Model (Base)** | ![bf16][badge-bf16] | 10.16 GB | [![][gh-Comfy--Org]](https://huggingface.co/Comfy-Org/MOSS-Image/resolve/main/diffusion_models/moss_image_base_bf16.safetensors) |
| **Image Model (Base)** | ![int8][badge-int8] | 5.41 GB | [![][gh-Comfy--Org]](https://huggingface.co/Comfy-Org/MOSS-Image/resolve/main/diffusion_models/moss_image_base_int8_convrot.safetensors) |
| **Image Model (SFT)** | ![bf16][badge-bf16] | 10.16 GB | [![][gh-Comfy--Org]](https://huggingface.co/Comfy-Org/MOSS-Image/resolve/main/diffusion_models/moss_image_sft_bf16.safetensors) |
| **Image Model (SFT)** | ![int8][badge-int8] | 5.41 GB | [![][gh-Comfy--Org]](https://huggingface.co/Comfy-Org/MOSS-Image/resolve/main/diffusion_models/moss_image_sft_int8_convrot.safetensors) |
| **Text Encoder** | ![bf16][badge-bf16] | 19.31 GB | [![][gh-Comfy--Org]](https://huggingface.co/Comfy-Org/MOSS-Image/resolve/main/text_encoders/qwen3.5_9b_bf16.safetensors) |
| **Text Encoder** | ![int8][badge-int8] | 9.75 GB | [![][gh-Comfy--Org]](https://huggingface.co/Comfy-Org/MOSS-Image/resolve/main/text_encoders/qwen3.5_9b_int8_convrot.safetensors) |
| **Text Encoder** | ![w4a8][badge-w4a8] | 7.34 GB | [![][gh-Comfy--Org]](https://huggingface.co/Comfy-Org/MOSS-Image/resolve/main/text_encoders/qwen3.5_9b_w4a8_gptq.safetensors) |
| **VAE** | ![fp32][badge-fp32] | 0.34 GB | [![][gh-Comfy--Org]](https://huggingface.co/Comfy-Org/MOSS-Image/resolve/main/vae/flux2-vae.safetensors) |
| **VAE (small)** | ![fp32][badge-fp32] | 0.25 GB | [![][gh-Comfy--Org]](https://huggingface.co/Comfy-Org/MOSS-Image/resolve/main/vae/flux2-vae_small.safetensors) |
| **Color-depth control LoRA** | ![bf16][badge-bf16] | 0.31 GB | [![][gh-Comfy--Org]](https://huggingface.co/Comfy-Org/MOSS-Image/resolve/main/loras/moss_image_color_depth_control.safetensors) |
| **Edit LoRA rank 64** | ![bf16][badge-bf16] | 0.30 GB | [![][gh-Comfy--Org]](https://huggingface.co/Comfy-Org/MOSS-Image/resolve/main/loras/moss_image_edit_lora_rank_64_bf16.safetensors) |

> [!TIP]
> `ConvRot` is ComfyUI's native rotated-channel integer format. A recent build loads those rows with the standard diffusion-model and text-encoder loaders; no custom nodes needed.
>
> Destination folders: **Image Model** → `models/diffusion_models/`, **Text Encoder** → `models/text_encoders/`, **VAE** → `models/vae/`, both LoRA rows → `models/loras/`.
>
> The single-file **Text Encoder** is 19.31 GB while the four official diffusers shards total 16.79 GB — different packaging, not a different model. The two LoRA rows are community work by [darask0](https://huggingface.co/darask0/MOSS-Image-color-depth-control) and [chinoll](https://huggingface.co/chinoll/Moss-Image-Edit), repacked here for convenience rather than official OpenMOSS releases.
>
> Ready-made graphs for this pack live in [Comfy-Org/workflow_templates](https://github.com/Comfy-Org/workflow_templates/blob/main/templates/image_moss_image_t2i.json) (text-to-image, Base text-to-image, edit LoRA).

<p id="finetune" align="center">◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆</p>

## ◧ Fine-tunes &amp; merged checkpoints

Full DiT weight sets, not adapters. Each of these replaces the diffusion model in the official pipeline; the text encoder, VAE and pipeline code still come from the official repos unless the card says otherwise.

| Name | Type | Precision | Size | Links | Notes |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **SFT_clear** | ![Finetune][ltype-finetune] | ![bf16][badge-bf16] ![mxfp8][badge-mxfp8] ![fp8][badge-fp8] ![int8][badge-int8] | 10.16 / 6.00 / 5.86 / 5.87 GB | [![][gh-easygoing0114]](https://huggingface.co/easygoing0114/MOSS-Image-SFT_clear) | Fine-tune of SFT that cuts noise and raises saturation and contrast, aimed at anime illustration and photoreal. Four single-file cuts: BF16, MXFP8, FP8-svd-scaled, INT8 ConvRot HQ. |
| **MOSS-Image-heal** | ![Finetune][ltype-finetune] | ![bf16][badge-bf16] ![fp16][badge-fp16] ![int8][badge-int8] | 10.16 / 10.16 / 5.41 GB | [![][gh-KBlueLeaf]](https://huggingface.co/KBlueLeaf/MOSS-Image-heal) | Full fine-tune of Base with a healed residual stream: peak activation 3,083 vs 58,411 and 7.5× lower stream RMS, with outputs kept close to base (flow loss 0.4289 vs 0.4280). Ships ComfyUI single files and a diffusers transformer. Made with [KohakuMoss](https://github.com/KohakuBlueleaf/KohakuMossTrainer). |
| **AIO checkpoint** | ![All-in-one][ltype-aio] | ![bf16][badge-bf16] | 29.23 GB | [![][gh-marduk191]](https://huggingface.co/marduk191/Moss-Image-misc) | One file bundling the BF16 DiT, text encoder and FLUX.2 small VAE for ComfyUI's simple checkpoint loader. No folder layout to get wrong, and no way to swap components. |

> [!NOTE]
> The three **heal** formats are the same trained weights in different export layouts, not three training runs. **SFT_clear** ships four quantization cuts of one fine-tune — pick by VRAM budget, not by quality claims. Both keep the official text encoder and VAE; the **AIO** file is the only one here that carries them.

<p id="quant" align="center">◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆</p>

## ◈ Quantizations

Community conversions of the Base and SFT stacks. Sizes are total weight bytes per repo unless a single file is named. Distilled few-step builds live under [Turbo](#turbo); text-encoder-only conversions live under [Text encoders](#encoders).

<p id="quant-gguf" align="center">· · · · · · · · · · · · · ·</p>

### ▣ GGUF

DiT-only weights for [stable-diffusion.cpp](https://github.com/leejet/stable-diffusion.cpp) and ComfyUI-GGUF. Bring your own text encoder and VAE.

| Name | Precision | Size | Links | Notes |
| :--- | :---: | :---: | :---: | :--- |
| **SFT GGUF Q8_0** | ![Q8_0][badge-Q8_0] | 5.71 GB | [![][gh-axiomofmind]](https://huggingface.co/axiomofmind/Moss-Image-SFT-GGUF/resolve/main/MOSS-Image-SFT-Q8_0.gguf) | Highest-fidelity cut of the SFT DiT; the base expected by the [Beauty LoRA GGUF](#lora). |
| **SFT GGUF Q4_K_S** | ![Q4_K_S][badge-Q4_K_S] | 3.46 GB | [![][gh-axiomofmind]](https://huggingface.co/axiomofmind/Moss-Image-SFT-GGUF/resolve/main/MOSS-Image-SFT-Q4_K_S.gguf) | The small cut of the same repo. |
| **4-step merged Q4_0** | ![Q4_0][badge-Q4_0] | 3.33 GB | [![][gh-mischiefaudio]](https://huggingface.co/mischiefaudio/MOSS-Image-SFT-4step-merged-GGUF/resolve/main/MOSS-Image-SFT-4step-v2.0-merged-Q4_0.gguf) | The 4-step distillation is baked in, so it needs no LoRA at runtime. See [Turbo](#turbo-merged). The card demonstrates 1024² generation on a phone at 4 steps, CFG 1. |

<p id="quant-sdnq" align="center">· · · · · · · · · · · · · ·</p>

### ▣ SDNQ (4/8-bit)

Dynamic [SDNQ](https://github.com/Disty0/sdnq) quants in a **Modular Diffusers** layout: transformer and Qwen3.5 text encoder quantized, VAE left unquantized. Needs SDNQ v0.2.8+ and diffusers from `main` (0.41.0.dev0 or later).

| Name | Precision | Size | Links | Notes |
| :--- | :---: | :---: | :---: | :--- |
| **Base SDNQ INT8** | ![int8][badge-int8] | 15.29 GB | [![][gh-OzzyGT]](https://huggingface.co/OzzyGT/MOSS_Image_Base_sdnq_dynamic_8bit) | 8.97 GB text encoder + 5.14 GB transformer. |
| **Base SDNQ INT4** | ![int4][badge-int4] | 10.03 GB | [![][gh-OzzyGT]](https://huggingface.co/OzzyGT/MOSS_Image_Base_sdnq_dynamic_4bit) | 5.93 GB text encoder + 2.92 GB transformer. |
| **SFT SDNQ INT8** | ![int8][badge-int8] | 15.29 GB | [![][gh-OzzyGT]](https://huggingface.co/OzzyGT/MOSS_Image_SFT_sdnq_dynamic_8bit) | The SFT counterpart of the INT8 Base build. |
| **SFT SDNQ INT4** | ![int4][badge-int4] | 10.03 GB | [![][gh-OzzyGT]](https://huggingface.co/OzzyGT/MOSS_Image_SFT_sdnq_dynamic_4bit) | Cheapest complete diffusers stack on this list. |

> [!TIP]
> These four are **complete repos**, not DiT-only files: the byte totals include the text encoder, vision encoder, VAE and tokenizer. Pair them with the [custom blocks](#tools-diffusers) when running the Modular Diffusers pipeline. The cards ship side-by-side BF16/INT8/INT4 comparison images at the same prompt and seed.

<p id="quant-nunchaku" align="center">· · · · · · · · · · · · · ·</p>

### ▣ Nunchaku Lite (SVDQuant)

W4A4 SVDQuant transformers with a serialized BitsAndBytes **NF4** text encoder, BF16 compute and the original FP32 VAE. Both need a [Nunchaku Lite-capable Diffusers build](https://github.com/rootonchair/diffusers/tree/86960edbfda2a74d9764de73067b03701fea525e), Transformers 5.19.0, BitsAndBytes 0.50.2 and `DIFFUSERS_TRUST_REMOTE_KERNELS=true`.

| Name | Precision | Size | Links | Notes |
| :--- | :---: | :---: | :---: | :--- |
| **SFT NVFP4 r32** | ![nvfp4][badge-nvfp4] | 9.53 GB | [![][gh-lite--infer]](https://huggingface.co/lite-infer/moss-image-sft-nunchaku-lite-nvfp4-r32) | 3.05 GB transformer at rank 32 with GPTQ. **NVFP4 needs an NVIDIA Blackwell GPU or newer.** |
| **SFT INT4 r128** | ![int4][badge-int4] | 9.84 GB | [![][gh-lite--infer]](https://huggingface.co/lite-infer/moss-image-sft-nunchaku-lite-int4-r128) | 3.36 GB transformer at rank 128; the higher-rank, hardware-independent build. |

<p id="quant-lowbit" align="center">· · · · · · · · · · · · · ·</p>

### ▣ FP8 &amp; INT8 ConvRot

Single-file DiTs for ComfyUI's standard loaders. The [official Comfy-Org repack](#official-comfy) already carries INT8 ConvRot cuts of both checkpoints; this row is the low-VRAM alternative built for a specific node pack.

| Name | Precision | Size | Links | Notes |
| :--- | :---: | :---: | :---: | :--- |
| **Kohaku FP8 row weights** | ![fp8][badge-fp8] | 5.17 / 5.17 GB | [![][gh-Andylinux]](https://huggingface.co/Andylinux/MOSS-Image-Kohaku-FP8) | Base and SFT DiT cut for [MOSS LowVRAM Optimized](#workflow). Each row file supports FP8 or 16-bit activation arithmetic in the node pack. Text encoder, tokenizer and VAE are separate. |

> [!NOTE]
> **SFT_clear** also ships FP8, MXFP8 and INT8 ConvRot HQ cuts of its own fine-tune (see [Fine-tunes](#finetune)), and the [Spectrallic 8-step builds](#turbo-merged) are INT8 ConvRot with distillation merged in. ConvRot files are not interchangeable across exporters: some releases keep `proj.q`/`proj.k`/`proj.v` separate while the Comfy-Org layout fuses them into `layers.N.proj.qkv`.

<p id="turbo" align="center">◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆</p>

## ▸ Turbo &amp; step distillation

Everything here trades sampling steps for fidelity. The official model is **50 steps, CFG 3, shift 5**; the distilled lines cut that to 2–16 network evaluations, usually CFG-free. OpenMOSS does not ship an official Turbo checkpoint — all of these are community work, and **their adapters are not interchangeable**.

<p id="turbo-lineage" align="center">· · · · · · · · · · · · · ·</p>

### ▣ The lineage

Most few-step builds descend from one DMD chain; the MFD and 16-step lines are independent.

```text
OpenMOSS-Team/MOSS-Image-SFT   50 steps · CFG 3 · shift 5
        │
        ├─► lodestones 8-step data-space DMD (2600 updates)
        │        └─► darask0 4-step DMAD (500 iters) ──► akhaliq IDM 2-step (1000 iters)
        │                 └─► akhaliq SFT-4-steps v2.0 ──► merged 4-step Q4_0 GGUF
        │                                             └─► Spectrallic 8-step INT8 ConvRot
        ├─► congruency Turbo MFD            4–8 steps  (independent, Mean Flow)
        └─► Coffmeteor Turbo Lite          16 steps · CFG 1  (independent, beta)
```

The [IDM card](https://huggingface.co/akhaliq/MOSS-Image-IDM-2-Steps) documents the chain and an honest regression: its **v2 edit continuation degraded general text-to-image**, so v1 export 750 is the recommended T2I checkpoint. The 8-step DMD repos are marked work-in-progress by their author. Pick one family and keep its sampling settings.

<p id="turbo-adapters" align="center">· · · · · · · · · · · · · ·</p>

### ▣ Distilled adapters

| Name | Steps | Precision | Size | Links | Notes |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **IDM 2 steps** | 2 | ![bf16][badge-bf16] | 2.18 GB | [![][gh-akhaliq]](https://huggingface.co/akhaliq/MOSS-Image-IDM-2-Steps) | Two DiT forwards at 1024², no CFG. Community Intrinsic Distribution Matching; v1 at `runs/idm/export/000750/` for T2I, v2 folded adapter for editing. The repo is 89.79 GB including training caches. |
| **rapid-moss 4-step** | 4 | ![bf16][badge-bf16] | 1.19 GB | [![][gh-darask0]](https://huggingface.co/darask0/MOSS-Image_4-step_adapter_lora) | DMAD adapter on the 8-step DMD student, CFG-free. Ships `lora.safetensors` + `time_embedding.safetensors`. |
| **SFT 4-step v2.0** | 4 | ![bf16][badge-bf16] ![fp32][badge-fp32] | 1.19 / 20.33 GB | [![][gh-akhaliq]](https://huggingface.co/akhaliq/MOSS-Image-SFT-4-steps) | The widely used 4-step line; also ships a 20.33 GB FP32 merged DiT and full diffusers layout. Parent of the merged GGUF below. |
| **SFT 8-step DMD** | 8 | ![bf16][badge-bf16] | 1.19 GB | [![][gh-lodestones]](https://huggingface.co/lodestones/MOSS-Image-SFT-8-steps-dmd) | Reference data-space DMD distill; `lora.safetensors`, `lora_latest.safetensors`, generator and full transformer. 23.98 GB repo. |
| **Base 8-step DMD** | 8 | ![bf16][badge-bf16] | 1.19 GB | [![][gh-lodestones]](https://huggingface.co/lodestones/MOSS-Image-Base-8-steps-dmd) | The same distillation built on Base rather than SFT; ships `lora_dmd2.safetensors`. |
| **Turbo MFD** | 4–8 | ![bf16][badge-bf16] | 0.08 GB × 5 | [![][gh-congruency]](https://huggingface.co/congruency/MOSS-Image-Turbo-MFD) | Mean Flow Distillation. Base v0.1 and SFT v0.1 (50 aux / 20 student), SFT v0.2 (12 aux / 6 student). `.comfy.safetensors` builds included. |
| **Turbo Lite 16step-cfg1** | 16 | ![bf16][badge-bf16] ![fp8][badge-fp8] | 0.59 GB | [![][gh-Coffmeteor]](https://huggingface.co/Coffmeteor/MOSS-Image-Turbo) | Beta rank-128 LoRA, CFG 1, Euler/simple, shift 5, strength 1.0. Also ships 10.16 GB BF16 and 5.41 GB FP8 merged DiTs plus workflows. The card says anatomy is **not** improved. |

<p id="turbo-merged" align="center">· · · · · · · · · · · · · ·</p>

### ▣ Merged few-step DiTs

Distillation baked into the weights, so no LoRA loader is needed. Good drop-ins for ComfyUI and stable-diffusion.cpp.

| Name | Steps | Precision | Size | Links | Notes |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **Base 8-step DMD INT8 ConvRot** | 8 | ![int8][badge-int8] | 5.41 GB | [![][gh-Spectrallic]](https://huggingface.co/Spectrallic/MOSS-Image-Base-8step-DMD-int8-convrot) | lodestones DMD merged into Base, quantized to INT8 ConvRot. Tensor names, dtypes and shapes match the Comfy-Org Base file exactly. |
| **SFT 8-step DMD INT8 ConvRot** | 8 | ![int8][badge-int8] | 5.41 GB | [![][gh-Spectrallic]](https://huggingface.co/Spectrallic/MOSS-Image-SFT-8step-DMD-int8-convrot) | The SFT counterpart; a direct replacement for `moss_image_sft_int8_convrot.safetensors`. |
| **SFT 4-step merged Q4_0 GGUF** | 4 | ![Q4_0][badge-Q4_0] | 3.33 GB | [![][gh-mischiefaudio]](https://huggingface.co/mischiefaudio/MOSS-Image-SFT-4step-merged-GGUF) | v2.0 4-step distillation merged into the SFT DiT, then quantized. Runs on stable-diffusion.cpp; see the [GGUF table](#quant-gguf). |

> [!WARNING]
> Do **not** stack a few-step adapter on a merged few-step checkpoint. The [Shift Scheduler](https://github.com/ukr8b3g-cmyk/ComfyUI-MOSS-Shift-Scheduler) card also notes its schedules were developed specifically for SFT + a DMD acceleration LoRA, with normal 50-step generation largely untested.

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

<p id="encoders" align="center">◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆</p>

## ⚲ Text encoders &amp; prompt engines

MOSS-Image conditions on **Qwen3.5 9B**, the largest single component at **16.79 GB** in the official four-shard layout (**19.31 GB** in the Comfy-Org single-file pack). Everything below either shrinks that encoder or rewrites prompts before it reaches the DiT. None of it is required for a standard install.

### ▣ Alternative text encoders

| Name | Base | Precision | Size | Links | Notes |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **Mini encoder (0.8B / 2B / 4B)** | Qwen3.5 0.8B / 2B / 4B | ![int8][badge-int8] ![w4a8][badge-w4a8] | 12.50 GB | [![][gh-cyberlangke]](https://huggingface.co/cyberlangke/moss-image-mini-encoder) | Students are frozen; only a per-token linear projection into the DiT's 4096-dim space is trained. Five pre-quantized student weights plus matching `*_linear.safetensors` adapters. Needs the [custom nodes](https://github.com/1cyberlangke1/moss-image-mini-encoder); the card publishes an 8-tier × 10-prompt cosine/timing matrix. |
| **Moqs 2B** | Qwen3.5 2B | ![bf16][badge-bf16] | 3.85 GB | [![][gh-nebulette]](https://huggingface.co/nebulette/moss-image-2b-encoder) | Feature-projection encoder with tokenizer and preprocessor configs. The card frames it as a dataset rebuild covering tags and natural-language captions, so existing LoRAs are expected to work out of the box. |
| **MLX Q8-G64 text encoder** | Qwen3.5 9B | ![int8][badge-int8] | 10.04 GB | [![][gh-Ouinche]](https://huggingface.co/Ouinche/moss-image-qwen3.5-te-mlx-q8g64) | Apple MLX 8-bit affine (`group_size=64`) for 16 GB Apple Silicon Macs; **Gated DeltaNet recurrent projections stay bf16** to preserve the delta-rule state. Reported 17 s cold / ~4 s warm prompt encoding vs 61 s on CPU, 10 GB peak vs 19 GB, cosine 0.9995. Built for [DiffusionBear](https://github.com/OuincheWinch/DiffusionBear). |
| **Comfy-Org w4a8 GPTQ TE** | Qwen3.5 9B | ![w4a8][badge-w4a8] | 7.34 GB | [![][gh-Comfy--Org]](https://huggingface.co/Comfy-Org/MOSS-Image/resolve/main/text_encoders/qwen3.5_9b_w4a8_gptq.safetensors) | The cheapest ready-to-use text encoder from the [official repack](#official-comfy); no retraining or projection. |

### ▣ Prompt engines

| Name | Type | Links | Notes |
| :--- | :---: | :---: | :--- |
| **Prompt Enhancer (official)** | ![Agent skill][ltype-skill] | [![][gh-OpenMOSS]](https://github.com/OpenMOSS/MOSS-Image/tree/main/skills/moss-image-prompt-enhancer) | Agent skill for GPT-6-class assistants with local file and Python tools. Converts image ideas into detailed captions or **JSON Layout prompts**, with a bundled script that turns pixel-coordinate drafts into compact centered integer coordinates. It prepares prompts only — it does not call an image service. |
| **Prompt Enhancer I2I** | ![Ollama][ltype-pe] | [![][gh-DutchIT]](https://huggingface.co/DutchIT/moss-image-prompt-enhancer-i2i) | Packages the official skill as a local [Ollama](https://ollama.com) model so it runs offline without an agent. Built on Huihui Qwen3-VL-8B-Instruct-abliterated. Two modes: **Caption** returns one English prose description, **Layout** returns a JSON draft with pixel bounding boxes. Output is the prompt only, no preamble. |

> [!TIP]
> A mini encoder and its projection adapter are a **matched pair** — the card publishes separate `*_linear.safetensors` files for bf16, int8 and w4a8 student builds. Mixing them changes the cosine figures quoted in its benchmark. The MLX encoder is text-encoder-only; the rest of the Apple Silicon stack comes from DiffusionBear.

<p id="workflow" align="center">◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆</p>

## ⬡ Workflows &amp; custom nodes

Ready-made ComfyUI graphs and the node packs that make them work. JSON-only repos are graph collections, not model-weight releases.

### ▣ Workflow collections

| Name | Graphs | Links | Notes |
| :--- | :---: | :---: | :--- |
| **Community workflows 326** | 6 | [![][gh-AI--KSK]](https://huggingface.co/AI-KSK/moss-image-community-workflows-326) | Character reference, MAX ULTRA QHD, SFT style lab, beauty, step lab (50/8/4) and layout. Each graph has a RunningHub and Civitai mirror plus a `SHA256SUMS.json`. |
| **Five workflows 324** | 5 | [![][gh-AI--KSK]](https://huggingface.co/AI-KSK/moss-image-five-workflows-324) | Comparison-oriented: colour-depth LoRA, three-preset speed test, Base vs SFT, JSON layout, plain SFT starter. 70 nodes total including five notes; the card explicitly says it is not a completed benchmark. |
| **4K workflow set** | 6 | [![][gh-AiNinja94]](https://huggingface.co/AiNinja94/Moss-Image-ComfyUI-Workflow/tree/main) | Native 4K with RoPE NTK scaling, latent and pixel upscale, image-to-image 4K, plus colour-depth and bounding-box control graphs. |
| **Comfy-Org templates** | 3 | [![][cb-Comfy--Org]](https://github.com/Comfy-Org/workflow_templates/blob/main/templates/image_moss_image_t2i.json) ┊ [![][cb-Comfy--Org]](https://github.com/Comfy-Org/workflow_templates/blob/main/templates/image_moss_image_edit.json) | Official text-to-image, Base text-to-image and image-edit templates from Comfy-Org's `workflow_templates` repo; also linked from the [repack card](#official-comfy). |

### ▣ Custom nodes

| Name | License | Links | Notes |
| :--- | :---: | :---: | :--- |
| **MOSS Shift Scheduler** | MIT | [![][gh-ukr8b3g--cmyk]](https://github.com/ukr8b3g-cmyk/ComfyUI-MOSS-Shift-Scheduler) | Three general-purpose sigma schedulers — Shift, Curve and Sigma — developed for SFT plus a DMD acceleration LoRA. The author warns normal 50-step use is barely tested and other models are unverified. |
| **MOSS Emphasis** | GPL-3.0 | [![][gh-ukr8b3g--cmyk]](https://github.com/ukr8b3g-cmyk/ComfyUI-MOSS-Emphasis) | Prompt weighting and NegPiP nodes. v0.11.0 adds a Forge-compatible native MOSS mode that processes positive and negative prompts independently; V-Scaling is off by default. |
| **MossPanelDataset** | MIT | [![][gh-sepiablue--ai]](https://github.com/sepiablue-ai/ComfyUI-MossPanelDataset) | Pick one of 196 manga panel layouts (2–6 panels) from previews, then fill characters, scenes and dialogue; inputs grow and shrink with the layout and restore when you switch back. Catalog works offline. |
| **ComfyUI Mobile Pocket** | MIT | [![][gh-Klark61]](https://github.com/Klark61/ComfyUI-Mobile-Pocket) | Phone UI for Android/iOS with a Generate screen that builds a form from saved workflows, plus the full node editor reworked for touch. |
| **MOSS LowVRAM Optimized** | — | [![][gh-Andy--linux]](https://github.com/Andy-linux/ComfyUI-MOSS-LowVRAM-Optimized) | Multi-precision inference with a VRAM budget and weight-scheduling. This is the node pack the [Kohaku FP8 row weights](#quant-lowbit) are cut for. |

> [!NOTE]
> The two ukr8b3g-cmyk packs are experimental and describe themselves as such. Both assume the SFT checkpoint; if you run Base, the [50-step defaults](#quick-start) are a safer starting point.

<p id="tools" align="center">◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆</p>

## ⚙ Tools &amp; integrations

Runtimes, layout conversions and pipelines that change how MOSS-Image loads rather than what it draws.

<p id="tools-diffusers" align="center">· · · · · · · · · · · · · ·</p>

### ▣ Modular Diffusers

The official repos ship a **custom pipeline** that must be loaded with `trust_remote_code=True`. [OzzyGT](https://huggingface.co/OzzyGT) re-laid the same BF16 weights into standard diffusers folders and published the blocks that rebuild MOSS-Image as a Modular Diffusers pipeline.

| Name | Type | Size | Links | Notes |
| :--- | :---: | :---: | :---: | :--- |
| **Base modular Diffusers** | ![Port][ltype-port] | 27.22 GB | [![][gh-OzzyGT]](https://huggingface.co/OzzyGT/MOSS_Image_Base_diffusers) | Official Base weights, standard `transformer/` + `text_encoder/` + `vision_encoder/` + `vae/` layout. |
| **SFT modular Diffusers** | ![Port][ltype-port] | 27.22 GB | [![][gh-OzzyGT]](https://huggingface.co/OzzyGT/MOSS_Image_SFT_diffusers) | The SFT counterpart; ready-to-use scripts live in [diffusers-recipes](https://github.com/asomoza/diffusers-recipes/blob/main/models/moss_image/README.md). |
| **MOSS-Image custom blocks** | ![Node pack][ltype-nodes] | 0.00 GB | [![][gh-OzzyGT]](https://huggingface.co/OzzyGT/moss_image_custom_blocks) | Twelve Python blocks (`encoders.py`, `denoise.py`, `decoders.py`, `before_denoise.py`) that load the transformer code from the repos above. No weights. |

> [!WARNING]
> All three need **diffusers from `main`** (`0.41.0.dev0` or later). The card notes that with every component on the GPU, the 2048×1152 example needs **more than 32 GB of VRAM** — use one of the [SDNQ builds](#quant-sdnq) as the component for smaller cards.

<p id="tools-runtimes" align="center">· · · · · · · · · · · · · ·</p>

### ▣ Runtimes &amp; apps

| Name | Type | Links | Notes |
| :--- | :---: | :---: | :--- |
| **SDNQ** | ![Engine][ltype-engine] | [![][gh-Disty0]](https://github.com/Disty0/sdnq) | Quantization engine behind the four [SDNQ repos](#quant-sdnq); v0.2.8 or newer. |
| **stable-diffusion.cpp** | ![Engine][ltype-engine] | [![][gh-leejet]](https://github.com/leejet/stable-diffusion.cpp) | Pure C/C++ runtime for the [GGUF DiT](#quant-gguf) and [GGUF LoRA](#lora); the merged 4-step build was demonstrated on a phone. |
| **DiffusionBear** | ![Utility][ltype-utility] | [![][gh-OuincheWinch]](https://github.com/OuincheWinch/DiffusionBear) | Local image studio for Apple Silicon; consumes the [MLX Q8-G64 text encoder](#encoders) to fit 16 GB Macs. |
| **moss-image-mini-encoder nodes** | ![Node pack][ltype-nodes] | [![][gh-1cyberlangke1]](https://github.com/1cyberlangke1/moss-image-mini-encoder) | Custom nodes, conversion script and the full benchmark behind the [mini text encoders](#encoders). |
| **MOSS + Qwen faceswap** | ![Pipeline][ltype-pipeline] | [![][gh-loyalchiiina]](https://github.com/loyalchiiina/moss-qwen-faceswap) | MOSS-Image-SFT body generation chained into Qwen-Image-2.1 BFS face swap plus hand redraw and 4× upscale, with a Gradio console. The card restricts use to AI-generated or explicitly authorised subjects and reports a 0.783 similarity measurement. |
| **KohakuMoss Trainer** | ![Training][ltype-training] | [![][gh-KohakuBlueleaf]](https://github.com/KohakuBlueleaf/KohakuMossTrainer) | Training framework used for [MOSS-Image-heal](#finetune). |

<p id="tools-gotchas" align="center">· · · · · · · · · · · · · ·</p>

### ▣ Gotchas

1. The official pipeline is **remote code**: `custom_pipeline="pipeline_moss_image"` and `trust_remote_code=True`. Install `diffusers>=0.38`, `transformers>=5.3`, `accelerate>=1.10`.
2. Guidance uses `uncond_skip_layers=[22]` for negative-branch layer skipping; pass `[]` to turn it off. CFG 3 means **two transformer forwards per step**, so 50 steps is 100 evaluations — that is what the [Turbo](#turbo) builds are cutting.
3. Width and height must be **positive multiples of 16** in the Python API. For layout prompts, generate at the **same dimensions** the coordinate draft used.
4. Keep the transformer and text encoder in **bf16** and the VAE plus initial noise in **fp32**: `torch_dtype={"default": torch.bfloat16, "vae": torch.float32}`.
5. Modular Diffusers, SDNQ and Nunchaku Lite all require diffusers from **`main`**, not a tagged release. Nunchaku Lite additionally pins Transformers 5.19.0 and BitsAndBytes 0.50.2, and NVFP4 needs Blackwell.
6. **ConvRot layouts are not portable.** Some exporters keep `proj.q`/`proj.k`/`proj.v` separate; the Comfy-Org and Spectrallic files fuse them into `layers.N.proj.qkv`. A loader that assumes one layout will fail on the other.
7. GGUF DiTs are **transformer-only** — bring the text encoder and VAE. A GGUF LoRA needs a GGUF base, not a safetensors one.
8. Few-step adapters belong to **different distillation families**; load exactly one, on the matching base, and never stack one on a merged few-step checkpoint.
9. Chinese prompts render natively. The [Prompt Enhancer](#encoders) is what turns short ideas into long English captions or JSON layouts.
10. **Apache-2.0 covers the MOSS-Image weights.** Pipeline code and third-party components keep their own licenses, and individual community LoRAs may be `other` — the [83-adapter pack](#lora), for example, carries a bespoke license.

---

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
new number. Regenerate and verify with:

```bash
python scripts/gen_badges.py
python scripts/build_readme.py
python scripts/check_links.py
```

`docs/` and `scripts/` are local tooling and source data: [`docs/classified.json`](docs/classified.json)
is the registry of every link with its section, kind, byte totals and HF metadata,
[`docs/links.txt`](docs/links.txt) is the normalized URL list, and `docs/raw/` holds
the fetched model cards and API responses. Those files are intentionally not
tracked; only the compiled document and its source blocks are published.

When adding a row, include the owner badge, precision badge, size and one short
clause saying what the artifact does. Sizes come from the Hugging Face API, not
from the card's prose.

<div align="center">

**Awesome MOSS-Image** · an unofficial community list for [OpenMOSS](https://github.com/OpenMOSS)'s MOSS-Image

</div>

<!-- SHIELDS -->
[hf-shield]: https://img.shields.io/badge/Hugging%20Face-FFD21E?style=for-the-badge&logo=huggingface&logoColor=black
[hf-url]: https://huggingface.co/OpenMOSS-Team/MOSS-Image-Base
[lic-shield]: https://img.shields.io/badge/model%20license-Apache--2.0-green?style=for-the-badge
[lic-url]: https://huggingface.co/OpenMOSS-Team/MOSS-Image-Base/blob/main/LICENSE
[commit-shield]: https://img.shields.io/badge/docs-auto--generated-blue?style=for-the-badge
[commit-url]: https://github.com/OpenMOSS/MOSS-Image
[prs-shield]: https://img.shields.io/badge/PRs-welcome-brightgreen?style=for-the-badge
[prs-url]: https://github.com/OpenMOSS/MOSS-Image/pulls

<!-- OWNER BADGES -->
[cb-Comfy--Org]: https://img.shields.io/badge/Comfy--Org-17a2b8?style=flat-square&logo=github&logoColor=white
[gh-1cyberlangke1]: https://img.shields.io/badge/1cyberlangke1-17a2b8?style=flat-square&logo=github&logoColor=white
[gh-AI--KSK]: https://img.shields.io/badge/AI--KSK-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-AiNinja94]: https://img.shields.io/badge/AiNinja94-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-akhaliq]: https://img.shields.io/badge/akhaliq-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-Andy--linux]: https://img.shields.io/badge/Andy--linux-17a2b8?style=flat-square&logo=github&logoColor=white
[gh-Andylinux]: https://img.shields.io/badge/Andylinux-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-Ansh92928219]: https://img.shields.io/badge/Ansh92928219-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-aoleg]: https://img.shields.io/badge/aoleg-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-axiomofmind]: https://img.shields.io/badge/axiomofmind-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-chinoll]: https://img.shields.io/badge/chinoll-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-Coffmeteor]: https://img.shields.io/badge/Coffmeteor-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-Comfy--Org]: https://img.shields.io/badge/Comfy--Org-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-comfyanonymous]: https://img.shields.io/badge/comfyanonymous-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-congruency]: https://img.shields.io/badge/congruency-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-cyberlangke]: https://img.shields.io/badge/cyberlangke-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-darask0]: https://img.shields.io/badge/darask0-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-deadman44]: https://img.shields.io/badge/deadman44-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-Disty0]: https://img.shields.io/badge/Disty0-17a2b8?style=flat-square&logo=github&logoColor=white
[gh-DutchIT]: https://img.shields.io/badge/DutchIT-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-easygoing0114]: https://img.shields.io/badge/easygoing0114-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-genai--craft]: https://img.shields.io/badge/genai--craft-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-Helives]: https://img.shields.io/badge/Helives-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-KBlueLeaf]: https://img.shields.io/badge/KBlueLeaf-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-Klark61]: https://img.shields.io/badge/Klark61-17a2b8?style=flat-square&logo=github&logoColor=white
[gh-KohakuBlueleaf]: https://img.shields.io/badge/KohakuBlueleaf-17a2b8?style=flat-square&logo=github&logoColor=white
[gh-leejet]: https://img.shields.io/badge/leejet-17a2b8?style=flat-square&logo=github&logoColor=white
[gh-lite--infer]: https://img.shields.io/badge/lite--infer-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-lodestones]: https://img.shields.io/badge/lodestones-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-loyalchiiina]: https://img.shields.io/badge/loyalchiiina-17a2b8?style=flat-square&logo=github&logoColor=white
[gh-marduk191]: https://img.shields.io/badge/marduk191-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-mischiefaudio]: https://img.shields.io/badge/mischiefaudio-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-nebulette]: https://img.shields.io/badge/nebulette-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-OpenMOSS]: https://img.shields.io/badge/OpenMOSS-17a2b8?style=flat-square&logo=github&logoColor=white
[gh-OpenMOSS--Team]: https://img.shields.io/badge/OpenMOSS--Team-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-Ouinche]: https://img.shields.io/badge/Ouinche-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-OuincheWinch]: https://img.shields.io/badge/OuincheWinch-17a2b8?style=flat-square&logo=github&logoColor=white
[gh-OzzyGT]: https://img.shields.io/badge/OzzyGT-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-Playtime--AI]: https://img.shields.io/badge/Playtime--AI-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-pmczip]: https://img.shields.io/badge/pmczip-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-RunningHubAI]: https://img.shields.io/badge/RunningHubAI-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-sepiablue--ai]: https://img.shields.io/badge/sepiablue--ai-17a2b8?style=flat-square&logo=github&logoColor=white
[gh-ShiyumeMeguri]: https://img.shields.io/badge/ShiyumeMeguri-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-Spectrallic]: https://img.shields.io/badge/Spectrallic-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-Te2douNin9]: https://img.shields.io/badge/Te2douNin9-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-ukr8b3g--cmyk]: https://img.shields.io/badge/ukr8b3g--cmyk-17a2b8?style=flat-square&logo=github&logoColor=white

<!-- PRECISION BADGES -->
[badge-bf16]: https://img.shields.io/badge/bf16-0077cc?style=flat-square
[badge-fp32]: https://img.shields.io/badge/fp32-0077cc?style=flat-square
[badge-fp16]: https://img.shields.io/badge/fp16-0077cc?style=flat-square
[badge-f16]: https://img.shields.io/badge/F16-0077cc?style=flat-square
[badge-fp8]: https://img.shields.io/badge/fp8-28a745?style=flat-square
[badge-mxfp8]: https://img.shields.io/badge/mxfp8-6f42c1?style=flat-square
[badge-fp4]: https://img.shields.io/badge/fp4-20c997?style=flat-square
[badge-nvfp4]: https://img.shields.io/badge/nvfp4-6f42c1?style=flat-square
[badge-mxfp4]: https://img.shields.io/badge/mxfp4-6f42c1?style=flat-square
[badge-int8]: https://img.shields.io/badge/int8-17a2b8?style=flat-square
[badge-int4]: https://img.shields.io/badge/int4-ffc107?style=flat-square
[badge-w4a8]: https://img.shields.io/badge/w4a8-fe7d37?style=flat-square
[badge-w8a8]: https://img.shields.io/badge/w8a8-fe7d37?style=flat-square
[badge-Q2_K]: https://img.shields.io/badge/Q2__K-e05d44?style=flat-square
[badge-Q3_K_M]: https://img.shields.io/badge/Q3__K__M-fe7d37?style=flat-square
[badge-Q4_0]: https://img.shields.io/badge/Q4__0-dfb317?style=flat-square
[badge-Q4_K_S]: https://img.shields.io/badge/Q4__K__S-dfb317?style=flat-square
[badge-Q4_K_M]: https://img.shields.io/badge/Q4__K__M-dfb317?style=flat-square
[badge-Q5_0]: https://img.shields.io/badge/Q5__0-97c00f?style=flat-square
[badge-Q5_K_M]: https://img.shields.io/badge/Q5__K__M-97c00f?style=flat-square
[badge-Q6_K]: https://img.shields.io/badge/Q6__K-0077cc?style=flat-square
[badge-Q8_0]: https://img.shields.io/badge/Q8__0-28a745?style=flat-square

<!-- TASK BADGES -->
[task-t2i]: https://img.shields.io/badge/text%20%E2%86%92%20image-0077cc?style=flat-square
[task-i2i]: https://img.shields.io/badge/image%20%E2%86%92%20image-6f42c1?style=flat-square
[task-both]: https://img.shields.io/badge/both-17a2b8?style=flat-square

<!-- TYPE BADGES -->
[ltype-finetune]: https://img.shields.io/badge/Finetune-6f42c1?style=flat-square
[ltype-aio]: https://img.shields.io/badge/All--in--one-17a2b8?style=flat-square
[ltype-control]: https://img.shields.io/badge/Control-17a2b8?style=flat-square
[ltype-fix]: https://img.shields.io/badge/Fix-0077cc?style=flat-square
[ltype-collection]: https://img.shields.io/badge/Collection-6f42c1?style=flat-square
[ltype-character]: https://img.shields.io/badge/Character-fe7d37?style=flat-square
[ltype-style]: https://img.shields.io/badge/Style-6f42c1?style=flat-square
[ltype-nsfw]: https://img.shields.io/badge/NSFW-b02a37?style=flat-square
[ltype-skill]: https://img.shields.io/badge/Agent%20Skill-fe7d37?style=flat-square
[ltype-pe]: https://img.shields.io/badge/PE-6f42c1?style=flat-square
[ltype-port]: https://img.shields.io/badge/Port-17a2b8?style=flat-square
[ltype-nodes]: https://img.shields.io/badge/Node%20pack-fe7d37?style=flat-square
[ltype-engine]: https://img.shields.io/badge/Engine-28a745?style=flat-square
[ltype-utility]: https://img.shields.io/badge/Utility-28a745?style=flat-square
[ltype-pipeline]: https://img.shields.io/badge/Pipeline-17a2b8?style=flat-square
[ltype-training]: https://img.shields.io/badge/Training-6f42c1?style=flat-square
[ltype-notebook]: https://img.shields.io/badge/Notebook-0077cc?style=flat-square
[ltype-workflow]: https://img.shields.io/badge/Workflow-17a2b8?style=flat-square
[ltype-quant]: https://img.shields.io/badge/Quant-28a745?style=flat-square
[ltype-distill]: https://img.shields.io/badge/Distilled-6f42c1?style=flat-square
[ltype-layout]: https://img.shields.io/badge/Layout-17a2b8?style=flat-square
[ltype-te]: https://img.shields.io/badge/TE-17a2b8?style=flat-square
[ltype-lora]: https://img.shields.io/badge/LoRA-fe7d37?style=flat-square
