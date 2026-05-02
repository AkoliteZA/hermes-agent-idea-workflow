---
name: idea-superpowers-suite
description: "Use when running the full idea workflow: capture a rough idea, expand it into a design doc, research similar products, and generate implementation artifacts as separate Markdown files."
version: 1.1.0
author: Hermes Agent
license: MIT
metadata:
  hermes:
    tags: [idea-workflow, note-taking, product-design, implementation, research, superpowers]
    related_skills: [idea-to-design-doc, idea-to-implementation-doc, writing-plans]
---
# Idea Superpowers Suite

This skill is a **superpowers-style umbrella workflow** for idea development.

It is intentionally separate from the more focused skills:
- `idea-to-design-doc`
- `idea-to-implementation-doc`

Use this skill when the user wants a *bigger, multi-step workflow* that feels like a reusable system rather than a one-off prompt.

## Purpose

Turn a rough thought into a structured chain of artifacts:
1. **Idea capture** — create or update a local idea note from the user's high-level concept
2. **Clarifying interview** — ask follow-up questions to capture the rough plan, idea philosophy, target experience, and key constraints
3. **Design doc** — review and flesh the idea into a full design document covering product, UX, and relevant technical aspects
4. **Implementation review** — research similar products and translate the design into build strategy
5. **Build-ready output** — produce one Markdown implementation/build file containing tasks, agent prompts, testing/verification, and acceptance criteria

## Design philosophy

This workflow should feel like a set of reusable powers:
- **Capture fast**
- **Clarify deeply**
- **Research before building**
- **Separate product thinking from engineering thinking**
- **Save every stage as its own Markdown artifact**

This umbrella absorbs the focused stages previously split out as:
- `idea-to-design-doc` for a product/design-only pass
- `idea-to-implementation-doc` for a technical implementation-only pass

## Default file layout

Use local Markdown files first. If the user later wants Obsidian export, treat that as a separate step.

### Mode selection

At the start, choose one of two modes unless the user explicitly picks one:

- **Lite mode** — for sketches, small utilities, early thoughts, and ideas the user wants captured quickly. Create only the minimum useful artifacts.
- **Full mode** — for serious app/product ideas that may later be built by an AI coding agent or Superpowers. Create the complete staged artifact set.

Default to Lite mode when the idea is vague or the user sounds exploratory. Default to Full mode when the user asks for a product spec, implementation plan, build handoff, research pass, or agent-ready artifact.

### Lite mode layout

For lightweight ideas, use:

```text
ideas/<idea-slug>.md
ideas/index.md
```

Optionally add only if requested:

```text
ideas/<idea-slug>.implementation.md
ideas/<idea-slug>.build-prompt.md
```

### Full mode layout

For full app/product ideas, use a staged folder layout so every phase has a durable artifact:

```text
ideas/<idea-slug>/
  README.md
  00-idea-capture.md
  01-design-doc.md
  02-implementation-spec.md
  03-agent-build-handoff.md
  04-spec-review.md
```

Stage meanings:
- `README.md` — package index: status, artifact map, current verdict, open decisions, and next action.
- `00-idea-capture.md` — raw idea, philosophy, goals, early notes, and interview answers.
- `01-design-doc.md` — product/UX/technical design doc.
- `02-implementation-spec.md` — implementation strategy, system design, tasks, testing approach.
- `03-agent-build-handoff.md` — final single-file build handoff for another AI agent or Superpowers.
- `04-spec-review.md` — readiness review before execution: PASS / PASS WITH CHANGES / FAIL.

## Workflow stages

### Stage 1: Capture

When the user says they have an idea, create a working note with:
- title
- short summary
- rough problem statement
- a few initial bullet points

If needed, ask for a name, but prefer to proceed with a temporary title and refine later.

### Stage 2: Interview

Ask one question at a time to flesh out the idea. Use `references/interview-question-bank.md` for stronger prompts.

In Lite mode, ask only the few questions needed to capture the idea clearly, then draft.

In Full mode, cover:
- who it is for
- what problem it solves
- what the app should do
- how it should behave
- how the user should experience it
- what is in scope / out of scope
- what the main screens or sections are
- what data, integrations, or platform constraints matter
- where data should live and who should operate it: local-only, self-hosted, Cloudflare, AWS, another cloud, or undecided
- which platforms are required: browser/web-only, Windows desktop, Mac desktop, cross-platform desktop, mobile web, iOS, Android, or no mobile app
- whether the product is one app or multiple surfaces/services, such as a desktop recorder plus hosted web dashboard plus background worker
- technical defaults the agent recommends, then lets the user accept or change: database/storage, backend/runtime, frontend framework, auth, hosting, file/object storage, queues/jobs, realtime/sync, search, analytics/observability, testing, and deployment/CI
- authentication, authorization, secrets, API keys, public/private sharing, and what must never be committed or exposed to clients
- what would make the result feel excellent, not merely functional

