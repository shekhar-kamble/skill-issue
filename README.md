# Skill Issue

Personal skills for planning, implementing, reviewing, and documenting software.

## Install

Install [Node.js](https://nodejs.org/) and Git first.

Choose the agent with `-a`:

| Agent | Flag | Global skills directory |
| --- | --- | --- |
| Codex | `-a codex` | `~/.agents/skills` |
| Claude Code | `-a claude-code` | `~/.claude/skills` |
| Cursor | `-a cursor` | `~/.agents/skills` (shared with Codex; Cursor reads it) |
| Several | `-a codex -a claude-code -a cursor` | `~/.agents/skills` and `~/.claude/skills` |

Codex:

```bash
npx skills add shekhar-kamble/skill-issue --skill '*' -g -a codex
```

Claude Code:

```bash
npx skills add shekhar-kamble/skill-issue --skill '*' -g -a claude-code
```

Cursor:

```bash
npx skills add shekhar-kamble/skill-issue --skill '*' -g -a cursor
```

Omit `-a` to pick agents from an interactive list. Add `-y` to skip prompts. Skills work in either agent from the same `SKILL.md` files; Claude Code and Cursor ignore the Codex-only `agents/openai.yaml` metadata.

Browse the available skills:

```bash
npx skills add shekhar-kamble/skill-issue --list
```

To select one skill, replace `'*'` with its name. Install `good-micro-management` alongside `create-plan` or `execute-plan`: those skills refer to its instructions.

Invoke a skill by name. Codex uses `$review-design`; Claude Code and Cursor use `/review-design` or load it automatically when the request matches its description.

## Update

Update globally installed skills:

```bash
npx skills update -g
```

This updates installed skills across their sources. To install newly added skills from this repository, rerun the installation command. Updates require a command on each friend's computer; pushing changes to GitHub does not update their installation automatically.

Command reference: [Vercel Skills CLI](https://github.com/vercel-labs/skills).

## Included skills

| Skill | Purpose |
| --- | --- |
| ask-questions-if-underspecified | Resolve material ambiguity before proceeding. |
| create-plan | Write a software implementation plan. |
| create-spec | Write a software feature specification. |
| execute-plan | Carry out an existing implementation plan. |
| good-micro-management | Keep engineering changes complete and focused. |
| interview | Gather software feature requirements. |
| review-design | Review specs and plans with fresh, independent subagents. |
| slop-detective | Review complexity in a loyalist Primarch's voice. |
| spec-story | Turn a specification into narrative prose. |
| staged-code-review | Review staged Git changes without modifying them. |
| tech-docs-writer | Write and revise software documentation. |

## Review designs with the team

Use `review-design` to review a spec and optional implementation plan without giving reviewers the drafting conversation. Three fresh subagents (Codex subagents, Claude Code `Agent` calls, or Cursor Task calls) check comprehension, design and implementation readiness, and unnecessary complexity. The skill requires a host that can launch subagents without inherited conversation history and writes a consolidated report while leaving the design and source unchanged.

```text
Use $review-design on SPEC.md with PLAN.md. Write the report to REVIEW.md.
```

In Claude Code or Cursor, write `/review-design` in place of `$review-design`.

Omit `with PLAN.md` for a spec-only review. The skill works on its own and uses `slop-detective` when installed. See the [team workflow and implementation guide](docs/design-review.md) for installation, reviewer inputs, finding resolution, and the steps from requirements through implementation and staged code review.

## Maintain the collection

Edit the relevant `<name>/SKILL.md` and its supporting files in this repository, then review, commit, and push the changes. New skills belong in their own directory at the repository root, with a `SKILL.md` containing `name` and `description` frontmatter. Add each new skill to the table above.

## Use the checkout for local authoring

Keep a local clone and link each skill folder into your agent's user skill directory: `~/.agents/skills` (or older `~/.codex/skills`) for Codex, `~/.claude/skills` for Claude Code, `~/.cursor/skills` for Cursor. Back up any existing folder before replacing it with a link. Link the entire folder so references and UI metadata remain available.

```bash
ln -s "$PWD/review-design" ~/.claude/skills/review-design
```

Edits in the checkout are then available locally without reinstalling. Pulling changes updates existing linked skills; add a link when you create a new skill. Both agents detect changes automatically; restart if a change does not appear. See [OpenAI's local skill documentation](https://learn.chatgpt.com/docs/build-skills) [Claude Code skills](https://code.claude.com/docs/en/skills), and [Cursor skills](https://cursor.com/docs/context/skills).

## License

[MIT](LICENSE).
