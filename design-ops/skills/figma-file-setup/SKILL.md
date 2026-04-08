---
name: figma-file-setup
description: Set up and organise Infinum Figma project files — file structure, page and frame naming conventions, component rules, asset export naming, and prototype interaction standards.
---
# Figma File Setup

You are an expert in organising Infinum Figma projects so files are navigable, handoff-ready, and consistent across designers.

## What You Do

You help set up and audit the structure, naming, and organisation of Figma project files.

## Project File Structure

A typical Infinum project uses these five Figma files:

| File | Purpose |
|---|---|
| **FigJam file** | Discovery, information architecture, workshops, journey maps, flowcharts, strategy outputs |
| **Wireframe file** | Main flows in low fidelity; used for early usability testing and alignment |
| **UI file** | All flows and screens in high fidelity, including all states (empty, loading, error, etc.) |
| **Design library file** | Published as a Figma library; contains the styleguide, components, and patterns |
| **Prototype file** | Separate interactive prototype (when needed) |

## Design Documentation Contents

A complete Design Library file contains:

### Styleguide
- Color styles and tokens
- Typography styles
- Effect styles (shadows, blurs)
- Grids and layout settings
- Spacing scale
- Brand assets
- Tone & voice reference
- Icon set
- Photo guidelines

### Component Library
All components with variants and states:
- Default
- Hover
- Focused
- Empty
- Loading
- Error

### Pattern Library
Categorised patterns with specs, usage guidelines, and examples. Most relevant on large or multi-designer projects.

## Page Naming

Use clear, logical names that reflect the content. Separate different flows into their own pages:

```
Onboarding / Home / Notifications / Archive / Components / Styleguide / Archive
```

## Frame (Screen) Naming

Always follow the format: **`Flow - Screen name - State`**

Examples:
- `Onboarding - Language picker - Selected`
- `Schedule - New visit - Filled`
- `My profile - Appliance details - Software version`
- `Rooms - Floor view - Tags`

Rules:
- No two screens should share the same name
- Name screens to reflect their flow, content, and state — not just a generic label
- Grid-align frames and visually separate flows so the file is easy to navigate
- Sub-flows (e.g. "Checkout" within "Store") should be visually distinct from the parent flow

## Component Rules

- Use Auto Layout and Variants when building components
- Keep Bounds consistent — assets of the same type (icons, images) should share the same bounding box size
- Name components logically; keep the naming hierarchy clean and consistent

## Asset Export Naming

| Asset | Web | iOS | Android |
|---|---|---|---|
| Icon | `ic-name` | `icon-name` | `ic_name` |
| Background | `bg-name` | `background-name` | `bg_name` |
| Image | `img-name` | `image-name` | `img_name` |

## Notes in Files

Leave notes under screens or flows when:
- A decision is complex or non-obvious
- The design might be questioned later
- Developer behaviour needs explaining

Use a consistent note component. Don't over-annotate — notes are for the non-obvious.

## Prototype Interaction Standards

When building Figma prototypes for demos, stakeholder walkthroughs, or usability testing:

- **Tap zones** — must be large enough (44px minimum touch target)
- **Modals** — appear from the bottom, dismiss downwards
- **Dialogs** — have a transparent overlay and dismiss on tap outside
- **Screen transitions** — follow Material navigation conventions
- **OS bars and browser chrome** — account for them; the default Figma frame height differs from real device experience

## UI Deliverable Checklist

Final UI deliverable includes:
- Styleguide
- Components with all variants and states
- Screens organised in flows/pages
- Archive of old iterations (don't delete, archive)
- Developer behaviour comments on non-obvious interactions
- App Store / Play Store assets (if applicable)
