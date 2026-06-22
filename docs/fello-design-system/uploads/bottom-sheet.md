# Bottom Sheet Component — Strict Specification

When building any bottom sheet dialog, drawer from the bottom, or mobile-first overlay panel, follow these specifications exactly. Do NOT use shadcn Sheet, Radix Dialog, Tailwind drawers, or any other bottom sheet system.

> **TOKEN RULE:** Every colour in this file is expressed as a CSS variable token.
> In **tables / prose** write `var(--token)` (#HEX).
> In **Tailwind classes** write `bg-[var(--bg-token)]`, `text-[var(--text-token)]`, `border-[var(--border-token)]`, etc.
> In **inline styles** write `'var(--token)'`.
> Do NOT use raw hex values outside the parenthetical annotation.

**Note:** The close button uses the Dialog Close Button spec (see `dialog-close-button.md`). Buttons in the footer use the Button spec (see `button.md`). The inner Header / Content / Footer structure matches the Slide-Out Dialog spec (see `slide-out-dialog.md`).

---

## Architecture Overview

```
Backdrop (fixed overlay covering entire viewport)
└── Bottom Sheet Panel (bottom-anchored, horizontally centered, rounded top)
    ├── Header (title + description + close button)
    │   └── Row 1: Left (Title + Description) ←→ Right (Close button)
    ├── Content (scrollable body, NO top border)
    └── Footer (supporting elements left ←→ action buttons right, top border)
```

**KEY STRUCTURAL RULES:**
1. The panel slides **up from the bottom** of the viewport.
2. On desktop, the panel is **centered horizontally** with 80px margin on each side — it does NOT touch the left or right viewport edges.
3. On mobile (< 1023px), the panel is **full width** (no side margins).
4. Border-radius is **12px on top corners only** — the bottom corners are always flat (0px) since the panel is anchored to the bottom of the viewport.
5. Shadow goes **upward** — `var(--shadow-up-md)`.
6. Inner structure (Header, Content, Footer) is **identical to the Slide-Out Dialog** — see `slide-out-dialog.md`.

---

## Backdrop / Overlay

```jsx
<div
  className="fixed inset-0 z-50"
  style={{ background: 'var(--bg_overlay)' }}
  onClick={onClose}
/>
```

| Property | Value |
|----------|-------|
| Position | `fixed inset-0 z-50` |
| Background | `var(--bg_overlay)` (rgba(73, 88, 131, 0.56)) |

---

## Bottom Sheet Panel (Container)

```jsx
<div
  className="fixed z-50 flex flex-col overflow-hidden"
  style={{
    bottom: 0,
    left: '50%',
    transform: 'translateX(-50%)',
    width: 'calc(100% - 160px)',    // 80px margin each side
    height: 'calc(100dvh - 80px)', // leaves 80px gap at the top
    background: 'var(--bg-primary)',
    border: '1px solid var(--border-tertiary)',
    borderRadius: '12px 12px 0 0', // top corners rounded, bottom flat
    boxShadow: 'var(--shadow-up-md)',
  }}
>
  {/* Header */}
  {/* Content */}
  {/* Footer */}
</div>
```

### Container Properties

| Property | Value | CSS |
|----------|-------|-----|
| Position | Fixed, bottom of viewport | `fixed`, `bottom: 0` |
| Horizontal | Centered | `left: 50%`, `transform: translateX(-50%)` |
| Width (desktop) | `calc(100% - 160px)` — 80px margin each side | `style={{ width: 'calc(100% - 160px)' }}` |
| Width (mobile < 1023px) | `100%` — full width, no side margins | `w-full` |
| Height | `calc(100dvh - 80px)` — leaves 80px gap at top | `style={{ height: 'calc(100dvh - 80px)' }}` |
| Background | `var(--bg-primary)` (#FFFFFF) | `bg-[var(--bg-primary)]` |
| Border | 1px solid `var(--border-tertiary)` (#E8ECF1) | `border border-[var(--border-tertiary)]` |
| Border radius | 12px top-left, 12px top-right, 0 bottom-right, 0 bottom-left | `style={{ borderRadius: '12px 12px 0 0' }}` |
| Box shadow | `var(--shadow-up-md)` (0 -4px 8px 0 rgba(73, 88, 131, 0.08)) | `style={{ boxShadow: 'var(--shadow-up-md)' }}` |
| Overflow | hidden | `overflow-hidden` |
| Layout | flex column | `flex flex-col` |
| Z-index | 50 | `z-50` |

> **IMPORTANT:** Do NOT use `borderRadius: '12px'` — the bottom corners must be `0`. Always use `borderRadius: '12px 12px 0 0'` via inline style.

---

## Width Behaviour

Unlike Modal and Slide-Out Dialog, the Bottom Sheet does **not** use size-based widths (extra-small / small / medium etc.). It always uses a percentage-based width:

| Breakpoint | Width |
|------------|-------|
| Desktop (≥ 1023px) | `calc(100% - 160px)` — 80px margin on each side |
| Mobile (< 1023px) | `100%` — full width |

---

## Animation

The panel slides up from below the viewport.

| Property | Value |
|----------|-------|
| Direction | Slides **up** from below |
| Transition | `transform 0.3s ease` |
| Initial state (closed) | `translateX(-50%) translateY(100%)` — fully below viewport |
| Final state (open) | `translateX(-50%) translateY(0)` — flush with bottom of viewport |

```jsx
<div
  className="fixed z-50 flex flex-col overflow-hidden transition-transform duration-300 ease-out"
  style={{
    bottom: 0,
    left: '50%',
    transform: isOpen
      ? 'translateX(-50%) translateY(0)'
      : 'translateX(-50%) translateY(100%)',
    width: 'calc(100% - 160px)',
    height: 'calc(100dvh - 80px)',
    background: 'var(--bg-primary)',
    border: '1px solid var(--border-tertiary)',
    borderRadius: '12px 12px 0 0',
    boxShadow: 'var(--shadow-up-md)',
  }}
>
```

---

## Inner Structure — Header, Content, Footer

The bottom sheet uses the **exact same** Header, Content, and Footer structure as the Slide-Out Dialog. Refer to `slide-out-dialog.md` for full details. Key points:

### Header
- Bottom border: `1px solid var(--border-tertiary)` (#E8ECF1)
- Padding: `12px 12px 12px 24px` (more left padding)
- Title: 18px / 28px / 600 weight / `var(--text-primary)` (#353E5A)
- Description: 12px / 18px / 400 weight / `var(--text-secondary)` (#495883)
- Close button always present top-right — see `dialog-close-button.md`

### Content
- `flex-1 overflow-x-hidden overflow-y-auto`
- Padding: `24px` all sides
- **NO top border** — border is on the header bottom, not the content top

### Footer
- Top border: `1px solid var(--border-tertiary)` (#E8ECF1)
- Padding: `12px 24px`
- Background: white (transparent — NOT the gray `var(--bg-secondary_subtle)`)
- Layout: split — supporting elements left, action buttons right
- Button gap: `12px`
- Mobile: buttons become full-width (`width: 100%`)

---

## Complete Implementation Pattern

```jsx
function BottomSheet({
  isOpen, onClose, title, description,
  children, footer, supportingElements, tabs,
}) {
  if (!isOpen) return null;

  return (
    <>
      {/* Backdrop */}
      <div
        className="fixed inset-0 z-50"
        style={{ background: 'var(--bg_overlay)' }}
        onClick={onClose}
      />

      {/* Bottom Sheet Panel */}
      <div
        className="fixed z-50 flex flex-col overflow-hidden transition-transform duration-300 ease-out"
        role="dialog"
        aria-modal="true"
        aria-labelledby="bottom-sheet-title"
        style={{
          bottom: 0,
          left: '50%',
          transform: 'translateX(-50%) translateY(0)',
          width: 'calc(100% - 160px)',
          height: 'calc(100dvh - 80px)',
          background: 'var(--bg-primary)',
          border: '1px solid var(--border-tertiary)',
          borderRadius: '12px 12px 0 0',
          boxShadow: 'var(--shadow-up-md)',
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
                id="bottom-sheet-title"
                className="text-[var(--text-primary)]"
                style={{ fontSize: '18px', lineHeight: '28px', fontWeight: 600 }}
              >
                {title}
              </h2>
              {description && (
                <p className="text-[var(--text-secondary)] text-xs leading-[18px] font-normal flex items-center gap-2 break-words">
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

## Mobile Responsive (< 1023px)

```jsx
{/* Width switches to 100% on mobile */}
<div
  style={{
    width: window.innerWidth < 1023 ? '100%' : 'calc(100% - 160px)',
    // ... other styles unchanged
  }}
>
```

Or with Tailwind responsive classes for width:
```jsx
<div
  className="w-full lg:w-[calc(100%-160px)]"
  style={{
    bottom: 0,
    left: '50%',
    transform: 'translateX(-50%)',
    // ...
  }}
>
```

---

## Color Reference

| Element | Token | Hex |
|---------|-------|-----|
| Backdrop | `var(--bg_overlay)` | rgba(73, 88, 131, 0.56) |
| Panel background | `var(--bg-primary)` | #FFFFFF |
| Panel border | `var(--border-tertiary)` | #E8ECF1 |
| Header bottom border | `var(--border-tertiary)` | #E8ECF1 |
| Footer top border | `var(--border-tertiary)` | #E8ECF1 |
| Panel shadow | `var(--shadow-up-md)` | 0 -4px 8px 0 rgba(73, 88, 131, 0.08) |
| Title text | `var(--text-primary)` | #353E5A |
| Description text | `var(--text-secondary)` | #495883 |

---

## How Bottom Sheet Differs from Slide-Out and Modal

| Aspect | Modal | Slide-Out | Bottom Sheet |
|--------|-------|-----------|--------------|
| Anchor | Top-center (80px from top) | Right edge (or left for LTR) | Bottom edge |
| Shadow | None | Left shadow | **Up shadow** |
| Border radius | 12px all corners | 12px all corners | **12px top only, 0 bottom** |
| Width | Size-based (384–1280px) | Size-based (384px–90vw) | **`calc(100% - 160px)`** desktop, `100%` mobile |
| Height | Auto, max `calc(100dvh - 160px)` | `calc(100dvh - 16px)` | **`calc(100dvh - 80px)`** |
| Animation | Fade/drop from top | Slide from right/left | **Slide up from below** |
| Side margins | None | 8px all around | **80px each side** (desktop only) |
| Content border | Top border ✓ | No top border | **No top border** (matches slide-out) |
| Footer background | Gray `var(--bg-secondary_subtle)` | White | **White** (matches slide-out) |

---

## CRITICAL Rules

1. **Border radius is `12px 12px 0 0`** — top corners rounded, bottom corners FLAT. ALWAYS use inline `style={{ borderRadius: '12px 12px 0 0' }}`. Never use `rounded-xl` (which rounds all corners).
2. **Shadow is upward** — `var(--shadow-up-md)` (0 -4px 8px 0 rgba(73, 88, 131, 0.08)). NOT left or right shadow.
3. **Width is percentage-based** — `calc(100% - 160px)` on desktop. Size props (extra-small, medium, etc.) do NOT apply.
4. **Height is `calc(100dvh - 80px)`** — leaves an 80px gap at the top so the backdrop is visible above.
5. **Panel anchors at `bottom: 0`** — NOT `top`. The panel's bottom edge is flush with the viewport bottom.
6. **Horizontally centered via `left: 50%; transform: translateX(-50%)`** — same centering technique as Modal.
7. **Mobile (< 1023px): width becomes `100%`** — no side margins on small screens.
8. **Inner structure matches Slide-Out Dialog** — header has bottom border, content has NO top border, footer has top border and white background (NOT gray).
9. **Backdrop is identical to Modal and Slide-Out** — `var(--bg_overlay)` (rgba(73, 88, 131, 0.56)).
10. **Animation slides up** — initial `translateY(100%)`, final `translateY(0)`, combined with the horizontal `translateX(-50%)`.
