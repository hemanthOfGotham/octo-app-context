# Toast Component — Strict Specification

When building any toast notification, snackbar, or temporary message, follow these specifications exactly. Do NOT use default Tailwind toast styles, shadcn toasts, or any other toast system.

> **TOKEN RULE:** Every color in this spec is expressed as a CSS variable token. In Tailwind classes use `bg-[var(--token)]` / `text-[var(--token)]` / `border-[var(--token)]`. In inline styles use `'var(--token)'`. In prose and tables, show `var(--token)` (#hex) for reference.

---

## Toast Anatomy

A toast is a floating notification anchored to the bottom-center of the viewport:

```
[icon]  [message text]                    [action button]  [close button]
```

- Layout: `display: flex`, `justify-content: space-between`, `align-items: center`
- Gap: 12px between info section and actions section
- Within each section: 6px gap between elements

---

## Positioning

| Property | Value |
|----------|-------|
| Position | Fixed, bottom-center of viewport |
| Bottom offset | 40px from viewport bottom |
| Horizontal | Centered |
| Z-index | Above all page content (overlay level) |
| Max width | `min(384px, 100vw - 32px)` — ensures 16px gap on both sides |
| Width | `fit-content` — shrinks to content |

---

## Visual Spec

| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `var(--tooltip-bg-primary-dark)` (#052033) | `bg-[var(--tooltip-bg-primary-dark)]` |
| Text color | `var(--text-white)` (#FFFFFF) | `text-white` |
| Border radius | 8px | `rounded-lg` |
| Padding | 12px all sides | `p-3` |
| Typography | 14px / 20px / weight 500 | `text-sm leading-5 font-medium` |
| Shadow | None (the dark background provides sufficient contrast) | — |

> **Key rule:** Toast background is ALWAYS `var(--tooltip-bg-primary-dark)` (#052033). Text is ALWAYS white. This does not change with variant.

---

## Variants

There are 5 toast variants. The variant ONLY affects the **icon** — background and text are always the same.

| Variant | Icon | Icon Color |
|---------|------|------------|
| **default** | No icon | — |
| **success** | ✓ Check circle (filled) | `var(--fg-success-primary)` (#31A172) |
| **info** | ℹ Info circle (filled) | `var(--fg-info-primary)` (#3D93F5) |
| **warning** | ⚠ Alert triangle (filled) | `var(--fg-warning-primary)` (#F08400) |
| **error** | ✕ Close circle (filled) | `var(--fg-error-primary)` (#F02C00) |

- Icon size: 20px × 20px
- Icon is shown only for non-default variants
- Icon color is variant-specific (colored icon on dark background)

---

## Actions

### Action Button (optional)
| Property | Value |
|----------|-------|
| Style | White hyperlink button |
| Text color | `var(--text-white)` (#FFFFFF) |
| Underline | Transparent by default, white on hover |
| Font | Matches button hyperlink white spec |

- Only shown when an action label is provided
- Clicking the action button triggers the callback AND closes the toast

### Close Button (always present)
| Property | Value |
|----------|-------|
| Style | Icon-only helper button |
| Icon | ✕ close |
| Color | White (inherits from toast text color) |
| Size | Small, no padding |

---

## Auto-Dismiss

- Default duration: **6 seconds** (6000ms)
- Can be overridden with a custom duration
- Only one toast visible at a time — showing a new toast closes the existing one
- Clicking close button or action button immediately dismisses

---

## Entrance Animation

```css
@keyframes toast-animate-in {
  from {
    transform: scale(0.75) translateY(12px);
  }
  to {
    transform: scale(1) translateY(0);
  }
}
```

- Duration: 150ms
- Fills forward (stays at final state)
- Scales from 75% to 100% while sliding up 12px

---

## Accessibility

| Property | Value |
|----------|-------|
| Role | `alert` (no action button) or `alertdialog` (with action button) |
| Live region | Implicit via role — screen readers announce the message |

---

## Structure — Two Sections

### Info Section (left)
- Contains icon + message
- `display: flex`, `align-items: center`, `gap: 6px`

### Actions Section (right)
- Contains action button + close button
- `display: flex`, `align-items: center`, `gap: 6px`
- `flex-shrink: 0` — never shrinks

---

## Implementation Pattern for Lovable

```jsx
{/* === Toast Container — Fixed at bottom center === */}
{/* Wrap this around your toast for positioning */}
<div className="fixed bottom-10 left-1/2 -translate-x-1/2 z-50">

  {/* === Default variant — no icon === */}
  <div
    role="alert"
    className="
      flex justify-between items-center gap-3
      p-3 rounded-lg
      bg-[var(--tooltip-bg-primary-dark)] text-white
      text-sm leading-5 font-medium
      w-fit max-w-[min(384px,calc(100vw-32px))]
      animate-[toast-animate-in_150ms_forwards]
    "
  >
    <div className="flex items-center gap-1.5">
      <span className="text-start break-words">Toast message goes here</span>
    </div>
    <div className="flex items-center gap-1.5 shrink-0">
      <button className="
        text-white p-0 bg-transparent border-none cursor-pointer
        inline-flex items-center justify-center
      " aria-label="Close">
        <XIcon className="w-5 h-5" />
      </button>
    </div>
  </div>
</div>


{/* === Success variant — with icon === */}
<div
  role="alert"
  className="
    flex justify-between items-center gap-3
    p-3 rounded-lg
    bg-[var(--tooltip-bg-primary-dark)] text-white
    text-sm leading-5 font-medium
    w-fit max-w-[min(384px,calc(100vw-32px))]
  "
>
  <div className="flex items-center gap-1.5">
    <CheckCircleIcon className="w-5 h-5 text-[var(--text-success-primary)] shrink-0" />
    <span className="text-start break-words">Changes saved successfully</span>
  </div>
  <div className="flex items-center gap-1.5 shrink-0">
    <button className="
      text-white p-0 bg-transparent border-none cursor-pointer
      inline-flex items-center justify-center
    " aria-label="Close">
      <XIcon className="w-5 h-5" />
    </button>
  </div>
</div>


{/* === Error variant — with action button === */}
<div
  role="alertdialog"
  className="
    flex justify-between items-center gap-3
    p-3 rounded-lg
    bg-[var(--tooltip-bg-primary-dark)] text-white
    text-sm leading-5 font-medium
    w-fit max-w-[min(384px,calc(100vw-32px))]
  "
>
  <div className="flex items-center gap-1.5">
    <XCircleIcon className="w-5 h-5 text-[var(--text-error-primary)] shrink-0" />
    <span className="text-start break-words">Failed to save changes</span>
  </div>
  <div className="flex items-center gap-1.5 shrink-0">
    {/* Action button — white hyperlink style */}
    <button className="
      text-white text-sm leading-5
      underline decoration-transparent hover:decoration-white
      bg-transparent border-none cursor-pointer
      whitespace-nowrap
      transition-all duration-150 ease-out
    " style={{ fontWeight: 600 }}>
      Retry
    </button>
    <button className="
      text-white p-0 bg-transparent border-none cursor-pointer
      inline-flex items-center justify-center
    " aria-label="Close">
      <XIcon className="w-5 h-5" />
    </button>
  </div>
</div>


{/* === Warning variant === */}
<div
  role="alert"
  className="
    flex justify-between items-center gap-3
    p-3 rounded-lg
    bg-[var(--tooltip-bg-primary-dark)] text-white
    text-sm leading-5 font-medium
    w-fit max-w-[min(384px,calc(100vw-32px))]
  "
>
  <div className="flex items-center gap-1.5">
    <AlertTriangleIcon className="w-5 h-5 text-[var(--text-warning-primary)] shrink-0" />
    <span className="text-start break-words">Connection unstable</span>
  </div>
  <div className="flex items-center gap-1.5 shrink-0">
    <button className="
      text-white p-0 bg-transparent border-none cursor-pointer
      inline-flex items-center justify-center
    " aria-label="Close">
      <XIcon className="w-5 h-5" />
    </button>
  </div>
</div>
```

### Toast Animation CSS (add to global styles)

```css
@keyframes toast-animate-in {
  from {
    transform: scale(0.75) translateY(12px);
  }
  to {
    transform: scale(1) translateY(0);
  }
}
```

### Key Implementation Rules

1. **Background is ALWAYS `var(--tooltip-bg-primary-dark)` (#052033) — never changes with variant
2. **Text is ALWAYS white** — never changes with variant
3. **Variant only affects the icon** — its presence and color
4. **Default variant has NO icon** — just message text
5. **Icon is 20px**, uses variant-specific color on dark background
6. **Max width is `min(384px, 100vw - 32px)`** — ensures 16px margins on small screens
7. **Toast is positioned fixed at bottom-center**, 40px from viewport bottom
8. **Only ONE toast visible at a time** — new toast replaces existing
9. **Auto-dismisses after 6 seconds** by default
10. **Action button closes the toast** after triggering its callback
11. **Use `role="alert"`** (no action button) or **`role="alertdialog"`** (with action button)
12. **Entry animation**: scale from 0.75 to 1 + slide up 12px, 150ms duration
13. **Close button is always present** — icon-only, white color
14. **Message text uses `break-words`** to handle long text gracefully
