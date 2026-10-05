---
name: commit-and-push
description: Make a focused, safe Git commit and push for the current development task. Use whenever the user asks to "commit e push", "commit and push", or otherwise requests publishing local changes. Avoid unrelated changes, secrets, force pushes, and noisy reporting.
---

# Commit and Push

Publish only the work that belongs to the current request. A request to commit and push authorizes a normal commit and non-force push of the relevant changes, but never authorizes rewriting history, deleting refs, changing remotes, or publishing credentials.

## Select the correct changes

Inspect the repository state, branch, relevant diff, staged diff, remotes, and upstream before staging. Infer the intended file set from the current task and changes made during it.

- Stage explicit, task-relevant paths. Do not use broad staging commands such as git add -A or git add . in a dirty worktree unless every changed file was intentionally produced by the current task.
- Treat unrelated modified, untracked, or staged files as user-owned. Leave them untouched.
- Do not narrate unrelated changes merely because they exist. Mention them only when they materially block a coherent commit/push, cause a conflict, make the requested scope unsafe to determine, or indicate a likely secret that is about to be published.
- If the task itself did not change files, say so and do not create an empty commit unless the user explicitly asks for one.

## Protect sensitive data

Before committing, inspect the names and diffs of the proposed staged files without exposing secret values in output.

- Never add or newly publish environment files, private keys, tokens, passwords, certificates, credential stores, or production configuration containing secrets.
- Distinguish safe templates such as .env.example from real local secret files such as .env.
- Respect existing ignore rules. If a sensitive file is already tracked or staged, stop before the commit, state only the file path and risk category, and ask for a safe remediation choice.
- Do not print, copy, redact manually, or place credentials in commit messages, command lines, logs, documentation, or remote URLs.

## Commit with context

- Review the staged diff and run the smallest relevant verification available for the touched area when proportionate to risk.
- Derive the commit message from the current task and the staged diff. Prefer the repository's established convention; otherwise use a concise conventional prefix when it accurately fits, such as feat, fix, docs, refactor, test, or chore.
- Make the subject specific enough to explain the user-visible or technical outcome. Avoid generic messages such as update, changes, work in progress, or temporary unless that is genuinely the intended outcome.
- Do not ask the user to write the message when the task and diff provide enough evidence. Ask only when the staged changes span unrelated outcomes or their intent cannot be determined safely.
- Do not amend, reset, restore, clean, stash, rebase, or alter Git configuration unless the user explicitly asks.
- If a commit hook or test fails, report the actual failure and do not bypass it with --no-verify unless the user explicitly authorizes that exception.

## Push safely

- Determine the current branch and its upstream; do not assume main.
- If the branch has no upstream but origin is configured and appropriate, use a normal first push with upstream tracking.
- Do not force-push, delete remote branches, switch remotes, or change remote URLs.
- If the remote is ahead, protected, unavailable, or rejects the push, stop and explain the exact blocker. Do not auto-merge, pull, rebase, or resolve conflicts without user direction when it could change history or combine others' work.

## Report only what matters

After a successful push, report the commit hash, commit message, branch, remote, verification run (if any), and that the push completed. Keep unrelated working-tree changes out of the handoff unless they prevented safe completion.