Do **not** push into stack decisions too early unless the user has already made a technical constraint explicit.
When the workflow reaches technical planning, propose a practical default stack based on the idea and constraints first, explain why, then ask the user to accept or change it. Do not force the user to invent database/cloud/framework choices from scratch.

### Stage 3: Design doc

Write a clean design doc that captures the agreed product direction and enough technical shape to support implementation planning.

Suggested sections:
- one-line summary
- problem / purpose
- product philosophy
- target user
- core concept
- desired behavior
- key features
- layout / information architecture
- UX notes
- technical shape
- data / integrations / platform needs
- hosting / data location / deployment preference
- platform targets: web, desktop, mobile, or combinations
- recommended technical defaults and accepted/changed decisions
- non-goals
- open questions
- next steps

### Stage 4: Research pass

For the specific idea note, look for similar products and note:
- what already exists
- what is common / commodity
- what is different
- what should be avoided
- where the idea fits in the market

### Stage 5: Implementation thinking

Translate the idea into a build plan that includes:
- major system pieces
- data needs
- recommended database/storage choice and why: SQLite, Postgres, MySQL, DynamoDB, Cloudflare D1/KV/R2, S3, local files, or no database
- hosting/deployment target and whether data stays local or goes to a cloud provider such as Cloudflare or AWS
- platform target decisions: browser-only, Windows app, Mac app, mobile app, or responsive web/mobile-web only
- app topology decisions: single app, desktop + web, mobile + API, workers, upload agents, or other split surfaces
- frontend structure
- backend/service needs if any
- recommended technical stack defaults for frontend, backend/runtime, auth, database, object/file storage, queues/jobs, realtime/sync, search, observability/logging, testing, and deployment/CI
- security/secrets model for credentials, API keys, admin access, public links, and client/server boundaries
- integration points
- workflow/milestones
- risks and tradeoffs
- executable build tasks
- agent-ready prompts/handoff instructions
- testing and verification plan
- acceptance criteria / “done means” checklist
- a build sequence a developer or AI coding agent could follow

Stay practical. Avoid over-designing. The final implementation artifact should be usable as the single Markdown source of truth for an agent building the program.

For technical decisions, use a **recommend-then-confirm** flow:
1. Infer sensible defaults from the product constraints.
2. Present them as a concise recommendation table with rationale.
3. Ask whether the user accepts the defaults or wants to change any item.
4. Record accepted defaults as decisions and changed items as explicit overrides.
5. If the user is unsure, proceed with the recommended defaults and mark them as assumptions.

### Stage 6: Final agent build handoff

Create or update `README.md` using `templates/idea-package-readme-template.md` so the artifact folder has an obvious status/index page.

Create `03-agent-build-handoff.md` as the final single-file handoff that another agent can use as source of truth. Use `idea-to-implementation-doc/templates/agent-build-handoff-template.md` as the required structure. Load the `idea-to-implementation-doc` skill when creating this handoff, because the canonical handoff template lives there rather than in this umbrella skill's linked template list.

The handoff must include:
- mission
- product vision
- non-negotiable requirements
- out of scope
- technical architecture
- implementation phases
- build tasks
- testing requirements
- verification commands/checks
- acceptance criteria
- “done means” checklist
- prompt for the build agent
- explicit Superpowers handoff instructions

### Stage 7: Spec review / readiness gate

Before handing the spec to Superpowers or a build agent, create `04-spec-review.md`.

Review the final handoff for:
- whether the product goal is clear
- whether requirements are testable
- whether unresolved product decisions remain
- whether unresolved technical decisions remain
- whether acceptance criteria are concrete
- whether the “done means” section is specific
- whether a fresh agent could build from the file without asking obvious questions
- whether testing and verification requirements are included
- whether non-goals are clear enough to prevent scope creep

Use one of these verdicts:
- `PASS` — ready to feed into Superpowers.
- `PASS WITH CHANGES` — mostly ready; patch listed issues first.
- `FAIL` — too ambiguous or incomplete to build safely.

### Stage 8: Review / plan review

If the user wants a check or critique pass, review the artifacts as separate tracks rather than collapsing them into one package.

Review for:
- whether each note keeps its own scope
- whether the design doc stays product-focused while including necessary technical shape
- whether the implementation doc stays build-focused
- whether the final handoff is truly agent-ready
- whether the spec review is honest about gaps
- whether any umbrella summary accidentally duplicates the focused docs
- whether a copied plan or review note should be saved as its own Markdown artifact

This stage should preserve separation of concerns, not merge everything together.


