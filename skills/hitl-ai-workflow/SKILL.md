---
name: hitl-ai-workflow
description: The user's human-in-the-loop (HITL) workflow for AI coding agents. Use this on every multi-step coding task — the agent MUST pause at every commit boundary and wait for explicit approval before proceeding.
---

# HITL AI Workflow Guide

The user works in a strict human-in-the-loop mode with AI agents. The agent
is a proposer; the user is the approver. This applies on every branch.

## Core Rule

**One logical change → one staged commit → one proposed commit message → STOP.**

The agent NEVER runs `git commit` autonomously. The user runs the commit
themselves after reviewing the proposal.

## Per-Commit Loop

For each logical change the agent intends to make:

1. **Plan** — state in 1–3 sentences what the change is and why it stands alone as a commit.
2. **Implement** — make exactly that change; nothing more, nothing less. Do not opportunistically refactor adjacent code.
3. **Validate** — run the smallest relevant test/lint command and confirm it passes. Report the result inline.
4. **Stage** — `git add` only the files belonging to this logical change.
5. **Propose the commit message** — emit it in a fenced code block, formatted per the `conventional-git-commit-message-style` skill.
6. **STOP** — end the turn. Do not start the next change. Do not run `git commit`. Wait for the user's explicit approval (e.g. "approved", "go ahead", "I committed it, proceed").
7. **Only on approval, proceed** — move to the next logical change and repeat from step 1.

If the user requests revisions, revise in place, re-stage, re-propose, and STOP again.

## What Counts as "One Logical Change"

A single commit should:

- Touch one feature area or one cohesive refactor.
- Be revertable in isolation without leaving the codebase broken.
- Be reviewable in a single sitting.
- Pass tests on its own (no "this will work once the next commit lands").

If a planned change requires several files spanning unrelated concerns,
split it into a Rolling Commit Queue (numbered list of upcoming commits)
and execute one item at a time.

## Hard Prohibitions

The agent MUST NOT:

- Run `git commit`, `git push`, `git rebase`, `git reset --hard`, `git merge`, or `git checkout <branch>` without explicit per-action approval.
- Bundle multiple logical changes into one commit "for convenience".
- Skip the proposal step and announce "I'll commit this now".
- Continue to the next planned change after staging — even if the next change feels small or obviously correct.
- Add files outside the scope of the current logical change to the staging area.
- Use `--no-verify`, `--force`, or any flag that bypasses safety checks.
- Reference internal decision IDs (e.g. "D20"), doc section numbers, or planning artifacts in code, docstrings, or commit messages.
- Delete or move files that may be the user's in-progress work.

## What the Agent SHOULD Do

- Maintain or consult a Rolling Commit Queue when work spans multiple commits.
- Re-validate (run tests) after every revision, not only after the first draft.
- Confirm staged state with `git status --short` before proposing the message, and include the relevant portion in the response.
- Use the proposed message format from the `conventional-git-commit-message-style` skill verbatim.
- Clearly mark deferred or follow-on work as out of scope for the current commit.
- Ask before introducing a new dependency, file, or top-level abstraction.
- Ask before deviating from the queue or the plan.

## Branch Discipline

- Stay on the branch the user designated for the feature.
- Do not create new branches without approval.
- Do not switch branches mid-task.
- All commits for a feature land on its designated branch in the order specified by the Rolling Commit Queue.

## Response Template at Each Pause

When proposing a commit, the agent's final turn before stopping should contain:

1. A one-line summary of what changed.
2. Test/validation result (e.g. "73/73 unit tests pass").
3. The `git status --short` output for the staged files.
4. The proposed commit message in a fenced code block.
5. The explicit hand-off line: *"Please review and commit when satisfied."* — then end the turn.

## Handling User Corrections

If the user undoes a change or signals scope creep:

- Acknowledge briefly.
- Do NOT re-apply the reverted change.
- Reset the planned scope to what the user explicitly approved.
- Resume from the most recent approved state — never from the agent's last in-flight state.
