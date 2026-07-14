---
layout: post
title: "HybridFlow/verl: 3D-HybridEngine weight resharding and the single-controller/SPMD hybrid model"
description: How verl gets training and rollout to share GPUs without paying a full weight reshuffle every step.
date: 2026-07-21
categories: paper-notes
tags: verl rl-post-training distributed
published: false # DRAFT — remove this line (and move the file into _posts/) to publish
---

<!--
DRAFT — outline only.
This file lives in _drafts/, so Jekyll will not build it.
Preview drafts locally with:  bundle exec jekyll serve --drafts
To publish: fill it in, delete the `published: false` line, and move it to
_posts/YYYY-MM-DD-hybridflow-verl-3d-hybridengine.md
-->

## Outline

- **The problem RLHF poses to a training framework.** Rollout, reward, ref-logprob, and actor update all want different parallelism strategies and different memory profiles — and they run in a loop, on the same GPUs. Naive colocation means paying an all-gather / reshard on every phase transition.
- **Single-controller vs. SPMD, and why HybridFlow is both.**
  - Single-controller: one process describes the *dataflow* between roles (actor / critic / rollout / reference). Easy to express PPO/GRPO variants; terrible if it's the granularity you execute at.
  - Multi-controller (SPMD): each role's workers run the same program on their shard. Fast, but the inter-role dataflow becomes painful to express.
  - The hybrid: single-controller *between* roles, SPMD *within* a role. What this buys, and what it costs.
- **3D-HybridEngine.** The core trick: training and generation want different 3D (tensor/pipeline/data) parallel layouts, so switching between them is a weight resharding problem.
  - What the training layout looks like vs. the rollout layout.
  - How the resharding is done without a full round-trip through CPU/host memory — the "zero-redundancy" claim.
  - Where the remaining cost actually lands (memory spike? collective volume? bubble?).
- **`WorkerGroup` / `ResourcePool` / `DataProto` in the codebase.** Map the paper's abstractions onto the actual verl classes, so the paper is readable *as code*.
- **What this means for a rollout scheduler.** If the engine can reshard cheaply, the rollout phase's GPU budget is not fixed — which changes what an admission policy is even allowed to do. Tie back to [the scheduler project]({{ '/projects/rl-rollout-scheduler/' | relative_url }}).

## Sources to work through

- HybridFlow paper (TODO: link)
- verl source: `single_controller/`, `workers/`, `trainer/ppo/`
