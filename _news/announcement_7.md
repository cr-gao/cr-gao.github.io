---
layout: post
date: 2026-09-15 09:00:00-0500
inline: true
related_posts: false
---

Hybrid rollout switching is merged in <a href="https://github.com/verl-project/verl-omni">verl-omni</a> (<a href="https://github.com/verl-project/verl-omni/pull/575">#575</a>): the v1 separate-async diffusion trainer now lends its co-located rollout GPUs to generation when the replay buffer runs short, cutting Wan2.2 video step time by 18% at a fixed GPU budget.
