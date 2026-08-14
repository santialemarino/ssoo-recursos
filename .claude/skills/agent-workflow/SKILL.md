---
name: agent-workflow
description: Orchestrator for agents working in ssoo-recursos. Read this first — it says which other skills to load, how to verify a resource, and what not to commit.
---

# Agent workflow (ssoo-recursos)

Resources for an operating systems course: self-contained HTML pages that get
sent to students by link and projected in class, plus whatever internal tooling
the teaching staff ends up needing.

## 1. Which skill to load

| You are about to | Load |
|---|---|
| Create or edit an HTML resource | **html-resource** |
| Make a commit | **commit** |

That is the whole set. This repo stays small on purpose — do not add a skill for
something that happens once.

## 2. Repo layout

```
index.html            resource index (GitHub Pages landing page)
<slug>/index.html     one self-contained resource per folder
<slug>/README.md      the resource's own documentation
.claude/skills/       skills for Claude Code
.agents/skills/       byte-equal mirror for Codex
CLAUDE.md / AGENTS.md repo context, byte-equal mirror except the title line
```

Resource folders sit at the repo root, one per resource, so the published URL is
`.../ssoo-recursos/<slug>/`. A new one is added by hand to the list in
`index.html` and to the table in `README.md`.

## 3. Language rule

Two languages, and the split is not negotiable:

- **English** — everything read as code or process: identifiers, file and folder
  names, skills, `CLAUDE.md`, `AGENTS.md`, commit messages.
- **Spanish (Argentine, voseante)** — everything a *student* reads: every
  user-visible string in a resource. Also `README.md` files, which are
  descriptive material for the teaching staff.

Every user-visible Spanish string lives in the resource's data block. Never
hardcode one inside engine or render code.

## 4. No comments

**Code in this repo carries no comments.** Not banner comments, not explanatory
comments, not `TODO`s, not commented-out code — in HTML, CSS, JS, or config
files like `.gitignore`.

The two things comments would normally carry here have comment-free homes:

- **Section structure** — a single-file resource splits its data, engine and UI
  into separate `<script>` elements with `id="data"`, `id="engine"`, `id="ui"`.
  The tag says what the block is.
- **Expected results** — anything worth asserting (a program's expected final
  values, for instance) goes in the data block as a real field the code checks at
  load time, not as a comment nobody runs.

If a piece of code needs a comment to be understood, rename the thing or split it
until it does not. Prose explanation belongs in a `README.md` or in these skills,
where it is read on purpose.

## 5. Verification before calling anything done

There is no build and no test runner. Verification is manual and not optional:

1. Open the file over `file://` (double-click), no server. It has to work.
2. Browser console clean, across the whole resource.
3. Check at 1280×720 (the lecture-hall projector) and at 390 px wide (a phone).
4. If the resource has steps or states, walk it fully forward and fully backward.

## 6. Docs

- `README.md` is in Spanish and written for teaching staff, not developers. Add a
  row when you add a resource.
- Every resource has its own `<slug>/README.md` next to its `index.html`: what it
  teaches, how to change a text, what must not be touched. That is where the
  resource is documented — the root `README.md` only carries one line per resource.
- `CLAUDE.md` / `AGENTS.md` describe how to work in the repo. They change when a
  convention changes, not when content is added.
- Student-facing text is always reviewed by a member of the teaching staff before
  it ships. If you drafted new text, say so explicitly when you deliver.

## 7. Mirrors: `.claude/skills/` ↔ `.agents/skills/`

The two trees are kept **byte-equal**. Claude reads the first, Codex the second.
Any difference that is not a transient mid-edit state is a bug.

- When **editing** an existing skill, change both copies in the same pass and do
  not `cp` one over the other: make the same edit in both, then verify with
  `diff -r .claude/skills .agents/skills`. When **creating** a new skill, copying
  the freshly written file into the other tree is fine — there is nothing to
  clobber.
- Do not introduce agent-specific references that would not resolve on the other
  side (one agent's memory-link syntax, names of gitignored files only one agent
  has). Write the rule inline in both copies instead.
- When in doubt, mirror it.

The same applies to `CLAUDE.md` and `AGENTS.md`: same content, and the only
permitted difference is the title on the first line.

## 8. What not to commit

- Never `git add .` or `git add -A`. Always stage files individually by name.
- Never `.claude/plans/` or `.claude/settings.local.json`.
- Never temporary or scratch `.md` files unless the user names them.

## 9. Habits

- A resource is **one** file. No build, no bundler, no dependencies, no
  `node_modules`. If something seems to need a tool, the resource is almost
  always overbuilt.
- Before adding a feature to an existing resource, ask whether the student
  opening it alone at 2 a.m. will understand it with nobody there to explain it.
  If not, it does not go in.
