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

## Adding New Skills

To add a new skill, create a dedicated directory at the repository root:

```text
my-skill/
└── SKILL.md
```

Then document the skill in this README.

Skills should:

- Have a clear and focused purpose.
- Be reusable across different repositories whenever possible.
- Make use of the agent's existing context instead of unnecessarily asking the developer for information it can already determine.
- Avoid duplicating platform-specific logic unless it is required.
- Include clear instructions for the AI agent.

## Purpose

The goal of this repository is to build a reusable collection of AI-assisted development workflows that can be shared across projects and used independently of the coding agent.

As new workflows are created, they can be added as additional skills without changing the overall repository structure.