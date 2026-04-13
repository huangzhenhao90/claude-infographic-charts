---
name: claude-svg-charts
description: >
  Generate visual charts, diagrams, and interactive widgets in the style of Claude's artifact system.
  Produces clean, flat, professional outputs — either standalone SVG files or self-contained HTML pages —
  with a 9-color ramp palette, proper typography, and light/dark mode support.
  Use this skill whenever the user asks to "draw a chart", "make a diagram", "visualize data",
  "generate a flowchart", "create an interactive explainer", "build a mockup", "make a dashboard",
  "draw an SVG", "make an HTML widget", or any request involving visual output.
  Also trigger for "Claude style chart", "artifact style diagram", "flat design", or any
  request to explain something visually. When in doubt, use this skill.
---

# Claude-style visual output generator

You produce clean, flat, professional visuals that replicate the aesthetic of Claude's interactive
artifact system. The output is either a standalone `.svg` file or a self-contained `.html` file —
the choice depends on what the user needs (see the format decision section below).

## Step 1 — Pick the module

Five output categories. Read the reference file for detailed guidance on whichever module applies:

| Module | Reference | When to use |
|--------|-----------|-------------|
| **diagram** | `references/diagram.md` | Flowcharts, architecture maps, structural diagrams, "how does X work" |
| **chart** | `references/chart-types.md` | Bar, line, donut/pie, scatter, heatmap, timeline, progress bar |
| **mockup** | `references/mockup.md` | UI cards, dashboards, metric displays, form layouts, product screens |
| **interactive** | `references/interactive.md` | Sliders, buttons, live calculations, step-through explainers |
| **art** | `references/art.md` | SVG illustration, geometric patterns, generative / decorative art |

**Routing by intent** (not by subject):

| User says | Module | Notes |
|---|---|---|
| "Walk me through the process / what are the steps" | diagram → flowchart | Sequential boxes + arrows |
| "What's the architecture / how is it organized" | diagram → structural | Nested containers |
| "How does X actually work / explain X" | diagram → illustrative | Spatial metaphor, not a flowchart |
| "Show me the data / compare these numbers" | chart | Pick the right chart type |
| "Design a dashboard / mockup a UI" | mockup | Looks like a real product |
| "Let me explore / what if I change..." | interactive | Controls + live result |
| "Draw / illustrate / create a pattern" | art | Expressive, rich, fills canvas |

When nothing fits clearly: explanatory content → editorial layout; bounded object → card layout.

---

## Step 2 — Choose the output format

This is the most important decision. Make it based on what the output actually needs.

### Use pure SVG when:
- The visual is **static** — no user interaction, no live calculation
- The user wants to **embed in a doc, slide, or web page** as an image
- Output type: flowchart, architecture diagram, bar/line/donut chart, structural diagram, illustration, art pattern
- No JavaScript needed
- Delivers as: `.svg` file

**SVG file shell:**
```svg
<?xml version="1.0" encoding="UTF-8"?>
<svg xmlns="http://www.w3.org/2000/svg" width="100%" viewBox="0 0 680 H"
     font-family="system-ui, -apple-system, 'Segoe UI', sans-serif">
  <style>
    /* only what you need */
  </style>
  <!-- content -->
</svg>
```

### Use HTML when:
- The visual needs **JavaScript** — sliders, buttons, toggles, live calculations
- It uses **Chart.js** (a JS library — cannot run inside SVG)
- It has **CSS animation** (convection, flowing arrows, pulsing elements)
- It's an **interactive mockup** with hover states, click handlers, or state
- It's a **stepper / step-through** explainer (prev/next buttons)
- Output type: interactive explainer, animated illustrative diagram, Chart.js chart, dashboard with controls
- Delivers as: `.html` file (fully self-contained, no external dependencies except CDN scripts)

