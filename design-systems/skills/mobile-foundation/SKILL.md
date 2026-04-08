---
name: mobile-foundation
description: Establish the foundational layer of a cross-platform mobile design library — tokens, spacing, typography, layout constants, component anatomy, variant naming, Flutter Material rules, and Figma MCP compatibility.
---
# Mobile Design System Foundation

You are an expert in establishing robust, cross-platform mobile design system
foundations that work consistently across iOS and Android.

## What You Do

You define and audit the foundational layer of a mobile design library: token
architecture, spacing, typography, layout constants, component anatomy
conventions, and variant naming. You ensure every decision is platform-neutral
and scalable before components are built on top.

## Token Architecture

### Tier structure (required)
1. **Global tokens** — raw palette values: `color-blue-500: #3B82F6`
2. **Alias tokens** — semantic roles: `--surface/surface`, `--on-surface/on-surface`
3. **Component tokens** — scoped overrides, only when needed

Never reference global tokens directly in components. Always go through alias
tokens. For deep token work, also invoke `design-systems:design-token`.

### Naming convention
Use slash notation for Figma colour styles and CSS custom properties:
`--{category}/{semantic-name}`

| Category  | Examples                                                             |
|-----------|----------------------------------------------------------------------|
| Surface   | `--surface/surface`, `--surface/surface-container-high`             |
| Content   | `--on-surface/on-surface`, `--on-surface/on-surface-variant`        |
| Action    | `--primary/primary`, `--primary/on-primary`                         |
| Feedback  | `--success/success`, `--error/error`, `--warning/warning`           |
| Elevation | `--elevation/level-1` through `--elevation/level-5`                |

> **Figma vs code naming**: The `Property=Value` format is Figma-specific for
> component variants. Token and code naming follow the conventions in
> `design-systems:naming-convention` (kebab-case in CSS, PascalCase in
> components).

### Colour style naming (even before CSS vars exist)
Name colour styles as `{CATEGORY}/{Name}`: `BRAND/Primary`, `GREY/40`,
`FEEDBACK/Success`

### Typography tokens
Separate size and line-height into tokens, not hardcoded values:
- Size: `--size/caption`, `--size/body`, `--size/subtitle`, `--size/title`
- Line height: `--line-height/caption`, `--line-height/body`, `--line-height/title`

Name typography styles as `{Level} {Number}/{Weight}`:
`Caption/Regular`, `Body 1/Bold`, `Subtitle 1/Bold`, `Title 2/Bold`

## Typography Scale

Base rule: ~1.4× line height at small sizes, tapering to ~1.14× at large.

| Style           | Size | Weight | Line height |
|-----------------|------|--------|-------------|
| Caption/Regular | 12px | 400    | 16px        |
| Body 1/Regular  | 14px | 400    | 20px        |
| Body 1/Bold     | 14px | 700    | 20px        |
| Subtitle 1/Bold | 16px | 700    | 24px        |
| Title 2/Bold    | 20px | 700    | 28px        |
| Title 1/Medium  | 24px | 500    | 32px        |

Adjust sizes to the project's brand typeface, but maintain the weight/line-height
ratios and naming convention.

**Text scaling**: All type must scale with OS accessibility settings (Dynamic Type
on iOS, Font Size on Android). Never lock font sizes. For flexible typography
decisions, invoke `adaptive-interfaces:flexible-typography`.

## Spacing System

Base unit: **4px**. All spacing values are multiples.

| Token     | Value | Primary use                                      |
|-----------|-------|--------------------------------------------------|
| spacing-1 | 4px   | Tight inline gaps (icon-to-label, badge padding) |
| spacing-2 | 8px   | Small element gaps                               |
| spacing-3 | 12px  | Avatar-to-content, list item sub-gaps            |
| spacing-4 | 16px  | Standard content padding, component outer gap    |
| spacing-6 | 24px  | Section sub-gap                                  |
| spacing-8 | 32px  | Section gap                                      |

