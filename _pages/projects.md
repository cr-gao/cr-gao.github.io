---
layout: page
title: projects
permalink: /projects/
description: Coursework systems projects, plus my upstream open-source work on LLM training and inference frameworks.
nav: true
nav_order: 3
horizontal: false
---

<!-- pages/projects.md -->
<div class="projects">
{% if site.enable_project_categories and page.display_categories %}
  <!-- Display categorized projects -->
  {% for category in page.display_categories %}
  <a id="{{ category }}" href=".#{{ category }}">
    <h2 class="category">{{ category }}</h2>
  </a>
  {% assign categorized_projects = site.projects | where: "category", category %}
  {% assign sorted_projects = categorized_projects | sort: "importance" %}
  <!-- Generate cards for each project -->
  {% if page.horizontal %}
  <div class="container">
    <div class="row row-cols-1 row-cols-md-2">
    {% for project in sorted_projects %}
      {% include projects_horizontal.liquid %}
    {% endfor %}
    </div>
  </div>
  {% else %}
  <div class="row row-cols-1 row-cols-md-3">
    {% for project in sorted_projects %}
      {% include projects.liquid %}
    {% endfor %}
  </div>
  {% endif %}
  {% endfor %}

{% else %}

<!-- Display projects without categories -->

{% assign sorted_projects = site.projects | sort: "importance" %}

  <!-- Generate cards for each project -->

{% if page.horizontal %}

  <div class="container">
    <div class="row row-cols-1 row-cols-md-2">
    {% for project in sorted_projects %}
      {% include projects_horizontal.liquid %}
    {% endfor %}
    </div>
  </div>
  {% else %}
  <div class="row row-cols-1 row-cols-md-3">
    {% for project in sorted_projects %}
      {% include projects.liquid %}
    {% endfor %}
  </div>
  {% endif %}
{% endif %}
</div>

---

<h2 id="open-source"><a href=".#open-source">open source</a></h2>

Upstream contributions to LLM training and inference frameworks. Details and numbers are on the [cv](/cv/) page.

#### [verl-omni](https://github.com/verl-project/verl-omni) · diffusion/omni RL post-training · committer · 15 PRs merged

- [#495](https://github.com/verl-project/verl-omni/pull/495) · _merged_ · `[trainer, tests, doc] feat: async teacher scheduling for diffusion OPD on v1 separate_async`
- [#498](https://github.com/verl-project/verl-omni/issues/498) · _RFC_ · one-step-off teacher scheduling for diffusion OPD on the v1 async trainer
- [#513](https://github.com/verl-project/verl-omni/pull/513) · _merged_ · `[trainer, tests] fix: union colocated reward output into the v1 diffusion trainer batch`
- [#493](https://github.com/verl-project/verl-omni/pull/493) · _merged_ · `[trainer, recipe, tests, doc] feat: multi-teacher OPD on the v1 sync diffusion trainer`
- [#482](https://github.com/verl-project/verl-omni/pull/482) · _merged_ · `[trainer, cfg, tests] fix: give each ray worker group its own rendezvous port slice`
- [#427](https://github.com/verl-project/verl-omni/pull/427) · _merged_ · `[worker, trainer, cfg, tests, doc] feat: multi-teacher routing and standalone teacher pool for diffusion OPD`
- [#325](https://github.com/verl-project/verl-omni/pull/325) · _merged_ · `[worker, trainer, cfg, tests, doc] feat: teacher runtime MVP for diffusion OPD (#293)`
- [#300](https://github.com/verl-project/verl-omni/pull/300) · _merged_ · `[trainer, algo, cfg] feat: add teacher-anchored continuous distillation losses for diffusion OPD`
- [#292](https://github.com/verl-project/verl-omni/pull/292) · _merged_ · `[doc] fix: drop incorrect threshold caveat from rollout correction doc`
- [#291](https://github.com/verl-project/verl-omni/pull/291) · _merged_ · `[trainer, doc, tests] feat: log rollout-train log-prob consistency metrics by default`
- [#280](https://github.com/verl-project/verl-omni/issues/280) · _RFC_ · rollout–training numerical consistency across SD3.5-Medium, Qwen-Image-20B, and BAGEL-7B
- [#281](https://github.com/verl-project/verl-omni/pull/281) · _merged_ · `[doc, cfg] chore: document reward-server profiling in reward docs`
- [#279](https://github.com/verl-project/verl-omni/pull/279) · _merged_ · `[omni] fix: drop stale vllm-omni 0.22 step-count mapping in BAGEL adapter`
- [#256](https://github.com/verl-project/verl-omni/pull/256) · _merged_ · `[trainer, reward, cfg, doc] feat: profile reward-model rollout servers`
- [#252](https://github.com/verl-project/verl-omni/pull/252) · _merged_ · `[trainer, doc] feat: profile rollout servers + lightweight profiling recipe for FlowGRPO`
- [#246](https://github.com/verl-project/verl-omni/pull/246) · _merged_ · `[reward] fix: drop duplicate reward/<key>/score metric for multi-reward training`
- [#242](https://github.com/verl-project/verl-omni/pull/242) · _merged_ · `[reward] fix: drop duplicate critic/score metric for single-reward training`

#### [sglang-omni](https://github.com/sgl-project/sglang-omni) · Qwen3-Omni inference performance

- [#1161](https://github.com/sgl-project/sglang-omni/pull/1161) · _merged_ · `[Qwen3-Omni Perf] Thinker prefill: batch multimodal embedding merge, remove per-request host syncs`

#### [vllm-omni](https://github.com/vllm-project/vllm-omni) · video diffusion parallelism & acceleration: SANA-Video and MAGI-2 Preview

- [#7174](https://github.com/vllm-project/vllm-omni/pull/7174) · _in review_ · `[Diffusion] MAGI-2: regional compile coverage`
- [#7187](https://github.com/vllm-project/vllm-omni/pull/7187) · _in review_ · `[Bugfix][Diffusion] Keep nested layerwise blocks on the host`
- [#5940](https://github.com/vllm-project/vllm-omni/pull/5940) · _merged_ · `[Model] Add sequence parallelism to SANA-Video 2B`
- [#5861](https://github.com/vllm-project/vllm-omni/pull/5861) · _merged_ · `[Model] Add TP and CFG parallelism to SANA-Video 2B`
- [#5882](https://github.com/vllm-project/vllm-omni/pull/5882) · _merged_ · `[Model] Add Cache-DiT and CPU offload support for SANA-Video 2B`
- [#5866](https://github.com/vllm-project/vllm-omni/pull/5866) · _merged_ · `[Bugfix][Diffusion] Fix FLASH_ATTN cross-attention key-padding unpad`
- [#5269](https://github.com/vllm-project/vllm-omni/pull/5269) · _merged_ · `[Bugfix] Skip non-positive scheduled token spans in omni stage runners`
- [#5059](https://github.com/vllm-project/vllm-omni/pull/5059) · _in review_ · `[Core] Merge-on-load fast path for diffusion LoRA (shadow wrappers)`
