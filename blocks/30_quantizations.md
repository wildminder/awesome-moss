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

