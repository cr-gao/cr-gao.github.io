---
layout: about
title: about
permalink: /
subtitle: LLM infrastructure — inference & RL post-training systems.

profile:
  align: right
  image: prof_pic.png
  image_circular: false # crops the image to make it circular
  more_info: >
    <p>MSCS, UT Austin (2026–)</p>
    <p>BSE CE, Michigan (2026)</p>
    <p>BE ME, SJTU (2026)</p>

selected_papers: true # includes a list of papers marked as "selected={true}"
social: true # includes social icons at the bottom of the page

announcements:
  enabled: true # includes a list of news items
  scrollable: true # adds a vertical scroll bar if there are more than 3 news items
  limit: 5 # leave blank to include all the news in the `_news` folder

latest_posts:
  enabled: false
  scrollable: true # adds a vertical scroll bar if there are more than 3 new posts items
  limit: 3 # leave blank to include all the blog posts
---

I'm Chenrui, an M.S. student in Computer Science at [UT Austin](https://www.cs.utexas.edu/), previously at the [University of Michigan](https://www.eecs.umich.edu/) and Shanghai Jiao Tong University.

I work on **LLM and diffusion systems**: RL post-training frameworks and inference engines, almost all of it upstream. I am a committer of [verl-omni](https://github.com/verl-project/verl-omni) and contribute to [verl](https://github.com/verl-project/verl), [vllm-omni](https://github.com/vllm-project/vllm-omni) and [sglang-omni](https://github.com/sgl-project/sglang-omni).

## RL post-training

- **On-policy distillation for diffusion RL** · verl-omni<br>
  Built end to end: [distillation losses](https://github.com/verl-project/verl-omni/pull/300), a [frozen-teacher runtime](https://github.com/verl-project/verl-omni/pull/325) and [multi-teacher routing](https://github.com/verl-project/verl-omni/pull/427). The student matches its RL teacher about **5×** faster than training with GRPO.
- **Async trainer throughput** · verl-omni<br>
  [Async teacher scheduling](https://github.com/verl-project/verl-omni/pull/495) cut the teacher wait from 3.6 s to 0.02 s, and [hybrid rollout switching](https://github.com/verl-project/verl-omni/pull/575) raised Wan2.2 video throughput by **22%** on the same GPUs.
- **Training–sampling mismatch** · verl, verl-omni<br>
  [Score centering](https://github.com/verl-project/verl/pull/8010) kept training stable with an fp8 sampler where plain policy gradient collapsed, and a [mapping fix](https://github.com/verl-project/verl-omni/pull/279) cut BAGEL's log-prob drift **157×**. _In progress:_ [bitwise on-policy RL](https://github.com/verl-project/verl-omni/issues/637) for Qwen3-Omni, at zero mismatched tokens on the full model in a two-step closed-loop run.

## Inference engines

- **Video diffusion acceleration** · vllm-omni<br>
  [Sequence parallelism for linear attention](https://github.com/vllm-project/vllm-omni/pull/5940) in SANA-Video, **1.80×** on two GPUs and **3.57×** with [CFG parallelism](https://github.com/vllm-project/vllm-omni/pull/5861) on four, and [regional `torch.compile`](https://github.com/vllm-project/vllm-omni/pull/7174) for MAGI-2. _In progress:_ [SANA-Video 2.0](https://github.com/vllm-project/vllm-omni/pull/8028).
- **Engine core** · vllm-omni<br>
  A [keep-mode pause](https://github.com/vllm-project/vllm-omni/pull/7685) that quiets a busy diffusion replica without discarding finished work, plus fixes for [silent sleep and wake-up failures](https://github.com/vllm-project/vllm-omni/pull/7811), an [orchestrator hang](https://github.com/vllm-project/vllm-omni/pull/7812) and a [batch-mixing attention bug](https://github.com/vllm-project/vllm-omni/pull/5866). _In progress:_ [prefix caching under preemption](https://github.com/vllm-project/vllm-omni/pull/8034).
- **Multimodal prefill** · sglang-omni<br>
  A [batched embedding merge](https://github.com/sgl-project/sglang-omni/pull/1161) for Qwen3-Omni: host syncs fell from 96 to 0 per batch and the merge ran **2.1–4.1×** faster.

Before this I did robotics research at CMU's [ARCS Lab](https://arcs-lab.github.io/) (VAMP-MR, IROS 2026) and at UC Irvine. Details and numbers are on the [cv](/cv/) page.
