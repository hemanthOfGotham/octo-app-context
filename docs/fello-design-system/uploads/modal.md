# Modal Dialog Component — Strict Specification

When building any centered modal dialog, confirmation dialog, or popup dialog, follow these specifications exactly. Do NOT use shadcn Dialog, Tailwind modals, Radix Dialog, or any other modal system.

> **TOKEN RULE:** Every colour in this file is expressed as a CSS custom-property token — e.g. `var(--bg-primary)` — with the original hex in parentheses for reference only. When implementing, always use the `var(--*)` form (in Tailwind: `bg-[var(--bg-primary)]`, `text-[var(--text-primary)]`, etc.). Never hard-code a raw hex value.

**Note:** The close button uses the Dialog Close Button spec (see `dialog-close-button.md`). Buttons in the footer use the Button spec (see `button.md`).

---

## Architecture Overview

```
Backdrop (fixed overlay covering entire viewport)
└── Modal Panel (centered, rounded white card)
    ├── Header (title + optional description + close button)
    │   ├── Left: Title (h2) + Description (paragraph)
    │   └── Right: Optional action button(s) + Close button
    ├── Content (scrollable body, separated by top border)
    │   └── Any content
    └── Footer (action buttons on gray background)
        └── Buttons (right-aligned on desktop, full-width on mobile)
```

