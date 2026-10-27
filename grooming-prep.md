---
name: grooming-preparation
description: Review a draft Azure DevOps story against the team's
  Definition of Ready and prepare prioritized questions and a concise
  briefing for backlog grooming.
---

# Grooming Preparation

## Purpose

Help the Senior Product Owner and development manager prepare a story
for the weekly backlog grooming meeting.

## Inputs

Accept a work item reference or a draft story. Use the team's approved
Definition of Ready and story template when available.

When an approved Azure DevOps connection is available, retrieve the
referenced work item. If it is unavailable or access fails, ask the
user to provide the relevant content.

## Workflow

1. Summarize the business outcome, users, and proposed scope.
2. Review the story against the team's Definition of Ready.
3. Identify ambiguous requirements and acceptance criteria that are
   not observable or testable.
4. Identify relevant alternate paths, validation, errors,
   permissions, data handling, audit needs, and dependencies.
5. Separate business questions from technical questions.
6. Prioritize questions that could change scope or prevent the team
   from planning the work.
7. Recommend whether the story is ready for grooming or needs
   preparation first. Explain the recommendation.

## Output

Provide:

- Short story summary
- Confirmed information
- Prioritized questions for grooming
- Questions for a developer or technical lead
- Draft improvements to acceptance criteria
- Open decisions and suggested owners
- Readiness recommendation and reasons

## Boundaries

- Do not resolve business decisions or invent missing requirements.
- Label suggested acceptance criteria as drafts.
- Do not create or update an Azure DevOps work item.
- A person makes the final readiness and prioritization decisions.
