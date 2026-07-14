---
layout: page
title: projects
permalink: /projects/
# TODO — point the build-log link at /blog/category/build-log/ once the first build-log post is live
description: LLM inference and RL training systems. The first two are being built in public — progress lands in the <a href="/blog/">build-log</a>.
nav: true
nav_order: 3
display_categories: [in-progress, completed]
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

Upstream contributions to LLM training and inference frameworks.

#### [verl-omni](https://github.com/verl-project/verl-omni)

Multimodal RL training framework built on verl.

- [#242](https://github.com/verl-project/verl-omni/pull/242) — _merged_ — `[reward] fix: drop duplicate critic/score metric for single-reward training`
- [#246](https://github.com/verl-project/verl-omni/pull/246) — _merged_ — `[reward] fix: drop duplicate reward/<key>/score metric for multi-reward training`
- [#252](https://github.com/verl-project/verl-omni/pull/252) — _merged_ — `[trainer, doc] feat: profile rollout servers + lightweight profiling recipe for FlowGRPO`

#### [vLLM-Omni](https://github.com/vllm-project/vllm-omni)

<!-- TODO: replace this line with your own one-line description of the contribution. -->

- [#5059](https://github.com/vllm-project/vllm-omni/pull/5059) — _open_ — `[Core] Merge-on-load fast path for diffusion LoRA (shadow wrappers)`
