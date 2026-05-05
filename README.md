# Hermes Agent Idea Workflow

> Idea Workflow is the pre-build product/spec pipeline for Hermes Agent: it turns rough ideas into design docs, implementation specs, and agent-ready build handoffs.

Idea Workflow is a public reusable Hermes Agent skill/toolset for the idea-before-building phase. It helps an agent take a rough product, app, or tool idea and turn it into structured artifacts before coding starts.

It is not the coding or build execution system. It creates the source material that should feed a build workflow.

## What It Is

This repository contains Hermes Agent skills for moving from vague idea to build-ready specification. The workflow separates product thinking from implementation thinking so an agent can capture the user's intent, clarify the experience, create a design document, produce an implementation spec, and package everything into a single handoff file another agent can build from.

The toolset helps an agent produce:

1. Lite-mode quick idea note
2. Full-mode staged idea package
3. guided interview/questions
4. design document
5. optional UI design brief
6. implementation spec
7. final single-file agent build handoff
8. spec review gate
9. explicit handoff into the Superpowers workflow

## Where It Fits

```text
rough idea
  -> idea-workflow
  -> design doc
  -> implementation spec
  -> agent build handoff
  -> spec review
  -> Superpowers for GPT
  -> implementation planning
  -> coding
  -> review
  -> verification
```

The recommended next step after this toolset is Superpowers for GPT:

https://github.com/AkoliteZA/hermes-agent-supwerpowers-chatgpt

## Why Use It

Idea Workflow helps:

- prevent vague prompts from becoming vague builds
- capture product philosophy and UX intent
- separate product thinking from implementation thinking
- produce handoff files another agent can build from
- create clearer inputs for Superpowers-style coding workflows

## Modes

### Lite Mode

Lite Mode is for quick idea capture: sketches, small utilities, early thoughts, or ideas that only need enough structure to revisit later.

Outputs:

```text
ideas/<idea-slug>.md
ideas/index.md
```

### Full Mode

Full Mode is for serious product, app, or tool ideas that may later be built by an AI coding agent.

Outputs:

```text
ideas/<idea-slug>/
  README.md
  00-idea-capture.md
  01-design-doc.md
  02-ui-design-brief.md
  03-implementation-spec.md
  04-agent-build-handoff.md
  05-spec-review.md
```

If the optional UI stage is skipped, preserve the simpler layout:

```text
ideas/<idea-slug>/
  README.md
  00-idea-capture.md
  01-design-doc.md
  02-implementation-spec.md
  03-agent-build-handoff.md
  04-spec-review.md
```

## Included Skills

- `idea-superpowers-suite`
- `idea-to-design-doc`
- `idea-to-ui-design-brief`
- `idea-to-implementation-doc`

## Key Features

- Lite vs Full workflow
- guided interview flow
- reusable question bank
- staged Markdown artifacts
- optional Full-mode UI design brief
- optional image-generation concept prompts
- README/status index template
- implementation handoff template
- spec review gate
- `PASS`, `PASS WITH CHANGES`, `FAIL` review model
- user override phrase: `GREENLIGHT NEXT STAGE`
- Superpowers-aligned build handoff
- generic examples

## Override Phrase

```text
GREENLIGHT NEXT STAGE
```

If the user says this phrase, the agent should stop asking more product/spec questions and move to the next artifact, carrying unresolved issues into **Open Questions** or **Assumptions**.

## Quick Start

Install the skills under your Hermes skills directory:

```text
~/.hermes/skills/idea-workflow/
```

Manual install example:

```bash
mkdir -p ~/.hermes/skills/idea-workflow
cp -R idea-superpowers-suite idea-to-design-doc idea-to-ui-design-brief idea-to-implementation-doc ~/.hermes/skills/idea-workflow/
```

Depending on how Hermes is running, skill loading may require a new Hermes session.

Example prompt for Full Mode:

```text
I have an idea for an app. Use idea-workflow in Full mode and help me turn it into a build-ready handoff.
```

Example prompt for Lite Mode:

```text
Use idea-workflow in Lite mode and just capture this idea.
```

## Relationship to Superpowers for GPT

Idea Workflow creates the source material. Superpowers for GPT is the recommended execution system after the spec is ready.

In the Full Mode path, Idea Workflow should produce a single agent build handoff. If the optional UI stage is skipped, that file is:

```text
03-agent-build-handoff.md
```

If the optional UI stage is used, the handoff becomes:

```text
04-agent-build-handoff.md
```

The user can then feed that file into Superpowers for GPT to plan, split, implement, test, review, and verify the actual build.

Recommended follow-up repository:

https://github.com/AkoliteZA/hermes-agent-supwerpowers-chatgpt

## Repository Layout

```text
idea-superpowers-suite/
  SKILL.md
  references/
    example-automation-script-build-handoff.md
    example-cli-tool-build-handoff.md
    example-saas-web-app-build-handoff.md
    interview-question-bank.md
  templates/
    idea-package-readme-template.md

idea-to-design-doc/
  SKILL.md

idea-to-ui-design-brief/
  SKILL.md

idea-to-implementation-doc/
  SKILL.md
  templates/
    agent-build-handoff-template.md
```

## Status

Status: v1 feature complete / beta - ready for real idea trials.

The workflow is structurally complete, but it still needs field testing against real ideas and real Hermes sessions.

Latest updates:

- v0.1.3 adds `idea-to-ui-design-brief`, an optional Full-mode UI stage for screen-level UI direction, optional image-generation concept prompts, post-build UI redesign loops, and updated artifact numbering when the UI stage is used.
- v0.1.2 / skill v1.1.0 adds recommend-then-confirm technical defaults across the workflow, including data location, platform targets, database/storage, hosting/deployment, app topology, auth/secrets, and stack recommendations for build-ready handoffs.
- `idea-superpowers-suite` v1.0.1 clarifies that the canonical agent handoff template lives at `idea-to-implementation-doc/templates/agent-build-handoff-template.md` and that the `idea-to-implementation-doc` skill should be loaded when creating `03-agent-build-handoff.md`.

## Privacy Note

The examples in this repository are generic public examples. They are intentionally not based on private product plans.
