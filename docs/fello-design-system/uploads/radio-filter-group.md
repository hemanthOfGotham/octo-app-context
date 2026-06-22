# Radio Filter Group Component — Strict Specification

When building any segmented control, button group selector, or toggle pill group for filtering, follow these specifications exactly. Do NOT use default Tailwind button groups, shadcn toggle groups, or any other segmented control system.

> **TOKEN RULE:** Every colour in this file is expressed as a CSS custom-property token — `var(--token)` — with the raw hex in parentheses only as a comment. When writing Tailwind classes use the `[var(--token)]` syntax (e.g. `bg-[var(--bg-secondary)]`). Never hard-code a hex value.

---

## Radio Filter Group Anatomy

A row of pill-style buttons where exactly one can be selected at a time:

```
Primary Variant:
┌─────────────────────────────────────────┐
│ ┌──────────┐┌──────────┐┌──────────┐   │
│ │  Option 1 ││ *Active* ││  Option 3 │   │  ← Gray container with border
│ └──────────┘└──────────┘└──────────┘   │
└─────────────────────────────────────────┘
                  ↑ Selected = white bg + border

Secondary Variant:
┌─────────────────────────────────────────┐
│  Option 1   ┌─────────┐  Option 3      │  ← Gray container with padding
│             │ *Active* │               │
│             └─────────┘               │
└─────────────────────────────────────────┘
                  ↑ Selected = teal bg + white text
```

Two distinct variants:
- **Primary**: Outlined container, selected item gets white bg + border
- **Secondary**: Padded container, selected item gets teal bg + white text

---

## Primary Variant (`felloRadioFilterGroup`)

### Container

