# Slide-Out Dialog Component — Strict Specification

When building any side panel, slide-out drawer, detail panel, or right-hand dialog, follow these specifications exactly. Do NOT use shadcn Sheet, Tailwind drawers, Radix Sheet, or any other slide-out system.

> **TOKEN RULE:** Every colour in this file is expressed as a CSS variable token.
> In **tables / prose** write `var(--token)` (#HEX).
> In **Tailwind classes** write `bg-[var(--bg-token)]`, `text-[var(--text-token)]`, `border-[var(--border-token)]`, etc.
> In **inline styles** write `'var(--token)'`.
> Do NOT use raw hex values outside the parenthetical annotation.

**Note:** The close button uses the Dialog Close Button spec (see `dialog-close-button.md`). Buttons in the footer use the Button spec (see `button.md`).

---

## Architecture Overview

```
Backdrop (fixed overlay covering entire viewport)
└── Slide-Out Panel (right-aligned, full-height, rounded white card)
    ├── Header (title + description + optional badge/tabs + close button)
    │   ├── Row 1: Left (Title + Description) ←→ Right (Badge, Tabs, Buttons, Close)
    │   └── Row 2 (optional): Tabs below header (with left/right padding)
    ├── Content (scrollable body, NO top border)
    │   └── Any content
    └── Footer (supporting elements left ←→ action buttons right, top border)
        ├── Left: Supporting elements
        └── Right: Action buttons (12px gap)
```

**KEY STRUCTURAL RULES:**
1. The panel slides in from the **right edge** of the viewport (or left for RTL).
2. The panel is a **rounded white card** with a left shadow — it does NOT touch the viewport edges (8px margin all around).
3. Header has a **bottom border**. Footer has a **top border**. Content has **no borders**.
4. Footer layout is **split**: supporting elements on the left, action buttons on the right.
5. The close button is ALWAYS present in the header (top-right) unless explicitly hidden.

---

## Backdrop / Overlay

Same as Modal — see `modal.md`.

```jsx
<div
  className="fixed inset-0 z-50"
  style={{ background: 'var(--bg_overlay)' }}
  onClick={onClose}
/>
```

| Property | Value |
|----------|-------|
| Background | `var(--bg_overlay)` (rgba(73, 88, 131, 0.56)) |
| Position | `fixed inset-0 z-50` |

---

## Slide-Out Panel (Container)

```jsx
<div
  className="fixed z-50 flex flex-col overflow-hidden"
  style={{
    top: '8px',
    right: '8px',
    width: '770px',
    height: 'calc(100dvh - 16px)',
    background: 'var(--bg-primary)',
    border: '1px solid var(--border-tertiary)',
    borderRadius: '12px',
    boxShadow: 'var(--shadow-left-md)',
  }}
>
  {/* Header */}
  {/* Content */}
  {/* Footer */}
</div>
```

### Sizes

| Size | Width | Notes |
|------|-------|-------|
| Extra Small | 384px | Narrow panels |
| Small | 480px | Compact panels |
| **Medium (default)** | **770px** | Standard detail panel |
| Large | min(1025px, calc(100vw - 16px)), actual: 75vw | Wide panel |
| Extra Large | min(1200px, calc(100vw - 16px)), actual: 90vw | Very wide panel |

### Container Properties

| Property | Value | CSS |
|----------|-------|-----|
| Position | Fixed, top-right | `fixed`, `top: 8px`, `right: 8px` |
| Height | calc(100dvh - 16px) | Full viewport minus 8px margin top + bottom |
| Background | `var(--bg-primary)` (#FFFFFF) | `bg-[var(--bg-primary)]` |
| Border | 1px solid `var(--border-tertiary)` (#E8ECF1) | `border border-[var(--border-tertiary)]` |
| Border radius | 12px | `rounded-xl` or `rounded-[12px]` |
| Box shadow | `var(--shadow-left-md)` (-4px 0 8px 0 rgba(34, 42, 63, 0.08)) | shadow-left-md |
| Overflow | hidden | `overflow-hidden` |
| Layout | flex column | `flex flex-col` |
| Z-index | 50 | `z-50` |

---

## Header

```jsx
{/* Header container — has bottom border */}
<div style={{ borderBottom: '1px solid var(--border-tertiary)' }}>

  {/* Row 1: Title area + actions */}
  <div
    className="flex items-center justify-between gap-4"
    style={{ padding: '12px 12px 12px 24px' }}
  >
    {/* Left: Title + Description */}
    <div>
      <h2
        className="text-[var(--text-primary)]"
        style={{ fontSize: '18px', lineHeight: '28px', fontWeight: 600 }}
      >
        {titleText}
      </h2>
      {description && (
        <p className="text-[var(--text-secondary)] text-xs leading-[18px] font-normal flex items-center gap-2 break-words">
          {descriptionIcon && <span className="text-[var(--text-primary)]">{descriptionIcon}</span>}
          {description}
        </p>
      )}
    </div>

    {/* Right: Optional badge, tabs, buttons + close button */}
    <div className="flex items-center gap-4">
      {/* Optional: Badge */}
      {/* Optional: Tab group (top position) */}
      {/* Optional: Action buttons */}
      {/* Close button — see dialog-close-button.md */}
      <CloseButton onClick={onClose} />
    </div>
  </div>

  {/* Row 2 (optional): Tabs below header */}
  {tabs && (
    <div style={{ paddingLeft: '24px', paddingRight: '12px' }}>
      {tabs}
    </div>
  )}
</div>
```

### Header Properties

| Property | Value | CSS |
|----------|-------|-----|
| Border bottom | 1px solid `var(--border-tertiary)` (#E8ECF1) | `border-b border-[var(--border-tertiary)]` |
| Row 1 padding | 12px top, 12px right, 12px bottom, **24px left** | `style={{ padding: '12px 12px 12px 24px' }}` |
| Row 1 layout | Flex, items-center, space-between | `flex items-center justify-between` |
| Row 1 gap | 16px | `gap-4` |
| Row 2 padding (tabs) | 0 top, 12px right, 0 bottom, 24px left | `pl-6 pr-3` |

### Title Typography (Slide-Out)

| Property | Value | Tailwind |
|----------|-------|----------|
| Element | `<h2>` | — |
| Mixin | title-xl | — |
| Font size | 18px | `text-lg` |
| Line height | 28px | `leading-7` |
| Font weight | 600 | `font-semibold` |
| Color | `var(--text-primary)` (#353E5A) | `text-[var(--text-primary)]` |

### Description Typography (Slide-Out)

| Property | Value | Tailwind |
|----------|-------|----------|
| Element | `<p>` | — |
| Mixin | paragraph-sm | — |
| Font size | 12px | `text-xs` |
| Line height | 18px | `leading-[18px]` |
| Font weight | 400 | `font-normal` |
| Color | `var(--text-secondary)` (#495883) | `text-[var(--text-secondary)]` |
| Description icon color | `var(--text-primary)` (#353E5A) | `text-[var(--text-primary)]` |

---

## Content (Body)

```jsx
<div
  className="flex-1 overflow-x-hidden overflow-y-auto"
  style={{ padding: '24px' }}
>
  {/* Any content */}
</div>
```

| Property | Value | CSS / Tailwind |
|----------|-------|----------------|
| Padding | 24px all sides (default) | `p-6` |
| Padding (gap="0") | 0 | `p-0` |
| Flex | 1 (fills remaining space) | `flex-1` |
| Overflow X | hidden | `overflow-x-hidden` |
| Overflow Y | auto (scrollable) | `overflow-y-auto` |
| Border | **NONE** | — |

**Unlike the Modal, the Slide-Out Content has NO top border.** The header's bottom border provides the visual separation.

---

## Footer

```jsx
<div
  className="flex items-center justify-between gap-2"
  style={{
    padding: '12px 24px',
    borderTop: '1px solid var(--border-tertiary)',
  }}
>
  {/* Left: Supporting elements */}
  <div>
    {supportingElements}
  </div>

  {/* Right: Action buttons */}
  <div className="flex gap-3">
    <button className="...">Cancel</button>
    <button className="...">Save</button>
  </div>
</div>
```

| Property | Value | CSS / Tailwind |
|----------|-------|----------------|
| Padding | 12px top/bottom, 24px left/right | `py-3 px-6` |
| Border top | 1px solid `var(--border-tertiary)` (#E8ECF1) | `border-t border-[var(--border-tertiary)]` |
| Layout | Flex, items-center, space-between | `flex items-center justify-between` |
| Gap (outer) | 8px | `gap-2` |
| Gap (buttons) | 12px | `gap-3` |
| Background | transparent (inherits white) | — |

**Slide-Out Footer is DIFFERENT from Modal Footer:**
- Footer has a **top border** (not gray background)
- Footer background is **white** (not `var(--bg-secondary_subtle)` (#F7F9FA))
- Layout is **split**: supporting elements left, buttons right
- Buttons container has **12px gap** (not 16px)

---

## Color Reference

| Element | Token |
|---------|-------|
| Backdrop | `var(--bg_overlay)` (rgba(73, 88, 131, 0.56)) |
| Panel background | `var(--bg-primary)` (#FFFFFF) |
| Panel border | `var(--border-tertiary)` (#E8ECF1) |
| Header bottom border | `var(--border-tertiary)` (#E8ECF1) |
| Footer top border | `var(--border-tertiary)` (#E8ECF1) |
| Panel shadow | `var(--shadow-left-md)` (-4px 0 8px 0 rgba(34, 42, 63, 0.08)) |
| Title text | `var(--text-primary)` (#353E5A) |
| Description text | `var(--text-secondary)` (#495883) |
| Description icon | `var(--text-primary)` (#353E5A) |

---

## Animation

| Property | Value |
|----------|-------|
| Slide-in direction | From right (enters viewport from right edge) |
| Transition | `transform 0.3s ease` or `margin-right 0.3s ease` |
| Initial state | Off-screen to the right |
| Final state | Right-aligned with 8px margin |

### Implementation

```jsx
// Simple slide-in animation with CSS transition
<div
  className="fixed z-50 flex flex-col overflow-hidden transition-transform duration-300 ease-out"
  style={{
    top: '8px',
    right: '8px',
    transform: isOpen ? 'translateX(0)' : 'translateX(calc(100% + 8px))',
    // ... other styles
  }}
>
```

---

## Complete Implementation Pattern

```jsx
function SlideOutDialog({
  isOpen, onClose, title, description, descriptionIcon,
  children, footer, supportingElements, size = 'medium', tabs,
}) {
  if (!isOpen) return null;

  const widthMap = {
    'extra-small': '384px',
    'small': '480px',
    'medium': '770px',
    'large': '75vw',
    'extra-large': '90vw',
  };

  return (
    <>
      {/* Backdrop */}
      <div
        className="fixed inset-0 z-50"
        style={{ background: 'var(--bg_overlay)' }}
        onClick={onClose}
      />

      {/* Slide-Out Panel */}
      <div
        className="fixed z-50 flex flex-col overflow-hidden transition-transform duration-300 ease-out"
        role="dialog"
        aria-modal="true"
        aria-labelledby="slideout-title"
        aria-describedby={description ? 'slideout-description' : undefined}
        style={{
          top: '8px',
          right: '8px',
          width: widthMap[size],
          height: 'calc(100dvh - 16px)',
          background: 'var(--bg-primary)',
          border: '1px solid var(--border-tertiary)',
          borderRadius: '12px',
          boxShadow: 'var(--shadow-left-md)',
        }}
      >
        {/* === HEADER === */}
        <div style={{ borderBottom: '1px solid var(--border-tertiary)' }}>
          <div
            className="flex items-center justify-between gap-4"
            style={{ padding: '12px 12px 12px 24px' }}
          >
            <div>
              <h2
                id="slideout-title"
                className="text-[var(--text-primary)]"
                style={{ fontSize: '18px', lineHeight: '28px', fontWeight: 600 }}
              >
                {title}
              </h2>
              {description && (
                <p
                  id="slideout-description"
                  className="text-[var(--text-secondary)] text-xs leading-[18px] font-normal flex items-center gap-2 break-words"
                >
                  {descriptionIcon && (
                    <span className="text-[var(--text-primary)]">{descriptionIcon}</span>
                  )}
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

          {/* Optional: Tabs below header */}
          {tabs && (
            <div style={{ paddingLeft: '24px', paddingRight: '12px' }}>
              {tabs}
            </div>
          )}
        </div>

        {/* === CONTENT === */}
        <div
          className="flex-1 overflow-x-hidden overflow-y-auto"
          style={{ padding: '24px' }}
        >
          {children}
        </div>

        {/* === FOOTER === */}
        {(footer || supportingElements) && (
          <div
            className="flex items-center justify-between gap-2"
            style={{
              padding: '12px 24px',
              borderTop: '1px solid var(--border-tertiary)',
            }}
          >
            <div>{supportingElements}</div>
            <div className="flex gap-3">{footer}</div>
          </div>
        )}
      </div>
    </>
  );
}
```

---

## Mobile Behavior (< 1024px)

On screens narrower than 1024px, the slide-out transforms into a **bottom sheet**:

| Property | Change |
|----------|--------|
| Position | Bottom of viewport (not right side) |
| Width | 100% |
| Margin | 0 (no gap from edges) |
| Shadow | None |
| Border radius | Kept on top corners only |
| Height | calc(100dvh - 64px) |
| Slide direction | From bottom (not right) |
| Footer buttons | Full-width (`width: 100%`) |

---

## Key Differences from Modal

| Aspect | Modal | Slide-Out |
|--------|-------|-----------|
| Position | Centered, top: 80px | Right edge, top: 8px, full height |
| Height | Auto (max-height constrained) | Full viewport height minus 16px |
| Shadow | None | Left shadow (`var(--shadow-left-md)`) |
| Margin | None | 8px all around |
| Header title | heading-sm (18px/28px) | title-xl (18px/28px) |
| Header description | paragraph-md (14px) | paragraph-sm (12px) |
| Header border | None | Bottom border `var(--border-tertiary)` (#E8ECF1) |
| Content border | Top border `var(--border-tertiary)` (#E8ECF1) | None |
| Footer background | `var(--bg-secondary_subtle)` (#F7F9FA) (gray) | transparent (white) |
| Footer border | None | Top border `var(--border-tertiary)` (#E8ECF1) |
| Footer layout | Right-aligned buttons | Split: supporting left, buttons right |
| Footer button gap | 16px | 12px |
| Header slots | Close button only | Badge, tabs, buttons, close button |

---

## CRITICAL Rules

1. **Slide-out is a full-height panel on the right** with 8px margin from all viewport edges. Height is `calc(100dvh - 16px)`.
2. **Left shadow** (`var(--shadow-left-md)`) — the shadow falls to the LEFT of the panel.
3. **Border radius is 12px** on all corners (it's a floating card, not flush against the edge).
4. **Header has a BOTTOM border** (`1px solid var(--border-tertiary)` (#E8ECF1)). Content has NO borders. Footer has a TOP border.
5. **Footer is split layout**: supporting elements on the left, action buttons on the right with `gap: 12px`.
6. **Footer background is WHITE** (transparent/inherits) — NOT the gray `var(--bg-secondary_subtle)` (#F7F9FA) used in Modal footer.
7. **Header padding is asymmetric**: `12px 12px 12px 24px` — more padding on the left.
8. **Title uses title-xl** (18px/28px/600) — same size as Modal's heading-sm (18px/28px) but uses the `title` type mixin (content font scale), not the `heading` type mixin.
9. **Description uses paragraph-sm** (12px/18px/400) — SMALLER than Modal's paragraph-md.
10. **Default size is Medium (770px)**. Large uses 75vw (min 1025px). Extra Large uses 90vw (min 1200px).
11. **Content padding is 24px** by default. Can be `0` when gap="0" mode is used.
12. **Close button is always present** in the header top-right. See `dialog-close-button.md`.
13. **On mobile (< 1024px)** the slide-out becomes a bottom sheet — full width, slides from bottom.
14. **Backdrop is the same as Modal** — `var(--bg_overlay)` (rgba(73, 88, 131, 0.56)).
