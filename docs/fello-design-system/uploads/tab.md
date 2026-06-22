# Tab Component — Strict Specification

When building any tab group, tab navigation, or segmented control, follow these specifications exactly. Do NOT use default Tailwind tab styles, shadcn tabs, or any other tab system.

> **TOKEN RULE:** Every color in the code examples below uses CSS variable tokens or Tailwind palette names — never raw hex. When implementing, copy the token references exactly. See `design-tokens.md` for definitions.

---

## Tab Anatomy

A tab group consists of a header row of tab items and a body panel:

```
[Tab 1]  [Tab 2]  [Tab 3 (active)]  [Tab 4]
─────────────────────────────────────────────
[Tab panel content for active tab]
```

- Header: `display: flex`, `align-items: center`, `justify-content: flex-start`
- Each tab: `display: flex`, `align-items: center`, `justify-content: center`
- Active state shown via indicator (varies by type)
- Tabs can contain leading icons and trailing badges

---

## Tab Types

There are 3 tab types, each with a distinct active indicator style:

### Primary
2px orange indicator line overlaying a 1px gray separator — both at `bottom: 0` of the header (NO `border-bottom` on header)

### Secondary
Pill border overlapping container border via z-index/negative-margin — borders share the same edge, NOT stacked

### Tertiary
Background color change (no border, no underline)

---

## Type Specifications

### Primary Tabs

| Property | Value | Tailwind |
|----------|-------|----------|
| Height | 40px | `h-10` |
| Padding | 10px vertical, 4px horizontal | `py-2.5 px-1` |
| Gap between tabs | 16px | `gap-4` |
| Gap within tab (icon ↔ label) | 6px | `gap-1.5` |
| Border radius | None | — |
| Typography | 14px / 20px / weight 500 | `text-sm leading-5 font-medium` |

