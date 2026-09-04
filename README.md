# ComfyUI-Anima-Delta-Q35

Runtime for **Anima Delta Mix 3.8B Qwen 3.5**.

Install this folder into `ComfyUI/custom_nodes/`.
Keep `comfyui-anima-3-8B` installed. This pack imports it at runtime and does not copy that source.

Menu: **Anima Delta Mix**

## Nodes

| Canvas | Class id |
|---|---|
| DeltaMix Q35 Extract Connector | `AnimaQ35Extract` |
| DeltaMix Q35 Attach | `AnimaQ35Attach` |
| DeltaMix Q35 Prompt | `AnimaQ35Prompt` |
| Prompt Text Delta | `PromptTextDelta` |
| Prompt Dual Delta | `PromptDualDelta` |

## Files

- checkpoint: `DeltaMix3.8BQ4B.safetensors` (or the name on the card)
- sidecar: `anima_connector_only.safetensors` → `models/diffusion_models/`
- `qwen_3_06b_base.safetensors`
- `qwen35_4b.safetensors`
- `qwen_image_vae.safetensors`

Build the sidecar once with **Extract Connector** from official Anima 3.8B v1.1, or use the extra file from the model page.

## Graph

UNETLoader
    → DeltaMix Q35 Attach (sidecar)
        → DeltaMix Q35 Prompt  (pos)   ← CLIP 0.6B + Qwen 3.5 4B
        → DeltaMix Q35 Prompt  (neg)
            → CFGGuider → SamplerCustomAdvanced → VAE → Save Image

Prompt Text Delta → Prompt Dual Delta → the two Prompt nodes

Leave the two Q35 Prompt nodes folded. Edit only the text nodes.

First pass in the published workflow: `res_multistep` + `beta`, 40 steps, CFG 6.
The Detail group is off. Turn it on for a second pass (`dpmpp_2m`, 15 steps, denoise 0.25, CFG 4.5). Same MODEL and same conditioning. Do not add a second Attach.

Load a gallery PNG (Comfy metadata) or the workflow json on the card.

## Credits

- CircleStone Labs — Anima
- lylogummy — Anima 3.8B v1.1 chassis and Semantic Connector v2
- GumGum10 — comfyui-anima-3-8B (MIT, runtime dependency)
- Delta Mix — extract / attach / prompt glue
