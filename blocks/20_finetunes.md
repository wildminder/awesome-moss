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

