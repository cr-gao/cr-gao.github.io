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

I'm Chenrui, an M.S. student in Computer Science at [UT Austin](https://www.cs.utexas.edu/). I did my B.S.E. in Computer Engineering at the [University of Michigan](https://www.eecs.umich.edu/) as part of a dual-degree program with Shanghai Jiao Tong University.

I work on **LLM and diffusion systems**, in two areas: RL post-training frameworks and inference engines. Nearly all of it is upstream open-source work. I am a committer of [verl-omni](https://github.com/verl-project/verl-omni) and contribute to [verl](https://github.com/verl-project/verl), [vllm-omni](https://github.com/vllm-project/vllm-omni) and [sglang-omni](https://github.com/sgl-project/sglang-omni).

## RL post-training systems

**On-policy distillation for diffusion RL** · verl-omni. I brought on-policy distillation to diffusion RL, starting from [teacher-anchored KL and flow-matching losses](https://github.com/verl-project/verl-omni/pull/300) and a [frozen-teacher runtime](https://github.com/verl-project/verl-omni/pull/325) that runs as a third forward-only worker beside the actor and the reference model. On SD3.5-Medium with an OCR reward, the student's reward rose from 0.23 to 0.62–0.71, matching the FlowGRPO teacher about 5× faster than training with GRPO. I then extended it to [multiple teachers](https://github.com/verl-project/verl-omni/pull/427), with each sample routed to a teacher that is either co-located with the actor or in its own resource pool, ported it to the [v1 trainer](https://github.com/verl-project/verl-omni/pull/493), and added an [asynchronous teacher scheduler](https://github.com/verl-project/verl-omni/pull/495) that scores batch _k_ while the actor updates on batch *k*−1, cutting the teacher-phase wait from 3.6 s to 0.02 s. I became a committer after this line landed.

**Hybrid rollout switching** · verl-omni. When generation falls behind training, the trainer now [lends the actor's co-located GPUs to rollout](https://github.com/verl-project/verl-omni/pull/575) at the end of a step and reclaims them once enough samples are ready. On a Wan2.2 video recipe with a fixed three-GPU budget, median step time fell from 378 s to 308 s and throughput rose by 22%, with reward unchanged. Making a busy diffusion replica quiet for this meant aborting work it had already finished, which led to the keep-mode pause described under inference engines.

**Score centering** · verl. I implemented [score centering](https://github.com/verl-project/verl/pull/8010), an additive correction for the mismatch between the model that samples and the model that trains, in verl's policy-gradient loss. In a reproduction of the paper's experiment with an fp8 sampler, plain policy gradient collapsed at step 72 and token-level importance sampling degraded after its peak, while score centering stayed stable through all 400 steps.

**Rollout–training consistency** · verl-omni, verl, vllm-omni. I [measured](https://github.com/verl-project/verl-omni/issues/280) how far rollout and training log-probs drift apart across three diffusion pipelines, shipped the resulting [per-timestep monitoring](https://github.com/verl-project/verl-omni/pull/291) as a default, and [fixed](https://github.com/verl-project/verl-omni/pull/279) a stale step-count mapping that cut BAGEL's log-prob drift 157×, down to bf16 noise. _In progress:_ [bitwise on-policy RL](https://github.com/verl-project/verl-omni/issues/637) for the Qwen3-Omni thinker, whose trainer and sampler are separate implementations. The fix is split between a [batch-invariant mode](https://github.com/verl-project/verl/pull/8026) in verl's training engine and an [HF-numerics mode](https://github.com/vllm-project/vllm-omni/pull/8186) in vllm-omni. On the full 48-layer model it reaches zero mismatched tokens over two closed-loop RL steps with expert weights frozen, at a 41% cost in rollout throughput.

## Inference engines

**Video diffusion acceleration** · vllm-omni. For SANA-Video I derived a [sequence-parallel scheme for ReLU linear attention](https://github.com/vllm-project/vllm-omni/pull/5940), where the usual Ulysses and Ring schemes do not apply: the attention state sums over the sequence, so one all-reduce of a small matrix is enough at any sequence length. It runs 1.80× faster on two GPUs and 3.57× combined with CFG parallelism on four. I also added [tensor and CFG parallelism](https://github.com/vllm-project/vllm-omni/pull/5861) (3.02× combined) and [Cache-DiT with CPU offload](https://github.com/vllm-project/vllm-omni/pull/5882) (1.56×). For MAGI-2 Preview I applied [regional `torch.compile`](https://github.com/vllm-project/vllm-omni/pull/7174) around the kernels that have to stay eager, which cut the denoising step from 1.93 s to 1.48 s. _In progress:_ [sequence parallelism for SANA-Video 2.0](https://github.com/vllm-project/vllm-omni/pull/8028), at 1.90× end to end on two GPUs with the single-GPU path bit-exact.

**Engine core** · vllm-omni. I added a [keep-mode pause](https://github.com/vllm-project/vllm-omni/pull/7685) to diffusion stages, so a busy replica can be made quiet for sleep or a weight sync without discarding finished work. I also fixed [sleep and wake-up calls that reported success after the worker had failed](https://github.com/vllm-project/vllm-omni/pull/7811), an [orchestrator hang after a diffusion subprocess died](https://github.com/vllm-project/vllm-omni/pull/7812), and a [FlashAttention padding bug](https://github.com/vllm-project/vllm-omni/pull/5866) that silently mixed samples within a batch. _In progress:_ [prefix-cache delivery across preemption](https://github.com/vllm-project/vllm-omni/pull/8034), which under forced preemption cuts the speech word-error-rate increase from +0.104 to +0.014.

**Multimodal prefill** · sglang-omni. I rewrote the [multimodal embedding merge](https://github.com/sgl-project/sglang-omni/pull/1161) on Qwen3-Omni's prefill path from a per-request Python loop into one batched GPU scatter. Device-to-host syncs fell from 96 to 0 per batch and the merge ran 2.1–4.1× faster.

## Before this

I did robotics research at CMU's [ARCS Lab](https://arcs-lab.github.io/) with Prof. Jiaoyang Li, where I built the SIMD-vectorized core of VAMP-MR (IROS 2026), and at UC Irvine with Prof. Sven Koenig on topological multi-agent pathfinding. The full record, with every number, is on the [cv](/cv/) page.
