# Git Commit Message Style Skill

## Purpose
This skill defines a preferred style for writing git commit messages, focusing on clarity, conciseness, and active voice.

## Guidelines

- **Active Voice (Present Tense):**
  - Write all phrases in the commit message using active voice and present tense.

- **Concise Title:**
  - Keep the commit title under 82 characters.
  - Use a generic description; avoid mentioning specific method, variable, class, or code names.
  - Do not include punctuation (e.g., periods) at the end of the title.
  - Begin the title with a capital letter.

- **Bullet Point Body:**
  - Structure the commit body as bullet points.
  - Use 2–4 bullet points to describe the changes (more or fewer if necessary).
  - Each bullet point should be a clear, active phrase.
  - Do not include punctuation (e.g., periods) at the end of bullet points.
  - Begin each bullet point with a capital letter.

## Examples

### Title-only commit (for simple changes)
```
Change image path in deployment.yaml
```

### Commit with a body (2–3 bullets)
```
Fix duplicate document uploads to AI Search index

- When batch uploading is enabled, upload only the current batch of documents
- Prevent duplicate entries in the Azure Search index
- Ensure document count in index matches expected output
```

```
Refactor ingestion pipeline module CLI options

- Refactor ingestion pipeline module options when running as a script in the command line, and fix boolean options
- Add local folder to store PDF files in class init
- Update docstrings to specify default values
- Rename some variables to be more descriptive or readable
```

```
Add Ingress Gateway workflow for all environments

- Supports start/stop actions and multiple envs
- Uses shared codescanworkflows template
- Configures required secrets and inputs
```


### Commit with a long body (5+ bullets, for major refactors or restructuring)
```
Restructure project layout and remove deprecated components

- Moved backend Dockerfile and requirements.txt to the project root to support containerized deployment workflows, including AKS
- Renamed backend/ to app/ and relocated its contents to src/api/ for clearer separation of concerns and alignment with internal project conventions
- Updated script and module names to reflect their new roles within the src/api/ structure
- Relocated inference scripts to src/inference/ to maintain consistency across internal repositories
- Removed deprecated frontend components including Dockerfile, requirements, setup scripts, and Streamlit app
- Deleted legacy ingestion and inference pipelines that are no longer part of the deployment scope
- Renamed backend README and connector modules to src/api/ to match the new structure
```

## Usage
Use this skill to generate git commit messages that follow the outlined style and guidelines. The AI Agent is not responsible for creating the actual git commit. The user will review each generated message, and if satisfied, manually make the git commit in their repository. This skill is intended to support thoughtful, consistent commit history.