## Progression rules and override phrase

Respect Lite vs Full mode when deciding how much process to apply.

### Lite mode rules

- Capture the idea quickly and do not force research or implementation planning.
- Ask at most 3-5 clarifying questions before drafting unless the user wants more.
- Do not create `02-implementation-spec.md`, `03-agent-build-handoff.md`, or `04-spec-review.md` unless the user asks to upgrade to Full mode.
- If the user says the idea is just a note, keep it as a note.

### Full mode rules

- Do not move from capture to design until the target user, problem, and core behavior are clear enough to summarize.
- Do not move from design to implementation if open product questions would change the architecture or MVP scope.
- Do not create the final build handoff until implementation phases, testing requirements, acceptance criteria, and Done Means are present.
- Do not mark the spec review `PASS` if verification commands/checks are missing or if major product/technical decisions remain unstated.
- If open questions remain but do not block an MVP, mark them explicitly and use `PASS WITH CHANGES` if small patches are needed.

### User override phrase

The user can force progression with this exact phrase:

> **GREENLIGHT NEXT STAGE**

When the user says **GREENLIGHT NEXT STAGE**, move to the next stage even if you would normally keep asking questions. Do not argue. Briefly note the risk, carry forward unresolved items under **Open Questions / Assumptions**, and continue.

Examples:
- "GREENLIGHT NEXT STAGE — write the design doc."
- "GREENLIGHT NEXT STAGE — make the implementation spec with what we have."
- "GREENLIGHT NEXT STAGE — produce the handoff even with open questions."

This override does not allow unsafe actions or credential exposure. It only overrides product/spec completeness gates.

## Operating rules

- Keep the workflow modular.
- Prefer separate files for separate stages.
- Save progress as Markdown.
- When a user confirms a product/architecture decision during idea development, patch the working idea note immediately so the decision is not lost.
- Only add generic, shareable examples or reusable patterns to this skill. Do not include private user-specific product plans in skill references.
- If the user says "stop" or "that's enough," stop questioning and draft immediately.
- If the user wants a lighter experience, use Lite mode.
- If the user wants the whole process, use Full mode and continue through the later stages.

## Reference files

- `references/interview-question-bank.md` — reusable question bank for Lite and Full mode interviews.
- `references/example-cli-tool-build-handoff.md` — generic example final handoff for a small developer CLI tool.
- `references/example-saas-web-app-build-handoff.md` — generic example final handoff for a small SaaS/web app.
- `references/example-automation-script-build-handoff.md` — generic example final handoff for a practical automation workflow.

## Templates

- `idea-to-implementation-doc/templates/agent-build-handoff-template.md` — required structure for final agent/Superpowers build handoff documents.
- `templates/idea-package-readme-template.md` — README/status index for Full mode idea folders.

## Naming convention

Use the same idea slug/title across stages. Lite mode may use `ideas/my-app.md`; Full mode should use `ideas/my-app/README.md` plus numbered stage files.

## Suggested response style

Be concise, organized, and exploratory.

When actively interviewing, ask one question and wait.
When producing docs, show the output path and a short summary of what was created.

## Relationship to the focused skills

This umbrella skill should behave like the orchestrator of the workflow, while the focused skills remain available for narrower use cases.

- Use `idea-to-design-doc` when the user only wants the design stage.
- Use `idea-to-implementation-doc` when the user only wants the implementation stage.
- Use `idea-superpowers-suite` when the user wants the full system.

## Relationship to the Superpowers build toolset

This `idea-workflow` toolset is the **front-end product/spec pipeline**. It should take a high-level idea and produce a strong design + implementation spec.

The `superpowers-gpt` toolset is the **execution discipline pipeline**. After the idea workflow produces the final build-ready Markdown spec, hand that spec to Superpowers to validate, plan, split, implement, test, review, and verify.

Expected handoff:
1. `idea-superpowers-suite` captures the idea and asks clarifying questions.
2. `idea-to-design-doc` produces the design/spec document.
3. `idea-to-implementation-doc` produces the single build-ready Markdown file with tasks, prompt, tests, acceptance criteria, and a Superpowers-aligned handoff that asks Superpowers to inspect, plan, execute, review, and verify rather than blindly code.
4. `superpowers-using-superpowers` routes the build request.
5. `superpowers-writing-plans` turns the build-ready spec into exact implementation tasks with files, commands, and validation.
6. `superpowers-executing-plans` or `superpowers-subagent-driven-development` implements the plan.
7. `superpowers-requesting-code-review` and `superpowers-verification-before-completion` ensure the agent cannot honestly say “done” until fresh evidence proves it works.

The idea workflow should not try to replace the 14-skill Superpowers execution suite. It should create the high-quality input that makes the Superpowers suite effective.
