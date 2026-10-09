---
name: create-plan
description: Write or revise a software implementation plan when a planning document is requested.
---

# Create plan

Before starting, read and follow [good-micro-management](../good-micro-management/SKILL.md) unless its full instructions are already loaded in this conversation. Apply its guidance throughout this workflow, within the user's authorized scope and applicable project restrictions. If unavailable, report that limitation rather than silently skipping it.

Inspect the affected code, applicable project instructions, existing spec or plan, and configured validation commands. Expand discovery only to resolve a concrete dependency or risk. Resolve blocking unknowns with the user; record other assumptions and open decisions. Challenge an approach when repository evidence shows a simpler or safer path.

Write at the requested path, or `PLAN.md` in the project root. Update the relevant portion of an existing plan when requested; otherwise avoid overwriting unrelated content.

Include the goal, scope, observable success criteria, key assumptions or open decisions, and ordered action items. Name affected areas and dependencies when known; do not invent file paths. Make each item a coherent outcome with relevant validation. Include rollout and rollback constraints when the change needs them; distinguish proposed checks from checks actually performed and respect restrictions on test execution. For large work, detail the current phase and summarize later phases.

Do not implement unless requested. If the user also requested execution, continue within that authorization after preparing the plan. Avoid a rigid template when a shorter plan is clearer.

Before delivering, check the plan against good-micro-management for complete scope, coherent steps, affected boundaries, relevant risks, and permitted verification.
