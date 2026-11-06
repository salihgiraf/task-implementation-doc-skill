---
name: task-implementation-doc-skill
description: Turn a rough feature prompt into a short Technical Implementation Document with 8 fixed headings, strictly within the prompt's scope. Works in Claude, Cursor, GitHub Copilot, ChatGPT and Gemini.
---

# Technical Implementation Document from a Rough Prompt

Use this skill when the user pastes a rough feature note (bullets, table updates, role rules, mail steps) and wants a Technical Implementation Document for developers.

These instructions are tool-neutral. They work in Claude, Cursor, GitHub Copilot, ChatGPT and Gemini. Do not depend on any feature that belongs to only one tool.

## What the input looks like

A rough prompt is a short list. For example: a new API and CRM page, a filter (status and type), who can view or approve, the table updates on approval (column = value), mail to a list of emails, a history entry, and sometimes a "Dependencies" note. Treat it as the single source of truth for names, values and scope.

## Scope rules (follow strictly)

1. Do not introduce additional features or functionality.
2. Do not expand the original project scope.
3. Do not invent database fields, APIs or business logic. Use only the tables, columns, values, endpoints, roles and rules that are in the rough prompt (or, in repo mode, found in the codebase).
4. Identify missing information as **Clarification Required**. Write it inline, in the section where it is needed, as: `Clarification Required: <what is missing>`. Do not guess. Do not add a separate open-questions section.
5. Use the same document structure for every developer: the same 8 headings, in the same order, with the same table columns. Do not add, remove, rename or reorder headings.
6. Include only relevant requirements. Leave out generic filler. If a heading has nothing relevant, write one line: "Not applicable. <reason>".

## Works in any AI tool

First check what your environment can do, then follow the matching mode.

| Mode | When | What to do |
| --- | --- | --- |
| Repo mode | You can read the project files (Cursor, GitHub Copilot in agent or workspace mode, Claude Code, or any agent with the repo open) | Search the codebase for dependencies and real names. Write the document as one markdown file in the project, for example `docs/<feature-name>-implementation.md`, and tell the user the path. |
| Chat mode | You cannot see the project (ChatGPT, Gemini, Claude chat, Copilot chat without the repo) | Work only from the rough prompt. In Dependencies, list what the prompt implies and mark it "to be checked in the code". Give the full document in one reply as markdown, so the user can copy it. If a doc or file tool is available, create the doc there instead. |

In chat mode, if the user can paste a few related files (models, routes, status constants), ask once and use them to check dependencies.

Use only plain markdown: headings, tables, bullets and `- [ ]` tasks. These render in every tool.

## Output rules

1. Use exactly these 8 headings, in this order, as main headings:
   1. Logical Scenarios
   2. Workflow Understanding
   3. Dependencies
   4. Integration
   5. Database Requirements And Database Mapping
   6. Impact Analysis
   7. Testing Requirements
   8. Developer Self-Review Checklist
2. Title at the top: the feature name, with today's date under it.
3. Keep it short. One lead sentence per section, then one table or a few bullets. No sub-headings, no long paragraphs.
4. Language: simple Indian English. Short sentences. Easy to understand.
5. Do not include code, code blocks, SQL or JSON samples.
6. Do not name any framework, language, library, package or tool (for example a backend framework, a frontend library, a test tool, a mail testing tool) unless the rough prompt itself names it. Say "backend", "CRM page", "queue", "mail service" instead.
7. Use the exact table names, column names, values, role names and texts from the rough prompt. Put them in code formatting inside sentences and tables.
8. If a name or text in the rough prompt looks like a spelling mistake (for example a role name or a status value), do not change it silently. In repo mode, check the real name in the code and use that. Otherwise use it as written and add `Clarification Required: confirm the name "<name>"` in the relevant section.

## Flow chart is optional

- A flow chart is not mandatory.
- Before drawing one, ask the user: "Do you need a flow chart in Workflow Understanding?" Ask this together with any other question, in one message.
- If the user cannot be asked (unattended run), skip the chart and say so in one line.
- Without a chart, Workflow Understanding is a lead sentence plus short numbered steps that follow the rough prompt. With a chart, keep the same steps under it. Use a Mermaid block in markdown output. If the tool cannot render Mermaid, use a simple text flow with arrows instead.

## Dependencies (section 3)

Section 3 is about what this feature uses and what it can affect. It is not a list of code files.

