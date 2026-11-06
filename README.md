# Task Implementation Doc Skill

Turn a rough feature prompt into a short Technical Implementation Document with 8 fixed headings:

1. Logical Scenarios
2. Workflow Understanding
3. Dependencies
4. Integration
5. Database Requirements And Database Mapping
6. Impact Analysis
7. Testing Requirements
8. Developer Self-Review Checklist

The output is a Word (.docx) file, short, in simple Indian English, with no code and no technology names unless your prompt names them. A flow chart is included only if your prompt asks for one.

## Scope rules

- No additional features or functionality, and no expansion of the original scope.
- No invented database fields, APIs or business logic.
- Gaps are closed by the skill itself, from the code or the plainest reading of your prompt. The document has no clarification or open-question items, and the skill does not ask you questions.
- The same document structure for every developer.
- Only relevant requirements are included.

## Install with npx

You need Node.js installed. The commands use the open `skills` CLI (the same one used for skills from skills.sh). It copies the skill into the skills folder of each tool.

### Install for all supported tools (asks you to choose)

```bash
npx skills add salihgiraf/task-implementation-doc-skill
```

### Install for one tool

| Tool | Command |
| --- | --- |
| Claude Code | `npx skills add salihgiraf/task-implementation-doc-skill -a claude-code` |
| Codex | `npx skills add salihgiraf/task-implementation-doc-skill -a codex` |
| Cursor | `npx skills add salihgiraf/task-implementation-doc-skill -a cursor` |
| GitHub Copilot | `npx skills add salihgiraf/task-implementation-doc-skill -a github-copilot` |

### Install for several tools at once

```bash
npx skills add salihgiraf/task-implementation-doc-skill -a claude-code -a codex -a cursor -a github-copilot
```

### Project or global

- By default the skill is installed in the current project (for example `.claude/skills/`). Commit it, so the whole team gets it.
- Add `-g` to install for your user, so it works in every project.

```bash
npx skills add salihgiraf/task-implementation-doc-skill -g -a claude-code
```

### Other useful commands

```bash
# See what is inside the repository before installing
npx skills add salihgiraf/task-implementation-doc-skill --list

# Install without questions
npx skills add salihgiraf/task-implementation-doc-skill --skill task-implementation-doc-skill -a claude-code -y

# See installed skills
npx skills list

# Check and update
npx skills check
npx skills update

# Remove
npx skills remove task-implementation-doc-skill
```

The exact folders depend on the tool and on the version of the CLI. Run `npx skills add` and it shows where it will install.

## How to use

1. Open your project in the tool (Claude Code, Codex, Cursor or GitHub Copilot).
2. Paste a rough prompt: bullets about the feature, the tables to update, the roles and the mails.
3. Ask: "Create the implementation document for this."
4. The skill writes the document straight away and replies with the file path.

The document is a Word file. In a tool that can read your project, it is saved as `docs/<feature-name>-implementation.docx`.

## ChatGPT and Gemini

These tools do not read skill folders. Open `skills/task-implementation-doc-skill/SKILL.md`, copy the text, and paste it into the instructions of a custom GPT (ChatGPT) or a Gem (Gemini). Without access to your project, the skill works only from your prompt.

## Repository layout

```text
skills/
  task-implementation-doc-skill/
    SKILL.md
README.md
```
