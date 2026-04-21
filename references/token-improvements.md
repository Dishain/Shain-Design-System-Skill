# Token System — Evolution Guide

Read this when planning token system changes, adding new tokens, restructuring
collections, preparing a new mode, or scaling to a second brand.

---

## 1. Adding a new semantic token

When components start hardcoding a value, that is a signal a semantic token is missing.

Checklist before adding:

1. **Does it belong in an existing category?** (`action`, `destructive`, `bg`, `text`,
   `border`, `icon`, `overlay`, `success`, `warning`, `info`).
2. **Is it a state variant of an existing token?** Then extend as
   `{category}/{purpose}/{state}` — not a new top-level token.
3. **Does every comparable token have the same shape?** If `action/primary/hover`
   exists, adding `destructive/hover` but skipping `success/hover` creates gaps.
4. **Does it need values in every mode?** A semantic token must resolve in every
   active mode (Light, Dark, and any density/brand modes).

---

## 2. Adding a new mode (Dark, Compact, or a new brand)

1. Add the mode to the semantic collection(s) only. Primitives never need modes.
2. Fill in values for every semantic token. No blanks — placeholders first, refine later.
3. Verify every component in Figma with the mode switched on. Look for contrast, icon
   legibility, and focus ring visibility.
4. Update automated token export so downstream CSS/TS reflects the new mode.
5. Re-run contrast audits. Dark modes frequently fail WCAG on subtle-gray text.

---

## 3. Focus, overlay, and a11y tokens to include from day one

These are the tokens most often missed in the first pass:

| Token | Typical value | Why |
|-------|---------------|-----|
| `focus/ring` | brand blue at 40% opacity | Keyboard focus indicator — accessibility |
| `overlay/scrim` | near-black at 50% opacity | Modal / drawer backdrop |
| `bg/surface-hover` | `bg/surface` +4% luminance | Hover feedback on cards, list rows |
| `bg/surface-pressed` | `bg/surface` +8% luminance | Pressed feedback |
| `text/disabled` | `text/secondary` at 50% | Disabled form controls |
| `text/inverse` | near-white | Text on filled action buttons |
| `text/error` | distinct from `destructive/default` | Field-level error copy |

Missing any of these pushes designers to hardcode values.

---

## 4. Keeping naming predictable

- Lowercase, slash-separated in Figma; kebab-case in CSS (`action/primary/default` →
  `--action-primary-default`).
- Never encode the color in the name (`red-hover` is wrong).
- Stateful tokens live at the third level (`.../hover`, `.../pressed`,
  `.../disabled`). Keep that shape consistent across categories.
- Programmatic transforms (Figma → JSON → CSS) should need zero special cases.

---

## 5. Component-specific tokens — when they earn their keep

A component-specific token is worth creating when:

- The component needs a value that would be wrong anywhere else
  (`input/ring-width`, `tooltip/max-width`).
- The component is themed independently from the rest of the system
  (a marketing page CTA with its own color).

Otherwise, reference semantic tokens directly. Every extra layer is one more thing
to keep in sync.

---

## 6. Deprecating old values

When retiring a token or Paint Style:

1. Add a description on the Figma variable/style:
   `⚠️ Deprecated — use {new/token}`.
2. Stop using it in new work immediately.
3. Sweep existing components in a dedicated PR (don't mix with feature work).
4. Remove the old token only after usage is zero.

---

## 7. When to plan a migration, not a patch

Consider a full token restructure (rather than incremental additions) if:

- Multiple categories use inconsistent naming (`UI/Red_hover` alongside `action/primary`).
- Adding a new mode would require renaming more than ~20% of tokens anyway.
- Components have diverged and reference a mix of primitive and semantic tokens.

A migration is a single PR that renames, reroutes, and re-exports everything at once.
Piecemeal renames cause long periods where two conventions are live.

---

## Priority summary

| Priority | What | Effort | Impact |
|----------|------|--------|--------|
| P0 | Semantic layer with state naming | — (baseline) | — |
| P0 | Light + Dark mode on semantic collection | — (baseline) | — |
| P1 | Focus ring, overlay, inverse text, disabled text tokens | Low | High |
| P2 | Component-specific tokens for themed areas | Medium | Medium |
| P3 | Density modes (Comfortable/Compact) | Medium | Medium |
| P3 | Multi-brand mode | High | High if needed |
