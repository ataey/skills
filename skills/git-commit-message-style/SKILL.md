---
name: git-commit-message-style
description: Write clear, consistent git commit messages following a defined commit message style. Trigger this skill whenever the user requests a git commit message and provides a diff of the changes to be committed.
---

# Git Commit Message Style

## Purpose
Define a consistent, high‑clarity style for generating git commit messages. This skill ensures commit messages remain concise, readable, and helpful for future maintainers.

## General Principles
- Prefer clarity over cleverness.
- Focus on *what* changes and *why* they matter.
- Avoid leaking code specifics (e.g., method names, variable names, filenames) unless explicitly required.

## Commit Title (Header)
- Use **active voice** in **present tense**.
- Keep the title **under 82 characters**.
- Start the title with a **capital letter**.
- Use a **generic description** of the change.
- **Do not** end the title with punctuation.
- **Do not** include explicit code identifiers (class names, method names, etc.).

### Examples
- “Improve handling of invalid input”
- “Add support for optional configuration values”
- “Refactor logic for better readability”

## Commit Body (Bullet Points)
- Use **bullet points** only (no paragraphs).
- Each bullet is a **concise, active phrase**.
- Begin each bullet with a **capital letter**.
- Do **not** include punctuation at the end of bullets.
- Include **2–4 bullets** when appropriate (flexible).
- Describe:
  - What changed
  - Why it changed
  - Any notable implications or side effects

## Additional Notes
- Avoid referencing external issue numbers unless explicitly requested.
- Avoid imperative verbs like “Fixing” or “Fixed”; use present tense (“Fix”, “Improve”, “Add”).
- If diff contains noisy changes (formatting, renamed files), summarize at a high level.

## Examples

### Title-only commit (for simple changes)
```
Change image path in deployment.yaml
```

### Commit with a body (2–3 bullets)
```
Fix duplicate document uploads to AI Search index

- Upload only the current batch of documents when batch uploading is enabled
- Prevent duplicate entries in the Azure Search index
- Ensure document count in index matches expected output
```

```
Refactor ingestion pipeline module CLI options

- Refactor ingestion pipeline module options when running as a script in the command line and fix boolean options
- Add local folder to store PDF files during class initialization
- Update docstrings to specify default values
- Rename variables to improve readability
```

```
Add Ingress Gateway workflow for all environments

- Support start/stop actions across multiple environments
- Use shared codescanworkflows template
- Configure required secrets and inputs
```


### Commit with a long body (5+ bullets, for major refactors or restructuring)
```
Restructure project layout and remove deprecated components

- Move backend Dockerfile and requirements.txt to the project root to support containerized deployment workflows including AKS
- Rename backend/ to app/ and relocate its contents to src/api/ for clearer separation of concerns and alignment with internal project conventions
- Update script and module names to reflect their new roles within the src/api/ structure
- Relocate inference scripts to src/inference/ to maintain consistency across internal repositories
- Remove deprecated frontend components including Dockerfile, requirements, setup scripts, and Streamlit app
- Delete legacy ingestion and inference pipelines that are no longer part of the deployment scope
- Rename backend README and connector modules to src/api/ to match the new structure
```

## Usage
Use this skill to generate git commit messages that follow the outlined style and guidelines. The AI Agent is not responsible for creating the actual git commit. The user will review each generated message, and if satisfied, manually make the git commit in their repository. This skill is intended to support thoughtful, consistent commit history.