**Gray Separator Line (on the header):**
| Property | Value |
|----------|-------|
| Type | 1px tall absolutely-positioned div (NOT `border-bottom`) |
| Color | `var(--border-tertiary)` (#E8ECF1) |
| Position | Absolute, bottom: 0, left: 0, width: 100% |

**Active Indicator (on the active tab):**
| Property | Value |
|----------|-------|
| Type | 2px tall absolutely-positioned span, full width |
| Color | `var(--border-brand-solid)` (#FF725C) |
| Position | Absolute, bottom: 0, left: 0, z-index: 1 |
| Alignment | Overlays the gray separator at the exact same `bottom: 0` pixel row |

**Colors:**
| State | Background | Text |
|-------|-----------|------|
| Default | Transparent | Inherits — `var(--text-primary)` (#353E5A) |
| Hover | `var(--bg-secondary)` (#F1F4F7) | Inherits |
| Focus-visible | `var(--bg-tertiary)` (#E8ECF1) | Inherits |
| Active | Transparent | Inherits |
| Active + Hover | `var(--bg-brand-primary_alt)` (#FDE7E3) | Inherits |
| Active + Focus | `var(--bg-brand-primary)` (#FDF1EF) | Inherits |
| Disabled | Transparent, 40% opacity | Inherits |

---

### Secondary Tabs

| Property | Value | Tailwind |
|----------|-------|----------|
| Height | 32px | `h-8` |
| Padding | 6px vertical, 12px horizontal | `py-1.5 px-3` |
| Gap between tabs | 0 (tabs sit flush inside container) | `gap-0` |
| Gap within tab (icon ↔ label) | 6px | `gap-1.5` |
| Border radius | 6px | `rounded-md` |
| Border | 1px solid transparent (default) | `border border-transparent` |
| Typography | 14px / 20px / weight 500 | `text-sm leading-5 font-medium` |

**Container (Header):**
| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `var(--bg-secondary)` (#F1F4F7) | `bg-[var(--bg-secondary)]` |
| Border | 1px solid `var(--border-secondary-solid)` (#8EA1AF) | `border border-[var(--border-secondary-solid)]` |
| Border radius | 6px | `rounded-md` |
| Padding | 0 (tabs fill the container) | — |

**⚠️ BORDER ALIGNMENT TECHNIQUE:** The container has a real 1px `border`. The active pill uses `margin: -1px` to expand outward and overlap the container border on all sides. The active pill's own `border` (same color `var(--border-secondary-solid)` (#8EA1AF)) then visually replaces the container border where they overlap, creating a seamless shared-edge effect. Inactive pills do NOT have negative margin — only the active pill does.

**Colors:**
| State | Background | Border |
|-------|-----------|--------|
| Default | `var(--bg-secondary)` (#F1F4F7) | Transparent |
| Hover | `var(--bg-secondary_hover)` (#E8ECF1) | Transparent |
| Focus-visible | `var(--bg-quaternary)` (#CCD6DC) | Transparent |
| Active | `var(--bg-primary)` (#FFFFFF) | `var(--border-secondary-solid)` (#8EA1AF) |
| Active + Hover | `var(--bg-secondary)` (#F1F4F7) | `var(--border-secondary-solid)` (#8EA1AF) |
| Active + Focus | `var(--bg-tertiary)` (#E8ECF1) | `var(--border-secondary-solid)` (#8EA1AF) |
| Disabled | Inherits, 40% opacity | — |

---

### Tertiary Tabs

| Property | Value | Tailwind |
|----------|-------|----------|
| Height | 32px | `h-8` |
| Padding | 6px | `p-1.5` |
| Gap between tabs | 4px | `gap-1` |
| Gap within tab (icon ↔ label) | 6px | `gap-1.5` |
| Border radius | 6px | `rounded-md` |
| Typography | 14px / 20px / weight 500 | `text-sm leading-5 font-medium` |

**Container (Header):**
| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `var(--bg-primary)` (#FFFFFF) | `bg-[var(--bg-primary)]` |
| Border radius | 6px | `rounded-md` |
| Border | None | — |

**Colors:**
| State | Background | Box Shadow |
|-------|-----------|------------|
| Default | Transparent | None |
| Hover | `var(--bg-secondary)` (#F1F4F7) | None |
| Focus-visible | `var(--bg-secondary)` (#F1F4F7) | `var(--ring-gray-primary)` |
| Active | `var(--bg-brand-secondary)` (#E7FDFD) | None |
| Active + Hover | `var(--bg-brand-secondary_alt)` (#CAFBFB) | None |
| Active + Focus | `var(--bg-brand-secondary_alt)` (#CAFBFB) | `var(--ring-brand-secondary)` |
| Disabled | Transparent, 40% opacity | None |

---

## Icon Support

### Leading Icon
| Property | Value |
|----------|-------|
| Size | 16px × 16px |
| Display | Inline-flex, centered |
| Position | Before the label text |

### Trailing Badge (Count)

The count badge is **identical across all three tab types** (primary, secondary, tertiary). It always uses:

| Badge Property | Value |
|---------------|-------|
| `type` | `grey` |
| `fillType` | `light` |
| `shape` | `rounded` (fully circular pill) |
| `size` | `small` |
| Background | `var(--bg-tertiary)` (#E8ECF1) |
| Text color | `var(--text-primary)` (#353E5A) |
| Typography | 10px / 16px / weight 500 (caption-xs) |
| Padding | 0 6px |
| Min-width | 16px |
| Border-radius | 9999px (fully rounded) |
| Position | After the label text |

**IMPORTANT:** Do NOT use a teal/brand-colored badge for secondary or tertiary tabs. Do NOT use a filled/dark badge. The badge is ALWAYS grey-light (gray pill) regardless of which tab type it appears in.

```jsx
{/* Trailing count badge — same for primary, secondary AND tertiary tabs */}
<span
  className="inline-flex items-center justify-center rounded-full"
  style={{
    background: 'var(--bg-tertiary)',
    color: 'var(--text-primary)',
    fontSize: '10px',
    lineHeight: '16px',
    fontWeight: 500,
    padding: '0 6px',
    minWidth: '16px',
  }}
>
  9+
</span>
```

### Icon-Only Mode
| Property | Value |
|----------|-------|
| Padding | 8px (square padding) |
| Label | Hidden — only icon shown |
| Tooltip | Shows label text on hover |
| Accessibility | `aria-label` required |

---

## Disabled State

| Property | Value | Tailwind |
|----------|-------|----------|
| Opacity | 40% | `opacity-40` |
| Cursor | Default | `cursor-default` |
| Interaction | No hover/focus effects | `pointer-events-none` |

---

## Accessibility

| Property | Value |
|----------|-------|
| Header role | `tablist` |
| Tab role | `tab` |
| Panel role | `tabpanel` |
| `aria-selected` | Reflects active state |
| `aria-disabled` | Reflects disabled state |
| Keyboard | Arrow keys navigate, Enter/Space select |

---

## Implementation Pattern for Lovable

### ⚠️ PRIMARY TABS — Indicator Alignment (CRITICAL)

The active tab's 2px orange indicator line must sit **exactly on the same visual line** as the gray separator. The header container itself has **NO border-bottom**. Instead:

1. The header is `position: relative` with a gray `::after` pseudo-element acting as the separator line
2. Each active tab has its own `::after` at `bottom: 0` with the orange indicator
3. The tab's orange line **overlays** the gray line at the same pixel row because both sit at `bottom: 0` of the same flex row

In React/Tailwind, achieve this by placing the gray separator as an absolutely-positioned element at the bottom of the header, and the active indicator as an absolutely-positioned element at the bottom of the tab. Both are at the same `bottom: 0`, so the orange overlays the gray perfectly.

### ⚠️ SECONDARY TABS — Border Alignment (CRITICAL)

The container has a real 1px `border` in `var(--border-secondary-solid)` (#8EA1AF). The active pill uses `margin: -1px` to push outward by 1px on all sides, expanding to overlap the container border. The active pill's own 1px `border` (same `var(--border-secondary-solid)` (#8EA1AF) color) then visually **replaces** the container border where they overlap — creating a seamless shared edge with no double-border artifacts.

```jsx
{/* === Primary Tabs === */}
<div>
  {/* Tab Header — NO border-bottom on the container itself */}
  <div className="relative flex items-center gap-4" role="tablist">
    {/* Gray separator line — sits at bottom of header, behind everything */}
    <div className="absolute bottom-0 left-0 w-full h-px bg-[var(--border-tertiary)]" />

    {/* Inactive tab */}
    <button
      role="tab"
      aria-selected="false"
      className="
        relative flex items-center justify-center gap-1.5
        h-10 py-2.5 px-1
        text-sm leading-5 font-medium text-[var(--text-primary)]
        bg-transparent border-none outline-none cursor-pointer
        hover:bg-[var(--bg-secondary)]
        focus-visible:bg-[var(--bg-tertiary)]
        transition-colors duration-150 ease-out
      "
    >
      Overview
    </button>

    {/* Active tab — indicator OVERLAYS the gray line at bottom: 0 */}
    <button
      role="tab"
      aria-selected="true"
      className="
        relative flex items-center justify-center gap-1.5
        h-10 py-2.5 px-1
        text-sm leading-5 font-medium text-[var(--text-primary)]
        bg-transparent border-none outline-none cursor-pointer
        hover:bg-[var(--bg-brand-primary_alt)]
        focus-visible:bg-[var(--bg-brand-primary)]
        transition-colors duration-150 ease-out
      "
    >
      Details
      {/* Active indicator — 2px orange line at bottom: 0, same position as gray line */}
      <span className="absolute bottom-0 left-0 w-full h-0.5 bg-[var(--border-brand-solid)] z-[1]" />
    </button>

    {/* Disabled tab */}
    <button
      role="tab"
      aria-disabled="true"
      className="
        relative flex items-center justify-center gap-1.5
        h-10 py-2.5 px-1
        text-sm leading-5 font-medium text-[var(--text-primary)]
        bg-transparent border-none outline-none
        opacity-40 cursor-default pointer-events-none
      "
    >
      Settings
    </button>
  </div>

  {/* Tab Panel */}
  <div role="tabpanel" className="py-4">
    Panel content here
  </div>
</div>


{/* === Secondary Tabs === */}
<div>
  {/* Tab Header — real border on container */}
  <div
    className="inline-flex items-center rounded-md bg-[var(--bg-secondary)] border border-[var(--border-secondary-solid)]"
    role="tablist"
  >
    {/* Active tab — margin: -1px expands pill to overlap container border */}
    <button
      role="tab"
      aria-selected="true"
      className="
        flex items-center justify-center gap-1.5
        h-8 py-1.5 px-3
        -m-px
        rounded-md
        text-sm leading-5 font-medium text-[var(--text-primary)]
        bg-[var(--bg-primary)]
        border border-[var(--border-secondary-solid)]
        outline-none cursor-pointer z-[1]
        hover:bg-[var(--bg-secondary)]
        focus-visible:bg-[var(--bg-tertiary)]
        transition-colors duration-150 ease-out
      "
    >
      Tab One
    </button>

    {/* Inactive tab */}
    <button
      role="tab"
      aria-selected="false"
      className="
        flex items-center justify-center gap-1.5
        h-8 py-1.5 px-3
        rounded-md
        text-sm leading-5 font-medium text-[var(--text-primary)]
        bg-[var(--bg-secondary)]
        border border-transparent
        outline-none cursor-pointer
        hover:bg-[var(--bg-secondary_hover)]
        focus-visible:bg-[var(--bg-quaternary)]
        transition-colors duration-150 ease-out
      "
    >
      Tab Two
    </button>

    {/* Inactive tab */}
    <button
      role="tab"
      aria-selected="false"
      className="
        flex items-center justify-center gap-1.5
        h-8 py-1.5 px-3
        rounded-md
        text-sm leading-5 font-medium text-[var(--text-primary)]
        bg-[var(--bg-secondary)]
        border border-transparent
        outline-none cursor-pointer
        hover:bg-[var(--bg-secondary_hover)]
        focus-visible:bg-[var(--bg-quaternary)]
        transition-colors duration-150 ease-out
      "
    >
      Tab Three
    </button>
  </div>

  <div role="tabpanel" className="py-4">
    Panel content here
  </div>
</div>


{/* === Tertiary Tabs === */}
<div>
  {/* Tab Header */}
  <div
    className="inline-flex items-center gap-1 bg-[var(--bg-primary)] rounded-md"
    role="tablist"
  >
    {/* Inactive tab */}
    <button
      role="tab"
      aria-selected="false"
      className="
        flex items-center justify-center gap-1.5
        h-8 p-1.5
        rounded-md
        text-sm leading-5 font-medium text-[var(--text-primary)]
        bg-transparent border-none outline-none cursor-pointer
        hover:bg-[var(--bg-secondary)]
        focus-visible:bg-[var(--bg-secondary)] focus-visible:shadow-[var(--ring-gray-primary)]
        transition-colors duration-150 ease-out
      "
    >
      Week
    </button>

    {/* Active tab */}
    <button
      role="tab"
      aria-selected="true"
      className="
        flex items-center justify-center gap-1.5
        h-8 p-1.5
        rounded-md
        text-sm leading-5 font-medium text-[var(--text-primary)]
        bg-[var(--bg-brand-secondary)] border-none outline-none cursor-pointer
        hover:bg-[var(--bg-brand-secondary_alt)]
        focus-visible:bg-[var(--bg-brand-secondary_alt)] focus-visible:shadow-[var(--ring-brand-secondary)]
        transition-colors duration-150 ease-out
      "
    >
      Month
    </button>

    {/* Inactive tab */}
    <button
      role="tab"
      aria-selected="false"
      className="
        flex items-center justify-center gap-1.5
        h-8 p-1.5
        rounded-md
        text-sm leading-5 font-medium text-[var(--text-primary)]
        bg-transparent border-none outline-none cursor-pointer
        hover:bg-[var(--bg-secondary)]
        focus-visible:bg-[var(--bg-secondary)] focus-visible:shadow-[var(--ring-gray-primary)]
        transition-colors duration-150 ease-out
      "
    >
      Year
    </button>
  </div>

  <div role="tabpanel" className="py-4">
    Panel content here
  </div>
</div>
```

### Tab Type Quick Reference

| Type | Height | Active Indicator | Container BG | Active BG |
|------|--------|-----------------|-------------|-----------|
| Primary | 40px | 2px bottom line `var(--border-brand-solid)` (#FF725C) overlaying 1px gray `var(--border-tertiary)` (#E8ECF1) separator | None (separator is a child div, NOT border-bottom) | Transparent |
| Secondary | 32px | White bg + `var(--border-secondary-solid)` (#8EA1AF) border (active pill uses `-m-px` to overlap container border) | `var(--bg-secondary)` (#F1F4F7) with `border border-[var(--border-secondary-solid)]` | `var(--bg-primary)` (#FFFFFF) |
| Tertiary | 32px | `var(--bg-brand-secondary)` (#E7FDFD) teal bg | White (no border) | `var(--bg-brand-secondary)` (#E7FDFD) |

### Key Implementation Rules

1. **Primary tab header has NO `border-bottom`** — the gray separator is an absolutely-positioned 1px `var(--border-tertiary)` (#E8ECF1) div at `bottom: 0` inside the header. The active tab's 2px `var(--border-brand-solid)` (#FF725C) indicator also sits at `bottom: 0` with `z-index: 1`, so it overlays the gray line perfectly at the same pixel row. NEVER use `border-b` on the header — it creates a gap.
2. **Secondary container has a real `border border-[var(--border-secondary-solid)]`**. The active pill uses `-m-px` (margin: -1px) to expand outward and overlap the container border on all sides. The active pill's own `border border-[var(--border-secondary-solid)]` visually replaces the container border where they overlap. Only the ACTIVE pill gets `-m-px` — inactive pills have no negative margin.
3. **Tertiary tabs use `var(--bg-brand-secondary)` (#E7FDFD) background** as the active indicator — no border, no underline
4. **All tabs use 14px/20px/500 typography** — same across all 3 types
5. **Gap differs by type**: Primary = 16px (`gap-4`), Secondary = 0 (flush), Tertiary = 4px (`gap-1`)
6. **Disabled tabs have 40% opacity** and `pointer-events-none`
7. **Primary hover on active** uses `var(--bg-brand-primary_alt)` (#FDE7E3), on inactive uses `var(--bg-secondary)` (#F1F4F7)
8. **Secondary hover on active** uses `var(--bg-secondary)` (#F1F4F7), on inactive uses `var(--bg-secondary_hover)` (#E8ECF1)
9. **Tertiary hover on active** uses `var(--bg-brand-secondary_alt)` (#CAFBFB), on inactive uses `var(--bg-secondary)` (#F1F4F7)
10. **Focus rings differ**: Tertiary has visible box-shadow rings; Primary/Secondary use background change only
11. **Icon-only tabs** use square 8px padding and require `aria-label`
12. **Use `role="tablist"`, `role="tab"`, `role="tabpanel"`** with `aria-selected` for accessibility
