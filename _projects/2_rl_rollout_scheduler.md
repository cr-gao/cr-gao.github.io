---
layout: page
title: RL Rollout Scheduler on verl
description: <em>In progress — building in public.</em> Reframes oversample-and-cancel in RL rollout as a priority-based admission problem, and studies the length-correlated bias naive cancellation introduces.
# img: assets/img/your-thumbnail.jpg  # TODO (optional): project card thumbnail
importance: 2
category: in-progress
permalink: /projects/rl-rollout-scheduler/
# github: https://github.com/... # TODO (optional): repo link
---

<!-- TODO: once the first build-log post is published, point this at /blog/category/build-log/ -->

**Status: in progress — building in public.** Progress notes go into the [build-log]({{ '/blog/' | relative_url }}) category.

An RL rollout scheduler built on [verl](https://github.com/volcengine/verl).

## The problem

RL post-training generates far more rollouts than it ends up training on. The standard trick is **oversample-and-cancel**: launch more generations than you need, take the first $$N$$ that finish, kill the rest. It's a straggler mitigation, and it works — the batch stops waiting on its slowest trajectory.

But "the first $$N$$ that finish" is not a neutral sample. It is **length-biased**: short trajectories finish first, so cancellation systematically drops long ones. That's costly in general, and it's *especially* costly for reasoning RL, where long trajectories are exactly the ones carrying high GRPO learning value. You are throwing away the samples with the most signal because they were slow.

## The reframing

Treat oversampling not as *launch-then-kill*, but as an **admission** problem. Oversampled requests become **low-priority backfill** that competes for GPU throughout generation, rather than being cancelled at the finish line:

- Priority classes decide who gets to run at each step, not who survives at the end.
- Backfill requests soak up capacity the primary requests aren't using — and yield it back when the primaries need it.
- The cancellation decision is spread across the whole generation, where it can be made with information (progress, projected length) rather than at the deadline where it can only be made by "who's done."

## What I'm measuring

The goal is a **throughput–quality frontier**, not a single point: quantify the bias that naive cancellation introduces, then show bias-bounded alternatives that buy back quality at known throughput cost.

- **Partial rollout with pause/resume** — a preempted trajectory's KV state is checkpointed and resumed later instead of discarded, so the sample survives even if it loses its slot.
- **Importance-weighted truncation** — keep the truncated sample but correct for the selection probability, so the gradient estimate stays (approximately) unbiased.
- **Randomized cancellation** — decouple the cancel decision from completion order, trading some straggler mitigation for a much smaller length correlation.
