---
layout: post
title: "Mini inference engine, part 0: paged KV cache + continuous batching"
description: Starting the engine — block allocator, block tables, and a decode loop that lets requests join and leave mid-flight.
date: 2026-08-04
categories: build-log
tags: build-log inference kv-cache
published: false # DRAFT — remove this line (and move the file into _posts/) to publish
---

<!--
DRAFT — outline only.
This file lives in _drafts/, so Jekyll will not build it.
Preview drafts locally with:  bundle exec jekyll serve --drafts
To publish: fill it in, delete the `published: false` line, and move it to
_posts/YYYY-MM-DD-mini-inference-engine-part-0.md
-->

## Outline

- **What part 0 is.** The smallest thing that is honestly an inference engine: a paged KV cache and a decode loop that doesn't stall on the longest sequence in the batch. No speculative decoding, no MoE — those come later.
- **Why not just `past_key_values`.** The naive HF loop reserves KV for `max_seq_len` per request, so a batch of 8 with one long generation wastes most of the cache. Measure it: reserved vs. actually-used bytes, on a realistic length distribution. This number is the whole motivation.
- **The block allocator.**
  - Fixed-size blocks (16 tokens?), a free list, and a block table per sequence.
  - Where the indirection actually goes: the attention kernel needs to gather KV through the block table, so this is not purely a bookkeeping change.
  - What I'm using for the kernel to start (PagedAttention from an existing kernel lib vs. writing it) and why.
- **The decode loop / continuous batching.**
  - Step granularity: at every step, retire finished sequences, admit waiting ones, re-form the batch.
  - Prefill vs. decode: chunked prefill or separate phases? What I picked and what it costs.
  - The scheduler surface this exposes — which becomes the interesting part in part 1.
- **Numbers.** Throughput (tok/s) and KV utilization vs. the naive baseline, on the same hardware and the same length distribution. Include the setup so it's reproducible.
- **What broke.** (Keep this section honest — it's the point of a build log.)

## Next up

- Part 1: the request scheduler — admission, preemption, and what to do when the cache is full.
- Prefix sharing, once there's a scheduler worth sharing against. See the [SGLang deep dive]({{ '/blog/2026/sglang-cache-aware-scheduling/' | relative_url }}).
