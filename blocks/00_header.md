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

