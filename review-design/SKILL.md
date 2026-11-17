---
name: review-design
description: Review a software specification and optional implementation plan with fresh, independent subagents for comprehension, design risks, readiness, and unnecessary complexity.
---

# Review design

Coordinate independent reviews of written design artifacts. Keep the authoring conversation in the parent agent; give reviewers the documents and evidence needed for their assigned role without the drafting history. This skill explicitly requests subagent delegation.

## 1. Establish the scope

Use the requested spec, or `SPEC.md` in the project root. Include an implementation plan only when requested; a spec-only review is valid. Locate named artifacts before asking for missing paths. Read applicable project instructions and identify the relevant source areas from the documents and user input. Do not invent requirements or treat parent-only decisions as documented facts.

Use the requested report path, or `REVIEW.md` beside the spec. Do not overwrite an existing report unless updating it was requested; otherwise choose an unused numbered filename such as `REVIEW-2.md`. The parent may write this report. During the review phase, keep the spec, plan, source, dependencies, and Git state unchanged. Do not execute application code, tests, builds, linters, or formatters as part of this review. Inspect source and existing tests as evidence; respect project restrictions.

## 2. Establish fresh contexts

Inspect the available native subagent tool and explicitly disable inherited conversation history. For Codex schemas exposing `fork_turns`, use `fork_turns: "none"`; for schemas exposing `fork_context`, use `fork_context: false`. In Claude Code, launch each reviewer with the `Agent` tool using a non-fork type such as `general-purpose` (the default), which starts with only the prompt you write; never use `subagent_type: "fork"`, which inherits the conversation. In Cursor, launch each reviewer with the Task tool as a general-purpose subagent; Cursor subagents start with a clean context and receive only the prompt, so put everything the reviewer needs in it, and do not resume or reuse a subagent that has seen other roles' material. In other hosts, use the documented equivalent. Do not assume a tool name or a fresh-sounding agent role establishes isolation.

If fresh contexts cannot be established, stop the independent review and report the limitation. Do not simulate the reviewers in the parent conversation or call an ordinary self-review independent. Preserve any completed work and label missing reviews incomplete.

Pass each reviewer its role-specific instructions, absolute input paths, applicable project restrictions, and output requirements. Do not pass the drafting transcript, parent-written explanations of the intended design, expected findings, other reviewers' reports, or private rationale. Tell reviewers not to retrieve conversation history or memory to fill design gaps. Fresh conversation context does not isolate shared files, host instructions, or memory services; disclose any known limitation.

## 3. Dispatch the reviewers

Spawn one fresh subagent per role below. Run them in parallel when capacity permits (in Claude Code, issue all `Agent` calls in one message; in Cursor, all Task calls in one message), otherwise sequentially with a new context for each. Reviewers must remain read-only, return their reports to the parent, and avoid delegating further. Do not edit the reviewed artifacts while they run. In Claude Code and Cursor, a subagent is not tool-restricted by default (Cursor offers `readonly: true` only for custom subagents), so state the read-only rule in its prompt and verify afterward that no reviewed file changed.

| Role | Inputs | Review instructions |
| --- | --- | --- |
| Comprehension | Spec only, plus the review rubric and applicable restrictions | Paraphrase the proposed behavior and the existing system as described. Identify undefined project-specific terms, contradictions, missing prerequisites, unstated assumptions, and ambiguous observable outcomes. Read the entire spec before flagging omissions; do not open linked documents or source to repair its explanations. General engineering terms need no glossary. |
| Design and readiness | Spec, requested plan, and relevant source and tests | Check contracts, ownership, security boundaries, failure handling, concurrency, compatibility, and rollout when relevant. Verify claims against the current code. With a plan, check real paths and symbols, dependency order, coverage of spec requirements, and observable acceptance criteria. Without a plan, assess the design and mark plan readiness unassessed; do not demand implementation detail for every spec. |
| Simplicity | Spec, requested plan, and relevant source | Look for unnecessary abstractions, speculative flexibility, missed reuse, and avoidable changes to existing architecture. Show a concrete maintenance cost or correctness risk and the smallest complete alternative. Preserve useful domain and safety boundaries. Do not report taste-only preferences. |

For the simplicity reviewer, use the installed `slop-detective` skill when available: resolve its actual `SKILL.md` path from the skill inventory or a verified sibling directory and include that absolute path in its handoff. Otherwise use the rubric above and disclose the fallback. This skill has no required sibling-skill dependency; do not install anything during review.

Require every reviewer to return:

- The documents and source paths actually read, plus inaccessible or missing inputs.
- Findings ordered by impact, with a document section or line, the triggering scenario or ambiguity, its consequence, supporting evidence, and a suggested correction when clear.
- Questions and verification limits separately from demonstrated defects. For comprehension, missing explanations are findings; for code-related claims, missing evidence is a limitation.
- An explicit statement when no actionable findings are supported. Never invent findings to meet a quota.

Use project severity conventions, otherwise `P1` for a serious design flaw or blocking ambiguity, `P2` for a concrete risk or material gap, and `P3` for a localized issue with demonstrated impact.

## 4. Consolidate the evidence

Wait for actual completion of every requested reviewer before synthesizing. Report failures, timeouts, unread inputs, and missing reviewer responses as incomplete coverage; launching an agent is not a completed review. Do not silently replace a failed reviewer with a parent self-review.

Review the same document versions across roles. Record target document fingerprints before dispatch and recheck them before delivery. Have reviewers identify the source versions used when available and disclose changes observed while reading. Recheck source evidence supporting findings. If an input changed materially, refresh the affected review within the round limit below or mark its conclusions stale; do not present mixed versions as a completed review.

Deduplicate findings sharing a root cause while retaining evidence and reviewer disagreements. The parent may verify claims against the supplied artifacts and source, but must not dismiss a documented gap using facts known only from the conversation. Classify unsupported claims as questions or limitations. Write the consolidated report in plain language.

Include the reviewed paths and versions, each reviewer's completion status and actual reading scope, the comprehension paraphrase, findings in severity order, unresolved questions or disagreements, and verification limits. Explicitly state that tests were not run and whether plan readiness was assessed. A review with no supported findings is not proof of correctness or implementation approval.

## 5. Return findings and bound follow-up

Link the report and summarize the material findings and incomplete coverage. Review alone does not authorize repairs or implementation. If resolving findings is already requested, continue within that scope: record accepted, rejected, and deferred findings with reasons, and update the relevant documents in the parent authoring context. Do not add approval gates when the user has already authorized the work.

Default to one review round. When revisions and re-review are requested, run at most one follow-up round for affected roles using fresh contexts, for a total of two rounds per invocation. Include simplicity again when revisions introduce substantial design changes. If issues remain, report them rather than looping until clean. Further rounds require a new explicit request. Keep prior findings and their disposition visible when updating a report.
