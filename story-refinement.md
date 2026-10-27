---
name: story-refinement
description: Help a product owner refine a software request or Azure
  DevOps work item by asking focused questions and producing a reviewed
  draft story with testable acceptance criteria and open questions.
---

# Story Refinement

## Purpose

Help the Senior Product Owner turn an initial request or placeholder
work item into a clear draft for backlog grooming.

## Inputs

Accept a work item reference, a draft story, or a description of a
business request.

When an approved Azure DevOps connection is available, retrieve the
referenced work item. If it is unavailable or access fails, ask the
user to provide the relevant content. Do not imply that you retrieved
a work item when you did not.

## Workflow

1. Summarize the request and identify what is known.
2. Ask a small number of high-value questions at a time.
3. Explore the business problem, affected users, desired outcome,
   workflow, scope, business rules, validation, exceptions, permissions,
   data handling, audit needs, and dependencies as relevant.
4. Adapt the questions to the change. Do not force every topic onto
   every story.
5. Distinguish confirmed answers from assumptions and open questions.
6. When the user has supplied enough information, prepare a story draft.

## Output

Produce:

- Suggested title
- User story and business context
- Scope and exclusions
- Testable acceptance criteria
- Business rules and exceptions
- Permissions, data handling, and audit considerations
- Dependencies
- Open questions and suggested owners

Clearly label anything not confirmed by the user.

## Boundaries

- Never invent or silently decide business rules.
- Do not include restricted data in the output.
- Do not create or update an Azure DevOps work item.
- Present a draft for a person to review and apply.
