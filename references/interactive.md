# Interactive module

Interactive explainers with controls — sliders, buttons, live state displays, charts.

## When to use

The user wants to *explore* a concept by manipulating variables. The interactive format turns a static explanation into a tool for building intuition.

Good for: compound interest calculators, sorting algorithm visualizers, physics simulations, statistical distributions, conversion tools.

## Format: HTML with inline SVG

Use HTML for the controls and layout. Use inline SVG (embedded directly in the HTML) for the visual. This gives you the best of both worlds: proper form controls and precise vector graphics.

```html
<div style="display: flex; align-items: center; gap: 12px; margin: 0 0 1.5rem;">
  <label style="font-size: 14px; color: #5F5E5A;">Parameter</label>
  <input type="range" min="1" max="100" value="50" id="param"
         style="flex: 1; -webkit-appearance: none; height: 4px;
                background: rgba(0,0,0,0.15); border-radius: 2px;" />
  <span style="font-size: 14px; font-weight: 500; min-width: 36px;"
        id="param-out">50</span>
</div>

<svg width="100%" viewBox="0 0 680 300"
     font-family="system-ui, -apple-system, sans-serif">
  <!-- Visualization updates via JS -->
</svg>

<script>
  const slider = document.getElementById('param');
  const output = document.getElementById('param-out');
  slider.oninput = () => {
    output.textContent = Math.round(slider.value);
    updateVisualization(slider.value);
  };
</script>
```

## Control patterns

### Range slider
```html
<div style="display: flex; align-items: center; gap: 12px;">
  <label style="font-size: 14px; color: #5F5E5A;">Label</label>
  <input type="range" min="0" max="100" value="50" step="1"
         style="flex: 1;" />
  <span style="font-size: 14px; font-weight: 500; min-width: 36px;">50</span>
</div>
```
Always set `step` to emit round values. Always show the current value.

### Toggle
Use a checkbox styled as a toggle track:
- Track: 32px wide, 18px tall, rounded
- Thumb: 14px circle, white, slides on check

### Button group
For mode selection or step controls:
```html
<div style="display: flex; gap: 4px;">
  <button style="background: transparent; border: 0.5px solid rgba(0,0,0,0.3);
                 border-radius: 8px; padding: 6px 14px; font-size: 14px;
                 cursor: pointer;">Option A</button>
  <button style="...">Option B</button>
</div>
```

### Big result number
For calculators and converters:
```html
<div style="display: flex; align-items: baseline; gap: 8px; margin: 0 0 1.5rem;">
  <span style="font-size: 14px; color: #5F5E5A;">Input →</span>
  <span style="font-size: 24px; font-weight: 500;" id="result">$3,870</span>
</div>
```

## Animation rules (for illustrative interactives)

- Use CSS `@keyframes` animating only `transform` and `opacity`.
- Keep loops under ~2s.
- Wrap every animation in `@media (prefers-reduced-motion: no-preference)`.
- Animations should show how the system *behaves* — convection, rotation, flow — not just move for aesthetics.
- No physics engines or heavy libraries.

## Stepper pattern (for cycles)

When the topic is cyclic (Krebs cycle, event loop, GC mark-and-sweep):
- One panel per stage
- Dots or pills showing position: ● ○ ○
- Next button wraps from last stage back to first — that's the loop
- Each panel owns its inputs and products

```html
<div id="stepper">
  <div style="display: flex; justify-content: center; gap: 6px; margin-bottom: 16px;">
    <span class="dot active" style="width: 8px; height: 8px; border-radius: 50%;
          background: #534AB7;"></span>
    <span class="dot" style="width: 8px; height: 8px; border-radius: 50%;
          background: #D3D1C7;"></span>
    <span class="dot" style="..."></span>
  </div>
  <div id="stage-content">
    <!-- Stage content here, swapped by JS -->
  </div>
  <div style="display: flex; justify-content: space-between; margin-top: 16px;">
    <button onclick="prevStage()">Previous</button>
    <button onclick="nextStage()">Next</button>
  </div>
</div>
```

## Chart.js integration

For interactive charts with data that updates based on controls:

```html
<div style="position: relative; width: 100%; height: 300px;">
  <canvas id="myChart"></canvas>
</div>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.js"
        onload="initChart()"></script>
<script>
function initChart() {
  const ctx = document.getElementById('myChart');
  window.chart = new Chart(ctx, {
    type: 'line',
    data: { labels: [...], datasets: [{ data: [...] }] },
    options: { responsive: true, maintainAspectRatio: false }
  });
}
if (window.Chart) initChart();
</script>
```

Key Chart.js rules:
- Canvas cannot resolve CSS variables — use hardcoded hex.
- Set height ONLY on the wrapper div, never on canvas.
- Use `responsive: true, maintainAspectRatio: false`.
- Always disable default legend and build custom HTML legend.
- Use `onload` on the CDN script tag for reliable initialization.
