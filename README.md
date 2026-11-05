# TECHVERSE — The Interactive Digital Hardware Laboratory

A single-file, offline interactive hardware laboratory built for a school coding competition.
Open `index.html` in any modern browser (double-click works — no server, no internet, no install).

**Everything is one file:** `index.html` (~530 KB, ~10,100 lines, one `<script>`, 11 `<canvas>` elements).

## The rule that shapes the whole project

Only HTML5, vanilla ES6 JavaScript and the Canvas API. **No CSS in any form** — no `<style>`
element, no `style=` attributes, no stylesheet link, no `@import`, and no `element.style` writes in
the script. There is no stylesheet in the file at all.

`class` attributes are present, but only as **JavaScript hooks** — the same job an `id` does. With
zero stylesheets in the document, no class name can match any rule, so no class can change a pixel.
Every visual is painted by Canvas: 233 class hooks, 0 CSS rules.

The HTML shell therefore keeps the browser's default appearance, and every visual idea is painted
inside a canvas: dark panels, grids, diagrams, meters, the header logo, even the progress bars.

## What is inside

| # | Section | What it does |
|---|---------|--------------|
| 1 | Computer Hardware Lab | CPU, RAM, storage, GPU, motherboard, PSU, cooling, display, keyboard/mouse — each with an explanation and a live Canvas demonstration |
| 2 | Smartphone Lab | SoC floor plan, RAM vs flash, Li-ion charge model, LCD vs OLED, camera pipeline with OIS, radios |
| 3 | Virtual Disassembly | Canvas-drawn PC and phone, click a plate to identify it, exploded/assembled slider |
| 4 | Data Journey | Seven-stage launch animation (PC and phone), play/pause/reset/speed, per-stage explanations |
| 5 | Experiment Lab (A–F) | RAM pressure, storage nearly full, CPU saturation, VRAM shortage, battery heat, app power drain |
| 6 | PC Builder | Socket, RAM generation, form factor and power compatibility checks with an assembly preview |
| 7 | Knowledge Centre | Searchable, filterable reference with comparison tables and misconceptions |
| 8 | Assessments | Component identification, troubleshooting, MCQ, three difficulties, instant feedback, score, achievements, `localStorage` progress |
| 9 | Docs | Architecture, function index, Canvas/animation notes, accuracy commitments, judge Q&A, 5–7 minute demo script |

## Honest labelling

Every simulation says it is a model. Frame times, temperatures, watts and page counts are
educational approximations with stated assumptions — never presented as measurements of the
student's own machine. Sources of the physics that are simplified are explained in
**Section 9 → Simulations & accuracy**.

## Engine notes

* One `CanvasStage` class owns each canvas and a plain-object *scene* with `draw(api, time)`.
* A single shared `requestAnimationFrame` loop drives all stages; stages pause when off-screen,
  and **Motion: Off** in the header stops all animation.
* Scenes are drawn in a fixed 960-unit design space, so scene code never needs mobile variants.
  Below 620 px of available width the canvas keeps a readable size and the page scrolls sideways
  instead of shrinking 12 px labels to 4 px.
* `Draw.fitLabel` / `Draw.wrapText` measure text and shrink or wrap it, so labels never overflow
  a panel.
* Mouse, touch (`pointerdown/move/up`) and keyboard (Enter/Space on a focused canvas) all work.

## Rebuilding

The deliverable is the committed `index.html`. The authoring sources live outside this repository
(`build/` + a compiler script) and are compiled into the single file; the shipped file has no build
step and no dependencies.

## Verification

`node verify.js` (in `validation/`) loads this exact file in jsdom with a real Canvas 2D backend and
runs **35 checks** covering every section, control, scene and assessment mode.

**Last result: 35/35 passed, no runtime errors**, plus 44 rendered screenshots at 1024 px and again
at 400 px wide. Measured on the shipped file:

| Audit item | Count |
|---|---|
| `<style>` elements / `style=` attributes / stylesheet links | 0 / 0 / 0 |
| `element.style` writes, `setProperty`, `cssText`, `getComputedStyle`, CSSOM | 0 |
| `@import`, `@media`, `@font-face` | 0 |
| External URLs, inline `on*` handlers, `javascript:` URLs | 0 |
| `<script>` tags | 1 |
| Canvas elements | 11 (15 with dynamically mounted stages) |
| `class` attributes (JS hooks only) | 233 |