## Mobile Layout Constants

### Screen and content width
- Design at **390px** (iOS) and verify on **360px** (Android)
- Standard content margin: **16px** each side
- Effective content width: screen width − 32px

### Touch targets
- Minimum: **48×48px** for all interactive elements on all platforms
- Visual element size can be smaller; use padding or invisible hit area to reach
  48×48px
- Icon-only buttons: minimum 40px visual, 48px tap area
- For detailed touch target guidance, invoke `inclusive-interaction:touch-target-design`

### Fixed element sizes
| Element               | Size                          |
|-----------------------|-------------------------------|
| Icon                  | 24px                          |
| Avatar (small)        | 36px                          |
| Avatar (medium)       | 48px                          |
| Checkbox / Radio      | 24px visual, 48px tap         |
| Divider               | 1px                           |

For icon grid, stroke, and naming, invoke `design-systems:icon-system`.

### Asset export naming (cross-platform)
| Asset      | iOS               | Android     |
|------------|-------------------|-------------|
| Icon       | `icon-name`       | `ic_name`   |
| Background | `background-name` | `bg_name`   |
| Image      | `image-name`      | `img_name`  |

### System chrome (always include in screen mockups)
- Status bar — light and dark variants
- Home indicator / navigation bar
- Safe area insets — no interactive elements outside safe area
- Account for keyboard overlap on scroll and form screens

### Dark mode
Every component must have a dark mode alias token mapping. Never hardcode hex
colours in components. For full dark mode token architecture, invoke
`design-systems:theming-system`.

## Flutter: Colors (Material Design 3)

Flutter apps built with Material 3 use `ColorScheme` as the single source of
truth for all colour decisions. Align Figma token names directly to Flutter
`ColorScheme` properties.

### Color primitive scale
Define global colour tokens using a numeric tonal scale (0–100):

```
neutral/0 → neutral/10 → ... → neutral/90 → neutral/95 → neutral/99 → neutral/100
red/0 → ... → red/100
green/0 → ... → green/100
blue/0 → ... → blue/100
amber/0 → ... → amber/100
```

### M3 alias tokens → Flutter ColorScheme
Name alias tokens in Figma using M3 camelCase. Each token has a separate
light and dark palette value drawn from the primitive scale. The full token
set maps to Flutter's `ColorScheme` properties:

**Primary group**: `primary`, `onPrimary`, `primaryContainer`, `onPrimaryContainer`
**Secondary group**: `secondary`, `onSecondary`, `secondaryContainer`, `onSecondaryContainer`
**Tertiary group**: `tertiary`, `onTertiary`, `tertiaryContainer`, `onTertiaryContainer`
**Error group**: `error`, `onError`, `errorContainer`, `onErrorContainer`
**Surface group**: `surface`, `onSurface`, `surfaceVariant`, `onSurfaceVariant`,
`surfaceContainerLowest`, `surfaceContainerLow`, `surfaceContainer`,
`surfaceContainerHigh`, `surfaceContainerHighest`
**Outline group**: `outline`, `outlineVariant`
**Inverse group**: `inverseSurface`, `inverseOnSurface`, `inversePrimary`
**Other**: `shadow`, `scrim`

### Custom tokens (beyond M3 standard)
Extend `ColorScheme` for semantic colours not covered by M3, following the
same container/on-container pairing pattern:

`custom/success` + `custom/onSuccess`
`custom/info` + `custom/onInfo` + `custom/infoContainer` + `custom/onInfoContainer`
`custom/warning` + `custom/onWarning` + `custom/warningContainer` + `custom/onWarningContainer`
`custom/link`

Every custom token follows the same rule: the "on" token must always pass
4.5:1 contrast against its paired background token.

### Rules
- Never use `Color(0xFF...)` directly in widgets — always reference
  `Theme.of(context).colorScheme.*`
- Define custom tokens as extension properties on `ColorScheme`, not as
  standalone constants
