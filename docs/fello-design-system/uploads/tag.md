# Tag Component — Strict Specification

When building any tag, label chip, keyword token, or removable tag, follow these specifications exactly. Do NOT use default Tailwind tag styles, shadcn badges, or any other tag/chip system.

> **TOKEN RULE:** Every colour in this file is expressed as a CSS custom-property token — e.g. `var(--bg-primary)` — with the original hex in parentheses for reference only. When implementing, always use the `var(--*)` form (in Tailwind: `bg-[var(--bg-primary)]`, `text-[var(--text-primary)]`, etc.). Never hard-code a raw hex value.

---

## Tag Anatomy

A tag is an inline pill-like element with optional leading and trailing content:

```
[leading icon/dot]  [label text]  [trailing: count | close | none]
```

- Layout: `inline-flex`, `align-items: center`
- Shape: Rounded with `border-radius: 6px` (`rounded-md`) — NOT fully rounded
- Cursor: Pointer when enabled, none when disabled
- Content projects a leading icon/dot and a trailing element

---

## Sizes

There are 3 tag sizes:

### Small
| Property | Value | Tailwind |
|----------|-------|----------|
| Height | 20px | `h-5` |
| Padding | 2px vertical, 8px horizontal | `py-0.5 px-2` |
| Typography | 12px / 18px / weight 500 | `text-xs leading-[18px] font-medium` |
| Icon size | 12px | `text-xs` |
| Count badge | 14px × 14px | `w-3.5 h-3.5` |
| Count typography | 10px / 16px / weight 400 | `text-[10px] leading-4 font-normal` |
| Close icon | 10px | — |
| Close button | inherits | — |

### Medium (Default)
| Property | Value | Tailwind |
|----------|-------|----------|
| Height | 24px | `h-6` |
| Padding | 3px vertical, 8px horizontal | `py-[3px] px-2` |
| Typography | 14px / 20px / weight 500 | `text-sm leading-5 font-medium` |
| Icon size | 14px | `text-sm` |
| Count badge | 16px × 16px | `w-4 h-4` |
| Count typography | 10px / 16px / weight 400 | `text-[10px] leading-4 font-normal` |
| Close icon | 12px | — |
| Close button | inherits | — |

### Large
| Property | Value | Tailwind |
|----------|-------|----------|
| Height | 28px | `h-7` |
| Padding | 4px vertical, 8px horizontal | `py-1 px-2` |
| Typography | 14px / 20px / weight 500 | `text-sm leading-5 font-medium` |
| Icon size | 16px | `text-base` |
| Count badge | 20px × 20px | `w-5 h-5` |
| Count typography | 14px / 20px / weight 400 | `text-sm leading-5 font-normal` |
| Close icon | 14px | — |
| Close button | 20px × 20px | `w-5 h-5` |

---

## Trailing Types

A tag can have one of three trailing types:

| Type | Behavior |
|------|----------|
| **none** (default) | No trailing element — just the label |
| **count** | Shows a numeric badge after the label |
| **close-button** | Shows a small × close button after the label |

When a trailing element is present, the right padding reduces from 8px to 4px (`pr-1`).

---

## States — Complete Color Spec

