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

