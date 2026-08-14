# ssoo-recursos — context for Codex

Resources for an operating systems course: self-contained HTML pages that get
sent to students by link and projected in class, plus whatever internal tooling
the teaching staff ends up needing. No build, no dependencies, no backend.

## Start here

Always read the **agent-workflow** skill first. It says which other skills to
load, how to verify a resource, and what not to commit.

## Codex compatibility

Codex reads `AGENTS.md` for repository instructions and `.agents/skills/` for
skills. Both skill trees and both context files are kept **byte-equal** (the only
permitted difference is the title on the first line of `CLAUDE.md` /
`AGENTS.md`). Change a convention in both places in the same pass.

## Layout

```
index.html                   resource index (GitHub Pages landing page)
resources/<slug>/index.html  one self-contained resource per folder
resources/<slug>/README.md   maintenance notes, only when a resource needs them
.claude/skills/              skills for Claude Code
.agents/skills/              byte-equal mirror for Codex
```

## Skills

| Skill | When |
|---|---|
| `agent-workflow` | Always, first |
| `html-resource` | Creating or editing any page under `resources/` |
| `commit` | Making a commit or opening a branch |

That is the whole set. This repo stays small on purpose — do not add a skill for
something that happens once.

## Hard rules

- **No comments in code.** Not banner comments, not explanatory comments, not
  `TODO`s, not commented-out code — in HTML, CSS, JS, or config files. If code
  needs a comment to be understood, rename or split it until it does not. Prose
  goes in a `README.md`.
- A resource is **one self-contained HTML file**: no framework, no build, no
  bundler, no npm dependency, no runtime network calls, no `localStorage` or
  persistence of any kind. It has to work over `file://`.
- **English for code and process** — identifiers, file and folder names, skills,
  these context files, commit messages. **Spanish (Argentine, voseante) for
  anything a student reads**, plus `README.md` files, which are descriptive
  material for the teaching staff.
- Every user-visible string lives in the resource's data block, never hardcoded
  in engine or render code.
- Student-facing text is reviewed by a member of the teaching staff before it
  ships. If you drafted new text, say so when you deliver.
- Never `git add .` or `git add -A`: stage files individually by name.
- Never stage `.claude/plans/`, `.claude/settings.local.json`, or temporary `.md`
  files the user did not name.
- Verify manually before calling anything done: `file://`, clean console,
  1280×720 and 390 px, and a full forward-and-back pass if the resource has
  steps.

## Known open decisions

- **No visual identity defined.** There is no course palette or typeface. If a new
  resource picks colours, say so explicitly when delivering it.
- **No licence.** Deliberately absent; this is internal course material.
