---
name: spec-story
description: Turn a software spec or design into narrative prose when a story version is requested; preserve its technical requirements.
---

# Spec story

Read the entire source, including referenced material needed to understand its contracts. Identify the problem, existing state, design sequence, key decisions, failure modes, open questions, and completion criteria. Preserve exact technical constraints and distinguish requirements, examples, assumptions, and interpretation. Do not resolve an open design question through storytelling.

Write a coherent narrative for people building or reviewing the design. Lead with the problem, then explain the design and where it may fail. Use concrete examples and a natural voice. Keep headings, lists, tables, or code blocks when they improve comprehension; do not invent drama or details.

Write to the requested path. Otherwise place `STORY.md` beside `SPEC.md`, or append `-story` to the source stem. If the output exists and replacement was not requested, ask before overwriting. Check against the source for omissions and altered requirements.
