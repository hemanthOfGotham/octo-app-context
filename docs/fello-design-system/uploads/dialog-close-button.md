# Dialog Close Button Component — Strict Specification

When building a close button for any Modal or Slide-Out Dialog, follow these specifications exactly. Do NOT use shadcn's default close buttons, Radix close icons, or any other close button system.

> **TOKEN RULE:** Every colour in this spec is expressed as a CSS variable token. When writing Tailwind classes use the `bg-[var(--token)]` / `text-[var(--token)]` / `border-[var(--token)]` syntax. In inline styles use `'var(--token)'`. In prose and tables the original hex is kept only as a parenthetical annotation, e.g. `var(--bg-secondary)` (#F1F4F7).

This is the **highlighted icon-only SM button** used as the standard dialog close action.

---

## Anatomy

```
┌──────────┐
│    ✕     │   32×32px rounded square
└──────────┘
```

- A single square button containing an `X` (close) icon
- No label, no border — icon only
- Uses the **highlighted** button variant (subtle gray background)

---

## Dimensions

| Property | Value | Tailwind |
|----------|-------|----------|
| Width | 32px | `w-8` |
| Height | 32px | `h-8` |
| Border radius | 6px | `rounded-md` |
| Icon size | 16px | `w-4 h-4` |
| Padding | 8px (implicit from 32px size - 16px icon) | — |

---

## Colors

### Default State

| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `var(--bg-secondary)` (#F1F4F7) | `bg-[var(--bg-secondary)]` |
| Icon color | `var(--fg-tertiary)` (#6B748E) | `text-[var(--fg-tertiary)]` |

### Hover State

| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `var(--bg-secondary_hover)` (#E8ECF1) | `hover:bg-[var(--bg-secondary_hover)]` |
| Icon color | `var(--fg-tertiary)` (#6B748E) (unchanged) | — |

### Focus-Visible State

| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `var(--bg-secondary_hover)` (#E8ECF1) | `focus-visible:bg-[var(--bg-secondary_hover)]` |
| Box shadow | `0 0 0 4px rgba(34, 42, 63, 0.08)` (ring-gray-primary) | `focus-visible:ring-4 focus-visible:ring-[rgba(34,42,63,0.08)]` |

---

## Implementation

```jsx
<button
  className="
    flex items-center justify-center
    w-8 h-8 rounded-md
    bg-[var(--bg-secondary)] text-[var(--fg-tertiary)]
    cursor-pointer outline-none border-none
    hover:bg-[var(--bg-secondary_hover)]
    focus-visible:bg-[var(--bg-secondary_hover)]
    transition-colors duration-150 ease-out
  "
  onClick={onClose}
  aria-label="Close"
>
  <X className="w-4 h-4" />
</button>
```

### Using Lucide React Icon

```jsx
import { X } from 'lucide-react';

<button
  className="
    flex items-center justify-center
    w-8 h-8 rounded-md
    bg-[var(--bg-secondary)] text-[var(--fg-tertiary)]
    cursor-pointer outline-none border-none
    hover:bg-[var(--bg-secondary_hover)]
    focus-visible:bg-[var(--bg-secondary_hover)]
    transition-colors duration-150 ease-out
  "
  onClick={onClose}
  aria-label="Close"
>
  <X className="w-4 h-4" />
</button>
```

### Using Inline SVG

```jsx
<button
  className="
    flex items-center justify-center
    w-8 h-8 rounded-md
    bg-[var(--bg-secondary)] text-[var(--fg-tertiary)]
    cursor-pointer outline-none border-none
    hover:bg-[var(--bg-secondary_hover)]
    focus-visible:bg-[var(--bg-secondary_hover)]
    transition-colors duration-150 ease-out
  "
  onClick={onClose}
  aria-label="Close"
>
  <svg className="w-4 h-4" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">
    <line x1="18" y1="6" x2="6" y2="18" />
    <line x1="6" y1="6" x2="18" y2="18" />
  </svg>
</button>
```

---

## Where It's Used

This close button appears in the **top-right area** of:
- **Modal Header** — see `modal.md`
- **Slide-Out Dialog Header** — see `slide-out-dialog.md`

In both cases it sits inside a flex row with `gap-4` (16px), aligned to the right side of the header.

---

## Color Reference

| State | Background | Icon Color | Box Shadow |
|-------|------------|------------|------------|
| Default | `var(--bg-secondary)` (#F1F4F7) | `var(--fg-tertiary)` (#6B748E) | none |
| Hover | `var(--bg-secondary_hover)` (#E8ECF1) | `var(--fg-tertiary)` (#6B748E) | none |
| Focus | `var(--bg-secondary_hover)` (#E8ECF1) | `var(--fg-tertiary)` (#6B748E) | `0 0 0 4px rgba(34,42,63,0.08)` |

---

## CRITICAL Rules

1. **Size is ALWAYS 32×32px** — this is the SM icon-only button size. Do NOT use 24px (XS) or 40px (MD).
2. **Border radius is 6px** (`rounded-md`) — NOT 4px, NOT 8px.
3. **Background is `var(--bg-secondary)`** (#F1F4F7) (light gray) — NOT transparent, NOT white. This is the **highlighted** button variant.
4. **Icon color is `var(--fg-tertiary)`** (#6B748E) (medium gray-blue) — NOT black, NOT `var(--fg-primary)` (#353E5A).
5. **Icon is an X (close)** — a simple cross/times icon at 16px.
6. **No border** — the button has NO visible border (`border-none`).
7. **Hover changes background to `var(--bg-secondary_hover)`** (#E8ECF1) (slightly darker gray) — icon color stays the same.
8. **Always include `aria-label="Close"`** for accessibility.
9. **Transition is `150ms ease-out`** on background-color.
10. **This button is NOT the same as a regular icon button** — it uses the special `highlighted` variant with its own unique gray color scheme.
