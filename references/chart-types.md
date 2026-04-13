# Chart type selection guide

Pick the chart type based on what the user is trying to communicate.

## Data comparison → Bar chart

**When:** Comparing discrete categories, rankings, before/after.

**Vertical bar** (default):
- 4-12 categories. More than 12 → group or paginate.
- Bar width: 24-40px. Gap: 8-12px.
- Rounded top corners (rx=4 on top only via `<path>`).
- Y-axis: 4-6 grid lines, labeled at left.
- X-axis labels below, centered under bars.

**Grouped bar** (2-3 series):
- Bars in a group touch (0-2px gap). Groups separated by 12-20px.
- Each series uses a different ramp's 400-stop.
- Legend above chart showing series colors.

**Stacked bar:**
- Each stack segment is a different ramp's 400-stop.
- Segments stack bottom to top.
- Total value label above each stack (optional).

**Horizontal bar** (when labels are long):
- Swap axes. Labels at left, bars extend right.
- Good for rankings, survey responses, long category names.
- Minimum height: (bars * 40) + 80px.

## Trends over time → Line chart

**When:** Showing change over time, trajectories, comparisons of trends.

- X-axis: time. Y-axis: metric.
- Line: 2px stroke, 400-stop color.
- Data points: 4px radius circles, white center (2px stroke, matching line color).
- Area fill (optional): path closing to baseline, filled with 50-stop at 15% opacity.
- Multiple series: up to 3 lines. More → simplify. Each line a different ramp.
- Grid: horizontal lines only, 0.5px, low opacity.

**Sparkline** (minimal, inline):
- No axes, no labels, no grid. Just the line.
- ViewBox: `0 0 200 50` (small, inline).
- Line: 1.5px stroke.

## Part of whole → Pie / donut chart

**When:** Showing proportions, market share, composition.

- Prefer donut (inner radius ~60% of outer). Cleaner and allows center label.
- Max 5-6 slices. Group remaining into "Other" (gray).
- Use 400-stop of different ramps for slices.
- Labels: outside with thin leader lines (0.5px, gray). Include percentage.
- Center of donut: large primary number (24px, weight 500) + description below (12px).
- Start at 12 o'clock, go clockwise, largest slice first.

## Relationships → Scatter / bubble

**When:** Correlation, clustering, distribution across two dimensions.

- Points: 4-6px radius circles, 400-stop fill at 70% opacity.
- Bubble: radius encodes a third variable. 6-30px range.
- Both axes labeled with grid lines.
- Pad axis range 10% beyond data to prevent clipping.

## Process / workflow → Flowchart

**When:** Steps in sequence, decision points, data flow.

Read the detailed flowchart guidance in the main SKILL.md.

Key points:
- Max 4-5 nodes per diagram. More → split into multiple diagrams.
- Single direction flow (top-down or left-right).
- 60px minimum spacing between nodes.
- Arrows must not cross unrelated boxes. Use L-bend paths if needed.

## Architecture / hierarchy → Structural diagram

**When:** Containment, nesting, "what's inside what".

- Large outer containers with inner regions.
- Color ramps encode semantic grouping.
- Max 2-3 nesting levels.

## Progress / metrics → Gauge / progress bar

**When:** Single metric with a target or max value.

**Progress bar:**
```svg
<!-- Track -->
<rect x="80" y="150" width="520" height="8" rx="4" fill="#F1EFE8"/>
<!-- Fill -->
<rect x="80" y="150" width="364" height="8" rx="4" fill="#1D9E75"/>
<!-- Label -->
<text x="80" y="140" font-size="12" class="text-secondary">70% complete</text>
```

**Gauge / semicircle:**
- Use SVG arc paths.
- Track: gray-100. Fill: ramp-400.
- Center number: large (24px), weight 500.

## Comparison matrix → Heatmap

**When:** Two categorical axes with a numeric value at each intersection.

- Grid of `<rect>` elements.
- Color intensity from 50-stop (low) to 600-stop (high), single ramp.
- Row and column labels at left and top.
- Cell labels: value in center, 12px, colored for contrast.

## Timeline → Horizontal or vertical timeline

**When:** Events in chronological order.

- Vertical line or horizontal line as the spine.
- Event nodes: small circles (6px) on the spine.
- Labels branch left/right (or above/below) alternating.
- Date/time labels in text-secondary, event titles in text-primary.

## Decision help

If still unsure:

| User wants to show...          | Use           |
|-------------------------------|---------------|
| "How much" for each category  | Bar chart     |
| "How it changed over time"    | Line chart    |
| "What fraction of the total"  | Donut chart   |
| "How things are related"      | Scatter       |
| "The steps in the process"    | Flowchart     |
| "What's inside what"          | Structural    |
| "How far along"               | Progress bar  |
| "Pattern across two axes"     | Heatmap       |
| "When things happened"        | Timeline      |
