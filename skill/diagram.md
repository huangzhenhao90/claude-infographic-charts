# Diagram module

SVG flowcharts, structural diagrams, and illustrative (intuition) diagrams.

## Diagram type routing

The decision is about *intent*, not subject matter. Ask: is the user trying to *document* this, or *understand* it?

### Reference diagrams — for documentation

The user wants a map they can point at. Precision matters more than feeling.

**Flowchart** — steps in sequence, decisions branching, data transforming.
- Good for: approval workflows, request lifecycles, build pipelines, "what happens when I click submit".
- Trigger phrases: "walk me through the process", "what are the steps", "what's the flow".

**Structural diagram** — things inside other things, containment and hierarchy.
- Good for: file systems, VPC/subnet/instance, "what's inside a cell", system architecture.
- Trigger phrases: "what's the architecture", "how is this organized", "where does X live".

### Intuition diagrams — for understanding

The user wants to *feel* how something works. The goal is the right mental model, not a correct map.

**Illustrative diagram** — draw the mechanism with spatial metaphors.
- Physical things get cross-sections (water heaters, engines, lungs).
- Abstract things get spatial metaphors: an LLM is a stack of layers with tokens, gradient descent is a ball rolling down a loss surface, a hash table is a row of buckets.
- Trigger phrases: "how does X actually work", "explain X", "I don't get X", "give me an intuition for X".
- This is the default for "how does X work" with no further qualification. Don't chicken out into a flowchart because it feels safer.

### Route on the verb, not the noun

Same subject, different diagram depending on what was asked:

| User says                        | Type           | What to draw                                              |
|----------------------------------|----------------|----------------------------------------------------------|
| "how do LLMs work"              | Illustrative   | Token row, stacked layer slabs, attention threads         |
| "transformer architecture"       | Structural     | Labelled boxes: embedding, attention heads, FFN           |
| "how does attention work"        | Illustrative   | One query token, fan of lines to every key                |
| "how does gradient descent work" | Illustrative   | Contour surface, a ball, a trail of steps                 |
| "what are the training steps"    | Flowchart      | Forward → loss → backward → update                       |
| "how does TCP work"              | Illustrative   | Two endpoints, numbered packets in flight                 |
| "TCP handshake sequence"         | Flowchart      | SYN → SYN-ACK → ACK. Three boxes.                       |

Don't mix families in one diagram. If you need both, draw the intuition version first, then the reference version as a second SVG.

## Flowchart rules

**Planning**: Size boxes to fit their text generously. At 14px sans-serif, each char is ~8px wide.

**Spacing**: 60px minimum between boxes, 24px padding inside boxes, 12px between text and edges. Two-line boxes need at least 56px height.

**Vertical text placement**: Every `<text>` inside a box needs `dominant-baseline="central"`, with y set to the centre of its slot.

**Layout**: Prefer single-direction flows (all top-down or all left-right). Max 4-5 nodes per diagram.

**Over-budget prompts**: If the user lists 6+ components, decompose: (1) stripped overview with boxes only; (2) one diagram per interesting sub-flow, each with 3-4 nodes.

**Cycles don't get drawn as rings.** Use a linear layout with a return arrow, or use a stepper (HTML with next/prev buttons, wrapping from last to first).

**Arrows**: A line from A to B must not cross any other box or label. If the direct path crosses something, route around with an L-bend: `<path d="M x1 y1 L x1 ymid L x2 ymid L x2 y2" fill="none"/>`.

**Flowchart components**:

Single-line node (44px tall):
```svg
<g>
  <rect x="100" y="20" width="180" height="44" rx="8"
        fill="#EEEDFE" stroke="#534AB7" stroke-width="0.5"/>
  <text x="190" y="42" text-anchor="middle" dominant-baseline="central"
        font-size="14" font-weight="500" fill="#3C3489">Node title</text>
</g>
```

Two-line node (56px tall):
```svg
<g>
  <rect x="100" y="20" width="200" height="56" rx="8"
        fill="#EEEDFE" stroke="#534AB7" stroke-width="0.5"/>
  <text x="200" y="38" text-anchor="middle" dominant-baseline="central"
        font-size="14" font-weight="500" fill="#3C3489">Node title</text>
  <text x="200" y="56" text-anchor="middle" dominant-baseline="central"
        font-size="12" fill="#534AB7">Short subtitle</text>
</g>
```

Connector:
```svg
<line x1="200" y1="76" x2="200" y2="120" stroke="#888780" stroke-width="0.5"
      fill="none" marker-end="url(#arrow)"/>
```

Keep all nodes the same height when they have the same content type (all single-line = 44px, all two-line = 56px).

## Structural diagram rules

For concepts where physical or logical containment matters.

**Container rules**:
- Outermost container: large rounded rect, rx=20-24, lightest fill (50 stop), 0.5px stroke (600 stop). Label at top-left inside, 14px bold.
- Inner regions: medium rounded rects, rx=8-12, next shade fill (100-200 stop). Use a different color ramp if the region is semantically different.
- 20px minimum padding inside every container.
- Max 2-3 nesting levels.

**Layout**:
- Place inner regions side by side within the container, 16px+ gap.
- External inputs sit outside the container with arrows pointing in.
- Keep external labels short — one word or short phrase.

**Color in structural diagrams**: Nested regions need distinct ramps. Same class on parent and child gives identical fills and flattens the hierarchy. Pick a related ramp for inner structures (e.g., green envelope + teal circulation desk), contrasting ramp for functionally different regions (e.g., amber reading room).

## Illustrative diagram rules

For building *intuition*. The most ambitious and expressive type.

**Two flavours**:
- Physical subjects: simplified cross-sections, cutaways, schematics. A water heater = tall rect with a burner underneath.
- Abstract subjects: spatial metaphors. A transformer = horizontal slabs with attention threads. A hash function = funnel scattering into buckets.

**What changes from flowchart rules**:
- Shapes are freeform: `<path>`, `<ellipse>`, `<circle>`, `<polygon>`, curved lines.
- Layout follows the subject's geometry, not a grid.
- Color encodes intensity, not category. Warm = active/hot/high-weight, cool/gray = dormant/cold/low-weight.
- Layering and overlap are encouraged for shapes (not for text).
- Small shape-based indicators are allowed: triangles for flames, circles for bubbles, wavy lines for steam.
- One gradient per diagram is permitted — only to show a continuous physical property (temperature, pressure). Single `<linearGradient>`, two stops from same ramp.

**Fidelity ceiling**: These are schematics, not illustrations. Every shape should read at a glance. If a `<path>` needs more than ~6 segments, simplify. Recognisable silhouette beats accurate contour.

**Label placement**:
- Place labels outside the drawn object with thin leader lines (0.5px dashed).
- Pick one side for labels and put them all there.
- Reserve at least 140px of horizontal margin on the label side.
- Default to right-side labels with `text-anchor="start"`.

**Composition approach**:
1. Main object silhouette, centered in viewBox.
2. Internal structure: chambers, pipes, mechanical parts.
3. External connections: pipes, arrows, input/output labels.
4. State indicators last: color fills, animation elements.
5. Leave generous whitespace around the object for labels.

## Common mistakes to avoid

1. ViewBox too small → content clipped
2. Arrows through unrelated boxes → use L-bend detours
3. Labels on arrow lines → place in clear space
4. Text past viewBox edges → check text-anchor direction
5. Box too narrow for text → compute: chars × 8 + 2 × padding
6. Overlapping boxes in same row → compute total width before placing
7. Connector paths without `fill="none"` → renders as black shape