**HTML file shell:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<style>
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
body {
  font-family: system-ui, -apple-system, 'Segoe UI', sans-serif;
  background: #ffffff;
  color: #2C2C2A;
  padding: 2rem;
  max-width: 720px;
  margin: 0 auto;
}
@media (prefers-color-scheme: dark) {
  body { background: #1a1a1a; color: #e0e0e0; }
}
</style>
</head>
<body>
  <!-- content: inline SVG, Chart.js canvas, controls, etc. -->
  <script>
    /* logic here, after DOM */
  </script>
</body>
</html>
```

### Format decision table

| Output contains | Format |
|---|---|
| Static chart (bar, line, donut, scatter...) | SVG |
| Static diagram (flowchart, architecture, structural) | SVG |
| Illustrative diagram, no animation | SVG |
| Art / illustration / geometric pattern | SVG |
| Chart.js chart | HTML |
| Any slider / range input | HTML |
| Any button that changes the visual | HTML |
| CSS animation (convection, flow, pulse) | HTML |
| Interactive mockup with state | HTML |
| Stepper / step-through explainer | HTML |
| Dashboard with metric cards + chart | HTML |

**When the user asks for "interactive" version of something**: always HTML, even if the base would be SVG.

---

## Design philosophy (applies to both formats)

Three words: **flat, clean, compact**.

- No gradients, drop shadows, blur, glow, or neon effects. Ever.
- No emoji. Use SVG shapes or CSS shapes instead.
- Sentence case on all labels. Never Title Case, never ALL CAPS.
- Generous whitespace — cramped charts are the most common failure mode.
- Two font weights only: 400 (regular) and 500 (medium). Never 600 or 700.
- Colors encode meaning, not sequence. Don't rainbow-cycle through categories.
- Every element must be readable on both white and near-black backgrounds.
- Background is always transparent for SVG; body bg is white/dark-auto for HTML.

---

## Color system (9 ramps, both formats)

| Ramp   | 50      | 100     | 200     | 400     | 600     | 800     | 900     |
|--------|---------|---------|---------|---------|---------|---------|---------|
| purple | #EEEDFE | #CECBF6 | #AFA9EC | #7F77DD | #534AB7 | #3C3489 | #26215C |
| teal   | #E1F5EE | #9FE1CB | #5DCAA5 | #1D9E75 | #0F6E56 | #085041 | #04342C |
| coral  | #FAECE7 | #F5C4B3 | #F0997B | #D85A30 | #993C1D | #712B13 | #4A1B0C |
| pink   | #FBEAF0 | #F4C0D1 | #ED93B1 | #D4537E | #993556 | #72243E | #4B1528 |
| gray   | #F1EFE8 | #D3D1C7 | #B4B2A9 | #888780 | #5F5E5A | #444441 | #2C2C2A |
| blue   | #E6F1FB | #B5D4F4 | #85B7EB | #378ADD | #185FA5 | #0C447C | #042C53 |
| green  | #EAF3DE | #C0DD97 | #97C459 | #639922 | #3B6D11 | #27500A | #173404 |
| amber  | #FAEEDA | #FAC775 | #EF9F27 | #BA7517 | #854F0B | #633806 | #412402 |
| red    | #FCEBEB | #F7C1C1 | #F09595 | #E24B4A | #A32D2D | #791F1F | #501313 |

**Assignment rules:**
- Use 2-3 ramps per visual. More = noise.
- Prefer **purple, teal, coral, pink** for general categories.
- Reserve **blue** for info, **green** for success/positive, **amber** for warning, **red** for error/negative.
- Use **gray** for neutral structural elements (axes, grid lines, borders).
- Text on colored fills: use 800 or 900 stop from the same ramp — never black.
- Title + subtitle on same bg must use different stops (800 title / 600 subtitle).

**Light/dark quick pick:**
- Light mode: 50 fill + 600 stroke + 800 title / 600 subtitle
- Dark mode: 800 fill + 200 stroke + 100 title / 200 subtitle

---

## SVG setup rules

```svg
<svg xmlns="http://www.w3.org/2000/svg" width="100%" viewBox="0 0 680 H"
     font-family="system-ui, -apple-system, 'Segoe UI', sans-serif">
```

- **ViewBox width is always 680.** Load-bearing — do not change it.
- **H** = bottommost element's (y + height) + 40px buffer. Calculate, don't guess.
- Safe area: x=40 to x=640, y=40 to y=(H-40).
- No DOCTYPE, no `<html>`, no `<head>` — raw `<svg>` only.
- No negative coordinates.

**Arrow marker** — include in `<defs>` for any diagram:
```svg
<defs>
  <marker id="arrow" viewBox="0 0 10 10" refX="8" refY="5"
          markerWidth="6" markerHeight="6" orient="auto-start-reverse">
    <path d="M2 1L8 5L2 9" fill="none" stroke="context-stroke"
          stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
  </marker>
</defs>
```

**Light/dark style block for SVG:**
```svg
<style>
  .text-primary   { fill: #2C2C2A; }
  .text-secondary { fill: #5F5E5A; }
  .text-tertiary  { fill: #888780; }
  .grid-line      { stroke: rgba(0,0,0,0.08); stroke-width: 0.5; }
  .connector      { stroke: #888780; stroke-width: 0.5; fill: none; }
  @media (prefers-color-scheme: dark) {
    .text-primary   { fill: #e0e0e0; }
    .text-secondary { fill: #a0a0a0; }
    .text-tertiary  { fill: #707070; }
    .grid-line      { stroke: rgba(255,255,255,0.08); }
    .connector      { stroke: #B4B2A9; }
  }
</style>
```

**SVG typography:**
- Diagram node title: 14px, weight 500
- Diagram node subtitle: 12px, weight 400
- Chart axis labels: 11-12px, weight 400
- `dominant-baseline="central"` on all text inside boxes
- `text-anchor="middle"` for centered text
- `<text>` never auto-wraps — each line needs `<tspan x="..." dy="1.2em">`

**Font width estimation** (system-ui):
- 14px weight 500: ~8px per character
- 14px weight 400: ~7.5px per character
- 12px weight 400: ~6.5px per character
- Box width check: `chars × px_per_char + 2 × padding` must fit

**Stroke width:** 0.5px for borders and diagram edges — thin strokes feel refined.

**Connector paths** always need `fill="none"` — SVG defaults to black fill.

**Rect rounding:** `rx="4"` default, `rx="8"` for emphasis, `rx` ≥ half height = pill.

No rotated text. No icons inside boxes (text only, except illustrative diagrams). No comments.

---

## HTML output rules

**Structure order:** `<style>` → content HTML → `<script>` last (scripts execute after streaming).

**CSS variables for HTML widgets:**
```css
/* text */
color: #2C2C2A;             /* primary */
color: #5F5E5A;             /* secondary */
color: #888780;             /* tertiary */
/* borders */
border: 0.5px solid rgba(0,0,0,0.15);    /* default */
border: 0.5px solid rgba(0,0,0,0.3);     /* hover/emphasis */
/* surfaces */
background: #ffffff;        /* primary card */
background: #F1EFE8;        /* secondary / metric card bg */
/* radius */
border-radius: 8px;         /* most elements */
border-radius: 12px;        /* cards */
```

Dark mode in HTML — use `@media (prefers-color-scheme: dark)`:
```css
@media (prefers-color-scheme: dark) {
  body { background: #1a1a1a; color: #e0e0e0; }
  .card { background: #2a2a2a; border-color: rgba(255,255,255,0.15); }
  .metric { background: #2a2a2a; }
  .text-secondary { color: #a0a0a0; }
  .grid-line { stroke: rgba(255,255,255,0.08); }
}
```

**Inline SVG inside HTML:** same viewBox="0 0 680 H" rules. No standalone `<svg>` file attributes needed.

**Chart.js pattern:**
```html
<div style="position: relative; width: 100%; height: 320px;">
  <canvas id="chart1"></canvas>
</div>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.js"
        onload="initChart()"></script>
<script>
function initChart() {
  new Chart(document.getElementById('chart1'), {
    type: 'bar',
    data: { labels: [...], datasets: [{ data: [...], backgroundColor: '#534AB7' }] },
    options: {
      responsive: true,
      maintainAspectRatio: false,
      plugins: { legend: { display: false } }
    }
  });
}
if (window.Chart) initChart();
</script>
```

Chart.js rules:
- Canvas cannot resolve CSS variables — use hardcoded hex only.
- Set height on wrapper div only, never on canvas element.
- Always disable default legend; build custom HTML legend with square swatches.
- CDN: `cdnjs.cloudflare.com` only. Other origins are blocked by sandbox CSP.
- Multiple charts: unique IDs (`chart1`, `chart2`...).

**Controls pattern (slider):**
```html
<div style="display:flex; align-items:center; gap:12px; margin-bottom:1.5rem;">
  <label style="font-size:14px; color:#5F5E5A; white-space:nowrap;">Label</label>
  <input type="range" min="0" max="100" value="50" step="1" id="sl" style="flex:1;"
         oninput="document.getElementById('sl-out').textContent=Math.round(this.value); update()">
  <span id="sl-out" style="font-size:14px; font-weight:500; min-width:32px;">50</span>
</div>
```

Always `step` to emit clean values. Always show live readout. Round every number before display.

**Metric card:**
```html
<div style="display:grid; grid-template-columns:repeat(3,1fr); gap:12px; margin-bottom:1.5rem;">
  <div style="background:#F1EFE8; border-radius:8px; padding:1rem;">
    <div style="font-size:13px; color:#5F5E5A;">Users</div>
    <div style="font-size:24px; font-weight:500;">12,847</div>
  </div>
</div>
```

**Raised card:**
```html
<div style="background:#fff; border:0.5px solid rgba(0,0,0,0.15);
            border-radius:12px; padding:1rem 1.25rem;">
  <!-- content -->
</div>
```

---

## Pre-output checklist

For **SVG**:
1. ViewBox H = max(y + height across all elements) + 40
2. All content within x=40..640
3. Box width check: chars × px_per_char + 2×padding fits
4. No arrows crossing unrelated boxes
5. Text on colored fill uses same-ramp 800/900
6. Title and subtitle use different color stops
7. Sentence case everywhere
8. No gradients, shadows, blur, decorative effects
9. All `<path>` connectors have `fill="none"`
10. `@media (prefers-color-scheme: dark)` in `<style>`

For **HTML**:
1. `<style>` block comes first, `<script>` last
2. All colors are hardcoded hex (no CSS vars in Chart.js data)
3. Numbers rounded before display (`Math.round`, `.toFixed`, `toLocaleString`)
4. Slider has `step` set and live readout
5. Chart.js uses `onload="initChart()"` + `if (window.Chart) initChart()` fallback
6. Dark mode `@media` block covers all surfaces and text
7. No `position: fixed` anywhere
8. No tabs or `display:none` content (stacked vertically only)
9. `font-family` explicitly set on body

---

## Quick examples

### Static bar chart → SVG

```svg
<?xml version="1.0" encoding="UTF-8"?>
<svg xmlns="http://www.w3.org/2000/svg" width="100%" viewBox="0 0 680 320"
     font-family="system-ui, -apple-system, 'Segoe UI', sans-serif">
  <style>
    .text-secondary { fill: #5F5E5A; }
    .grid-line { stroke: rgba(0,0,0,0.08); stroke-width: 0.5; }
    @media (prefers-color-scheme: dark) {
      .text-secondary { fill: #a0a0a0; }
      .grid-line { stroke: rgba(255,255,255,0.08); }
    }
  </style>
  <text x="80" y="26" font-size="16" font-weight="500" fill="#2C2C2A">Monthly signups</text>
  <line x1="80" y1="60"  x2="640" y2="60"  class="grid-line"/>
  <line x1="80" y1="110" x2="640" y2="110" class="grid-line"/>
  <line x1="80" y1="160" x2="640" y2="160" class="grid-line"/>
  <line x1="80" y1="210" x2="640" y2="210" class="grid-line"/>
  <line x1="80" y1="250" x2="640" y2="250" stroke="#B4B2A9" stroke-width="0.5"/>
  <text x="70" y="60"  text-anchor="end" dominant-baseline="central" class="text-secondary" font-size="11">400</text>
  <text x="70" y="110" text-anchor="end" dominant-baseline="central" class="text-secondary" font-size="11">300</text>
  <text x="70" y="160" text-anchor="end" dominant-baseline="central" class="text-secondary" font-size="11">200</text>
  <text x="70" y="210" text-anchor="end" dominant-baseline="central" class="text-secondary" font-size="11">100</text>
  <rect x="120" y="90"  width="48" height="160" rx="3" fill="#534AB7"/>
  <rect x="220" y="110" width="48" height="140" rx="3" fill="#534AB7"/>
  <rect x="320" y="70"  width="48" height="180" rx="3" fill="#534AB7"/>
  <rect x="420" y="130" width="48" height="120" rx="3" fill="#534AB7"/>
  <rect x="520" y="100" width="48" height="150" rx="3" fill="#534AB7"/>
  <text x="144" y="268" text-anchor="middle" class="text-secondary" font-size="12">Jan</text>
  <text x="244" y="268" text-anchor="middle" class="text-secondary" font-size="12">Feb</text>
  <text x="344" y="268" text-anchor="middle" class="text-secondary" font-size="12">Mar</text>
  <text x="444" y="268" text-anchor="middle" class="text-secondary" font-size="12">Apr</text>
  <text x="544" y="268" text-anchor="middle" class="text-secondary" font-size="12">May</text>
  <rect x="80" y="296" width="10" height="10" rx="2" fill="#534AB7"/>
  <text x="96" y="301" dominant-baseline="central" class="text-secondary" font-size="12">New users</text>
</svg>
```

### Interactive chart → HTML

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<style>
body { font-family: system-ui, sans-serif; padding: 2rem; max-width: 720px; margin: 0 auto; background: #fff; color: #2C2C2A; }
.ctrl { display:flex; align-items:center; gap:12px; margin-bottom:1.5rem; }
label { font-size:14px; color:#5F5E5A; white-space:nowrap; }
input[type=range] { flex:1; }
.val { font-size:14px; font-weight:500; min-width:40px; }
@media (prefers-color-scheme: dark) {
  body { background: #1a1a1a; color: #e0e0e0; }
  label { color: #a0a0a0; }
}
</style>
</head>
<body>
<div class="ctrl">
  <label>Year</label>
  <input type="range" min="1" max="30" value="10" step="1" id="yr"
         oninput="document.getElementById('yr-out').textContent=this.value; update()">
  <span class="val" id="yr-out">10</span>
</div>
<div style="display:flex; align-items:baseline; gap:8px; margin-bottom:1.5rem;">
  <span style="font-size:14px; color:#5F5E5A;">$1,000 grows to</span>
  <span style="font-size:28px; font-weight:500;" id="result">$2,594</span>
</div>
<div style="position:relative; width:100%; height:280px;">
  <canvas id="chart1"></canvas>
</div>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.js"
        onload="initChart()"></script>
<script>
const rate = 0.10;
let chart;
function compound(yrs) { return +(1000 * Math.pow(1 + rate, yrs)).toFixed(2); }
function initChart() {
  const labels = Array.from({length: 30}, (_, i) => i + 1);
  const data   = labels.map(compound);
  chart = new Chart(document.getElementById('chart1'), {
    type: 'line',
    data: { labels, datasets: [{ data, borderColor: '#534AB7', backgroundColor: 'rgba(83,74,183,0.08)', tension: 0.3, pointRadius: 0 }] },
    options: { responsive: true, maintainAspectRatio: false, plugins: { legend: { display: false } }, scales: { x: { ticks: { maxTicksLimit: 10 } } } }
  });
}
function update() {
  const yrs = parseInt(document.getElementById('yr').value);
  document.getElementById('result').textContent = '$' + compound(yrs).toLocaleString();
  if (chart) chart.update();
}
if (window.Chart) initChart();
</script>
</body>
</html>
```
