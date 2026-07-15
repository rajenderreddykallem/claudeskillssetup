---
description: Generate working prototype code from a Figma design or frame. Use when the user shares a Figma link/frame, has the Figma MCP connection available, or asks to turn a design into code, a prototype, or a UI mockup into a real component.
---

# Figma to Prototype

The job is turning design *intent* into code that fits this codebase — not transcribing pixel
coordinates. A static frame is missing information a prototype needs; surface those gaps instead
of silently guessing.

## Get design data, in order of preference

1. **Figma MCP connection**, if available in this session (e.g. the official Figma plugin —
   `/plugin install figma@claude-plugins-official` — or an equivalent connector). Use it to read
   the selected frame: layer structure, auto-layout/flex behavior, spacing, type styles, color
   and variable bindings.
2. **Exported data the user provides**: a Figma REST API JSON export, exported PNG/SVG frames, or
   a design-tokens file (colors/spacing/type scale).
3. If neither is available, say so and ask for one — don't attempt to reproduce a design from a
   verbal description or a plain share-link URL alone; that produces a plausible-looking guess,
   not a faithful prototype.

## Translate design → code

- **Reuse this codebase's existing component library and design tokens first.** If there's
  already a `Button`, `Card`, spacing scale, or color tokens, map the Figma frame onto those
  rather than hardcoding new pixel/hex values that happen to match. Hardcode only when there's
  genuinely no existing primitive, and call that out as a gap between the design system and the
  codebase — worth flagging to the design/eng lead, not silently patching over.
- Match auto-layout direction, gap, and padding to the equivalent flex/grid properties instead of
  copying absolute positions.
- Preserve the component naming from Figma (frame/layer name) in a comment or the component's
  name where reasonable, so there's a traceable link from code back to the design node — this
  pairs with the `feature-traceability` skill when the prototype becomes a tracked feature.

## Scope: this is a prototype, not production polish

State explicitly what's real vs. stubbed: mock data vs. a real API call, a fake auth check vs.
real auth, static copy vs. i18n-ready strings. Don't gold-plate — no premature abstraction, no
handling for input shapes the design doesn't show.

## Flag what a static frame can't tell you

A Figma frame typically shows one state. Call out, rather than silently invent, what's undefined:

- Interaction states: hover, focus, active, disabled, error, loading, empty
- Responsive behavior beyond the breakpoints actually shown
- Accessibility: color contrast against the token values, focus order, alt text for images, ARIA
  roles for custom components
- Animation/transition specifics (Figma smart-animate settings are a rough guide at best)

## Output

List the files created/modified, then a short **"gaps vs. design"** section: everything you
approximated, stubbed, or had to assume, so the user can resolve it with the designer instead of
discovering it later.
