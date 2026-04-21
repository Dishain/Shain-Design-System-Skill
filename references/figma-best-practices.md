# Figma Design System Best Practices — Reference

Read this when building or reviewing Figma components, working with design tokens
architecture, platform-specific guidelines, or export settings.

## Table of Contents

1. [File & Page Organization](#file--page-organization)
2. [Atomic Design Methodology](#atomic-design-methodology)
3. [Design Tokens Architecture](#design-tokens-architecture)
4. [Component Building](#component-building)
5. [Naming Conventions](#naming-conventions)
6. [Auto Layout](#auto-layout)
7. [Variants and Properties](#variants-and-properties)
8. [Responsive & Constraints](#responsive--constraints)
9. [Platform-Specific Guidelines](#platform-specific-guidelines)
10. [Export Guidelines](#export-guidelines)
11. [Anti-Patterns](#anti-patterns)
12. [Component Checklist](#component-checklist)

---

## File & Page Organization

### Recommended file structure

```
Design System/
├── Foundations.fig       (Colors, Typography, Spacing, Elevation, Radius)
├── Components.fig        (Atoms, Molecules, Organisms, Templates)
└── Documentation.fig     (Usage Guidelines, Do's/Don'ts, Code Examples)
```

### Page naming

Foundations: 📐 Grid System, 🎨 Colors, 📝 Typography, 📏 Spacing, ⬆️ Elevation
Components: ⚛️ Atoms, 🧬 Molecules, 🏗️ Organisms, 📱 Templates, 🔧 WIP

---

## Atomic Design Methodology

**Atoms:** Buttons, Input fields, Icons, Labels, Badges, Avatars
**Molecules:** Search bar, Card header, Form field, Nav item
**Organisms:** Navigation bar, Data table, Card, Modal, Form section
**Templates:** Dashboard layout, Settings screen, List/Detail views

---

## Design Tokens Architecture

### Three-tier system

```
Primitive → Semantic → Component-Specific
```

**Primitives (foundation):**
```
color/gray/50  → #FAFAFA
color/blue/500 → #0097DB
spacing/4      → 16px
```

**Semantic (purpose):**
```
bg/surface       → {color/gray/50}
text/primary     → {color/gray/900}
action/primary/default → {color/blue/500}
border/default   → {color/gray/200}
```

**Component-specific (optional):**
```
button/bg/primary → {action/primary/default}
card/border       → {border/default}
```

### Modes

- Every semantic collection should define at least `Light` and `Dark` modes.
- Primitives do not need modes; modes live on the semantic layer.
- Components reference only semantic tokens so theme swaps are transparent.

### Creating variables in Figma

1. Create collection (e.g. `Primitives/Colors`).
2. Define primitive variables with raw values.
3. Create a semantic collection referencing primitives; add `Light`/`Dark` modes.
4. Components bind fills, strokes, text, spacing, and radius to semantic tokens only.
5. Keep Paint Styles in sync (or deprecate them) so designers and engineers share one source.

### Variable naming

Pattern: `{category}/{subcategory}/{name}`
Examples: `color/gray/50`, `spacing/4`, `typography/size/lg`, `action/primary/default`

---

## Component Building

### Structure

```
Component: Button
├── Background (Frame with auto layout)
│   ├── Icon (Optional, component property)
│   ├── Label (Text)
│   └── Icon Right (Optional)
└── Overlay (For interaction states)
```

### Layer naming

- Name for purpose, not appearance
- Use PascalCase or kebab-case consistently
- Good: `Background`, `Icon`, `Label`, `Divider`
- Bad: `Frame 123`, `Rectangle 456`, `Group 789`

### Key rules

- Always use auto layout for buttons, cards, navigation, and adaptive components
- Keep nesting to 3 levels max (4 only with justification for complex organisms)
- Use main component references, never detached instances
- Set proper constraints for resizing

---

## Naming Conventions

### Components
```
[Category]/[Name]/[Variant]
Button/Primary/Default
Input/Text/Error
Card/Product/Compact
```

### Layers
- PascalCase for components: `NavigationBar`, `SearchInput`
- Descriptive purpose names: `IconContainer`, `ContentWrapper`

### Variables
```
{category}/{subcategory}/{name}
color/gray/50
spacing/4
action/primary/default
```

---

## Auto Layout

**Always use for:** buttons, cards, navigation, adaptive content
**Don't use for:** fixed decorative elements, illustrations

**Key settings:**
- Direction: Horizontal / Vertical
- Spacing: use spacing tokens (4, 8, 12, 16, 24px)
- Padding: use spacing tokens
- Resizing: Hug contents or Fill container

---

## Variants and Properties

### Variant properties (2–3 max, 4 with justification)

Used for visual styles: Size, Type/Style, State
Organize component sets by primary property (Size, then State).

### Boolean properties

Used for showing/hiding optional elements: Show Icon, Show Badge, Show Description

### Other property types

- **Text properties** for dynamic content (labels, placeholders)
- **Instance swap** for swappable elements (icons from library)

### Interaction states (for interactive components)

Default → Hover → Pressed → Focused → Disabled → Loading

---

## Responsive & Constraints

- Set proper constraints (horizontal/vertical fill, fixed, hug)
- Test at different sizes
- Use breakpoint tokens in code

**Web breakpoints:** Mobile 320-767px, Tablet 768-1023px, Desktop 1024px+

---

## Platform-Specific Guidelines

### iOS (SwiftUI)
- SF Symbols for icons, SF Pro font
- 44pt minimum touch target
- Standard spacing: screen margins 16/20pt, between sections 20/24pt
- Component naming: PascalCase (`NavigationBar`, `TabBar`, `CardView`)

### Android (Material Design)
- Material Icons, Roboto font
- 8dp grid system
- Screen margins 16/24dp, component padding 16dp

### Web
- Responsive design for multiple breakpoints
- Support mouse and touch
- Flexible layouts

---

## Export Guidelines

**Icons:** SVG, outline strokes, flatten transforms, `icon-[name].svg`
**Images:** PNG/WebP, @1x/@2x/@3x for iOS, mdpi/hdpi/xhdpi/xxhdpi for Android
**Tokens:** JSON/CSS export
**Components:** SVG or PNG with all states

---

## Anti-Patterns

1. Absolute positioning instead of auto layout
2. Too many variants (100+ = bad design)
3. Hardcoded values instead of tokens
4. Unnamed or poorly named layers
5. No component documentation
6. Components referencing primitives directly (bypass semantic layer)
7. Mixing presentation and content layers
8. Componentizing everything (only reusable things)
9. Ignoring accessibility
10. Deeply nested components (>3-4 levels)
11. Inconsistent spacing (use tokens, not arbitrary values)

---

## Component Checklist

### Structure
- Uses auto layout
- Clear layer names
- ≤3 nesting levels (4 with justification)
- Main component references
- Proper constraints

### Properties
- 2–3 variant properties
- Booleans for optional elements
- Text properties for dynamic content
- Instance swap for icons
- Sensible defaults

### Tokens
- Semantic variables for all colors (no primitives on components)
- Variables for spacing
- Text styles for typography
- Effect styles for shadows
- No hardcoded values

### States
- Default, Hover, Pressed, Disabled, Focus

### Accessibility
- 44×44px touch targets
- 4.5:1 text contrast, 3:1 UI contrast
- Alt text on images/icons
- Visible focus states
- Keyboard navigation support

### Testing
- Different sizes
- Short and long content
- Light and dark mode
- No overflow
