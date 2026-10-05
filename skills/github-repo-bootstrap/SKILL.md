---
name: github-repo-bootstrap
description: Connect a local project to a newly created GitHub repository and complete a safe first push. Use when the user supplies a GitHub repository URL and wants the required Git setup, remote configuration, initial commit, and push verified. Do not use for ordinary changes in an already-published repository.
---

# GitHub Repository Bootstrap

Use the supplied GitHub repository URL to publish the current local project safely. The normal target is a newly created, empty GitHub repository. Do not assume that it is empty, that the current branch is named main, that Git identity/authentication is configured, or that the existing remote can be replaced.

## Scope and authorization

- Work in the project the user identified, or the current repository when that is unambiguous.
- Confirm the URL is a GitHub remote URL before mutating Git configuration. Accept standard HTTPS or SSH GitHub URLs; do not expose embedded credentials in output.
- A request to bootstrap and push authorizes the initial commit and push for that named repository, but not force-pushes, overwrite of an existing remote, deletion of refs, or publishing secrets.
- Preserve all existing local commits and remotes unless the user explicitly chooses a different migration.

## Preflight

Inspect before changing anything:

1. Current directory and whether it is already a Git worktree.
2. Current branch, status, recent commits, remotes, and remote URLs.
3. The supplied repository's remote heads with a read-only check such as git ls-remote.
4. Authentication readiness when relevant (for example, gh auth status if GitHub CLI is installed, or a read-only remote check).

Review files that would be included in an initial commit. Do not stage or push likely credentials, private keys, local environment files, or generated build artifacts that should remain local. Respect an existing .gitignore. If sensitive files or ambiguous secrets are found, stop and ask the user how to handle them; do not auto-commit or print their contents.

## Bootstrap an untracked local directory

If the directory is not yet a Git repository:

1. Initialize it with the intended initial branch, normally git init -b main.
2. Inspect git status before staging.
3. Stage the intended project files only after the secret check.
4. Create a concise initial commit.

If Git reports missing user.name or user.email, ask the user to configure their real identity or authorize the exact configuration. Never invent identity values.

## Configure the GitHub remote

- If origin is absent, add the supplied URL as origin.
- If origin exists and already matches the supplied URL, retain it.
- If origin exists but differs, do not run git remote set-url automatically. Explain the mismatch and ask whether to use another remote name or replace origin.
- Verify with git remote -v after configuration.

Use the active local branch rather than assuming main. For a first publication, git push -u origin HEAD is usually safer than hardcoding a branch name.

## Handle remote state safely

- If the GitHub repository has no heads, push the current local branch with upstream tracking.
- If the remote already has commits or a default README, license, or gitignore, do not force-push and do not automatically use --allow-unrelated-histories.
- Explain the divergence and offer safe choices: pull/rebase or merge after inspecting history, push to a new branch, or have the user explicitly authorize a carefully scoped replacement. Continue only after their choice.
- Never use --force, --force-with-lease, deletion refspecs, or history rewrites for this workflow unless the user explicitly requests it after being shown the target and consequence.

## Verify completion

After the push, verify:

- origin points to the supplied GitHub repository;
- the current branch has an upstream;
- the remote contains the pushed branch and expected HEAD commit;
- git status is clean or clearly report any intentional remaining local changes.

Report the exact remote URL, branch, commit, and verification result. If authentication, branch protection, repository permissions, or push protection blocks the push, state the precise blocking message and the shortest user action needed. Do not claim a repository was published without confirming the remote received the branch.
