---
name: task-implementation-doc-skill
description: Turn a rough feature prompt into a short Technical Implementation Document (Word .docx file) with 8 fixed headings, strictly within the prompt's scope. Works in Claude, Cursor, GitHub Copilot, ChatGPT and Gemini.
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
4. Close every gap yourself, so the document has no open questions and no "Clarification Required" text anywhere. See "Closing gaps without asking" below. Closing a gap never means adding a feature: it means picking the plainest reading of the prompt, or leaving the unknown detail out.
5. Use the same document structure for every developer: the same 8 headings, in the same order, with the same table columns. Do not add, remove, rename or reorder headings.
6. Include only relevant requirements. Leave out generic filler. If a heading has nothing relevant, write one line: "Not applicable. <reason>".

## Closing gaps without asking

The user wants one finished document, with no questions to answer first and no open items inside it. A developer reading the document should find clear statements only. When the rough prompt is silent on something, go down this list and stop at the first step that works:

1. **Check the code (repo mode).** Take the real name, value, endpoint or rule from the codebase.
2. **Read the prompt as a whole.** Many gaps are answered elsewhere in the prompt, or follow directly from a stated rule (for example, a role that can approve can also view).
3. **Take the plainest default that matches the prompt's own pattern.** For example, a list that must be filtered "by status" uses the status values the prompt already names. Write it as a normal statement of the requirement, in the same way as every other row.
4. **Leave the detail out.** If a detail (a method, a URL, a response field, mail wording, an error code) is not in the prompt and cannot be taken safely from steps 1 to 3, do not write a placeholder and do not make one up. Write the row at the level the prompt supports, for example "API to list pending requests, available to the approver role". A shorter true row is better than a complete invented one.

The scope rules still hold: closing a gap must never add a table, column, API, role, rule, index or feature that the prompt does not ask for. Do not write the words "Clarification Required", "to be confirmed", "TBD", "assumed" or "to be checked in the code" in the document.

If a name in the prompt looks like a spelling mistake, use the real name from the code in repo mode. Without the code, use it as written.

If you made a choice in step 3 that a developer could reasonably see differently, you may add one short line to the chat reply (not the document) so the user can correct you. Do not ask questions and do not wait for an answer.

## Works in any AI tool

First check what your environment can do, then follow the matching mode.

| Mode | When | What to do |
| --- | --- | --- |
| Repo mode | You can read the project files (Cursor, GitHub Copilot in agent or workspace mode, Claude Code, or any agent with the repo open) | Search the codebase for dependencies and real names. Save the document as a Word file in the project, for example `docs/<feature-name>-implementation.docx`, and tell the user the path. |
| Chat mode | You cannot see the project (ChatGPT, Gemini, Claude chat, Copilot chat without the repo) | Work only from the rough prompt. In Dependencies, list what the prompt implies. Create the Word file and give the user the file to download. |

In chat mode, if the user has already pasted related files (models, routes, status constants), use them to check dependencies. Do not ask for files.

## Output file format: Word (.docx)

The final deliverable is always a Word document (`.docx`), because developers and QA share it by email and edit it in Word. Do not hand over the document as markdown or plain chat text.

- Name the file `<feature-name>-implementation.docx`.
- Use your tool's own way to create Word files: a docx or document skill if one is available, a document library in a script, or write markdown first and convert it to `.docx`. Pick whatever works in your environment. Do not rely on one specific tool.
- Use real Word structure: the feature name as the Title, the date under it, the 8 headings as Heading 1, real tables with a header row, bullets and numbered lists. Do not leave markdown symbols (`#`, `|`, `**`, backticks) in the file.
- Show the exact table names, column names, values and role names in a monospace font in place of backticks.
- Show the checklist items in section 8 as checkbox lines using the ☐ character.
- If your environment truly cannot create a file (for example a chat without file or code tools), give the full document in one reply as markdown and say in one line that it can be pasted into Word. This is the last option.

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
8. If a name or text in the rough prompt looks like a spelling mistake (for example a role name or a status value), use the real name from the code in repo mode. Otherwise use it as written. Do not flag it in the document.

## Flow chart is optional

