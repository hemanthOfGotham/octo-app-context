# Expansion Panel (Accordion) Component — Strict Specification

When building any accordion, collapsible section, expandable panel, or FAQ toggle, follow these specifications exactly. Do NOT use default Tailwind accordion, shadcn accordion, or any other collapsible system.

> **TOKEN RULE:** Every colour in this file is expressed as a CSS variable token.
> In **tables / prose** write `var(--token)` (#HEX).
> In **Tailwind classes** write `bg-[var(--bg-token)]`, `text-[var(--text-token)]`, `border-[var(--border-token)]`, etc.
> In **inline styles** write `'var(--token)'`.
> Do NOT use raw hex values outside the parenthetical annotation.

---

## Expansion Panel Anatomy

A clickable header that expands/collapses a body content area:

```
Collapsed:
┌──────────────────────────────────────┐
│  ▸  Panel Title                      │  ← Chevron + title
└──────────────────────────────────────┘

Expanded:
┌──────────────────────────────────────┐
│  ▾  Panel Title                      │  ← Chevron rotated 90°
├──────────────────────────────────────┤
│  Body content goes here...           │  ← Collapsible body
│                                      │
└──────────────────────────────────────┘
```

- Chevron arrow starts pointing right (▸), rotates 90° clockwise when expanded (▾)
- Body animates height from 0 to auto

---

## Types

### Default
- Bottom border separator between panels
- No outer border
- Hover/focus changes header background

### Card
- Full border around the entire panel
- Rounded corners
- Hover/focus changes border color to teal

---

## Sizes

### Small (SM)
| Element | Property | Value | Tailwind |
|---------|----------|-------|----------|
| Header | Padding | 6px vertical, 16px horizontal | `py-1.5 px-4` |
| Header | Padding left | 8px | `pl-2` |
| Header | Gap | 4px | `gap-1` |
| Title | Typography | 12px / 18px / weight 600 | `text-xs leading-[18px]` + `style={{ fontWeight: 600 }}` |
| Chevron | Size | 16px | `w-4 h-4` |
| Body | Padding | 8px vertical, 16px horizontal | `py-2 px-4` |
| Card variant | Border radius | 4px | `rounded` |

### Medium (MD) — Default
| Element | Property | Value | Tailwind |
|---------|----------|-------|----------|
| Header | Padding | 12px vertical, 16px horizontal | `py-3 px-4` |
| Header | Padding left | 12px | `pl-3` |
| Header | Gap | 4px | `gap-1` |
| Title | Typography | 14px / 20px / weight 600 | `text-sm leading-5` + `style={{ fontWeight: 600 }}` |
| Chevron | Size | 24px | `w-6 h-6` |
| Body | Padding | 12px vertical, 16px horizontal | `py-3 px-4` |
| Card variant | Border radius | 8px | `rounded-lg` |

### Large (LG)
| Element | Property | Value | Tailwind |
|---------|----------|-------|----------|
| Header | Padding | 12px vertical, 24px horizontal | `py-3 px-6` |
| Header | Padding left | 16px | `pl-4` |
| Header | Gap | 8px | `gap-2` |
| Title | Typography | 16px / 24px / weight 600 | `text-base leading-6` + `style={{ fontWeight: 600 }}` |
| Chevron | Size | 24px | `w-6 h-6` |
| Body | Padding | 12px vertical, 24px horizontal | `py-3 px-6` |
| Card variant | Border radius | 8px | `rounded-lg` |

---

## Colors

| Element | Value | Tailwind |
|---------|-------|----------|
| Header background | `var(--bg-primary)` (#FFFFFF) | `bg-[var(--bg-primary)]` |
| Header text | `var(--text-primary)` (#353E5A) | `text-[var(--text-primary)]` |
| Chevron color | Inherits text color | — |
| Default border-bottom | `var(--border-tertiary)` (#E8ECF1) | `border-b border-[var(--border-tertiary)]` |
| Card border | `var(--border-secondary)` (#CCD6DC) | `border border-[var(--border-secondary)]` |

---

## States

### Default Type
| State | Header Background | Tailwind |
|-------|------------------|----------|
| Default | `var(--bg-primary)` (#FFFFFF) | `bg-[var(--bg-primary)]` |
| Hover | `var(--bg-secondary)` (#F1F4F7) | `hover:bg-[var(--bg-secondary)]` |
| Focus-visible | `var(--bg-secondary)` (#F1F4F7) | `focus-visible:bg-[var(--bg-secondary)]` |

### Card Type
| State | Border Color | Tailwind |
|-------|-------------|----------|
| Default | `var(--border-secondary)` (#CCD6DC) | `border-[var(--border-secondary)]` |
| Hover | `var(--border-brand-secondary-solid)` (#0BA5A7) | `hover:border-[var(--border-brand-secondary-solid)]` |
| Focus-visible | `var(--border-brand-secondary-solid)` (#0BA5A7) | `focus-visible:border-[var(--border-brand-secondary-solid)]` |

---

## Chevron Animation

| Property | Value | Tailwind |
|----------|-------|----------|
| Collapsed | rotate(0deg) — pointing right | `rotate-0` |
| Expanded | rotate(90deg) — pointing down | `rotate-90` |
| Transition | transform 150ms ease-out | `transition-transform duration-150 ease-out` |

---

## Body Animation

| Property | Value |
|----------|-------|
| Collapsed | height: 0, overflow: hidden |
| Expanded | height: auto |
| Transition | height 300ms ease-in |

---

## Implementation Pattern for Lovable

```jsx
{/* === Default Type — Medium Size === */}
<div>
  {/* Panel 1 — Expanded */}
  <div className="border-b border-[var(--border-tertiary)]">
    <button
      className="
        flex items-center gap-1 w-full
        py-3 px-4 pl-3
        bg-[var(--bg-primary)] text-left
        cursor-pointer outline-none
        hover:bg-[var(--bg-secondary)]
        focus-visible:bg-[var(--bg-secondary)]
        transition-colors duration-150 ease-out
      "
      aria-expanded="true"
      onClick={() => toggle(0)}
    >
      {/* Chevron — rotated when expanded */}
      <ChevronRight className="w-6 h-6 text-[var(--text-primary)] rotate-90 transition-transform duration-150 ease-out" />
      <span
        className="text-sm leading-5 text-[var(--text-primary)]"
        style={{ fontWeight: 600 }}
      >
        What is your return policy?
      </span>
    </button>
    {/* Body — expanded */}
    <div className="py-3 px-4">
      <p className="text-sm leading-5 font-normal text-[var(--text-secondary)]">
        You can return items within 30 days of purchase for a full refund.
      </p>
    </div>
  </div>

  {/* Panel 2 — Collapsed */}
  <div className="border-b border-[var(--border-tertiary)]">
    <button
      className="
        flex items-center gap-1 w-full
        py-3 px-4 pl-3
        bg-[var(--bg-primary)] text-left
        cursor-pointer outline-none
        hover:bg-[var(--bg-secondary)]
        focus-visible:bg-[var(--bg-secondary)]
        transition-colors duration-150 ease-out
      "
      aria-expanded="false"
      onClick={() => toggle(1)}
    >
      <ChevronRight className="w-6 h-6 text-[var(--text-primary)] rotate-0 transition-transform duration-150 ease-out" />
      <span
        className="text-sm leading-5 text-[var(--text-primary)]"
        style={{ fontWeight: 600 }}
      >
        How do I track my order?
      </span>
    </button>
    {/* Body hidden when collapsed */}
  </div>
</div>


{/* === Card Type — Medium Size === */}
<div className="space-y-2">
  {/* Card Panel — Expanded */}
  <div className="
    border border-[var(--border-secondary)] rounded-lg overflow-hidden
    hover:border-[var(--border-brand-secondary-solid)]
    transition-colors duration-150 ease-out
  ">
    <button
      className="
        flex items-center gap-1 w-full
        py-3 px-4 pl-3
        bg-[var(--bg-primary)] text-left
        cursor-pointer outline-none
      "
      aria-expanded="true"
    >
      <ChevronRight className="w-6 h-6 text-[var(--text-primary)] rotate-90 transition-transform duration-150 ease-out" />
      <span
        className="text-sm leading-5 text-[var(--text-primary)]"
        style={{ fontWeight: 600 }}
      >
        Billing Information
      </span>
    </button>
    <div className="py-3 px-4">
      <p className="text-sm leading-5 font-normal text-[var(--text-secondary)]">
        Card content here...
      </p>
    </div>
  </div>

  {/* Card Panel — Collapsed */}
  <div className="
    border border-[var(--border-secondary)] rounded-lg overflow-hidden
    hover:border-[var(--border-brand-secondary-solid)]
    transition-colors duration-150 ease-out
  ">
    <button
      className="
        flex items-center gap-1 w-full
        py-3 px-4 pl-3
        bg-[var(--bg-primary)] text-left
        cursor-pointer outline-none
      "
      aria-expanded="false"
    >
      <ChevronRight className="w-6 h-6 text-[var(--text-primary)] rotate-0 transition-transform duration-150 ease-out" />
      <span
        className="text-sm leading-5 text-[var(--text-primary)]"
        style={{ fontWeight: 600 }}
      >
        Shipping Details
      </span>
    </button>
  </div>
</div>


{/* === Small Size — Default Type === */}
<div className="border-b border-[var(--border-tertiary)]">
  <button
    className="
      flex items-center gap-1 w-full
      py-1.5 px-4 pl-2
      bg-[var(--bg-primary)] text-left cursor-pointer outline-none
      hover:bg-[var(--bg-secondary)]
      transition-colors duration-150 ease-out
    "
    aria-expanded="true"
  >
    <ChevronRight className="w-4 h-4 text-[var(--text-primary)] rotate-90 transition-transform duration-150 ease-out" />
    <span
      className="text-xs leading-[18px] text-[var(--text-primary)]"
      style={{ fontWeight: 600 }}
    >
      Advanced Settings
    </span>
  </button>
  <div className="py-2 px-4">
    <p className="text-xs leading-[18px] font-normal text-[var(--text-secondary)]">
      Small panel content
    </p>
  </div>
</div>
```

### Size Quick Reference

| Size | Header Pad V/H | PL | Gap | Title | Chevron | Body Pad | Card Radius |
|------|---------------|-----|-----|-------|---------|----------|------------|
| SM | 6px / 16px | 8px | 4px | 12px/18px/600 | 16px | 8px 16px | 4px |
| MD | 12px / 16px | 12px | 4px | 14px/20px/600 | 24px | 12px 16px | 8px |
| LG | 12px / 24px | 16px | 8px | 16px/24px/600 | 24px | 12px 24px | 8px |

### Key Implementation Rules

1. **Chevron starts pointing RIGHT** and rotates 90° clockwise when expanded
2. **Default type uses border-bottom** `var(--border-tertiary)` (#E8ECF1) between panels — no outer border
3. **Card type uses full border** `var(--border-secondary)` (#CCD6DC) with rounded corners and `overflow-hidden`
4. **Card type hover changes border to teal** `var(--border-brand-secondary-solid)` (#0BA5A7), default type hover changes header bg
5. **Title weight is always 600** — use inline `style={{ fontWeight: 600 }}`
6. **Body animates height** from 0 to auto (300ms ease-in) — in Lovable, use CSS transitions or `animate-accordion-down`
7. **Chevron transition is 150ms** ease-out — faster than body animation
8. **Header is full-width clickable** — `w-full text-left cursor-pointer`
9. **Use `aria-expanded`** attribute on the header button for accessibility
10. **Padding-left is separate** from main horizontal padding — it's smaller to position the chevron closer to the edge
