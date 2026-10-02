---
layout: post
date: 2026-09-21 09:00:00-0500
inline: true
related_posts: false
---

A scheduler-level keep-mode pause for diffusion stages is merged in <a href="https://github.com/vllm-project/vllm-omni">vllm-omni</a> (<a href="https://github.com/vllm-project/vllm-omni/pull/7685">#7685</a>): a busy replica can now be made quiet for sleep or a weight sync without discarding finished work.
