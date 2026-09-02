---
name: commit
description: Commit message format, staging rules, and when to open a PR in ssoo-recursos. Use when creating a commit or a branch.
---

# Commits (ssoo-recursos)

## Format

```
type: imperative description
```

- **Lowercase**, no trailing period.
- **Imperative mood:** "add", "fix", "reword" — not "added", "fixes".
- **One line.** No body unless a non-obvious decision needs explaining.
- **English**, like everything else that is not student-facing.

## Types

| Type | When |
|---|---|
| `feat` | New resource, or new functionality in an existing one |
| `fix` | Fixes wrong behaviour or wrong content |
| `enh` | Improves something that already worked |
| `copy` | Rewording of narration, titles, or any student-visible text |
| `docs` | README, skills, CLAUDE.md/AGENTS.md |
| `chore` | Config, gitignore, repo plumbing |
| `style` | Visual presentation or code formatting only |

`copy` exists because most maintenance in this repo is rewriting didactic text,
and those commits are worth filtering separately from behaviour changes.

## Examples

```
feat: add instruction cycle simulator with examples 1 to 4
copy: reword the fetch operands stage narration
fix: correct the SP shown when popping the PSW in example 4
enh: add tooltip explaining why the pushed IP is the next instruction
docs: document how to add a new example
chore: mirror skills into .agents
```

## Branches and PRs

This is a single-maintainer course repo: **work directly on `main`** for content,
text fixes, and new resources.

Open a branch and a PR only when the change deserves review before a student sees
it — a complete new resource, or a large rewrite of a published one. In that
case:

- Branch: `type/kebab-case-description`.
- PR title: the description in sentence case, no type prefix. **Title and body in
  English**, like the commits they describe. The repo's first PRs drifted between
  English and Spanish; a PR is process, not student-facing material, so it follows
  the commit rule and not the `README.md` exception.
- **Do not hard-wrap the PR body.** One line per paragraph and per list item, however
  long. GitHub renders a single newline inside a PR body as a line break, so a body
  wrapped at 80-ish columns — the way every file in this repo is written — breaks at
  each of those columns instead of flowing, and reads as if the text were shifted.
  This is the one place where the repo's wrapping habit is wrong.
- Body: which resource it touches, what the change teaches, and **specifically
  what to review**. If there is new student-facing text, list it so it can be
  reviewed line by line.

The personal plugin's `/pr-formatter` skill covers the general PR format. This
repo does not need its own.

## What not to stage

- Never `git add .` or `git add -A`. Always stage files individually by name.
- `.claude/plans/`, `.claude/settings.local.json`.
- Temporary or scratch `.md` files the user did not name.
