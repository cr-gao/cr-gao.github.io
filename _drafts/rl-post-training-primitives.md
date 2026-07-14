---
layout: post
title: "RL post-training primitives: PPO, GRPO, GAE, and credit assignment (GiGPO / VinePPO)"
description: The small set of ideas you actually need to read any RL post-training paper, and where each one breaks.
date: 2026-07-28
categories: paper-notes
tags: rl-post-training ppo grpo credit-assignment
published: false # DRAFT — remove this line (and move the file into _posts/) to publish
---

<!--
DRAFT — outline only.
This file lives in _drafts/, so Jekyll will not build it.
Preview drafts locally with:  bundle exec jekyll serve --drafts
To publish: fill it in, delete the `published: false` line, and move it to
_posts/YYYY-MM-DD-rl-post-training-primitives.md
-->

## Outline

- **Why a primitives post.** Every RL post-training paper assumes the same handful of objects. Write them down once, precisely, and the papers get much cheaper to read.
- **PPO, from the objective down.**
  - The clipped surrogate objective, and what the ratio $$r_t(\theta) = \pi_\theta(a_t \mid s_t) / \pi_{\theta_\text{old}}(a_t \mid s_t)$$ is actually protecting against.
  - The critic, and why "value function over token prefixes" is a strange object for an LLM.
  - KL penalty vs. KL in the reward — the two places it shows up and why people confuse them.
- **GAE.** The bias–variance dial ($$\lambda$$), and what it means when your reward is a single scalar at the end of a 4k-token trajectory.
- **GRPO.** Drop the critic; use the *group* as the baseline. Advantage from group-relative normalization.
  - What this buys (no critic to train, no critic memory) and what it costs (needs $$n$$ samples per prompt — which is exactly what makes rollout throughput the bottleneck, and exactly why oversampling shows up).
  - The connection to my [rollout scheduler]({{ '/projects/rl-rollout-scheduler/' | relative_url }}): under GRPO, dropping a *long* trajectory from a group doesn't just lose a sample — it biases the group baseline.
- **The credit-assignment problem.** One reward, thousands of tokens. Which token deserves it?
  - **VinePPO** — Monte Carlo value estimates at intermediate states instead of a learned critic; what it costs in extra rollouts.
  - **GiGPO** — group-in-group: episode-level *and* step-level grouping to get finer-grained advantages for agentic/multi-turn settings.
  - The pattern to notice: both buy credit assignment with **more generation**, which pushes the cost right back into the inference engine.
- **The systems takeaway.** Every improvement in credit assignment here is paid for in rollout throughput. That's why the RL algorithm and the rollout scheduler can't be designed independently.

## Sources to work through

- PPO / GAE (Schulman et al.) — TODO: links
- DeepSeekMath (GRPO) — TODO
- VinePPO — TODO
- GiGPO — TODO
