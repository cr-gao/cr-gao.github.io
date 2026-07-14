---
layout: about
title: about
permalink: /
subtitle: LLM infrastructure — inference & training systems.

profile:
  align: right
  image: prof_pic.jpg # TODO: replace assets/img/prof_pic.jpg with a real photo
  image_circular: false # crops the image to make it circular
  more_info: >
    <p>Computer Engineering, UM–SJTU Joint Institute</p>
    <p>MSCS @ UT Austin, Fall 2026</p>

selected_papers: true # includes a list of papers marked as "selected={true}"
social: true # includes social icons at the bottom of the page

announcements:
  enabled: true # includes a list of news items
  scrollable: true # adds a vertical scroll bar if there are more than 3 news items
  limit: 5 # leave blank to include all the news in the `_news` folder

latest_posts:
  enabled: true
  scrollable: true # adds a vertical scroll bar if there are more than 3 new posts items
  limit: 3 # leave blank to include all the blog posts
---

I'm Chenrui, a Computer Engineering senior at the [UM–SJTU Joint Institute](https://www.ji.sjtu.edu.cn/), joining [UT Austin](https://www.cs.utexas.edu/)'s MSCS program in Fall 2026. I work on **LLM infrastructure** — inference engines and RL post-training / rollout systems. Previously I did robotics research at CMU's ARCS Lab on multi-agent path planning.

My focus now is deliberately on the systems side of large models: high-throughput inference, RL training infrastructure, and agentic RL rollout scheduling. Strong C++ and Python.

<!-- NOTE: `build-log` has no published post yet, so /blog/category/build-log/ does not exist and this
     links to the blog index instead. Switch it to {{ '/blog/category/build-log/' }} once you publish
     the first build-log post. Same for paper-notes. -->

Two things I'm currently building in public: a [mini inference engine]({{ '/projects/mini-inference-engine/' | relative_url }}) — paged KV cache, continuous batching, a request scheduler — and an [RL rollout scheduler on verl]({{ '/projects/rl-rollout-scheduler/' | relative_url }}) that reframes oversample-and-cancel as an admission-control problem. Progress goes into the [build log]({{ '/blog/' | relative_url }}); system internals and reading notes land in [deep dives]({{ '/blog/category/deep-dive/' | relative_url }}) and [paper notes]({{ '/blog/' | relative_url }}).