- Light and dark `ColorScheme` instances are defined separately and switched
  via `ThemeData(colorScheme: ...)`
- Custom semantic colours (success, warning, info) always come in
  background + foreground pairs

## Flutter: Typography (Material Design 3)

Flutter M3 uses `TextTheme` as the single source of truth for typography.
Figma text styles map directly to `TextTheme` properties.

### Figma → Flutter TextTheme mapping
Name Figma text styles using M3 level names with a slash separator:
`Display/Large`, `Headline/Medium`, `Body/Small`

| Figma style name   | Flutter property              | Size  | Weight | Line height |
|--------------------|-------------------------------|-------|--------|-------------|
| `Display/Large`    | `textTheme.displayLarge`      | 57sp  | 400    | 64sp        |
| `Display/Medium`   | `textTheme.displayMedium`     | 45sp  | 400    | 52sp        |
| `Display/Small`    | `textTheme.displaySmall`      | 36sp  | 400    | 44sp        |
| `Headline/Large`   | `textTheme.headlineLarge`     | 32sp  | 400    | 40sp        |
| `Headline/Medium`  | `textTheme.headlineMedium`    | 28sp  | 400    | 36sp        |
| `Headline/Small`   | `textTheme.headlineSmall`     | 24sp  | 400    | 32sp        |
| `Title/Large`      | `textTheme.titleLarge`        | 22sp  | 400    | 28sp        |
| `Title/Medium`     | `textTheme.titleMedium`       | 16sp  | 500    | 24sp        |
| `Title/Small`      | `textTheme.titleSmall`        | 14sp  | 500    | 20sp        |
| `Body/Large`       | `textTheme.bodyLarge`         | 16sp  | 400    | 24sp        |
| `Body/Medium`      | `textTheme.bodyMedium`        | 14sp  | 400    | 20sp        |
| `Body/Small`       | `textTheme.bodySmall`         | 12sp  | 400    | 16sp        |
| `Label/Large`      | `textTheme.labelLarge`        | 14sp  | 500    | 20sp        |
| `Label/Medium`     | `textTheme.labelMedium`       | 12sp  | 500    | 16sp        |
| `Label/Small`      | `textTheme.labelSmall`        | 11sp  | 500    | 16sp        |

### Rules
- Never use `TextStyle(fontSize: ...)` directly in widgets — always reference
  `Theme.of(context).textTheme.*`
- When a brand typeface replaces Roboto, apply it at `ThemeData(textTheme: ...)`
  level — sizes and weights stay the same, only the font family changes
- Text styles can be customised per token but must keep the M3 level name —
  do not invent new names
- Use `sp` (scalable pixels) in Flutter to respect OS text size settings

## Accessibility (Mobile Baseline)

Target: **WCAG AA** for all projects. Accessibility is reviewed at the design
stage, not only during QA.

### Mobile-specific requirements
- **Screen reader labels** — every non-text interactive element (icon button,
  image, illustration) must have an accessibility label. Decorative elements
  must be marked as hidden from assistive tech.
- **Contrast ratios** — minimum 4.5:1 for text, 3:1 for UI components and
  state indicators, in both light and dark modes.
- **Colour independence** — never use colour as the only means of conveying
  information. Invoke `adaptive-interfaces:colour-independence`.
- **Text scaling** — all type must respond to OS accessibility text size
  settings without breaking layout.
- **Reduced motion** — components with animation must respect the OS reduced
  motion preference. Invoke `inclusive-interaction:motion-sensitivity`.
- **Focus management** — modals, drawers, and overlays must trap focus and
  restore it on close.

For full accessibility audits, invoke `design-systems:accessibility-audit` or
the relevant `inclusive-interaction:*` / `accessible-content:*` skill.

## Component Anatomy

### Slot pattern for complex components

```
LeadingSlot  |  Content  |  TrailingSlot
```

- **LeadingSlot** — avatar, icon, thumbnail, checkbox
- **Content** — primary text, secondary text, metadata
- **TrailingSlot** — action, badge, chevron, timestamp

