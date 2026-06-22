# Checkbox Component — Strict Specification

When building any checkbox, selection toggle, or boolean check control, follow these specifications exactly. Do NOT use shadcn Checkbox, default HTML checkbox styling, or any third-party checkbox library.

**Note:** This checkbox component is used inside multiple other components — Grid selection columns (`grid.md`), List Item multi-select (`list-item.md`), Select Input multi-select options (`select-inputs.md`), and Filter Dropdown multi-select options (`filter-dropdown.md`). Always use this exact checkbox spec when any of those components need a checkbox.

> **TOKEN RULE:** Every color in the code examples below uses CSS variable tokens or Tailwind palette names — never raw hex. When implementing, copy the token references exactly. See `design-tokens.md` for definitions.

---

## Checkbox Anatomy

A checkbox is a horizontal flex container with a visual check box and optional label + sub-text:

```
[□ check box]  [label]
               [sub-text]
```

- Layout: `display: inline-flex`, `align-items: flex-start`
- Gap: varies by size (see below)
- **⚠️ ALIGNMENT FIX:** When a label or sub-text is present AND size is NOT small, the checkbox box gets `margin-top: 2px` (`mt-0.5`) to vertically align with the text. WITHOUT this nudge, the box sits too high relative to the label.
- The visual box is rendered via a styled `<div>` (or `::before` pseudo-element equivalent) — NOT a native browser checkbox
- The native `<input type="checkbox">` is visually hidden (`appearance: none`, `opacity: 0`, `position: absolute`) but still present for form integration and accessibility

---

## Sizes

There are 3 checkbox sizes:

### Small
| Property | Value |
|----------|-------|
| Box size | 16px × 16px |
| Box border-radius | 4px (radius-2) |
| Box margin-top | **0** (no nudge — small text aligns naturally) |
| Check icon | 10px × 8px (SVG path) |
| Indeterminate dash | 12px × 2px (SVG path) |
| Label typography | caption-sm: 12px / 18px / weight 500 |
| Sub-text typography | paragraph-sm: 12px / 18px / weight 400 |
| Gap (box to label) | 8px |

### Medium (Default)
| Property | Value |
|----------|-------|
| Box size | 16px × 16px |
| Box border-radius | 4px (radius-2) |
| Box margin-top | **2px** (`mt-0.5`) when label/sub-text is present |
| Check icon | 10px × 8px (SVG path) |
| Indeterminate dash | 12px × 2px (SVG path) |
| Label typography | caption-md: 14px / 20px / weight 500 |
| Sub-text typography | paragraph-md: 14px / 20px / weight 400 |
| Gap (box to label) | 8px |

### Large
| Property | Value |
|----------|-------|
| Box size | 20px × 20px |
| Box border-radius | 4px (radius-2) |
| Box margin-top | **2px** (`mt-0.5`) when label/sub-text is present |
| Check icon | 10px × 8px (SVG path) |
| Indeterminate dash | 12px × 2px (SVG path) |
| Label typography | caption-lg: 16px / 24px / weight 500 |
| Sub-text typography | paragraph-md: 14px / 20px / weight 400 |
| Gap (box to label) | 12px |

---

## Box Styling

```jsx
style={{
  width: boxSize,           // 16px or 20px
  height: boxSize,
  minWidth: boxSize,        // prevent shrinking
  minHeight: boxSize,
  borderRadius: '4px',      // radius-2 — always 4px
  border: '1px solid var(--border-primary)',  // default border (#BDC8D3)
  backgroundColor: 'var(--bg-primary)',  // (#FFFFFF)
  display: 'flex',
  alignItems: 'center',
  justifyContent: 'center',
  transition: 'border-color 150ms ease-out, box-shadow 150ms ease-out, background-color 150ms ease-out',
  cursor: 'pointer',
  position: 'relative',
  flexShrink: 0,
}}
```

---

## States — Complete Color Spec

