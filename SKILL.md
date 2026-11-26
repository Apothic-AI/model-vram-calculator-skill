---
name: vramcalc
description: Estimate GPU memory for LLM inference with the vramcalc CLI. Use when asked how much VRAM or GPU memory a model needs, whether a model fits on a given GPU, how many GPUs it needs, or what FP8 or INT4 quantization saves, for a Hugging Face model or a parameter count.
---

# vramcalc

`vramcalc` estimates the memory a model needs for inference. It gives figures at the model's published precision, at FP8, and at INT4. It also lists which GPUs hold the model on one card, or how many they need. It ports the aquanode GPU VRAM Calculator Space and covers its 1,333 models and 29 GPUs.

## Setup

1. Run `command -v vramcalc`.
2. If it is missing, install it with `go install github.com/Apothic-AI/vramcalc/cmd/vramcalc@latest`. That needs Go 1.22 or newer, with `$(go env GOPATH)/bin` on `PATH`. Or clone https://github.com/Apothic-AI/vramcalc and run `go build -o bin/vramcalc ./cmd/vramcalc`, then call that binary in place of `vramcalc`.

## Workflow

1. Get the model ID. Run `vramcalc models --json <query>` and take the exact `id` from the result. If nothing matches, use `--params` with the size in billions. Never guess an ID.
2. Estimate with `vramcalc --json <model-id>`. For a size that is not in the catalog, use `vramcalc --json --params <billions> --dtype <BF16|FP16|FP32|FP8>`.
3. For the GPU list at another precision, add `--precision native|fp8|int4`. The `precisions` array lists every row for the model: two for FP8-published models, three otherwise.
4. Report the numbers and the caveats below. Exit code 1 means the lookup failed. Exit code 2 means the arguments were wrong.

Always pass `--json` when you read the output.

## Reading the output

- `parameters`, `parameters_label`, and `published_precision` give the model size and its published format.
- `precisions[]` has one entry per row, with `key` (`native`, `fp8`, or `int4`), `label`, `weights_gib`, and `needed_gib`.
- `selected_precision` and `selected_label` name the row that drives `gpus`.
- `gpus[]` is sorted smallest card first. Each entry has `id`, `name`, `memory_gib`, `gpus_needed`, and `fits_on_one`.
- `model` is `null` when you used `--params`.

`vramcalc models --json` returns an array of `id`, `parameters`, `parameters_label`, and `published_precision`. `vramcalc gpus --json` returns an array of `id`, `name`, and `memory_gib`.

## Caveats to state

- `needed_gib` is weights × 1.2. That covers single-request inference at about 8K context. Longer contexts and larger batches need more memory for the KV cache.
- Figures are GiB. GPU sizes are the Space's values.
- Multi-GPU counts assume an even split across cards.
- Parameter counts are catalog totals. For mixture-of-experts models, that includes every expert.
- This is an estimate. Confirm with a real run before you buy hardware.
