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