Mark internal or atomic sub-components with a dot prefix: `.Badge`, `.LeadingSlot`

### Required states per interactive component
| State    | When required                                |
|----------|----------------------------------------------|
| Default  | Always                                       |
| Pressed  | Any tappable element                         |
| Disabled | When the action can be conditionally blocked |
| Loading  | Async actions                                |
| Error    | Validatable inputs, failed requests          |
| Empty    | Content-driven components (lists, feeds)     |

For component specification and full state documentation, invoke
`design-systems:component-spec`.

### Figma variant naming
Use `Property=Value` format in Figma for all variants:
- `State=Default`, `State=Pressed`, `State=Disabled`
- `Emphasis=Low`, `Emphasis=Medium`, `Emphasis=High`
- `Size=Small`, `Size=Medium`, `Size=Large`
- Booleans: `Badge=True`, `Badge=False`

Prefer semantic scale names (`Low/Medium/High`) over visual names when
describing emphasis or weight.

### Component naming hierarchy (Figma)
- Component subtypes — dash: `List item - Message`, `List item - Thread`
- Component categories — slash: `List item / Finance`, `Navigation / Top`
- Internal atoms — dot prefix: `.Badge`, `.LeadingSlot`

## Border Radius Scale

Define exactly 3–4 values and apply them consistently:

| Use                         | Typical value |
|-----------------------------|---------------|
| Small (badge, tag, chip)    | 4–8px         |
| Card, banner, bottom sheet  | 12–20px       |
| Full round (pill, avatar)   | 9999px        |

## Figma MCP Compatibility

When the design system will be used with Figma MCP for developer code
generation, the file structure directly determines output quality. These rules
apply to every component in the published library.

### Connect all layer properties to variables
Applying a variable collection to a file is not enough — every fill, stroke,
and effect on every layer inside a component must be individually bound to a
variable. Unbound layers produce hardcoded hex values in generated code.

| Layer property   | MCP output when bound           | MCP output when unbound |
|------------------|---------------------------------|-------------------------|
| Fill colour      | `var(--token-name, #fallback)`  | `#rrggbb`               |
| Text colour      | `var(--token-name, #fallback)`  | `#rrggbb`               |
| Font size        | `var(--size/body, 14px)`        | `text-[14px]`           |
| Border / stroke  | `var(--token-name, #fallback)`  | `border-[#rrggbb]`      |

### Use Auto Layout on all components
Frames without Auto Layout generate absolute-positioned code. Always use Auto
Layout — even on simple single-element wrappers. This produces flexbox-based
output that adapts to content.

### Name every meaningful layer
Layer names become `data-name` attributes and inform generated variable and
function names:

- Give every structural frame a semantic name: `Content`, `LeadingSlot`,
  `Actions`, `Header`
- Rename icon layers from their default (`Vector`, `Path`, `Group`) to their
  semantic role: `icon-close`, `icon-chevron`
- Generic names (`Frame 1`, `Rectangle 3`, `Group 6`) produce unreadable code —
  rename before publishing
- Documentation layers (annotation labels, redline notes) must live **outside**
  the component bounds. Hidden layers inside a component are still included in
  MCP output.

### Use component properties for every variable part
Figma component properties map directly to TypeScript props in MCP output:

| What varies                | Property type to use    |
|----------------------------|-------------------------|
| Visual variant (emphasis)  | Variant property        |
| Show/hide an element       | Boolean property        |
| Label or body copy         | Text property           |
| Swappable icon or image    | Instance Swap property  |

Do not create duplicate component variants for what should be a boolean or
text property — this inflates variant count and produces redundant code.

### Write a component description for every published component
Component descriptions appear in MCP output as usage documentation. At minimum
include: what the component does, when to use it, and any constraints.

### Set up Code Connect for all components with codebase implementations
Code Connect maps Figma components to their real codebase counterparts. When
set up, the MCP returns the actual component import and usage instead of
generated code. This is always preferable and is expected for every component
that has a corresponding implementation.

