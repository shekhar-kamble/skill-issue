# Skill Issue

Personal skills for planning, implementing, reviewing, and documenting software.

## Install

Install [Node.js](https://nodejs.org/) and Git first.

Install all skills globally for Codex:

```bash
npx skills add shekhar-kamble/skill-issue --skill '*' -g -a codex
```

For Claude Code:

```bash
npx skills add shekhar-kamble/skill-issue --skill '*' -g -a claude-code
```

Browse the available skills:

```bash
npx skills add shekhar-kamble/skill-issue --list
```

To select one skill, replace `'*'` with its name. Install `good-micro-management` alongside `create-plan` or `execute-plan`: those skills refer to its instructions.

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
| slop-detective | Review complexity in a loyalist Primarch's voice. |
| spec-story | Turn a specification into narrative prose. |
| staged-code-review | Review staged Git changes without modifying them. |
| tech-docs-writer | Write and revise software documentation. |

## Maintain the collection

Edit the relevant `<name>/SKILL.md` and its supporting files in this repository, then review, commit, and push the changes. New skills belong in their own directory at the repository root, with a `SKILL.md` containing `name` and `description` frontmatter. Add each new skill to the table above.

## Use the checkout for local authoring

Keep a local clone and link each skill folder into your agent's user skill directory. Codex supports folder symlinks in `~/.agents/skills`; older setups may use `~/.codex/skills`. Back up any existing folder before replacing it with a link. Link the entire folder so references and UI metadata remain available.

Edits in the checkout are then available locally without reinstalling. Pulling changes updates existing linked skills; add a link when you create a new skill. Codex detects changes automatically; restart it if a change does not appear. See [OpenAI's local skill documentation](https://learn.chatgpt.com/docs/build-skills).

## License

[MIT](LICENSE).
