---
description: Create an isolated git worktree in .tree/<name> and execute the given requirement there
argument-hint: <requirement to implement in the isolated worktree>
---

Requirement: $ARGUMENTS

If the requirement is empty, ask the user for it and stop.

## 1. Pick the name
Derive a short kebab-case name (2-4 words, ASCII, lowercase, no spaces) from the requirement, e.g. `add-sound-effects`. It must not collide with an existing branch or directory (`git branch --list <name>`, `ls .tree`); if it does, pick a different one.

## 2. Create the worktree
1. Check `git status` and the current branch; report anything unusual (dirty tree, drift) before continuing.
2. Make sure `.tree/` is not tracked: if `git check-ignore .tree` fails, append `.tree/` to `.git/info/exclude` (local only, never edit `.gitignore` without asking).
3. Run exactly: `git worktree add .tree/<name>`
   (git creates a branch `<name>` from the current HEAD).
4. Show `git worktree list` and the absolute path of the new worktree.

## 3. Execute the requirement in isolation
- Do ALL work inside the worktree's absolute path: read, edit, run and verify there. Never modify files in the main checkout.
- Follow the project's CLAUDE.md conventions (read it from the worktree).
- Work in short verifiable steps and verify with real output (e.g. serve the worktree dir statically and check it).
- Do NOT stage, commit, push or delete the worktree unless the user explicitly authorizes it in the current turn.

## 4. Report (in Spanish, short)
Worktree path, branch name, what changed (`git -C <path> status --short`), how it was verified, and how to continue: commit from the worktree, merge, or remove with `git worktree remove .tree/<name>`.
