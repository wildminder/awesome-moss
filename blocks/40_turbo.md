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

