# ssoo-recursos — context for Claude Code

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
index.html           resource index (GitHub Pages landing page)
<slug>/index.html    one self-contained resource per folder
<slug>/README.md     the resource's own documentation
.claude/skills/      skills for Claude Code
.agents/skills/      byte-equal mirror for Codex
```

## Skills

| Skill | When |
|---|---|
| `agent-workflow` | Always, first |
| `html-resource` | Creating or editing any resource page |
| `commit` | Making a commit or opening a branch |

That is the whole set. This repo stays small on purpose — do not add a skill for
something that happens once.

## Chassis and canvas

Resources in the same family share a **chassis**. They do not share a **canvas**.

- **Chassis — shared, and not up for discussion:** the header with the example
  ladder, the narration panel, the step counter, the control bar, the keyboard
  bindings, the type scale and the palette roles. A student who meets the second
  resource two weeks after the first has to recognise it, and recognition comes
  from these. Concretely, and **all-or-nothing** — if one step-by-step resource
  has one of these, every other one gets it too: the same control set (primero /
  anterior / siguiente / último, plus play/pause where autoplay exists), a tooltip
  on every panel title, a closing card at the end of every example carrying the one
  sentence to remember, the same keys (arrows, `Inicio`, `Fin`), one row in the
  header for whatever an example lets the student choose — a filter, a variant —
  and never a second one, and one favicon for the whole repo.
- **Canvas — derived, never inherited:** which panels are on screen, how much
  space each one gets, and what the unit of a step is. Every example answers
  three questions for itself: what is the unit of a step, what mutates that the
  student has to watch, and therefore which panels earn their space. A panel is
  on screen when it is the subject or when it changes. Panels may leave; when one
  leaves, the narration says so once, in one clause. Nothing stays on screen
  because it was introduced earlier.

**Why this is written down.** The second resource took its panel grid from the
first by instruction, and nobody checked it. It happened to fit the examples
whose step is a line of source and whose mutation is the memory map, and it was
wrong for the ones whose step is an event and whose mutation is who holds the
CPU. The tell was in the planning document itself: its panel list had no CPU
panel, while one example's beats required one. A list derived from the examples
cannot be missing a panel the examples need — so that list had been copied, not
reasoned.

Finding it late was cheap only because the step engine was already
canvas-agnostic: examples are data, snapshots are precomputed, and the renderer
takes its panel set from the example. Keep it that way. If a resource ever needs
a panel grid hardcoded in its render code, that is the moment to stop and re-read
this section.

## Hard rules

- **No comments in code.** Not banner comments, not explanatory comments, not
  `TODO`s, not commented-out code — in HTML, CSS, JS, or config files. If code
  needs a comment to be understood, rename or split it until it does not. Prose
  goes in a `README.md`.
- A resource is **one self-contained HTML file**: no framework, no build, no
  bundler, no npm dependency, no runtime network calls. It has to work over
  `file://`. **`localStorage` is allowed for one purpose and no other:**
  remembering what the student already answered, so that changing example — or
  reloading the page — does not throw their work away. It is a convenience, never
  a dependency: every read and write goes inside a `try`/`catch`, and the resource
  has to load and behave the same with storage empty, full, refused or missing,
  because over `file://` some browsers deny it outright. Nothing a resource needs
  in order to teach may live only in storage. No `sessionStorage`, no cookies.
- **English for code and process** — identifiers, file and folder names, skills,
  these context files, commit messages, and pull request titles and bodies.
  **Spanish (Argentine, voseante) for anything a student reads**, plus `README.md`
  files, which are descriptive material for the teaching staff. A pull request
  describes a change to whoever reviews it, like a commit message does, so it is
  process and goes in English — even though the reviewer is a member of the
  teaching staff.
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
