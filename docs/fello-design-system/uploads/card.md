# Card Component — Strict Specification

When building any card container, content panel, selectable card, or display card, follow these specifications exactly. Do NOT use shadcn Card or Tailwind card utilities.

> **TOKEN RULE:** Every colour in this file is expressed as a CSS custom-property token — `var(--token)` — with the raw hex in parentheses only as a comment. When writing Tailwind classes use the `[var(--token)]` syntax (e.g. `bg-[var(--bg-secondary)]`). Never hard-code a hex value.

---

## Architecture

```
Card (base container)
├── Standard — default bordered container
├── Focusable — adds hover shadow + teal border + keyboard focus
├── Selectable — adds selected state with focus ring + selection banner
└── Display — subtle background, no border
```

---

## Base Card Structure

```jsx
<div
  style={{
    display: 'block',
    borderRadius: '8px',
    border: '1px solid var(--border-secondary)',
    padding: gapPadding,
    overflow: 'hidden',
    background: 'var(--bg-primary)',
    position: 'relative',
  }}
>
  {children}
</div>
```

---

## Gap (Padding) Variants

| Gap | Padding | Pixels |
|-----|---------|--------|
| `0` | 0 | 0px |
| `xs` | 12px | spacing-3 |
| `sm` | 16px | spacing-4 |
| **`md`** (default) | **20px** | spacing-5 |
| `lg` | 24px | spacing-6 |

---

## Shadow Variants

Shadows are only relevant for **Focusable** cards on hover. The shadow input controls which shadow the card gets on hover:

| Shadow | Hover box-shadow |
|--------|-----------------|
| `sm` | `var(--shadow-sm)` |
| **`md`** (default) | `var(--shadow-lg)` |
| `lg` | `var(--shadow-lg)` |

**Note:** The base card has **no box-shadow** by default. Shadow only appears on hover for focusable cards.

---

## Card Variants

### Standard (Default)

```jsx
style={{
  display: 'block',
  borderRadius: '8px',
  border: '1px solid var(--border-secondary)',
  padding: '20px',            // default md
  overflow: 'hidden',
  background: 'var(--bg-primary)',
  position: 'relative',
}}
```

No hover effects, no shadow.

### Focusable

Adds hover and focus-visible behavior. Set `tabIndex={0}`.

**Hover:**
```jsx
style={{
  boxShadow: 'var(--shadow-lg)',
  cursor: 'pointer',
  border: '1px solid var(--bg-brand-secondary-solid_hover)',
}}
```

**Focus-visible:**
```jsx
style={{
  boxShadow: 'var(--ring-brand-secondary)',
  border: '1px solid var(--bg-brand-secondary-solid_hover)',
}}
```

**Hover title color:** Any card title element gets `color: 'var(--text-brand-secondary)'` (#098486) on card hover/focus.

### Selectable

Extends Focusable (also has `tabIndex={0}` and hover behavior). Adds selection state.

**Selected state:**
```jsx
style={{
  boxShadow: 'var(--ring-brand-secondary)',
  border: '1px solid var(--bg-brand-secondary-solid_hover)',
}}
```

**Selection Banner (shown when selected + selectionText is provided):**
```jsx
<div
  style={{
    fontFamily: 'Inter, sans-serif',
    fontSize: '12px',
    lineHeight: '18px',
    fontWeight: 500,
    color: 'var(--text-white)',
    background: 'var(--bg-brand-secondary-solid)',
    height: '26px',
    textAlign: 'center',
    textTransform: 'uppercase',
    padding: '4px 0',
    position: 'relative',
    // Negative margins to extend beyond card padding:
    top: `-${gapPadding}`,
    left: `-${gapPadding}`,
    width: `calc(100% + ${gapPadding * 2}px)`,
  }}
>
  {selectionText}
</div>
```

**Layout shift:** When multiple cards are in a group and one is selected, unselected cards get `marginTop: '26px'` to compensate for the banner height.

### Display

```jsx
style={{
  display: 'block',
  borderRadius: '8px',
  border: 'none',
  padding: gapPadding,
  overflow: 'hidden',
  background: 'var(--bg-secondary_subtle)',
  position: 'relative',
}}
```

No shadow, no hover effects, no border.

---

## Fluid Width Content

A child that needs to extend edge-to-edge within the card (cancelling the card padding):

```jsx
<div style={{ margin: `-${gapPadding}` }}>
  {fullWidthContent}
</div>
```

---

## Color Reference

| Token | Hex | Usage |
|-------|-----|-------|
| `var(--bg-primary)` (#FFFFFF) | bg-primary | Standard card background |
| `var(--bg-secondary_subtle)` (#F7F9FA) | bg-content-card | Display card background |
| `var(--bg-brand-secondary-solid)` (#098486) | bg-brand-secondary-solid | Selection banner background |
| `var(--border-secondary)` (#CCD6DC) | border-secondary | Default card border |
| `var(--bg-brand-secondary-solid_hover)` (#0BA5A7) | border-brand-secondary-solid | Hover/focus/selected border |
| `var(--text-brand-secondary)` (#098486) | text-brand-secondary | Title text on hover |
| `var(--text-white)` (#FFFFFF) | text-white | Selection banner text |
| `var(--text-disabled)` (#8EA1AF) | text-disabled | Disabled card text |
| `var(--shadow-lg)` | shadow-lg | Focusable hover shadow (md) |
| `var(--shadow-sm)` | shadow-sm | Focusable hover shadow (sm) |
| `var(--shadow-lg)` | shadow-2xl | Focusable hover shadow (lg) |
| `var(--ring-brand-secondary)` | focus-brand-secondary-xs | Focus ring + selected ring |

---

## CRITICAL Rules

1. **Border radius is always 8px** (radius-4) — for all card variants.
2. **Default gap is `md` (20px)** — not 16px or 24px.
3. **Default shadow is `md`** — which maps to `var(--shadow-lg)` on hover, NOT `shadow-md`.
4. **Standard card has NO box-shadow** — shadow only appears on hover for focusable/selectable cards.
5. **Display card has NO border** (`border: none`) and uses `var(--bg-secondary_subtle)` (#F7F9FA) background.
6. **Selection banner height is exactly 26px** — uses caption-sm typography (12px/18px/500), uppercase, `var(--text-white)` (#FFFFFF) text on teal.
7. **Selection banner extends beyond card padding** — uses negative positioning equal to the card's gap padding value.
8. **Focus ring and selected state use the same shadow** — `var(--ring-brand-secondary)`.
9. **Hover border color is teal `var(--bg-brand-secondary-solid_hover)` (#0BA5A7)** — NOT the orange primary brand.
10. **Layout shift compensation is 26px** (`marginTop: 26px`) on non-selected cards when a sibling is selected.
