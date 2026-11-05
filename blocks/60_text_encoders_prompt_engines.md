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

