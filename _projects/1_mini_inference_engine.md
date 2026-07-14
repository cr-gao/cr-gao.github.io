---
layout: page
title: Mini Inference Engine
description: "<em>In progress — building in public.</em> A from-scratch LLM inference engine in the nano-vLLM tradition: paged KV cache, continuous batching, and a request scheduler."
# img: assets/img/your-thumbnail.jpg  # TODO (optional): project card thumbnail
importance: 1
category: in-progress
permalink: /projects/mini-inference-engine/
# github: https://github.com/... # TODO (optional): repo link
---

<!-- TODO: once the first build-log post is published, point this at /blog/category/build-log/ -->

**Status: in progress — building in public.** Progress notes go into the [build-log]({{ '/blog/' | relative_url }}) category.

An LLM inference engine written from scratch, in the [nano-vLLM](https://github.com/GeeeekExplorer/nano-vllm) tradition — small enough to read end to end, but with the pieces that actually make throughput happen.

## What's in it

- **Paged KV cache.** Fixed-size blocks and a block table per sequence, so KV memory is allocated in pages instead of one contiguous reservation per request. This is what kills the internal fragmentation that a naive `max_seq_len`-shaped cache suffers from, and it's the precondition for prefix sharing.
- **Continuous batching.** Requests join and leave the running batch at step granularity rather than waiting for a whole batch to finish decoding, so a finished short sequence frees its slot immediately instead of idling behind the longest generation in the batch.
- **Request scheduler.** Admission and preemption over a fixed KV budget: what gets to run this step, what gets queued, what gets evicted or recomputed when the cache is full.

## Where it's going

The differentiator I'm building toward — one or both of:

- **Speculative decoding.** A draft model (or self-speculation) proposing tokens that the target model verifies in a single forward pass. The interesting part is the interaction with the scheduler: the batch composition changes when accept lengths vary across sequences.
- **MoE-aware optimization.** Expert-parallel routing where the scheduling decision is no longer uniform across the batch — sequences in a step touch different experts, so the cost model that the scheduler reasons about stops being "tokens in the batch" and becomes something closer to "expert load imbalance."

## Why build it instead of reading about it

vLLM and SGLang are large enough that the mechanism is buried in the engineering. Reimplementing the core loop is the cheapest way to know *why* a decision is there — and it gives me a testbed small enough to change the scheduler and actually measure what happens.
