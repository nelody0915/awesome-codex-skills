---
name: repo-summarizer
description: Generate a concise, structured summary of any repository. Use when a user asks to summarize a repo, understand a codebase, or get an overview of a project's purpose, structure, tech stack, and key components.
metadata:
  short-description: Summarize a repository
---

# Repo Summarizer

Produce a clear, structured overview of a repository so developers can quickly understand its purpose, architecture, tech stack, and how to get started — without reading every file.

## When to Use

- "Summarize this repository"
- "Give me an overview of this codebase"
- "What does this project do?"
- "Explain the structure of this repo"
- "I just cloned this repo — what do I need to know?"

## Workflow

Operate in **read-only** mode throughout. Do not write or modify files.

### 1. Collect high-level context

Read these files if present (stop after finding the first match for each category):

| Category | Files to check |
|---|---|
| Project description | `README.md`, `README.rst`, `README.txt` |
| Package metadata | `package.json`, `pyproject.toml`, `Cargo.toml`, `go.mod`, `pom.xml`, `build.gradle`, `*.gemspec` |
| Dependency list | `requirements.txt`, `Pipfile`, `yarn.lock` / `package-lock.json` (name only, not full lock) |
| Entry points | `main.*`, `index.*`, `app.*`, `server.*`, `cmd/`, `bin/` |
| Configuration | `.env.example`, `docker-compose.yml`, `Dockerfile`, `*.config.*` |
| CI/CD | `.github/workflows/`, `.gitlab-ci.yml`, `Makefile`, `Justfile` |
| Architecture docs | `docs/`, `ARCHITECTURE.md`, `CONTRIBUTING.md`, `DESIGN.md` |

### 2. Scan the directory tree

Run a shallow directory listing (depth ≤ 3) to understand top-level layout:

```bash
find . -maxdepth 3 \
  -not -path '*/.git/*' \
  -not -path '*/node_modules/*' \
  -not -path '*/__pycache__/*' \
  -not -path '*/dist/*' \
  -not -path '*/build/*' \
  -not -path '*/.next/*' \
  | sort
```

### 3. Identify tech stack

From the files collected in step 1, determine:

- **Primary language(s)** — file extensions, package metadata
- **Frameworks / libraries** — dependencies in package manifests
- **Runtime / platform** — Node.js, Python, Go, JVM, Docker, serverless, etc.
- **Databases / storage** — look for ORM config, connection strings in `.env.example`, or dependency names
- **Testing tools** — test runner referenced in `scripts` / `Makefile`
- **CI/CD platform** — GitHub Actions, GitLab CI, CircleCI, etc.

### 4. Identify key components

Skim `src/`, `lib/`, `app/`, or equivalent top-level source folders. Map each significant subdirectory or module to a one-line purpose. Limit to the 5–10 most important components.

### 5. Find entry points and commands

From package metadata and CI config, extract:
- How to **install** dependencies
- How to **run** the project locally
- How to **run tests**
- How to **build** / **deploy**

### 6. Produce the summary

Output the following template — fill every section; omit a section only if genuinely not applicable:

```markdown
# <Project Name> — Repository Summary

## Purpose
<1–3 sentences: what this project does, who it is for, and the core problem it solves.>

## Tech Stack
| Layer | Technology |
|---|---|
| Language | … |
| Framework | … |
| Database | … |
| Testing | … |
| CI/CD | … |
| Deployment | … |

## Repository Structure
```
<top-level directory tree, annotated with a short comment per folder>
```

## Key Components
- **`<path>`** — <one-line purpose>
- **`<path>`** — <one-line purpose>
…

## Entry Points
- **Install**: `<command>`
- **Run locally**: `<command>`
- **Run tests**: `<command>`
- **Build**: `<command>`

## Architecture Overview
<2–4 sentences describing how the pieces fit together: data flow, request lifecycle, or module boundaries. If an `ARCHITECTURE.md` or docs folder exists, reference it.>

## Getting Started
<3–5 numbered steps a new contributor needs to run the project locally, extracted from README / Makefile / CI config.>

## Notes
<Any important caveats: required environment variables, external services, known limitations, or anything that would surprise a new contributor.>
```

## Output guidelines

- **Be concise**: prefer bullets and tables over prose paragraphs.
- **Be accurate**: copy commands verbatim from source files; do not invent commands.
- **Be honest**: if a section cannot be determined from the files, write "Not found in repo."
- **No hallucination**: every claim must be traceable to a file you read.
- Do not include the word-for-word file content; synthesize and summarize.
