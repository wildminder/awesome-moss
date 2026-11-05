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

