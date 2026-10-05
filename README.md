# AI Development Skills

A collection of reusable skills for AI coding agents.

This repository contains development skills that can be used with both **Claude Code** and **OpenAI Codex**. The goal is to centralize reusable workflows and make them available across different AI-assisted development environments.

## Available Skills

### Merge Request

The `merge-request` skill helps create a merge request from the current development session.

The skill is designed to make use of the context already available to the AI agent, including:

- The current Git branch.
- The changes made during the current session.
- The repository state.
- Relevant commits and Git history.
- The work completed during the development session.

It can generate an appropriate merge request title and description based on the actual work performed, reducing the need to manually provide information that is already available to the agent.

The skill is available in:

```text
merge-request/
└── SKILL.md
```

## Repository Structure

```text
.
├── README.md
└── merge-request/
    ├── .gitkeep
    └── SKILL.md
```

Each skill is contained in its own directory and includes a `SKILL.md` file with the instructions required by the AI coding agent.

## Claude Code & Codex

The skills in this repository are intended to be compatible with both **Claude Code** and **OpenAI Codex**.

The repository does not separate skills by platform. Instead, each skill is maintained as a standalone workflow that can be installed and used in either environment when supported.

## Global Installation

Skills can be installed globally so they are available across all repositories and projects for both Claude Code and OpenAI Codex.

A convenient approach on Windows is to keep a single local clone of this repository and create directory junctions from each agent's global skills directory to the skill directories in the repository.

This avoids maintaining separate copies of the same skill.

### 1. Clone the repository

Clone this repository to a permanent location on the local machine:

```powershell
git clone <repository-url> "$env:USERPROFILE\dev\ai-skills"
```

The repository can then be updated whenever a skill changes:

```powershell
git -C "$env:USERPROFILE\dev\ai-skills" pull
```

### 2. Create the global skills directories

Create the global skills directories for both agents:

```powershell
New-Item -ItemType Directory -Force "$env:USERPROFILE\.codex\skills" | Out-Null
New-Item -ItemType Directory -Force "$env:USERPROFILE\.claude\skills" | Out-Null
```

### 3. Create junctions for the skills

Create a junction for each skill that should be available globally.

For the `merge-request` skill:

```powershell
New-Item `
    -ItemType Junction `
    -Path "$env:USERPROFILE\.codex\skills\merge-request" `
    -Target "$env:USERPROFILE\dev\ai-skills\merge-request"
```

```powershell
New-Item `
    -ItemType Junction `
    -Path "$env:USERPROFILE\.claude\skills\merge-request" `
    -Target "$env:USERPROFILE\dev\ai-skills\merge-request"
```

The resulting structure will be:

```text
%USERPROFILE%
├── dev/
│   └── ai-skills/
│       └── merge-request/
│           └── SKILL.md
│
├── .codex/
│   └── skills/
│       └── merge-request/
│           └── SKILL.md  -> junction
│
└── .claude/
    └── skills/
        └── merge-request/
            └── SKILL.md  -> junction
```

Both agents therefore use the same local copy of the skill.

### Updating Skills

Because both global skill directories point to the repository using junctions, updating the repository automatically updates the version used by both agents.

After changes are pushed to the repository:

```powershell
git -C "$env:USERPROFILE\dev\ai-skills" pull
```

No additional installation or copying is required.

If a new skill is added to the repository, create a corresponding junction in both global skill directories.

For example:

```powershell
New-Item `
    -ItemType Junction `
    -Path "$env:USERPROFILE\.codex\skills\my-skill" `
    -Target "$env:USERPROFILE\dev\ai-skills\my-skill"
```

```powershell
New-Item `
    -ItemType Junction `
    -Path "$env:USERPROFILE\.claude\skills\my-skill" `
    -Target "$env:USERPROFILE\dev\ai-skills\my-skill"
```

### Why Use Junctions?

Using junctions provides a single source of truth for the skills:

```text
                    Git repository
                          │
                          ▼
                  local ai-skills/
                          │
              ┌───────────┴───────────┐
              ▼                       ▼
       Claude Code                OpenAI Codex
       global skills              global skills
              │                       │
              └────── same files ─────┘
```

This approach:

- Avoids duplicated skill files.
- Keeps Claude Code and Codex synchronized.
- Allows skills to be updated with a normal `git pull`.
- Makes version control straightforward.
- Keeps platform-specific installation details outside the skill itself.

> **Note:** The junction approach described above is intended for Windows. The equivalent setup on other operating systems can use symbolic links.

## Adding New Skills

To add a new skill, create a dedicated directory at the repository root:

```text
my-skill/
└── SKILL.md
```

Then document the skill in this README.

After adding the skill, create a junction for it in both global skill directories if it should be available globally.

Skills should:

- Have a clear and focused purpose.
- Be reusable across different repositories whenever possible.
- Make use of the agent's existing context instead of unnecessarily asking the developer for information it can already determine.
- Avoid duplicating platform-specific logic unless it is required.
- Include clear instructions for the AI agent.

## Purpose

The goal of this repository is to build a reusable collection of AI-assisted development workflows that can be shared across projects and used independently of the coding agent.

As new workflows are created, they can be added as additional skills without changing the overall repository structure.