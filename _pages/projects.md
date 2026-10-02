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

What the work is about is on the [about](/) page, and the numbers are on the [cv](/cv/) page. These links go to the pull requests themselves.

- **[verl-omni](https://github.com/verl-project/verl-omni)** · RL post-training for diffusion and omni models · committer · [all PRs →](https://github.com/verl-project/verl-omni/pulls?q=is%3Apr+author%3Acr-gao)
- **[verl](https://github.com/verl-project/verl)** · RL post-training for LLMs · [all PRs →](https://github.com/verl-project/verl/pulls?q=is%3Apr+author%3Acr-gao)
- **[vllm-omni](https://github.com/vllm-project/vllm-omni)** · serving for omni and diffusion models · [all PRs →](https://github.com/vllm-project/vllm-omni/pulls?q=is%3Apr+author%3Acr-gao)
- **[sglang-omni](https://github.com/sgl-project/sglang-omni)** · Qwen3-Omni serving · [all PRs →](https://github.com/sgl-project/sglang-omni/pulls?q=is%3Apr+author%3Acr-gao)