### Default (Unchecked)
| Property | Value |
|----------|-------|
| Border | 1px solid `var(--border-primary)` (#BDC8D3) |
| Background | `var(--bg-primary)` (#FFFFFF) |
| Box shadow | none |

### Hover (Unchecked)
| Property | Value |
|----------|-------|
| Border | 1px solid `var(--border-brand-secondary-solid)` (#0BA5A7) |
| Background | `var(--bg-primary)` (#FFFFFF) |

### Focus-Visible (Unchecked)
| Property | Value |
|----------|-------|
| Border | 1px solid `var(--border-brand-secondary-solid)` (#0BA5A7) |
| Background | `var(--bg-primary)` (#FFFFFF) |
| Box shadow | `var(--ring-brand-secondary)` — `0 0 0 4px rgba(11,165,167,0.24), 0 1px 2px 0 rgba(73,88,131,0.04)` (focus-brand-secondary-xs) |

### Checked
| Property | Value |
|----------|-------|
| Border | 1px solid `var(--border-brand-secondary-solid)` (#0BA5A7) |
| Background | `var(--bg-brand-secondary-solid)` (#098486) |
| Icon | White checkmark SVG (10×8px filled path) centered in box |

### Checked Hover
| Property | Value |
|----------|-------|
| Border | 1px solid `var(--border-brand-secondary-solid)` (#0BA5A7) |
| Background | `var(--bg-brand-secondary-solid_hover)` (#0BA5A7) |
| Icon | White checkmark |

### Checked Focus-Visible
| Property | Value |
|----------|-------|
| Border | 1px solid `var(--border-brand-secondary-solid)` (#0BA5A7) |
| Background | `var(--bg-brand-secondary-solid_hover)` (#0BA5A7) |
| Box shadow | `var(--ring-brand-secondary)` — `0 0 0 4px rgba(11,165,167,0.24), 0 1px 2px 0 rgba(73,88,131,0.04)` |
| Icon | White checkmark |

### Indeterminate
| Property | Value |
|----------|-------|
| Border | 1px solid `var(--border-brand-secondary-solid)` (#0BA5A7) |
| Background | `var(--bg-brand-secondary-solid)` (#098486) |
| Icon | White horizontal dash SVG (12×2px filled path) centered in box |

### Indeterminate Hover
| Property | Value |
|----------|-------|
| Border | 1px solid `var(--border-brand-secondary-solid)` (#0BA5A7) |
| Background | `var(--bg-brand-secondary-solid_hover)` (#0BA5A7) |
| Icon | White dash |

### Indeterminate Focus-Visible
| Property | Value |
|----------|-------|
| Border | 1px solid `var(--border-brand-secondary-solid)` (#0BA5A7) |
| Background | `var(--bg-brand-secondary-solid_hover)` (#0BA5A7) |
| Box shadow | `var(--ring-brand-secondary)` — `0 0 0 4px rgba(11,165,167,0.24), 0 1px 2px 0 rgba(73,88,131,0.04)` |
| Icon | White dash |

### Disabled (Unchecked)
| Property | Value |
|----------|-------|
| Border | 1px solid `var(--border-disabled_subtle)` (#CCD6DC) |
| Background | `var(--bg-disabled)` (#E8ECF1) |
| Cursor | default (not pointer) |
| Label color | `var(--text-disabled)` (#8EA1AF) |

### Disabled + Checked
| Property | Value |
|----------|-------|
| Border | 1px solid `var(--border-brand-secondary-solid)` at 50% opacity — rgba(9,132,134,0.5) |
| Background | `var(--bg-brand-secondary-solid_hover)` at 50% opacity — rgba(11,165,167,0.5) (#0BA5A780) |
| Icon | White checkmark (also slightly faded) |
| Cursor | default |
| Label color | `var(--text-disabled)` (#8EA1AF) |

### Disabled + Indeterminate
| Property | Value |
|----------|-------|
| Border | 1px solid `var(--border-brand-secondary-solid)` at 50% opacity — rgba(9,132,134,0.5) |
| Background | `var(--bg-brand-secondary-solid_hover)` at 50% opacity — rgba(11,165,167,0.5) |
| Icon | White dash (slightly faded) |
| Cursor | default |

### Error (Unchecked)
| Property | Value |
|----------|-------|
| Border | 1px solid `var(--border-error-solid)` (#F02C00) |
| Background | `var(--bg-primary)` (#FFFFFF) |

### Error Hover
| Property | Value |
|----------|-------|
| Border | 1px solid `var(--border-error-solid)` (#F02C00) |
| Background | `var(--bg-primary)` (#FFFFFF) |

### Error Focus-Visible
| Property | Value |
|----------|-------|
| Border | 1px solid `var(--border-error-solid)` (#F02C00) |
| Background | `var(--bg-primary)` (#FFFFFF) |
| Box shadow | `var(--ring-error-primary)` — `0 0 0 4px rgba(240,44,0,0.24), 0 1px 2px 0 rgba(73,88,131,0.04)` (focus-error-xs) |

### Error + Checked
| Property | Value |
|----------|-------|
| Border | 1px solid `var(--border-error-solid)` (#F02C00) |
| Background | `var(--bg-error-solid)` (#F02C00) |
| Icon | White checkmark |

### Error + Checked Focus-Visible
| Property | Value |
|----------|-------|
| Border | 1px solid `var(--border-error-solid)` (#F02C00) |
| Background | `var(--bg-error-solid)` (#F02C00) |
| Box shadow | `var(--ring-error-primary)` — `0 0 0 4px rgba(240,44,0,0.24), 0 1px 2px 0 rgba(73,88,131,0.04)` |

---

## Check Icons

### Checkmark (checked state)
A white filled SVG path, 10px wide × 8px tall, centered in the box:

```jsx
<svg width="10" height="8" viewBox="0 0 10 8" fill="none">
  <path d="M9 1L3.5 6.5L1 4" stroke="white" strokeWidth="1.5" strokeLinecap="round" strokeLinejoin="round" />
</svg>
```

### Indeterminate Dash
A white filled SVG path, 12px wide × 2px tall, centered in the box:

```jsx
<svg width="12" height="2" viewBox="0 0 12 2" fill="none">
  <path d="M1 1H11" stroke="white" strokeWidth="2" strokeLinecap="round" />
</svg>
```

---

## Label & Sub-text

```jsx
<div className="flex flex-col">
  <span style={{
    fontFamily: 'Inter, sans-serif',
    fontSize: labelFontSize,
    lineHeight: labelLineHeight,
    fontWeight: 500,                // caption weight
    color: disabled ? 'var(--text-disabled)' : 'var(--text-primary)',  // (#8EA1AF) / (#353E5A)
  }}>
    {label}
  </span>
  {subText && (
    <span style={{
      fontFamily: 'Inter, sans-serif',
      fontSize: subTextFontSize,
      lineHeight: subTextLineHeight,
      fontWeight: 400,              // paragraph weight
      color: disabled ? 'var(--text-disabled)' : 'var(--text-tertiary)',  // (#8EA1AF) / (#6B748E)
    }}>
      {subText}
    </span>
  )}
</div>
```

---

## Accessibility

| Property | Value |
|----------|-------|
| Role | Implicit via hidden `<input type="checkbox">` |
| `aria-checked` | `true`, `false`, or `mixed` (indeterminate) |
| `aria-disabled` | Reflects disabled state |
| `tabindex` | 0 when enabled |
| Keyboard | Space toggles, Enter toggles |

---

## Implementation Pattern for Lovable

```jsx
{/* === Medium Checkbox — Unchecked === */}
<label
  className="inline-flex items-start gap-2 cursor-pointer"
  style={{ color: 'var(--text-primary)' }}
>
  <div
    className={`
      flex items-center justify-center shrink-0
      mt-0.5
      w-4 h-4 rounded
      border transition-all duration-150 ease-out
      ${checked
        ? 'bg-[var(--bg-brand-secondary-solid)] border-[var(--border-brand-secondary-solid)] hover:bg-[var(--bg-brand-secondary-solid_hover)] hover:border-[var(--border-brand-secondary-solid)]'
        : 'bg-[var(--bg-primary)] border-[var(--border-primary)] hover:border-[var(--border-brand-secondary-solid)]'
      }
      focus-visible:ring-4 focus-visible:ring-[var(--ring-brand-secondary)]
    `}
    tabIndex={0}
    role="checkbox"
    aria-checked={indeterminate ? 'mixed' : checked}
    onKeyDown={(e) => { if (e.key === ' ' || e.key === 'Enter') { e.preventDefault(); toggle(); } }}
    onClick={toggle}
  >
    {checked && !indeterminate && (
      <svg width="10" height="8" viewBox="0 0 10 8" fill="none">
        <path d="M9 1L3.5 6.5L1 4" stroke="white" strokeWidth="1.5" strokeLinecap="round" strokeLinejoin="round" />
      </svg>
    )}
    {indeterminate && (
      <svg width="12" height="2" viewBox="0 0 12 2" fill="none">
        <path d="M1 1H11" stroke="white" strokeWidth="2" strokeLinecap="round" />
      </svg>
    )}
  </div>
  <div className="flex flex-col">
    <span className="text-sm leading-5 font-medium">Accept terms</span>
    <span className="text-sm leading-5 font-normal text-[var(--text-tertiary)]">
      I agree to the terms and conditions
    </span>
  </div>
</label>


{/* === Large Checkbox — Checked === */}
<label
  className="inline-flex items-start gap-3 cursor-pointer"
  style={{ color: 'var(--text-primary)' }}
>
  <div
    className="
      flex items-center justify-center shrink-0
      mt-0.5
      w-5 h-5 rounded
      bg-[var(--bg-brand-secondary-solid)] border border-[var(--border-brand-secondary-solid)]
      hover:bg-[var(--bg-brand-secondary-solid_hover)] hover:border-[var(--border-brand-secondary-solid)]
      transition-all duration-150 ease-out
    "
    tabIndex={0}
    role="checkbox"
    aria-checked="true"
  >
    <svg width="10" height="8" viewBox="0 0 10 8" fill="none">
      <path d="M9 1L3.5 6.5L1 4" stroke="white" strokeWidth="1.5" strokeLinecap="round" strokeLinejoin="round" />
    </svg>
  </div>
  <div className="flex flex-col">
    <span className="text-base leading-6 font-medium">Enable feature</span>
  </div>
</label>


{/* === Small Checkbox — Indeterminate === */}
<label
  className="inline-flex items-start gap-2 cursor-pointer"
  style={{ color: 'var(--text-primary)' }}
>
  <div
    className="
      flex items-center justify-center shrink-0
      w-4 h-4 rounded
      bg-[var(--bg-brand-secondary-solid)] border border-[var(--border-brand-secondary-solid)]
      hover:bg-[var(--bg-brand-secondary-solid_hover)] hover:border-[var(--border-brand-secondary-solid)]
      transition-all duration-150 ease-out
    "
    tabIndex={0}
    role="checkbox"
    aria-checked="mixed"
  >
    <svg width="12" height="2" viewBox="0 0 12 2" fill="none">
      <path d="M1 1H11" stroke="white" strokeWidth="2" strokeLinecap="round" />
    </svg>
  </div>
  <span className="text-xs leading-[18px] font-medium">Select all</span>
</label>


{/* === Disabled Checkbox — Unchecked === */}
<label
  className="inline-flex items-start gap-2 cursor-default"
  style={{ color: 'var(--text-disabled)' }}
>
  <div
    className="
      flex items-center justify-center shrink-0
      mt-0.5
      w-4 h-4 rounded
      bg-[var(--bg-disabled)] border border-[var(--border-disabled_subtle)]
    "
    tabIndex={-1}
    role="checkbox"
    aria-checked="false"
    aria-disabled="true"
  />
  <span className="text-sm leading-5 font-medium">Unavailable option</span>
</label>


{/* === Disabled Checkbox — Checked === */}
<label
  className="inline-flex items-start gap-2 cursor-default"
  style={{ color: 'var(--text-disabled)' }}
>
  <div
    className="
      flex items-center justify-center shrink-0
      mt-0.5
      w-4 h-4 rounded
      bg-[var(--bg-brand-secondary-solid_hover)]/50 border border-[var(--border-brand-secondary-solid)]/50
    "
    tabIndex={-1}
    role="checkbox"
    aria-checked="true"
    aria-disabled="true"
  >
    <svg width="10" height="8" viewBox="0 0 10 8" fill="none" className="opacity-80">
      <path d="M9 1L3.5 6.5L1 4" stroke="white" strokeWidth="1.5" strokeLinecap="round" strokeLinejoin="round" />
    </svg>
  </div>
  <span className="text-sm leading-5 font-medium">Locked option</span>
</label>


{/* === Error Checkbox — Unchecked === */}
<label
  className="inline-flex items-start gap-2 cursor-pointer"
  style={{ color: 'var(--text-primary)' }}
>
  <div
    className="
      flex items-center justify-center shrink-0
      mt-0.5
      w-4 h-4 rounded
      bg-[var(--bg-primary)] border border-[var(--border-error-solid)]
      hover:border-[var(--border-error-solid)]
      focus-visible:ring-4 focus-visible:ring-[var(--ring-error-primary)]
      transition-all duration-150 ease-out
    "
    tabIndex={0}
    role="checkbox"
    aria-checked="false"
  />
  <span className="text-sm leading-5 font-medium">Required field</span>
</label>


{/* === Checkbox without label (standalone) === */}
<div
  className={`
    flex items-center justify-center shrink-0
    w-4 h-4 rounded
    border transition-all duration-150 ease-out cursor-pointer
    ${checked
      ? 'bg-[var(--bg-brand-secondary-solid)] border-[var(--border-brand-secondary-solid)] hover:bg-[var(--bg-brand-secondary-solid_hover)] hover:border-[var(--border-brand-secondary-solid)]'
      : 'bg-[var(--bg-primary)] border-[var(--border-primary)] hover:border-[var(--border-brand-secondary-solid)]'
    }
  `}
  tabIndex={0}
  role="checkbox"
  aria-checked={checked}
  onClick={toggle}
>
  {checked && (
    <svg width="10" height="8" viewBox="0 0 10 8" fill="none">
      <path d="M9 1L3.5 6.5L1 4" stroke="white" strokeWidth="1.5" strokeLinecap="round" strokeLinejoin="round" />
    </svg>
  )}
</div>
```

---

## Size Quick Reference

| Size | Box W×H | Gap | Label | Sub-text | Tailwind Box |
|------|---------|-----|-------|----------|-------------|
| Small | 16×16 | 8px | 12px/18px/500 | 12px/18px/400 | `w-4 h-4` |
| Medium | 16×16 | 8px | 14px/20px/500 | 14px/20px/400 | `w-4 h-4` |
| Large | 20×20 | 12px | 16px/24px/500 | 14px/20px/400 | `w-5 h-5` |

---

## Color Reference

| Token | Value | Usage |
|-------|-------|-------|
| `var(--border-primary)` | #BDC8D3 | Default unchecked border |
| `var(--border-brand-secondary-solid)` | #0BA5A7 | Checked border |
| `var(--border-brand-secondary-solid)` | #0BA5A7 | Hover border (unchecked + checked) |
| `var(--border-disabled_subtle)` | #CCD6DC | Disabled unchecked border |
| `var(--border-error-solid)` | #F02C00 | Error state border |
| `var(--bg-brand-secondary-solid)` | #098486 | Checked background |
| `var(--bg-brand-secondary-solid_hover)` | #0BA5A7 | Checked hover background |
| `var(--bg-disabled)` | #E8ECF1 | Disabled unchecked background |
| `var(--fg-brand-secondary-disabled)` | #0BA5A780 | Disabled checked background (50% opacity) |
| `var(--bg-error-solid)` | #F02C00 | Error checked background |
| `var(--text-primary)` | #353E5A | Label text |
| `var(--text-tertiary)` | #6B748E | Sub-text |
| `var(--text-disabled)` | #8EA1AF | Disabled label/sub-text |
| `var(--ring-brand-secondary)` | `0 0 0 4px rgba(11,165,167,0.24), 0 1px 2px 0 rgba(73,88,131,0.04)` | Focus ring (normal) |
| `var(--ring-error-primary)` | `0 0 0 4px rgba(240,44,0,0.24), 0 1px 2px 0 rgba(73,88,131,0.04)` | Focus ring (error) |

---

## CRITICAL Rules

1. **Box border-radius is ALWAYS 4px** (radius-2) — NOT rounded-full, NOT 6px, NOT 8px.
2. **Small and Medium share the same 16×16 box size.** Only Large uses 20×20.
3. **Default unchecked border is `var(--border-primary)` (#BDC8D3)** — NOT `var(--border-disabled_subtle)` (#CCD6DC) (that's the disabled border).
4. **Checked and indeterminate states both use `var(--bg-brand-secondary-solid)` (#098486) teal background** with matching border.
5. **Hover always changes to `var(--bg-brand-secondary-solid_hover)` (#0BA5A7)** (lighter teal) — for both unchecked border and checked background.
6. **Focus ring is always teal** (`var(--ring-brand-secondary)`) except in error state where it's red (`var(--ring-error-primary)`).
7. **Indeterminate is a 12×2px white dash**, NOT a square or half-fill. It uses the same teal background as checked.
8. **Disabled + checked uses 50% opacity teal** (`var(--fg-brand-secondary-disabled)` / #0BA5A780) — NOT a gray fill.
9. **Error state overrides border and fill to `var(--border-error-solid)` (#F02C00)** — the error checkbox when checked has a red background, not teal.
10. **The visual checkbox icon is an SVG stroke path** (white `stroke`, NOT `fill`) — `strokeWidth: 1.5` for checkmark, `strokeWidth: 2` for dash.
11. **Transition is 150ms ease-out** on `border-color`, `box-shadow`, and `background-color`.
12. **Checkboxes WITHOUT labels** (standalone) are used in Grid selection columns and Select Input options — they render just the box with no text, same styling rules apply.
13. **When used inside Grid, List Item, or Select Inputs**, always use THIS checkbox component — NEVER build a different checkbox. See `grid.md`, `list-item.md`, `select-inputs.md`.
14. **⚠️ VERTICAL ALIGNMENT: The checkbox box MUST have `mt-0.5` (margin-top: 2px)** when a label or sub-text is present AND the size is Medium or Large. This nudges the 16px/20px box down to vertically align with the first line of text. WITHOUT this, the checkbox will appear too high relative to the label. Small size does NOT get this margin.
