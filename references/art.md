# Art and illustration module

SVG illustration, generative art, geometric patterns, and decorative visuals.

## When to use

The user wants a visual that's expressive rather than informational: "draw me a sunset", "create a geometric pattern", "make an abstract background", "design a logo concept".

## Aesthetic differences from diagrams

Art inverts many diagram rules. Where diagrams are sparse and functional, art is rich and expressive:

- **Fill the canvas** — art should feel rich, not sparse
- **Bold colors**: use mid-ramp stops (400-600) freely. Mix warm and cool.
- **Custom color blocks are fine** — art is the one place freestyle colors and `prefers-color-scheme` for dark mode variants are allowed
- **Layer overlapping opaque shapes for depth** — z-ordering is deliberate
- **Organic forms**: `<path>` curves, `<ellipse>`, `<circle>`
- **Texture via repetition**: parallel lines, dots, hatching — not raster effects
- **Geometric patterns**: `<g transform="rotate()">` for radial symmetry

## Technical rules (same as all SVG)

- ViewBox: `0 0 680 H`, width="100%"
- Background transparent
- No gradients except single `<linearGradient>` for physical properties
- No filters, no blur, no drop shadows
- No emoji
- Font sizes: 14px / 12px (if text is present at all)
- `@media (prefers-color-scheme: dark)` for custom colors

## Color approach for art

Unlike diagrams (where color encodes category), in art color is aesthetic:

- Use the full 9-ramp palette freely
- Mix warm and cool ramps for contrast
- Use opacity layering: shapes at 0.6-0.9 opacity over each other
- For physical scenes (sky, water, grass): use ALL hardcoded hex — never mix with theme classes. The scene should not invert in dark mode.

## Pattern techniques

**Radial symmetry**:
```svg
<g transform="translate(340, 300)">
  <g transform="rotate(0)"><circle cx="100" cy="0" r="20" fill="#534AB7" opacity="0.8"/></g>
  <g transform="rotate(60)"><circle cx="100" cy="0" r="20" fill="#1D9E75" opacity="0.8"/></g>
  <g transform="rotate(120)"><circle cx="100" cy="0" r="20" fill="#D85A30" opacity="0.8"/></g>
  <!-- ...repeat -->
</g>
```

**Organic layering**:
```svg
<ellipse cx="340" cy="400" rx="300" ry="80" fill="#E1F5EE" opacity="0.6"/>
<ellipse cx="300" cy="380" rx="250" ry="60" fill="#9FE1CB" opacity="0.7"/>
<ellipse cx="360" cy="360" rx="200" ry="40" fill="#5DCAA5" opacity="0.8"/>
```

**Line texture** (hatching):
```svg
<g stroke="#888780" stroke-width="0.5" opacity="0.3">
  <line x1="100" y1="100" x2="200" y2="200"/>
  <line x1="110" y1="100" x2="210" y2="200"/>
  <line x1="120" y1="100" x2="220" y2="200"/>
  <!-- ...repeat at regular intervals -->
</g>
```

## Composition tips

1. Start with the largest background shapes
2. Layer mid-ground elements with slight overlap
3. Add foreground details and small accents last
4. Use scale variation (large + small shapes together) for visual interest
5. Leave some breathing room — even art shouldn't be claustrophobic
6. Consider the aspect ratio: landscape scenes work best with wider viewBox height ratios, portraits with taller ones (but keep width at 680)
