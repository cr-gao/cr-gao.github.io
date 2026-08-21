---
layout: page
title: LLM Training Systems from Scratch
description: Stanford CS336 assignments — FSDP, a FlashAttention Triton kernel, and a KV cache, all inside a Transformer LM built from the ground up.
importance: 1
category: completed
permalink: /projects/cs336-llm-systems/
---

Working through the [Stanford CS336](https://stanford-cs336.github.io/) assignments: a Transformer language model built from the ground up, then the systems pieces that make training it fast. PyTorch, Triton, NCCL.

## What I wrote

- **A Transformer LM from scratch.** RoPE, a BPE tokenizer, and the training loop, with no reliance on `nn.Transformer` or a pretrained tokenizer.
- **FSDP from scratch.** Parameters are sharded across ranks; the forward all-gathers each layer's weights on demand, and backward hooks reduce-scatter gradients back into shards. fp32 master shards under a round-robin sharded optimizer keep the per-rank memory footprint at roughly 1/N of the full model state.
- **A FlashAttention Triton kernel.** Tiled, online-softmax attention that never materializes the full score matrix, with the backward pass recomputing blocks instead of storing them.
- **A KV cache.** Per-layer key/value buffers appended to at decode time so each generated token costs one attention step over the cache rather than a full recompute.

## Why

vLLM, verl, and FSDP are large enough that the mechanism is buried in the engineering. Reimplementing each core loop is the cheapest way to know _why_ a design decision is there, and it is the background I lean on when contributing to those frameworks upstream.
