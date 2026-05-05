---
name: repo-summarizer
description: Produce a concise, structured summary of any code repository — covering purpose, tech stack, architecture, key entry points, and how to get started. Use when asked to summarize, explain, overview, or document a codebase, repository, or project (e.g. "summarize this repo", "what does this project do", "give me an overview of the codebase", "document this project").
---

# Repo Summarizer

Generate a clear, structured summary of a repository that a new contributor or stakeholder can read in minutes.

## Workflow

1. **Collect context** — read in order of priority:
   - `README.md` / `README.rst` (or any top-level readme)
   - Root-level manifest: `package.json`, `pyproject.toml`, `Cargo.toml`, `go.mod`, `pom.xml`, `build.gradle`, etc.
   - CI config: `.github/workflows/`, `.circleci/`, `Makefile`, `Taskfile.yml`
   - Directory listing (top 2 levels) to understand layout

2. **Identify the essentials:**
   - **Purpose** — one-sentence description of what the project does and who it is for
   - **Tech stack** — language(s), primary frameworks, key dependencies
   - **Architecture** — major modules/packages and how they relate (briefly; no deep-dive)
   - **Entry points** — main binary / CLI / API surface / web server startup file
   - **Data flow** — high-level: input → processing → output (if applicable)
   - **How to run** — install, build, and run commands extracted from README or CI

3. **Produce the summary** using this structure:

   ```markdown
   ## Summary: <Project Name>

   **Purpose:** <one sentence>

   **Tech stack:** <language(s)> · <framework(s)> · <key libraries>

   ### Architecture
   <3–6 bullet points covering major components and their relationships>

   ### Entry points
   - `<file or command>` — <what it does>

   ### Getting started
   ```bash
   <install command>
   <run command>
   ```

   ### Key files & directories
   | Path | Role |
   |------|------|
   | `src/` | … |
   ```

4. **Quality checks:**
   - No invented details — flag anything uncertain as `(unclear — check <file>)`.
   - Keep the summary under 400 words unless the repo is very large.
   - If the repo is a monorepo, produce one top-level summary then a short sub-summary per package/service.

## Tips

- If no README exists, scan the top-level source files and CI scripts for clues.
- For large repos, focus on directories with the most recent git activity.
- Offer to expand any section (e.g. detailed architecture diagram, API surface) when asked.