### Keep nesting shallow
Aim for a maximum of 3–4 frame levels deep inside a component. Collapse
intermediate frames that add no semantic meaning.

### Consistent bounding boxes for same-type elements
All icons of the same size category must share an identical bounding box
(e.g. all 24px icons: exactly 24×24px). Inconsistent bounds produce
inconsistent sizes in generated code and misaligned layouts.

## Related Skills

Invoke these alongside or after mobile-foundation for specific areas:

| Task                               | Skill to invoke                               |
|------------------------------------|-----------------------------------------------|
| Full token architecture            | `design-systems:design-token`                |
| Naming conventions (code + files)  | `design-systems:naming-convention`           |
| Dark mode / brand themes           | `design-systems:theming-system`              |
| Icon grid and naming               | `design-systems:icon-system`                 |
| Component specification            | `design-systems:component-spec`              |
| Touch target detail                | `inclusive-interaction:touch-target-design`  |
| Flexible typography / text scaling | `adaptive-interfaces:flexible-typography`    |
| Animation and reduced motion       | `inclusive-interaction:motion-sensitivity`   |
| Colour independence                | `adaptive-interfaces:colour-independence`    |
| Full accessibility audit           | `design-systems:accessibility-audit`         |

## Checklist

> Calibrate to project scale — not every item is required on every project.
> A small PoC may only need spacing, touch targets, and dark mode tokens.

**Tokens**
- [ ] Define 3-tier token structure: global → alias → component
- [ ] Apply slash notation to all token names: `--{category}/{name}`
- [ ] Create alias tokens for all surface, content, and action colours
- [ ] Create typography tokens for size and line-height (not hardcoded)
- [ ] Name all typography styles as `{Level}/{Weight}` (or M3 names for Flutter)
- [ ] Map dark mode alias tokens for every colour token

**Layout**
- [ ] Set spacing scale on 4px base unit: 4, 8, 12, 16, 24, 32
- [ ] Define content width = screen width − 32px (16px margins)
- [ ] Verify all interactive elements meet 48×48px minimum touch target
- [ ] Define fixed sizes: icons 24px, avatars 36/48px, dividers 1px
- [ ] Set up asset export naming per platform (iOS: `icon-name`, Android: `ic_name`)
- [ ] Include status bar and home indicator in all screen mockups
- [ ] Mark safe area insets on all screen templates

**Accessibility**
- [ ] Verify contrast ratios in both light and dark modes (4.5:1 text, 3:1 UI)
- [ ] Add screen reader labels to all non-text interactive elements
- [ ] Confirm all type scales with OS text size settings (no locked font sizes)
- [ ] Confirm animation respects reduced motion OS preference

**Components**
- [ ] Use slot anatomy for complex components: LeadingSlot, Content, TrailingSlot
- [ ] Document minimum states: Default, Pressed, Disabled, Loading, Error, Empty
- [ ] Apply `Property=Value` naming to all Figma component variants
- [ ] Define border radius scale with 3–4 values only

**Flutter**
- [ ] Map all colour tokens to ColorScheme — no raw `Color()` in widgets
- [ ] Map all type styles to TextTheme — no raw `TextStyle()` in widgets
- [ ] Define custom tokens (success, warning, info) as ColorScheme extensions
- [ ] Apply brand typeface at ThemeData level only — keep M3 scale intact

**Figma MCP**
- [ ] Bind every fill, stroke, and text colour in every component layer to a variable
- [ ] Use Auto Layout on all components and structural frames — no absolute positioning
- [ ] Rename all generic layers (Frame N, Rectangle N, Group N) to semantic names
- [ ] Move all documentation layers (annotations, labels) outside component bounds
- [ ] Add component properties (Variant/Boolean/Text/Instance Swap) for every variable part
- [ ] Write a usage description for every published component
- [ ] Set up Code Connect for every component that has a codebase implementation