| Property | Value | Tailwind |
|----------|-------|----------|
| Display | Inline-flex | `inline-flex` |
| Border | 1px solid `var(--border-primary)` (#BDC8D3) | `border border-[var(--border-primary)]` |
| Border radius | 6px (radius-3) | `rounded-md` |
| Background | `var(--bg-secondary)` (#F1F4F7) | `bg-[var(--bg-secondary)]` |
| Overflow | Hidden | `overflow-hidden` |

### Filter Item (Unselected)

| Property | Value | Tailwind |
|----------|-------|----------|
| Border | 1px solid transparent | `border border-transparent` |
| Border radius | 6px | `rounded-md` |
| Padding | 6px 12px | `py-1.5 px-3` |
| Typography | 14px / 20px / weight 500 (caption-md) | `text-sm leading-5` + `style={{ fontWeight: 500 }}` |
| Color | `var(--text-primary)` (#353E5A) | `text-[var(--text-primary)]` |
| Cursor | Pointer | `cursor-pointer` |
| Transition | background-color 150ms ease-out | `transition-colors duration-150 ease-out` |

### Filter Item (Hover — Unselected, Not Disabled)

| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `var(--bg-secondary_hover)` (#E8ECF1) | `hover:bg-[var(--bg-secondary_hover)]` |

### Filter Item (Selected)

| Property | Value | Tailwind |
|----------|-------|----------|
| Border | 1px solid `var(--border-primary)` (#BDC8D3) | `border-[var(--border-primary)]` |
| Background | `var(--bg-primary)` (#FFFFFF) | `bg-[var(--bg-primary)]` |

### Filter Item (Disabled)

| Property | Value | Tailwind |
|----------|-------|----------|
| Opacity | 40% | `opacity-40` |
| Pointer events | None | `pointer-events-none` |

---

## Secondary Variant (`felloSecondaryRadioFilterGroup`)

### Container

| Property | Value | Tailwind |
|----------|-------|----------|
| Display | Inline-flex | `inline-flex` |
| Width | 100% | `w-full` |
| Padding | 4px | `p-1` |
| Gap | 2px | `gap-0.5` |
| Border radius | 6px | `rounded-md` |
| Background | `var(--bg-secondary)` (#F1F4F7) | `bg-[var(--bg-secondary)]` |
| Overflow | Hidden | `overflow-hidden` |

### Filter Item (Unselected)

| Property | Value | Tailwind |
|----------|-------|----------|
| Height | 24px | `h-6` |
| Padding | 0 12px | `px-3` |
| Border radius | 6px | `rounded-md` |
| Flex | 1 | `flex-1` |
| Display | Flex, centered | `flex items-center justify-center` |
| Typography | 12px / 18px / weight 500 (caption-sm) | `text-xs leading-[18px]` + `style={{ fontWeight: 500 }}` |
| Color | `var(--text-primary)` (#353E5A) | `text-[var(--text-primary)]` |
| Cursor | Pointer | `cursor-pointer` |
| Transition | background-color 150ms ease-out | `transition-colors duration-150 ease-out` |

### Filter Item (Hover — Unselected, Not Disabled)

| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `var(--bg-tertiary)` (#E8ECF1) | `hover:bg-[var(--bg-tertiary)]` |

### Filter Item (Selected)

| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `var(--bg-brand-secondary-solid_hover)` (#0BA5A7) | `bg-[var(--bg-brand-secondary-solid_hover)]` |
| Color | `var(--text-white)` (#FFFFFF) | `text-[var(--text-white)]` |

### Filter Item (Disabled)

| Property | Value | Tailwind |
|----------|-------|----------|
| Opacity | 40% | `opacity-40` |
| Pointer events | None | `pointer-events-none` |

---

## Content Options

Each filter item can contain:
- **Label text** (required) — rendered as `<span>` with caption typography
- **Leading icon** (optional) — icon before the label
- **Trailing icon** (optional) — icon after the label
- **Gap between items**: 6px (`gap-1.5` on each item host)

---

## Implementation Pattern for Lovable

```jsx
{/* === Radio Filter Group — Primary Variant === */}
<div className="inline-flex border border-[var(--border-primary)] rounded-md bg-[var(--bg-secondary)] overflow-hidden">
  {/* Option 1 — selected */}
  <button
    className="
      py-1.5 px-3 rounded-md
      border border-[var(--border-primary)] bg-[var(--bg-primary)]
      text-sm leading-5 text-[var(--text-primary)]
      transition-colors duration-150 ease-out
    "
    style={{ fontWeight: 500 }}
  >
    All
  </button>

  {/* Option 2 — unselected */}
  <button
    className="
      py-1.5 px-3 rounded-md
      border border-transparent bg-transparent
      text-sm leading-5 text-[var(--text-primary)]
      hover:bg-[var(--bg-secondary_hover)]
      transition-colors duration-150 ease-out
      cursor-pointer
    "
    style={{ fontWeight: 500 }}
  >
    Active
  </button>

  {/* Option 3 — unselected */}
  <button
    className="
      py-1.5 px-3 rounded-md
      border border-transparent bg-transparent
      text-sm leading-5 text-[var(--text-primary)]
      hover:bg-[var(--bg-secondary_hover)]
      transition-colors duration-150 ease-out
      cursor-pointer
    "
    style={{ fontWeight: 500 }}
  >
    Inactive
  </button>
</div>


{/* === Radio Filter Group — Secondary Variant === */}
<div className="inline-flex w-full p-1 gap-0.5 rounded-md bg-[var(--bg-secondary)] overflow-hidden">
  {/* Option 1 — selected (teal) */}
  <button
    className="
      flex-1 h-6 px-3 rounded-md
      flex items-center justify-center
      bg-[var(--bg-brand-secondary-solid_hover)] text-[var(--text-white)]
      text-xs leading-[18px]
      transition-colors duration-150 ease-out
    "
    style={{ fontWeight: 500 }}
  >
    Daily
  </button>

  {/* Option 2 — unselected */}
  <button
    className="
      flex-1 h-6 px-3 rounded-md
      flex items-center justify-center
      bg-transparent text-[var(--text-primary)]
      text-xs leading-[18px]
      hover:bg-[var(--bg-tertiary)]
      transition-colors duration-150 ease-out
      cursor-pointer
    "
    style={{ fontWeight: 500 }}
  >
    Weekly
  </button>

  {/* Option 3 — unselected */}
  <button
    className="
      flex-1 h-6 px-3 rounded-md
      flex items-center justify-center
      bg-transparent text-[var(--text-primary)]
      text-xs leading-[18px]
      hover:bg-[var(--bg-tertiary)]
      transition-colors duration-150 ease-out
      cursor-pointer
    "
    style={{ fontWeight: 500 }}
  >
    Monthly
  </button>
</div>


{/* === Primary with icon + disabled option === */}
<div className="inline-flex border border-[var(--border-primary)] rounded-md bg-[var(--bg-secondary)] overflow-hidden">
  <button
    className="
      inline-flex items-center gap-1.5
      py-1.5 px-3 rounded-md
      border border-[var(--border-primary)] bg-[var(--bg-primary)]
      text-sm leading-5 text-[var(--text-primary)]
    "
    style={{ fontWeight: 500 }}
  >
    <Grid className="w-4 h-4" />
    Grid View
  </button>

  <button
    className="
      inline-flex items-center gap-1.5
      py-1.5 px-3 rounded-md
      border border-transparent bg-transparent
      text-sm leading-5 text-[var(--text-primary)]
      hover:bg-[var(--bg-secondary_hover)]
      cursor-pointer
    "
    style={{ fontWeight: 500 }}
  >
    <List className="w-4 h-4" />
    List View
  </button>

  {/* Disabled option */}
  <button
    className="
      inline-flex items-center gap-1.5
      py-1.5 px-3 rounded-md
      border border-transparent bg-transparent
      text-sm leading-5 text-[var(--text-primary)]
      opacity-40 pointer-events-none
    "
    style={{ fontWeight: 500 }}
    disabled
  >
    Calendar
  </button>
</div>
```

### Key Implementation Rules

1. **Two distinct variants** — Primary (outlined) and Secondary (padded with teal selected state)
2. **Primary container border is `var(--border-primary)` (#BDC8D3)** with `var(--bg-secondary)` (#F1F4F7) background
3. **Primary selected: `var(--bg-primary)` (#FFFFFF) bg + `var(--border-primary)` (#BDC8D3) border** — stands out against the gray container
4. **Secondary container has 4px padding** (`p-1`) and 2px gap between items
5. **Secondary selected: `var(--bg-brand-secondary-solid_hover)` (#0BA5A7) bg with `var(--text-white)` (#FFFFFF) text** — vibrant active state
6. **Secondary items are `flex-1`** — they fill equal space in the container
7. **Secondary item height is 24px** (`h-6`) — compact pill buttons
8. **Primary uses caption-md** (14px/20px/500), Secondary uses caption-sm (12px/18px/500)
9. **Container border-radius is 6px** (`rounded-md`) on both variants
10. **Hover on unselected: `var(--bg-secondary_hover)` (#E8ECF1)** — slightly darker than container bg
11. **Disabled items are 40% opacity** with `pointer-events-none`
12. **Hidden radio input** — native `<input>` is `width:0; visibility:hidden` (use `<button>` in Lovable)
13. **Primary items use negative margins** (-1px) to overlap container border — in React, use border on selected only
14. **Transition is 150ms ease-out** on background-color only
15. **Items can have leading/trailing icons** with 6px gap (`gap-1.5`)
