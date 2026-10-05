---
name: merge-request
description: Generate a concise English GitLab/GitHub merge request title and description from the current coding session, repository state, branch name, and implemented changes. Use when the user asks to create, write, generate, or prepare a merge request, pull request, MR, or PR title and description.
---

# Merge Request Generator

Generate a concise, professional GitLab/GitHub merge request title and description in English.

The goal is to communicate clearly what was implemented and mention the technical details that are important for reviewers, without creating an unnecessarily long description.

## Context discovery

Before generating the merge request, determine the available context.

Prefer information in this order:

1. Current coding session context.
2. Current repository and branch information.
3. Changes made during the current session.
4. Relevant current working tree or diff information.
5. Information explicitly provided by the user.
6. Previously established context that is directly relevant to the current task.

When running inside a coding agent such as Codex or Claude Code, use the available repository and session context whenever possible.

The user should NOT be required to provide the branch name or explain the changes if this information is already available from the current session or repository.

If the user simply asks:

> Create the merge request

or:

> Generate the MR

analyze the current work and generate the title and description using the available context.

## Understanding the changes

Analyze the implementation as a whole rather than simply listing modified files.

Use the current session context and repository state to understand the purpose and scope of the work.

The diff and modified files are sources of information, but they should NOT be treated as the primary structure of the merge request description. Do not generate a list of changes based solely on the files that were modified.

Identify:

- The main functionality added or changed.
- The problem or purpose addressed by the implementation.
- Important behavior changes.
- Relevant architectural changes.
- Important API changes.
- Database changes.
- Security-related changes.
- Important integrations.
- Configuration changes.
- Compatibility considerations.
- Relevant tests, if they were actually added or executed.

Focus on changes that are relevant to a reviewer.

Do not turn the description into a code walkthrough or a list of modified files.

## Ticket extraction

Extract the ticket identifier from the current branch name whenever possible.

Examples:

- `feature/KY-78-teams-email-notifications` → `KY-78`
- `bugfix/IVS-456-fix-token-validation` → `IVS-456`
- `hotfix/ABC-123-fix-certificate-validation` → `ABC-123`
- `KY-78-add-email-notifications` → `KY-78`

A ticket identifier normally follows the pattern:

`LETTERS-NUMBERS`

If the branch contains a clear ticket identifier, always use it.

If the user explicitly provides a ticket identifier, prefer the explicitly provided identifier when it conflicts with the branch name.

If no ticket identifier can be determined, do not invent one.

If a ticket identifier is required but cannot be determined from the available context, ask the user for it.

## Title

The title MUST follow this exact format:

`TICKET: Short description`

Examples:

- `KY-78: Add email notification support`
- `IVS-456: Fix IAM token validation`
- `ABC-123: Add certificate rotation support`

Rules:

- The ticket identifier must be first.
- Always use `:` followed by a space.
- The description must be in English.
- Keep it concise.
- Clearly describe the purpose of the change.
- Use natural English.
- Do not use prefixes such as `feat:`, `fix:`, `chore:`, or `[KY-78]`.
- Do not simply copy the branch name.
- Do not include unnecessary implementation details.
- Do not end the title with a period.

## Description

The description must be written in English and should be concise.

Use the following structure when it adds value:

## Summary

Briefly explain what was changed and, when useful, why.

## Key changes

- Mention the most relevant changes.
- Mention important technical details.
- Mention behavior changes that reviewers need to know about.

Do not force sections when they are unnecessary.

For a small change, keep the description very short.

For a larger change, include enough detail for a reviewer to understand the implementation without reading the entire diff.

## What to include

Include details that materially help a reviewer understand the change:

- New functionality.
- Changed behavior.
- Important architectural changes.
- Important integrations.
- Security-related changes.
- Database changes.
- API changes.
- Compatibility considerations.
- Important configuration changes.
- Relevant tests when they are known.

## What NOT to include

Keep the description concise.

Do not:

- List every modified file.
- Describe every class or method.
- Explain obvious implementation details.
- Repeat the title unnecessarily.
- Mention trivial refactoring.
- Mention formatting-only changes.
- Mention simple renames.
- Generate a detailed code walkthrough.
- Include implementation details that are irrelevant to reviewers.
- Invent tests or implementation details.
- Claim that tests passed unless this is known.
- Claim that tests were added unless they were actually added.
- Claim that a bug was fixed unless the available context supports that conclusion.

The description should normally be readable in less than one minute.

## Accuracy

Never invent information.

Base the merge request on the actual work performed and the available repository/session context.

If there is enough context to produce a reasonable merge request, do so without asking unnecessary questions.

Only ask the user for clarification when a missing piece of information prevents generating an accurate title or description.

## Output

Return exactly two sections:

### Title

The generated merge request title.

### Description

The generated merge request description.

The output must be ready to copy directly into GitLab or GitHub.

Do not explain how the merge request was generated.