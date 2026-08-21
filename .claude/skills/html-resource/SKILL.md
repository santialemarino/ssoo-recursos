---
name: html-resource
description: How an HTML resource in this repo is built — single file, no build step, accessible, legible on a projector. Use when creating or editing any resource page.
---

# HTML resources (ssoo-recursos)

Each resource is **one file** at `<slug>/index.html`, in a folder at the repo
root. It gets sent by link to students who open it alone, and it gets projected in class. Those two
facts outrank any technical preference.

## Hard constraints

Not preferences. A resource that breaks one of these is wrong.

- **One self-contained `index.html`.** HTML, CSS and JS in the same file.
- **Vanilla JS.** No framework, no build step, no bundler, no npm dependency.
- **No external resources**, with one exception: a `<link>` to a web font. With no
  network it must fall back to a system font stack and look just as good. No JS or
  CSS from a CDN.
- **No `localStorage`, `sessionStorage`, cookies or persistence of any kind.**
  State lives in memory for the session.
- **No network calls at runtime.**
- **Must work opened over `file://`** by double-clicking, with no server.

If something seems to need a tool, a build step or a library, the resource is
almost always overbuilt. Simplify the resource.

## No comments

**Code carries no comments.** Not banner comments, not explanatory comments, not
`TODO`s, not commented-out code — in HTML, CSS or JS.

The structure has to be legible without them:

- Split the file into three `<script>` elements, in this order:
  `<script id="data">`, `<script id="engine">`, `<script id="ui">`. The tag says
  what the block is.
- Name things so the name is the explanation. If a function needs a comment,
  rename it or split it until it does not.
- Anything worth asserting (expected final values, invariants) goes in the data
  block as a real field the code checks at load time, not as a comment nobody
  runs.

Prose explanation belongs in a `README.md`, where it is read on purpose.

## Internal organisation

1. **`data`** — all declarative content: text, examples, configuration.
2. **`engine`** — the machine or logic, if the resource has one.
3. **`ui`** — render and controls.

Rules that hold that separation up:

- **Identifiers in English.**
- **Every user-visible string is in Spanish and lives in the data block.** Not one
  visible string hardcoded in engine or render code.
- A member of the teaching staff must be able to change wording, or add a case, by
  touching **only** the data block.

## Accessibility and legibility

The floor is a mediocre projector in a large hall, and a phone on the bus.

- Semantic HTML: `<button>` for what gets clicked, `<table>` for tables, headings
  in order. No `<div onclick>`.
- **High contrast.** Body text well above 4.5:1. No light grey on white for
  anything that has to be read.
- **Generous type.** Body text no smaller than 16 px; anything projected and read
  from the back of the hall, considerably larger.
- **Never information by colour alone.** Colour supports; text, position or an
  icon must say the same thing.
- **Keyboard.** Everything actionable is reachable with Tab and fires on Enter or
  Space. `:focus-visible` always visible, never a bare `outline: none`.
- Text that changes step by step gets `aria-live="polite"`.
- `<html lang="es">`, `<meta name="viewport" content="width=device-width, initial-scale=1">`.

## Animation

Animation exists so the eye can follow a change, not to decorate.

- **Short:** 120–200 ms. Nothing that makes anyone wait — students move fast.
- **`transform` and `opacity` only.** Never animate `width`, `height`, `top` or
  `left`: they jank and they shift layout.
- Sober easing (`ease-out` for entering, `ease-in` for leaving). No bounce, no
  elastic.
- **Never block input.** If the user hits "next" twice quickly, the second press
  responds even if the first animation has not finished.
- Always honour:

  ```css
  @media (prefers-reduced-motion: reduce) {
    *, *::before, *::after {
      animation-duration: 0.01ms !important;
      transition-duration: 0.01ms !important;
    }
  }
  ```

- Highlighting what changed in the current step and dimming it on the next is
  worth more than any transition.

## Layout

- Projector floor: **1280×720 with no vertical scrolling** for step-by-step
  resources. Everything that has to be seen together is seen together.
- On a phone, the same content in one column in the same order. Not the primary
  experience, but it must be legible.
- CSS Grid or Flexbox. Relative units. The `body` never scrolls horizontally; wide
  content (tables, diagrams) scrolls inside its own `overflow-x: auto` container.

## Canvas: derive it, never inherit it

A resource in the same family as an existing one shares its **chassis** — the
example ladder in the header, the narration panel, the step counter, the control
bar, the keyboard bindings, the type scale, the palette roles. That is where
recognition comes from, and it is not up for discussion.

It does **not** share its **canvas**: which panels are on screen, how much space
each one gets, and what the unit of a step is. Those are derived per example, and
the derivation is three questions:

1. What is the unit of a step here — a line of source, or an event?
2. What mutates, that the student has to watch?
3. Given 1 and 2, which panels earn their space, and which are dead weight
   stealing room from what matters?

What follows from that:

- A panel is on screen when it is the subject of the example, or when it changes
  during the example. **Panels may leave.** When one leaves, the narration says so
  once, in one clause. Nothing is on screen because it was introduced earlier.
- Weight is part of the answer. The subject gets the space; context gets a strip.
- The unit of a step is declared per example and shown on screen. It never
  changes silently.
- **The vocabulary ceiling is a house rule, not a preference.** A resource never
  introduces a noun that a later class will redefine, even when the word is the
  standard one. The first resource holds this line strictly: it says *programa* 75
  times and never says *proceso*, *PCB*, *planificador*, *quantum*, *hilo* or
  *marco* — it defers with "eso es la clase que viene". When a name is needed and
  the honest name is taken, name the thing that is actually happening instead:
  *la llamada* and *su lugar en el stack* instead of *frame* or *marco*, *los bytes
  pedidos* instead of *bloque*. It usually reads more concretely anyway.

- **Set layout styles when creating the element, not by mutating an old one.** If a
  grid's column widths or a row's height depend on the step or the example, build
  the container fresh with that style already on it instead of reassigning the
  style of the container that was already there. It reads more simply, and it
  avoids a real case that cost hours to find: under some conditions the browser
  accepts the inline-style assignment but never recalculates layout, leaving the
  page laid out with the previous step's measurements.

- **The renderer takes its panel set from the example data, not from its own
  source.** A panel grid hardcoded in render code is what makes a canvas get
  inherited by accident.

If the answers come out identical to the sibling resource's, the inherited layout
was right — but say in the delivery that it was checked, so the next person does
not have to wonder.

## Visual style

**The course has no defined visual identity yet.** Until it does, the rule is
sober and default:

- Light background, one primary colour, and colour used consistently within the
  resource — each colour role always means the same thing.
- No gradients, no heavy shadows, no glassmorphism, no emoji in the UI.
- Contrast and type size matter more than any aesthetic decision.

When a course palette or typeface exists, document it here and unify the published
resources. Until then, if a new resource picks colours, say so explicitly when
delivering it, so nobody naturalises a decision no one made.

## Verification before delivering

1. Open over `file://` (double-click) **with the network off**. Works, looks right.
2. Browser console clean across the whole resource.
3. At 1280×720: everything fits, no vertical scrolling.
4. At 390 px wide: legible, one column, `body` does not scroll horizontally.
5. Full keyboard pass, focus always visible.
6. Nothing breaks with `prefers-reduced-motion` on.
7. If it has steps or states: a full pass forward and back leaves the initial state
   identical.