- A flow chart is not mandatory, and the skill does not ask about it. Draw one only if the user asked for a flow chart in the prompt. Otherwise leave it out.
- Without a chart, Workflow Understanding is a lead sentence plus short numbered steps that follow the rough prompt. With a chart, keep the same steps under it. A Word file cannot render Mermaid, so draw the chart as an image inserted in the document if your tool can, or else as a simple text flow with arrows (for example `Step A → Step B → Step C`).

## Dependencies (section 3)

Section 3 is about what this feature uses and what it can affect. It is not a list of code files.

- **Repo mode:** search the codebase. Identify all internal and external dependencies: modules, features, functions, tables, jobs, mails, roles, settings and existing workflows that may be affected. Check other places that update the same status or the same tables. Report what you found in a table: Type, Item, Why it matters.
- **If the rough prompt has a Dependencies note** (for example "does not affect any other module" or "no external package is used"), use it as the stated position. In repo mode, still check the code. If the code shows a conflict, tell the user in the chat reply and write what the code shows.
- **External dependencies and packages:** mention them only if the rough prompt mentions them. If the prompt says none, or says nothing, write "None expected". Existing services that the feature must use (mail service, queue, role check) can be listed as existing, without product names.
- **Chat mode:** list what the rough prompt implies (tables, roles, settings, mail, queue) in the same table.

## No disclaimer notes in the document

The document contains only the title, the date and the 8 sections. Do not add a note, banner or footer about how it was written, for example that no project code was found, that it is based only on the rough prompt, or that items need to be checked in the code. Do not add a clarifications or open-questions section either. The user does not want any of this.

## Content of each section

Everything must come from the rough prompt (or, in repo mode, the code). Do not add new features, fields, APIs or rules. Where the prompt is silent on something the developer needs, close the gap as described in "Closing gaps without asking" and move on. Keep each point to one table row.

1. **Logical Scenarios** — one table: Type, Scenario, Expected result. Cover: normal, alternative, negative, empty data, duplicate data, invalid input, permission or role, edge cases, status management, error handling. Each scenario must follow from a requirement in the prompt (for example, what happens when a stated rule, filter, role or input is not met). Skip a type if nothing in the prompt relates to it. For error handling, give the code and a simple message only where the behaviour is clear from the prompt. Otherwise describe the expected behaviour in plain words (for example "request is rejected with a clear message") and leave out codes and exact text.
2. **Workflow Understanding** — lead sentence, optional chart, short numbered steps from the user's action to the final result, in the order given in the prompt. Say that the database changes are in one transaction only if the prompt says so.
3. **Dependencies** — as described above.
4. **Integration** — a table of the APIs and the CRM page that the prompt asks for (purpose, who can use it). Method, URL, parameters, response fields and mail content are written only if the prompt gives them. Otherwise leave that detail out of the row. Add only the integration points the prompt implies (for example mail, existing screens that read the new rows).
5. **Database Requirements And Database Mapping** — one table: Table, Action, Columns and values. Show the list filter, each update, each insert, and what stays NULL, exactly as the prompt says. Where the prompt gives a rule for building a list (for example an email list with an exclusion), write the rule in one short paragraph. If the prompt does not ask for a new table, column or index, write "No new table or column". Do not suggest anything beyond the prompt (index, unique key, NULL check).
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
- Check that the document has no "Clarification Required", "TBD", "to be confirmed" or "assumed" text, and no open questions.
- Check that the section references in the checklist point to sections that exist.
- Check that no technology name and no code block is present.
- Check that the file is a real `.docx` and that no disclaimer note is in it.
- Do not ask the user any question. The chat reply is one line with the file path of the Word document. Add one more short line only if you made a choice in step 3 of "Closing gaps without asking" that the user may want to correct.

## How to install (for the user)

Install with one command. It works for Claude Code, Codex, Cursor and GitHub Copilot:

`npx skills add swalihvmbazar/task-implementation-doc-skill`

Add `-a claude-code`, `-a codex`, `-a cursor` or `-a github-copilot` to choose the tool, and `-g` to install for all projects. See the README of the repository for all steps.

For ChatGPT and Gemini, paste the body of this file into the instructions of a custom GPT or a Gem.

Then paste the rough prompt and ask for the implementation document.