**⚠️ KEY STRUCTURAL RULES:**
1. The modal is a **white rounded card** centered on a dark semi-transparent backdrop.
2. The modal has exactly **3 sections**: Header, Content, Footer — in that order.
3. Content area has a **top border** separating it from the header.
4. Footer has a **gray background** (`var(--bg-secondary_subtle)` (#F7F9FA)) — it is NOT the same white as the rest.
5. The close button is ALWAYS present in the header (top-right) unless explicitly hidden.

---

## Backdrop / Overlay

```jsx
<div
  className="fixed inset-0 z-50"
  style={{ background: 'var(--bg_overlay)' }}
  onClick={onClose}
/>
```

| Property | Value | CSS |
|----------|-------|-----|
| Position | Fixed, full viewport | `fixed inset-0` |
| Z-index | 50 | `z-50` |
| Background | `var(--bg_overlay)` (rgba(73, 88, 131, 0.56)) | `'var(--bg_overlay)'` |

---

## Modal Panel (Container)

```jsx
<div
  className="fixed z-50 flex flex-col overflow-hidden"
  style={{
    top: '80px',
    left: '50%',
    transform: 'translateX(-50%)',
    width: '770px',                           // Medium (default)
    maxWidth: 'calc(100vw - 16px)',
    maxHeight: 'calc(100dvh - 160px)',        // 80px top + 80px bottom
    background: 'var(--bg-primary)',
    border: '1px solid var(--border-tertiary)',
    borderRadius: '12px',
  }}
>
  {/* Header */}
  {/* Content */}
  {/* Footer */}
</div>
```

### Sizes

| Size | Width | Tailwind |
|------|-------|----------|
| Extra Small | 384px | `w-[384px]` |
| Small | 480px | `w-[480px]` |
| **Medium (default)** | **770px** | **`w-[770px]`** |
| Large | 1025px | `w-[1025px]` |
| Extra Large | 1280px | `w-[1280px]` |

### Container Properties

| Property | Value | CSS / Tailwind |
|----------|-------|----------------|
| Position | Fixed, centered | `fixed`, `top: 80px`, `left: 50%`, `transform: translateX(-50%)` |
| Max width | calc(100vw - 16px) | `max-w-[calc(100vw-16px)]` |
| Max height | calc(100dvh - 160px) | `max-h-[calc(100dvh-160px)]` |
| Background | `var(--bg-primary)` (#FFFFFF) | `bg-[var(--bg-primary)]` |
| Border | 1px solid `var(--border-tertiary)` (#E8ECF1) | `border border-[var(--border-tertiary)]` |
| Border radius | 12px | `rounded-xl` or `rounded-[12px]` |
| Overflow | hidden | `overflow-hidden` |
| Layout | flex column | `flex flex-col` |
| Z-index | 50 | `z-50` |

---

## Header

```jsx
<div
  className="flex justify-between items-start gap-4"
  style={{ padding: '12px 12px 12px 24px' }}
>
  {/* Left side: title + description */}
  <div>
    <h2
      className="text-[var(--text-primary)]"
      style={{ fontSize: '18px', lineHeight: '28px', fontWeight: 600 }}
    >
      {titleText}
    </h2>
    {description && (
      <p className="text-[var(--text-secondary)] text-sm leading-5 font-normal mt-2 break-words">
        {description}
      </p>
    )}
  </div>

  {/* Right side: optional buttons + close button */}
  <div className="flex items-center gap-4">
    {/* Optional action buttons go here */}
    <CloseButton onClick={onClose} />
  </div>
</div>
```

### Header Properties

| Property | Value | CSS |
|----------|-------|-----|
| Layout | Flex, space-between, items-start | `flex justify-between items-start` |
| Padding | 12px top, 12px right, 12px bottom, **24px left** | `style={{ padding: '12px 12px 12px 24px' }}` |
| Gap | 16px | `gap-4` |

### Title Typography

| Property | Value | Tailwind |
|----------|-------|----------|
| Element | `<h2>` | — |
| Mixin | heading-sm | — |
| Font size | 18px | `text-lg` |
| Line height | 28px | `leading-7` |
| Font weight | 600 | `font-semibold` |
| Color | `var(--text-primary)` (#353E5A) | `text-[var(--text-primary)]` |

### Description Typography

| Property | Value | Tailwind |
|----------|-------|----------|
| Element | `<p>` | — |
| Mixin | paragraph-md | — |
| Font size | 14px | `text-sm` |
| Line height | 20px | `leading-5` |
| Font weight | 400 | `font-normal` |
| Color | `var(--text-secondary)` (#495883) | `text-[var(--text-secondary)]` |
| Margin top | 8px (below title) | `mt-2` |

---

## Content (Body)

```jsx
<div
  className="flex-1 overflow-x-hidden overflow-y-auto"
  style={{
    padding: '24px',
    borderTop: '1px solid var(--border-tertiary)',
  }}
>
  {/* Any content */}
</div>
```

| Property | Value | CSS / Tailwind |
|----------|-------|----------------|
| Padding | 24px all sides | `p-6` |
| Border top | 1px solid `var(--border-tertiary)` (#E8ECF1) | `border-t border-[var(--border-tertiary)]` |
| Flex | 1 (fills remaining space) | `flex-1` |
| Overflow X | hidden | `overflow-x-hidden` |
| Overflow Y | auto (scrollable) | `overflow-y-auto` |

---

## Footer

```jsx
<div
  className="flex items-center gap-4"
  style={{
    padding: '12px 24px',
    background: 'var(--bg-secondary_subtle)',
    justifyContent: 'flex-end',     // Desktop: right-aligned
  }}
>
  {/* Secondary button (e.g. Cancel) */}
  <button className="...">Cancel</button>
  {/* Primary button (e.g. Confirm) */}
  <button className="...">Confirm</button>
</div>
```

| Property | Value | CSS / Tailwind |
|----------|-------|----------------|
| Padding | 12px top/bottom, 24px left/right | `py-3 px-6` |
| Background | `var(--bg-secondary_subtle)` (#F7F9FA) | `bg-[var(--bg-secondary_subtle)]` |
| Layout | Flex, items-center | `flex items-center` |
| Gap | 16px | `gap-4` |
| Justify (desktop ≥ 1024px) | flex-end (right-aligned) | `justify-end` |
| Justify (mobile < 1024px) | — (buttons become full-width) | — |
| Button width (desktop) | auto | — |
| Button width (mobile) | 100% | `w-full` |

---

## Color Reference

| Element | Token | Hex |
|---------|-------|-----|
| Backdrop | `var(--bg_overlay)` | (rgba(73, 88, 131, 0.56)) |
| Panel background | `var(--bg-primary)` | (#FFFFFF) |
| Panel border | `var(--border-tertiary)` | (#E8ECF1) |
| Content top border | `var(--border-tertiary)` | (#E8ECF1) |
| Footer background | `var(--bg-secondary_subtle)` | (#F7F9FA) |
| Title text | `var(--text-primary)` | (#353E5A) |
| Description text | `var(--text-secondary)` | (#495883) |

---

## Animation

| Property | Value |
|----------|-------|
| Transition | `margin-bottom 0.3s ease` |
| Backdrop | Fade in (opacity 0 → 1) |

---

## Complete Implementation Pattern

```jsx
function Modal({ isOpen, onClose, title, description, children, footer, size = 'medium' }) {
  if (!isOpen) return null;

  const widthMap = {
    'extra-small': '384px',
    'small': '480px',
    'medium': '770px',
    'large': '1025px',
    'extra-large': '1280px',
  };

  return (
    <>
      {/* Backdrop */}
      <div
        className="fixed inset-0 z-50"
        style={{ background: 'var(--bg_overlay)' }}
        onClick={onClose}
      />

      {/* Modal Panel */}
      <div
        className="fixed z-50 flex flex-col overflow-hidden"
        role="dialog"
        aria-modal="true"
        aria-labelledby="modal-title"
        aria-describedby={description ? 'modal-description' : undefined}
        style={{
          top: '80px',
          left: '50%',
          transform: 'translateX(-50%)',
          width: widthMap[size],
          maxWidth: 'calc(100vw - 16px)',
          maxHeight: 'calc(100dvh - 160px)',
          background: 'var(--bg-primary)',
          border: '1px solid var(--border-tertiary)',
          borderRadius: '12px',
        }}
      >
        {/* === HEADER === */}
        <div
          className="flex justify-between items-start gap-4"
          style={{ padding: '12px 12px 12px 24px' }}
        >
          <div>
            <h2
              id="modal-title"
              className="text-[var(--text-primary)]"
              style={{ fontSize: '18px', lineHeight: '28px', fontWeight: 600 }}
            >
              {title}
            </h2>
            {description && (
              <p
                id="modal-description"
                className="text-[var(--text-secondary)] text-sm leading-5 font-normal mt-2 break-words"
              >
                {description}
              </p>
            )}
          </div>
          <div className="flex items-center gap-4">
            {/* Close button — see dialog-close-button.md */}
            <button
              className="
                flex items-center justify-center
                w-8 h-8 rounded-md
                bg-[var(--bg-secondary)] text-[var(--fg-tertiary)]
                cursor-pointer outline-none border-none
                hover:bg-[var(--bg-secondary_hover)]
                transition-colors duration-150 ease-out
              "
              onClick={onClose}
              aria-label="Close"
            >
              <X className="w-4 h-4" />
            </button>
          </div>
        </div>

        {/* === CONTENT === */}
        <div
          className="flex-1 overflow-x-hidden overflow-y-auto"
          style={{
            padding: '24px',
            borderTop: '1px solid var(--border-tertiary)',
          }}
        >
          {children}
        </div>

        {/* === FOOTER === */}
        {footer && (
          <div
            className="flex items-center gap-4 justify-end"
            style={{
              padding: '12px 24px',
              background: 'var(--bg-secondary_subtle)',
            }}
          >
            {footer}
          </div>
        )}
      </div>
    </>
  );
}
```

---

## Responsive Behavior (< 1024px)

| Property | Change |
|----------|--------|
| Footer buttons | Become `width: 100%` (full-width stacked) |
| Max width | Still `calc(100vw - 16px)` |

```jsx
{/* Footer — mobile responsive: buttons stack full-width on screens < 1024px */}
<div
  className="flex flex-col lg:flex-row lg:justify-end gap-4"
  style={{
    padding: '12px 24px',
    background: 'var(--bg-secondary_subtle)',
    borderBottomLeftRadius: '12px',
    borderBottomRightRadius: '12px',
  }}
>
  <button className="
    w-full lg:w-auto
    h-10 px-4 py-2.5 rounded-lg
    bg-[var(--bg-secondary)] text-[var(--text-primary)]
    border border-[var(--border-secondary-solid)]
    text-sm leading-5 font-semibold
    hover:bg-[var(--bg-secondary_hover)] hover:shadow-sm
    transition-[background-color,box-shadow] duration-150 ease-out
    cursor-pointer outline-none
  ">Cancel</button>
  <button className="
    w-full lg:w-auto
    h-10 px-4 py-2.5 rounded-lg
    bg-[var(--bg-brand-solid)] text-[var(--text-white)]
    text-sm leading-5 font-semibold
    hover:bg-[var(--bg-brand-solid_hover)] hover:shadow-sm
    transition-[background-color,box-shadow] duration-150 ease-out
    cursor-pointer border-none outline-none
  ">Confirm</button>
</div>
```

---

## CRITICAL Rules

1. **Modal is a white rounded card on a dark overlay** — background `var(--bg-primary)` (#FFFFFF), border `1px solid var(--border-tertiary)` (#E8ECF1), border-radius `12px`.
2. **Backdrop color is `var(--bg_overlay)` (rgba(73, 88, 131, 0.56))** — NOT black, NOT fully opaque. It is a dark blue-gray at ~56% opacity.
3. **Header padding is asymmetric**: `12px 12px 12px 24px` — left side has MORE padding than right.
4. **Content area has a top border** (`1px solid var(--border-tertiary)` (#E8ECF1)) separating it from the header. Content padding is `24px` all sides.
5. **Footer background is `var(--bg-secondary_subtle)` (#F7F9FA)** (light gray) — NOT the same white as the panel. This creates a subtle visual separation.
6. **Footer padding is `12px 24px`** (12px top/bottom, 24px left/right).
7. **Footer buttons are right-aligned on desktop** (`justify-end`) and **full-width on mobile** (< 1024px).
8. **Modal is positioned at `top: 80px`**, horizontally centered with `left: 50%; transform: translateX(-50%)`.
9. **Max height is `calc(100dvh - 160px)`** — 80px top offset + 80px bottom offset. Content scrolls within this constraint.
10. **Default size is Medium (770px)**. Do NOT use percentage widths.
11. **Close button is always present** in the top-right of the header unless explicitly hidden. See `dialog-close-button.md` for the exact close button spec.
12. **Title uses heading-sm** (18px/28px/600). Description uses paragraph-md (14px/20px/400, color `var(--text-secondary)` (#495883)).
13. **The modal must have `overflow: hidden`** on the outer panel — only the Content area scrolls internally.