- **Repo mode:** search the codebase. Identify all internal and external dependencies: modules, features, functions, tables, jobs, mails, roles, settings and existing workflows that may be affected. Check other places that update the same status or the same tables. Report what you found in a table: Type, Item, Why it matters.
- **If the rough prompt has a Dependencies note** (for example "does not affect any other module" or "no external package is used"), use it as the stated position. In repo mode, still check the code. If the code shows a conflict, tell the user in the chat reply and write what the code shows.
- **External dependencies and packages:** mention them only if the rough prompt mentions them. If the prompt says none, or says nothing, write "None expected". Existing services that the feature must use (mail service, queue, role check) can be listed as existing, without product names.
- **Chat mode:** list what the rough prompt implies (tables, roles, settings, mail, queue) and mark "to be checked in the code".

## Content of each section

Everything must come from the rough prompt. Do not add new features, fields, APIs or rules. Where the prompt is silent on something the developer needs, write `Clarification Required: ...` and move on. Keep each point to one table row.

1. **Logical Scenarios** — one table: Type, Scenario, Expected result. Cover: normal, alternative, negative, empty data, duplicate data, invalid input, permission or role, edge cases, status management, error handling. Each scenario must follow from a requirement in the prompt (for example, what happens when a stated rule, filter, role or input is not met). Skip a type if nothing in the prompt relates to it. For error handling, give the code and a simple message only where the behaviour is clear from the prompt. Otherwise write Clarification Required.
2. **Workflow Understanding** — lead sentence, optional chart, short numbered steps from the user's action to the final result, in the order given in the prompt. Say that the database changes are in one transaction only if the prompt says so.
3. **Dependencies** — as described above.
4. **Integration** — a table of the APIs and the CRM page that the prompt asks for (purpose, who can use it). Method, URL, parameters, response fields and mail content are written only if the prompt gives them. Otherwise write `Clarification Required: <item>`. Add only the integration points the prompt implies (for example mail, existing screens that read the new rows).
5. **Database Requirements And Database Mapping** — one table: Table, Action, Columns and values. Show the list filter, each update, each insert, and what stays NULL, exactly as the prompt says. Where the prompt gives a rule for building a list (for example an email list with an exclusion), write the rule in one short paragraph. If the prompt does not ask for a new table, column or index, write "No new table or column". Any suggestion beyond the prompt (index, unique key, NULL check) is written only as `Clarification Required`.
6. **Impact Analysis** — table: Area, Impact, Risk, What to do. Include only the areas the prompt touches (tables, status, screens, roles, settings, mail). If the prompt has a Dependencies note, keep it consistent with it. Add one line for deployment and one for rollback only if relevant.
7. **Testing Requirements** — test data to prepare (one line), a table of key tests with expected results that follow sections 1, 4 and 5, the database check after the action, a regression line for the areas in section 6, and a line of exit criteria before QA handover.
8. **Developer Self-Review Checklist** — exactly these 13 items as `- [ ]` tasks, each with a short note that points to the right section:
   - All logical scenarios identified
   - Dependencies identified
   - API requirements documented
   - API integration completed and tested
   - Database changes documented
   - Data saving and retrieval verified
   - Workflow verified
   - Error handling implemented
   - Security and permission checks completed
   - Positive and negative scenarios tested
   - No existing functionality affected
   - Code reviewed
   - Documentation updated

## Before you finish

- Check that every table, column, value, role and text from the rough prompt appears correctly in section 5 and in the tests.
- Check that nothing was added beyond the prompt: no extra feature, field, API, parameter or rule.
- Check that every missing detail is marked `Clarification Required` in its section.
- Check that the section references in the checklist point to sections that exist.
- Check that no technology name and no code block is present.
- In the chat reply, give one line about the document (or its file path), then a short list of all Clarification Required items, and the question about the flow chart if it was not asked yet.

## How to install (for the user)

Install with one command. It works for Claude Code, Codex, Cursor and GitHub Copilot:

`npx skills add swalihvmbazar/task-implementation-doc-skill`

Add `-a claude-code`, `-a codex`, `-a cursor` or `-a github-copilot` to choose the tool, and `-g` to install for all projects. See the README of the repository for all steps.

For ChatGPT and Gemini, paste the body of this file into the instructions of a custom GPT or a Gem.

Then paste the rough prompt and ask for the implementation document.
