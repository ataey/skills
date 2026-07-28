---
name: git-commit-style
description: Author git commit messages in the user's preferred Conventional Commits style with a bullet-list body. Use whenever proposing, drafting, or rewriting commit messages.
---

# Git Commit Style Guide

This skill defines the commit-message format the user expects from AI agents.
Apply it for every proposed commit on every branch.

## Format

```text
<type>(<scope>): <description>

- <body bullet 1>
- <body bullet 2>
- ...

[optional footer(s)]
```

Hard rules:

- Subject line MUST follow Conventional Commits 1.0.0: `<type>(<scope>): <desc>`.
- Subject MUST be lowercase, imperative ("add", "remove", "refactor"), no trailing period, ≤ 72 chars.
- Scope is REQUIRED unless the change is truly cross-cutting. Use the package, module, or feature area (e.g., `adapters`, `models`, `evals`, `evaluator`).
- Scope MUST be a single lowercase word or compound-word (no spaces, use camelCase or a single noun). Multi-word scopes are not allowed.
- One blank line between subject and body.
- Body MUST be a bullet list (hyphen + space). No prose paragraphs.
- Each bullet is a complete imperative-mood statement describing one logical aspect of the change.
- Each bullet MUST be a single line — no hard wrapping, no line breaks under the hyphen. Length is unrestricted; be as thorough as the change warrants.
- Bullets MUST NOT end with a period (or any other terminal punctuation).
- Use backticks around code symbols, file names, function names, env vars.
- Reference issues/PRs in footers (`Refs: #123`), not the body.
- Breaking changes: append `!` after scope (`feat(api)!: ...`) AND/OR add a `BREAKING CHANGE:` footer.

## Allowed Types

| Type | When to use |
|---|---|
| `feat` | New user-visible capability or public API addition |
| `fix` | Bug fix |
| `refactor` | Internal restructure with no behavior change |
| `perf` | Performance improvement |
| `test` | Adding or revising tests only |
| `docs` | Documentation only |
| `build` | Build system, packaging, dependencies |
| `ci` | CI/CD pipeline changes |
| `chore` | Tooling, config, housekeeping with no src impact |
| `style` | Formatting, whitespace, lint fixes with no logic change |

## Body Bullet Guidelines

- Lead with the *what*, follow with the *why* when not obvious.
- One concept per bullet. Split compound statements.
- Mention test coverage in its own bullet ("Add N unit tests covering …").
- Call out deferred or follow-on work in its own bullet so reviewers know what's intentionally out of scope.
- Do NOT restate the diff line-by-line; summarize at the behavior level.
- Do NOT reference internal decision IDs (e.g. "D20", "D24") or internal doc section numbers in commit messages.

## Examples

### feat with scope and bullet body

```text
feat(evals): add best-effort trace assessment attachment for eval outputs

- Add `attach_assessments_to_traces()` to map evaluator score columns onto trace-level assessments
- Extract assessment values from `result_df` rows using `*/value` columns with optional rationale/details context
- Support multiple client APIs by trying `log_feedback` and `log_assessment` first, then falling back to `set_trace_tag`
- Keep failures non-blocking with safe no-op behavior when traces or assessment APIs are unavailable
- Add unit coverage for missing-result no-op, missing-trace no-op, direct feedback logging, and tag-based fallback paths
```

### refactor with scope

```text
refactor(evals): split scorers by eval mode and reuse shared templates

- Replace single `ALL_SCORERS` export with mode-specific scorer sets (`ALL_SCORERS_ROW` and `ALL_SCORERS_TRACE`)
- Route single-turn eval runner to row-based scorers (`{{ inputs }}`) and simulator to trace-based scorers (`{{ conversation }}`)
- Introduce shared instruction templates with placeholder substitution to avoid duplication
- Add `_build_judges` factory to dynamically construct scorers for each evaluation mode
- Remove direct imports of prebuilt scorers and unused MLflow scorer types
- Improve maintainability by centralizing judge definitions and enabling consistent behavior across eval entrypoints
```

### feat (small surface, dataset only)

```text
feat(evals): add behavioral simulation test cases with scope tags

- Introduce `SIMULATION_TEST_CASES` to support behavior-driven conversation evaluations
- Add `SIMULATION_SCOPE_TAGS` for categorizing scenarios by scope, style, and workflow dimensions
- Expand dataset with diverse scenarios including out-of-scope handling, advice guardrails, tool workflows, and result interpretation
- Replace retirement-specific test cases with generalized simulation cases in the simulator
- Rename helper to `get_simulation_test_cases` and update all references
- Update descriptions and error messages to reflect generalized evaluation scope
```

### fix (single concern)

```text
fix(complaint_detection): handle tool-only turns without dropping the row

- Synthesize `[tool call initiated]` / `[tool:<name>]` markers when both userMessage and assistantText are empty
- Preserve tool invocations in the rendered transcript so the LLM can score them
- Add unit coverage for the tool-only synthesis path
```

### Breaking change

```text
feat(adapters)!: replace MongoRuntimeLoader with UCConversationLoader as default

- Wire `UCConversationLoader` as the default loader in `runner.py`
- Read source table from `VA_CONVERSATIONS_TABLE` env var (default `gwam_rtl_pi_dev_catalog.typed.va_conversations_t`)
- Move `pymongo` to the dev extra; production runs no longer require it

BREAKING CHANGE: `runner.py` no longer accepts `MONGODB_URI`; set `VA_CONVERSATIONS_TABLE` instead.
```

## Anti-Patterns to Avoid

- Title case or trailing period in the subject (`Feat(Evals): Add Thing.` ✗).
- Body written as a prose paragraph instead of bullets.
- Missing scope on a change that clearly belongs to one module.
- Vague subjects (`fix: bug`, `chore: updates`, `feat: improvements`).
- Bundling unrelated changes into one commit (split them).
- Mixing implementation and unrelated cleanup in the same commit.
- Mentioning internal decision IDs, doc section numbers, or planning-artifact references.

## Agent Workflow

When proposing a commit:

1. Stage the relevant files with `git add`.
2. Output the proposed message inside a fenced code block.
3. STOP and wait for user approval (see `hitl-ai-workflow` skill).
4. Do NOT run `git commit` yourself unless the user explicitly says so.
