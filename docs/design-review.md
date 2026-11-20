# Independent design review with the team's skills

Use this workflow when a feature needs a written design or implementation plan. The authoring agent retains the discussion and decisions. Fresh reviewers inspect the written artifacts without that conversation, so assumptions must be stated in the documents to survive review.

The skills in this repository were developed independently and were already used across separate chats. [`review-design`](../review-design/SKILL.md) automates the review handoffs by launching fresh subagents and consolidating their findings. Google's [Elephant-Goldfish Model](https://github.com/google/elephant-goldfish-model) prompted the switch from manually opening separate review chats to subagent delegation.

## Install and share

Prerequisites: Node.js 22.20 or later, Git, and an agent host that can launch subagents without inheriting the parent conversation. The Node.js requirement comes from the [Skills CLI](https://github.com/vercel-labs/skills#install-a-skill). The skill checks delegation capability at runtime and stops the independent review if it is unavailable. Codex supports delegation requested by skills; see [OpenAI's subagent documentation](https://learn.chatgpt.com/docs/agent-configuration/subagents).

After the new skill has been committed and pushed to this repository, teammates can install it globally for Codex:

```bash
npx skills add shekhar-kamble/skill-issue --skill review-design -g -a codex
```

For Claude Code, use the same command with `-a claude-code`, `-a cursor` for Cursor, or pass several `-a` flags to install for more than one agent. Installation discovers the skill; it does not establish that the current session supports isolated subagents. Check the host's available delegation tools when running a review.

To install the complete workflow, use the [repository installation instructions](../README.md#install). `review-design` works by itself. Installing `slop-detective` lets the simplicity reviewer reuse its engineering guidance and persona; otherwise a built-in simplicity rubric applies. `create-plan` and `execute-plan` retain their existing dependency on `good-micro-management`.

For local authoring before publication, follow [Use the checkout for local authoring](../README.md#use-the-checkout-for-local-authoring), or install from the checkout:

```bash
npx skills add . --skill review-design -g -a codex
```

Run the local command from this repository. These commands are installation instructions, not commands executed by the review skill. Teammates installing from GitHub receive the published version. Pushing changes does not update their machines automatically; use the [update instructions](../README.md#update). Rerun the add command to install a newly introduced skill.

## Run the workflow

Run the following prompts in the software project's authoring chat. `SPEC.md`, `PLAN.md`, and `REVIEW.md` are artifacts in that project, not files to create in the skill collection. Named paths can replace the defaults.

| Step | Action | Expected result |
| --- | --- | --- |
| 1. Establish requirements | Use `interview` when material stakeholder decisions are unresolved. | Confirmed requirements and explicit open questions. Skip the interview when requirements are already sufficient. |
| 2. Write the spec | Use `create-spec`. | `SPEC.md` describing behavior, design decisions, failure cases, and success criteria. |
| 3. Review the design | Use `review-design` with the spec. | A consolidated report from three independent reviewers. Plan readiness is unassessed. |
| 4. Resolve findings | Ask the authoring agent to evaluate findings and revise the spec. | Accepted, rejected, and deferred findings with reasons; decisions recorded in the spec. |
| 5. Plan implementation | Use `create-plan` after resolving material design decisions. | `PLAN.md` with affected areas, ordered outcomes, and permitted verification. |
| 6. Check readiness | Use `review-design` with both documents. | Independent checks that the plan matches the spec and current code. |
| 7. Implement | Request `execute-plan` when ready to authorize implementation. | Focused code changes and accurate progress recorded in the plan. |
| 8. Review staged code | Stage the desired changes yourself, then request `staged-code-review`. | A separate review of the Git index; design review does not replace this check. |

Example prompts:

```text
Use $create-spec to turn our agreed requirements into SPEC.md.
Record consequential assumptions and unresolved decisions explicitly.
```

```text
Use $review-design on SPEC.md. Write the report to REVIEW.md.
Review only; leave the design and source unchanged.
```

```text
Evaluate the findings in REVIEW.md against the requirements and code.
Fix supported findings in SPEC.md. Record accepted, rejected, and
deferred findings with reasons in REVIEW.md. Leave source unchanged.
```

```text
Use $create-plan to write PLAN.md from SPEC.md and the current code.
```

```text
Use $review-design on SPEC.md with PLAN.md for implementation readiness.
Update REVIEW.md. Review only; leave the spec, plan, and source unchanged.
```

```text
Use $execute-plan to implement PLAN.md. Follow project restrictions
on tests, dependencies, infrastructure, and Git operations.
```

Codex invokes skills with `$skill-name`, and Claude Code and Cursor with `/skill-name`; in other hosts, select the named skill from the picker. Separate spec and readiness reviews are useful for substantial features; when both documents already exist, one review of both is sufficient. No interview or formal plan is required for unrelated small changes.

## Choose models and reasoning per reviewer

Optionally specify a model, reasoning effort, or both for any reviewer in the invocation. The skill has no fixed model choices. Omitted fields use the host's normal defaults, inheriting the parent settings when no host override applies. A model-only override may use the selected model's default reasoning effort. Codex also supports configured subagent defaults and custom agents; see [OpenAI's model and reasoning controls](https://learn.chatgpt.com/docs/agent-configuration/subagents#choosing-models-and-reasoning).

For example, these are illustrative selections, not team defaults:

```text
Use $review-design on SPEC.md with PLAN.md. Write the report to REVIEW.md.

Use these reviewer settings:
- Comprehension: model gpt-6-luna, reasoning high
- Design and readiness: model gpt-6.1-sol, reasoning high
- Simplicity: model gpt-6.1-sol, reasoning medium

Keep all reviewers in fresh contexts. Report unavailable settings
instead of substituting. Record requested and effective settings
for each reviewer, and label effective settings unverified if hidden.
```

You can override just one role or one field:

```text
Use $review-design on SPEC.md.
Use high reasoning for Design and readiness.
Leave all other settings at their host defaults.
```

Available models, effort levels, and override controls depend on the host and account. The parent checks the native tool schema and available host metadata and passes selections through real dispatch controls. Putting a model name in a reviewer's prompt does not select that model. For Codex tools exposing `model` and `reasoning_effort`, those are the dispatch fields; `model_reasoning_effort` is the setting used in Codex configuration files. Other hosts may expose different controls or no per-reviewer reasoning override.

If a selection cannot be honored, the affected reviewer remains incomplete and the report explains why. Other independent reviewers can finish. The skill does not silently switch models, lower reasoning, or change host configuration. Requested follow-up reviews reuse the same selections unless you change them.

The report includes a table for each reviewer's requested model and effort, confirmed effective model and effort, and any default, unavailable, or unverified status. Effective values are confirmed only from native tool results or host metadata. Hidden settings remain unverified; a requested value or the reviewer's own claim is not proof of which settings ran. An observed mismatch makes that role incomplete.

## How the skill is implemented

The implementation consists of [`review-design/SKILL.md`](../review-design/SKILL.md) and [`agents/openai.yaml`](../review-design/agents/openai.yaml). The Markdown file instructs the parent agent to perform the workflow using native tools. The YAML file supplies Codex skill-picker metadata and is ignored by Claude Code and Cursor; it does not create agents or enforce isolation. No service, custom agent configuration, executable orchestration script, or model API integration is required.

The parent performs these steps:

1. Resolve the spec, explicitly requested plan, relevant source areas, project restrictions, and output path. The default spec is `SPEC.md` at the project root; a plan is never silently included.
2. Inspect the native subagent capability, resolve any per-reviewer model and reasoning overrides, and disable inherited conversation history. For Codex tools exposing `fork_turns`, set it to `"none"`; for tools exposing `fork_context`, set it to `false`. For Claude Code, launch each reviewer with the `Agent` tool using a non-fork type such as `general-purpose`; never `fork`, which inherits the conversation. For Cursor, launch each reviewer with the Task tool; Cursor subagents start with a clean context. Use the host's equivalent elsewhere. Stop if fresh contexts cannot be established; unsupported model or reasoning selections leave the affected role incomplete.
3. Record document fingerprints and dispatch the three roles below. Each receives absolute paths, its own rubric, restrictions, and reporting requirements. Parallel execution is preferred when capacity permits; sequential execution still requires a fresh context per role.
4. Wait for each actual result. Verify supporting evidence, check for changed inputs, consolidate duplicate findings, and preserve disagreements and coverage gaps.
5. Write the report and return its path. Resolve findings or implement only within the user's existing authorization.

| Reviewer | Reads | Checks |
| --- | --- | --- |
| Comprehension | The spec, rubric, and applicable restrictions. | Paraphrases the design, then finds unexplained project terms, contradictions, missing prerequisites, and unclear outcomes. It cannot consult code or linked documents to fill gaps. |
| Design and readiness | The spec, requested plan, and relevant source and tests. | Checks behavior, contracts, failure modes, compatibility, and relevant security or concurrency risks. With a plan, verifies affected paths, sequencing, coverage of requirements, and acceptance criteria. |
| Simplicity | The spec, requested plan, and relevant source. | Checks unnecessary complexity, existing functionality that could be reused, and avoidable architectural changes. Uses `slop-detective` when available, otherwise the built-in rubric. |

The parent supplies no drafting transcript, explanation of the intended answers, or other reviewer findings. Reviewers return the paths actually read and identify missing inputs. They do not edit files, launch more agents, or execute application code or validation commands. Only the parent writes the consolidated report.

Conversation isolation is a context boundary, not a separate filesystem or security sandbox. Project instructions and shared files may remain visible. Reviewers must not retrieve history or memory to supply missing requirements. The report discloses known isolation limits. See [EGM's Codex isolation notes](https://github.com/google/elephant-goldfish-model/blob/main/docs/codex.md#fresh-context-reviewers).

## Read and resolve the report

By default, the report is `REVIEW.md` beside the spec. An existing report is preserved unless updating it was requested; another unused filename such as `REVIEW-2.md` is used instead.

The report identifies reviewed document versions, reviewer completion and reading scope, requested and effective model and reasoning settings, the comprehension paraphrase, findings, unresolved questions or disagreements, and verification limits. Findings use project severity conventions, otherwise P1 for serious flaws or blocking ambiguity, P2 for material risks or gaps, and P3 for localized issues with demonstrated impact.

Each finding explains where the issue appears, what scenario or ambiguity triggers it, why it matters, the supporting evidence, and a correction when clear. Missing access is reported as incomplete coverage. A reviewer failure, timeout, or stale input cannot produce a clean completed review.

In the authoring chat, evaluate each finding against the requirements and code. Record the disposition and reason, then put accepted decisions into the spec or plan so later reviewers can see them. Facts known only from the conversation cannot justify dismissing a missing explanation in the spec.

One invocation runs one review round by default. If revisions and re-review are requested together, it permits one follow-up round for affected roles, using fresh contexts. Significant design changes also warrant another simplicity check. After two rounds, remaining issues are reported; another round needs an explicit request. Separate later invocations can review newly written plans or further changes.

## Verification and implementation boundaries

A clean design review is neither proof of correctness nor permission to implement. The skill does not run tests, builds, linters, or formatters; install dependencies; stage files; or commit changes. Findings may propose verification, while actual execution follows the user's authorization and project rules during implementation.

When changing this skill, check its frontmatter and UI metadata, local documentation links, report rules, and delegation instructions. Static checks establish packaging and consistency only. An end-to-end review must be verified separately in a host with native isolated subagents; installation alone does not establish that behavior.
