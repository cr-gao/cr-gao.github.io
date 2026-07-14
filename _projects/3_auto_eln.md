---
layout: page
title: auto-eln
description: <em>Completed — undergraduate capstone.</em> A Worker–Manager system for automated instrument data capture, with a template-driven schema.
# img: assets/img/your-thumbnail.jpg  # TODO (optional): project card thumbnail
importance: 3
category: completed
permalink: /projects/auto-eln/
# github: https://github.com/... # TODO (optional): repo link
---

**Status: completed** — my undergraduate capstone project.

An automated electronic-lab-notebook system that captures data off lab instruments without a human copying numbers around, built as a **Worker–Manager** architecture.

- **Manager** — Flask + SQLAlchemy service that owns the schema, the job assignments, and the captured records.
- **Worker** — a PySide6 desktop agent that runs next to the instrument, watches for new output, and ships it to the manager.
- **Storage** — PostgreSQL, with a **template-driven schema**: instrument/experiment types are described as templates rather than hardcoded tables, so a new instrument doesn't require a migration.

The interesting design pressure was that lab instruments are heterogeneous and mostly un-scriptable — the schema has to bend to the instrument, not the other way around.
