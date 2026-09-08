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

I'm Chenrui, an incoming M.S. student in Computer Science at [UT Austin](https://www.cs.utexas.edu/). I did my B.S.E. in Computer Engineering at the [University of Michigan](https://www.eecs.umich.edu/) as part of a dual-degree program with Shanghai Jiao Tong University.

I work on **LLM training and inference systems**, mostly as an upstream contributor to open-source RL post-training and serving frameworks:

- **[verl-omni](https://github.com/verl-project/verl-omni)** (committer) — RL post-training for diffusion and omni models: brought on-policy distillation to diffusion RL, from the teacher-anchored losses through multi-teacher routing, standalone teacher pools, and async teacher scheduling on the v1 trainer; also worked on rollout–training numerical consistency, profiling, and adapter correctness.
- **[sglang-omni](https://github.com/sgl-project/sglang-omni)** — inference performance for Qwen3-Omni: batching the multimodal prefill path and removing host syncs on the way to CUDA-graph coverage.
- **[vllm-omni](https://github.com/vllm-project/vllm-omni)** — parallelism and acceleration for video diffusion models: sequence, tensor, and CFG parallelism plus caching and offload for SANA-Video, regional `torch.compile` for MAGI-2 Preview, and attention-backend and offload bug fixes.

Before that I did robotics research at CMU's [ARCS Lab](https://arcs-lab.github.io/) with Prof. Jiaoyang Li, where I built the SIMD-vectorized core of [VAMP-MR](/publications/) (IROS 2026), and at UC Irvine with Prof. Sven Koenig on topological multi-agent pathfinding. Full details are on the [cv](/cv/) page.
