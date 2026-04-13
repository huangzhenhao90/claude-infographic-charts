# Mockup module

UI mockups, forms, cards, dashboards. HTML-based output that looks like a real product interface.

## Aesthetic

Flat, clean, white surfaces. Minimal 0.5px borders. Generous whitespace. No gradients, no shadows (except functional focus rings). Everything should feel native — like it belongs on the page, not embedded from somewhere else.

## Design tokens

- Borders: always `0.5px solid rgba(0,0,0,0.15)` (light mode) or `rgba(255,255,255,0.15)` (dark mode)
- Corner radius: 8px for most elements, 12px for cards
- Cards: white bg, 0.5px border, 12px radius, padding 1rem 1.25rem
- Spacing: rem for vertical rhythm (1rem, 1.5rem, 2rem), px for component-internal gaps (8px, 12px, 16px)
- No box-shadows except focus rings

## Metric cards

For summary numbers (revenue, count, percentage):
- Surface card with muted 13px label above, 24px/500 number below
- Background: light gray surface (no border)
- Radius: 8px, padding: 1rem
- Use in grids of 2-4 with gap: 12px

```html
<div style="display: grid; grid-template-columns: repeat(3, 1fr); gap: 12px;">
  <div style="background: #F1EFE8; border-radius: 8px; padding: 1rem;">
    <div style="font-size: 13px; color: #5F5E5A;">Total users</div>
    <div style="font-size: 24px; font-weight: 500; color: #2C2C2A;">12,847</div>
  </div>
  <div style="background: #F1EFE8; border-radius: 8px; padding: 1rem;">
    <div style="font-size: 13px; color: #5F5E5A;">Revenue</div>
    <div style="font-size: 24px; font-weight: 500; color: #2C2C2A;">$284k</div>
  </div>
  <div style="background: #F1EFE8; border-radius: 8px; padding: 1rem;">
    <div style="font-size: 13px; color: #5F5E5A;">Conversion</div>
    <div style="font-size: 24px; font-weight: 500; color: #2C2C2A;">3.2%</div>
  </div>
</div>
```

## Layout types

**Editorial** (explanatory content): no card wrapper, prose flows naturally.

**Card** (bounded objects like contact record, receipt): single raised card wraps the whole thing.

## Component patterns

### Interactive explainer
For "explain how X works" / "teach me about Y":
- Sliders, buttons, live state displays
- No card wrapper — whitespace is the container
- Pattern: label + range input + live readout

### Compare options
For "compare X and Y" / "help me choose":
- Side-by-side card grid
- `grid-template-columns: repeat(auto-fit, minmax(160px, 1fr))`
- Each option in a card with badges for key differentiators
- Recommended option: accent with 2px solid info-blue border + "Recommended" badge

### Data record
For contact cards, receipts, profiles:
- Single raised card wrapper
- Avatar/initials circle for people (44px, rounded, colored background)
- Key-value pairs in a table layout

### Form mockup
- Input fields: 36px height, 0.5px border, 8px radius
- Labels: 14px, secondary color, above the field
- Buttons: transparent bg, 0.5px border, hover state

## Mockup presentation rules

Contained mockups (mobile screens, chat threads, single cards, modals) should sit on a background surface so they don't float naked. Full-width mockups (dashboards, settings pages, data tables) do not need an extra wrapper.

## Number formatting

Round every displayed number. Never show JS float artifacts like 0.30000000000000004. Use appropriate precision: integers for counts, 1-2 decimals for percentages, locale formatting for currency.

## Dark mode considerations

For standalone SVG mockups, include `@media (prefers-color-scheme: dark)` rules:
- Swap white backgrounds to #1a1a1a
- Swap dark text to #e0e0e0
- Swap border opacity direction
- Metric card bg: #2a2a2a instead of #F1EFE8