### Default
| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `var(--bg-primary)` (#FFFFFF) | `bg-[var(--bg-primary)]` |
| Border | 1px solid `var(--border-secondary)` (#CCD6DC) | `border border-[var(--border-secondary)]` |
| Text color | `var(--text-primary)` (#353E5A) | `text-[var(--text-primary)]` |
| Cursor | Pointer | `cursor-pointer` |

### Hover
| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `var(--bg-primary)` (#FFFFFF) (unchanged) | `bg-[var(--bg-primary)]` |
| Border | 1px solid `var(--border-secondary-solid)` (#8EA1AF) | `hover:border-[var(--border-secondary-solid)]` |
| Text color | `var(--text-primary)` (#353E5A) (unchanged) | — |

### Focus-Visible
| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `var(--bg-primary)` (#FFFFFF) (unchanged) | `bg-[var(--bg-primary)]` |
| Border | 1px solid `var(--border-brand-secondary-solid)` (#0BA5A7) | `focus-visible:border-[var(--border-brand-secondary-solid)]` |
| Text color | `var(--text-primary)` (#353E5A) (unchanged) | — |

### Disabled
| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `var(--bg-disabled_subtle)` (#F1F4F7) | `bg-[var(--bg-disabled_subtle)]` |
| Border | 1px solid `var(--border-secondary)` (#CCD6DC) (unchanged) | `border-[var(--border-secondary)]` |
| Text color | `var(--text-disabled)` (#8EA1AF) | `text-[var(--text-disabled)]` |
| Cursor | Default | `cursor-default` |
| Interaction | Pointer events none | `pointer-events-none` |

---

## Count Badge Spec

| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `var(--bg-secondary)` (#F1F4F7) | `bg-[var(--bg-secondary)]` |
| Border | 1px solid `var(--border-tertiary)` (#E8ECF1) | `border border-[var(--border-tertiary)]` |
| Border radius | 4px | `rounded` |
| Layout | Inline-flex, centered | `inline-flex items-center justify-center` |
| Text color | Inherits from parent (text-primary or text-disabled) | — |

---

## Close Button Spec

| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `var(--bg-secondary)` (#F1F4F7) | `bg-[var(--bg-secondary)]` |
| Background hover | `var(--bg-secondary_hover)` (#E8ECF1) | `hover:bg-[var(--bg-secondary_hover)]` |
| Border radius | 4px | `rounded` |
| Padding | 2px | `p-0.5` |
| Layout | Inline-flex, centered | `inline-flex items-center justify-center` |
| Transition | `background-color 150ms ease-out` | `transition-colors duration-150 ease-out` |
| Icon | × (close/X mark) | — |

---

## Container Specs

| Property | Value | Tailwind |
|----------|-------|----------|
| Display | Inline flex | `inline-flex` |
| Align | Center | `items-center` |
| Gap | 6px | `gap-1.5` |
| Border radius | 6px | `rounded-md` |
| Transition | `border-color 150ms ease-out` | `transition-[border-color] duration-150 ease-out` |

---

## Text Overflow

- Tag label uses single-line ellipsis overflow
- `overflow: hidden`, `text-overflow: ellipsis`, `white-space: nowrap`
- Tailwind: `truncate`

---

## Implementation Pattern for Lovable

```jsx
{/* === Medium Tag — No trailing === */}
<button
  className="
    inline-flex items-center gap-1.5
    h-6 py-[3px] px-2
    rounded-md
    bg-[var(--bg-primary)] text-[var(--text-primary)]
    border border-[var(--border-secondary)]
    text-sm leading-5 font-medium
    cursor-pointer outline-none
    hover:border-[var(--border-secondary-solid)]
    focus-visible:border-[var(--border-brand-secondary-solid)]
    transition-[border-color] duration-150 ease-out
  "
>
  <span className="truncate">Default Tag</span>
</button>


{/* === Medium Tag — With count trailing === */}
<button
  className="
    inline-flex items-center gap-1.5
    h-6 py-[3px] pl-2 pr-1
    rounded-md
    bg-[var(--bg-primary)] text-[var(--text-primary)]
    border border-[var(--border-secondary)]
    text-sm leading-5 font-medium
    cursor-pointer outline-none
    hover:border-[var(--border-secondary-solid)]
    focus-visible:border-[var(--border-brand-secondary-solid)]
    transition-[border-color] duration-150 ease-out
  "
>
  <span className="truncate">Tagged Item</span>
  {/* Count badge */}
  <span className="
    inline-flex items-center justify-center
    w-4 h-4
    rounded
    bg-[var(--bg-secondary)]
    border border-[var(--border-tertiary)]
    text-[10px] leading-4 font-normal
  ">
    5
  </span>
</button>


{/* === Large Tag — With close button trailing === */}
<div
  className="
    inline-flex items-center gap-1.5
    h-7 py-1 pl-2 pr-1
    rounded-md
    bg-[var(--bg-primary)] text-[var(--text-primary)]
    border border-[var(--border-secondary)]
    text-sm leading-5 font-medium
    cursor-pointer
    hover:border-[var(--border-secondary-solid)]
    transition-[border-color] duration-150 ease-out
  "
>
  <span className="truncate">Removable</span>
  {/* Close button */}
  <button
    className="
      inline-flex items-center justify-center
      w-5 h-5 p-0.5
      rounded
      bg-[var(--bg-secondary)]
      hover:bg-[var(--bg-secondary_hover)]
      border-none outline-none cursor-pointer
      transition-colors duration-150 ease-out
    "
    aria-label="Remove tag"
    onClick={(e) => { e.stopPropagation(); }}
  >
    <XIcon className="w-3.5 h-3.5" />
  </button>
</div>


{/* === Small Tag — With leading icon === */}
<button
  className="
    inline-flex items-center gap-1.5
    h-5 py-0.5 px-2
    rounded-md
    bg-[var(--bg-primary)] text-[var(--text-primary)]
    border border-[var(--border-secondary)]
    text-xs leading-[18px] font-medium
    cursor-pointer outline-none
    hover:border-[var(--border-secondary-solid)]
    focus-visible:border-[var(--border-brand-secondary-solid)]
    transition-[border-color] duration-150 ease-out
  "
>
  <StarIcon className="w-3 h-3 shrink-0" />
  <span className="truncate">Small Tag</span>
</button>


{/* === Disabled Tag === */}
<div
  className="
    inline-flex items-center gap-1.5
    h-6 py-[3px] px-2
    rounded-md
    bg-[var(--bg-disabled_subtle)] text-[var(--text-disabled)]
    border border-[var(--border-secondary)]
    text-sm leading-5 font-medium
    cursor-default pointer-events-none
  "
  aria-disabled="true"
>
  <span className="truncate">Disabled Tag</span>
</div>
```

### Tag Size Quick Reference

| Size | Height | Padding V/H | Text | Count Size | Count Text |
|------|--------|-------------|------|------------|------------|
| Small | 20px | 2px / 8px | 12px/18px | 14×14 | 10px/16px |
| Medium | 24px | 3px / 8px | 14px/20px | 16×16 | 10px/16px |
| Large | 28px | 4px / 8px | 14px/20px | 20×20 | 14px/20px |

### Key Implementation Rules

1. **Border radius is 6px** (`rounded-md`) — NOT fully rounded like capsules
2. **Default border is `var(--border-secondary)` (#CCD6DC)**, hover changes to `var(--border-secondary-solid)` (#8EA1AF)
3. **Focus border is `var(--border-brand-secondary-solid)` (#0BA5A7)** (teal)
4. **Background stays `var(--bg-primary)` (#FFFFFF)** in default and hover states — only border changes on hover
5. **Gap between elements is 6px** (`gap-1.5`)
6. **Trailing element reduces right padding** from 8px to 4px (`pl-2 pr-1`)
7. **Count badge has its own border** `var(--border-tertiary)` (#E8ECF1) and background `var(--bg-secondary)` (#F1F4F7)
8. **Close button has hover state** — background changes from `var(--bg-secondary)` (#F1F4F7) to `var(--bg-secondary_hover)` (#E8ECF1)
9. **Font weight is always 500** (`font-medium`) for tag label
10. **Disabled state uses `var(--bg-disabled_subtle)` (#F1F4F7) background** with `var(--text-disabled)` (#8EA1AF) text and `pointer-events-none`
11. **Text truncates** with ellipsis — use `truncate` class
12. **Close button stops event propagation** — click on x should not trigger tag click
