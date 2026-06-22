# Breadcrumbs Component — Strict Specification

When building any breadcrumb navigation, follow these specifications exactly. Do NOT use default Tailwind breadcrumb styles, shadcn breadcrumbs, or any other breadcrumb system.

> **TOKEN RULE:** Every color in this spec is expressed as a CSS variable token. In Tailwind classes use `bg-[var(--token)]` / `text-[var(--token)]` / `border-[var(--token)]`. In inline styles use `'var(--token)'`. In prose and tables, show `var(--token)` (#hex) for reference.

---

## Breadcrumbs Anatomy

A breadcrumb trail is a horizontal flex row of clickable items separated by chevron (›) icons:

```
[Item 1]  ›  [Item 2]  ›  [Current Page]
```

- Container: `display: flex`, `gap: 4px` (`gap-1`)
- Each item except the last has a chevron separator after it
- The last item represents the current page and is visually distinct

---

## Sizes

There are 2 breadcrumb sizes:

| Size | Typography | Icon Size |
|------|-----------|-----------|
| **small** | 12px / 18px / weight 500 (`text-xs leading-[18px] font-medium`) | 14px |
| **medium** (default) | 14px / 20px / weight 500 (`text-sm leading-5 font-medium`) | 16px |

> Note: The chevron separator is always 16px regardless of breadcrumb size.

---

## Visual Spec

### Non-Last Items (navigation items)
| Property | Value | Tailwind |
|----------|-------|----------|
| Text color | `var(--text-tertiary)` (#6B748E) | `text-[var(--text-tertiary)]` |
| Cursor | Pointer | `cursor-pointer` |
| Underline (default) | Present but transparent | `underline decoration-transparent` |
| Underline (hover/focus) | Visible, inherits text color | `hover:decoration-current` |
| Right padding | 20px (`spacing(5)`) — reserves space for chevron | `pr-5` |
| Position | Relative — for chevron positioning | `relative` |

### Last Item (current page)
| Property | Value | Tailwind |
|----------|-------|----------|
| Text color | `var(--text-primary)` (#353E5A) | `text-[var(--text-primary)]` |
| Cursor | Default (not clickable) | `cursor-default` |
| Right padding | 0 — no chevron after last item | `pr-0` |
| Underline | None on hover | — |

### Chevron Separator
| Property | Value |
|----------|-------|
| Character | › (right-pointing chevron) |
| Position | Absolute, right: 0, top: 0 |
| Font size | 16px (fixed, does not change with breadcrumb size) |
| Color | Inherits from parent (`var(--text-tertiary)` #6B748E) |
| Only shown | On non-last items |

---

## Interaction States

| State | Effect |
|-------|--------|
| Default | Text with transparent underline |
| Hover (non-last) | Underline becomes visible (color inherits from text) |
| Focus-visible (non-last) | Same as hover — underline appears |
| Last item | No hover/focus underline, primary text color |

### Accessibility
- Each breadcrumb item has `role="button"`
- Each item is focusable (`tabindex="0"`)
- Responds to Enter and Space key presses
- Emits click event on activation

---

## Transition

- Underline color transitions smoothly: `transition: text-decoration-color 150ms ease-out`

---

## Implementation Pattern for Lovable

```jsx
{/* === Breadcrumbs — Medium size (default) === */}
<nav aria-label="Breadcrumb">
  <div className="flex gap-1">
    {/* Non-last item */}
    <button
      className="
        relative pr-5
        text-sm leading-5 font-medium
        text-[var(--text-tertiary)]
        underline decoration-transparent
        hover:decoration-current focus-visible:decoration-current
        cursor-pointer bg-transparent border-none outline-none
        transition-[text-decoration-color] duration-150 ease-out
      "
      role="button"
      tabIndex={0}
    >
      Dashboard
      {/* Chevron separator */}
      <span className="absolute right-0 top-0 text-[16px]" aria-hidden="true">›</span>
    </button>

    {/* Non-last item */}
    <button
      className="
        relative pr-5
        text-sm leading-5 font-medium
        text-[var(--text-tertiary)]
        underline decoration-transparent
        hover:decoration-current focus-visible:decoration-current
        cursor-pointer bg-transparent border-none outline-none
        transition-[text-decoration-color] duration-150 ease-out
      "
      role="button"
      tabIndex={0}
    >
      Settings
      <span className="absolute right-0 top-0 text-[16px]" aria-hidden="true">›</span>
    </button>

    {/* Last item — current page */}
    <span className="
      text-sm leading-5 font-medium
      text-[var(--text-primary)]
    ">
      Profile
    </span>
  </div>
</nav>


{/* === Breadcrumbs — Small size === */}
<nav aria-label="Breadcrumb">
  <div className="flex gap-1">
    <button
      className="
        relative pr-5
        text-xs leading-[18px] font-medium
        text-[var(--text-tertiary)]
        underline decoration-transparent
        hover:decoration-current focus-visible:decoration-current
        cursor-pointer bg-transparent border-none outline-none
        transition-[text-decoration-color] duration-150 ease-out
      "
      role="button"
      tabIndex={0}
    >
      Home
      <span className="absolute right-0 top-0 text-[16px]" aria-hidden="true">›</span>
    </button>

    <span className="
      text-xs leading-[18px] font-medium
      text-[var(--text-primary)]
    ">
      Current Page
    </span>
  </div>
</nav>
```

### Key Implementation Rules

1. **Container is `flex` with `gap-1`** (4px gap between items)
2. **Non-last items are `var(--text-tertiary)` (#6B748E), **last item is `var(--text-primary)` (#353E5A)
3. **Underline is always present** but transparent by default — becomes visible on hover/focus for non-last items only
4. **Chevron separator** is `›` positioned absolutely at `right: 0, top: 0` — always 16px font size
5. **Non-last items have `pr-5`** (20px right padding) to make space for the chevron
6. **Last item has no right padding**, no chevron, no hover underline
7. **Font weight is always 500** (`font-medium`) for all items
8. **All non-last items are keyboard accessible** — `role="button"`, `tabindex="0"`, respond to Enter/Space
9. **Chevron is `aria-hidden="true"`** — decorative, not read by screen readers
10. **Use a `<nav>` wrapper** with `aria-label="Breadcrumb"` for accessibility
