# Popup Component — Strict Specification

When building any popup, popover, dropdown panel, or floating content container, follow these specifications exactly. Do NOT use default Tailwind popovers, shadcn popovers, or any other popup system.

> **TOKEN RULE:** Every color in this file is expressed as a CSS variable token. In code use `bg-[var(--bg-token)]` / `text-[var(--text-token)]` / `border-[var(--border-token)]` classes. In inline styles use `'var(--token)'`. The parenthetical hex is for reference only — never use raw hex in production code.

---

## Popup Anatomy

A floating content container anchored to a trigger element:

```
                  ┌─────────────────────────┐
                  │  Header (optional)       │  ← Title + close button
                  ├─────────────────────────┤
                  │                         │
                  │  Body content            │  ← Scrollable if overflow
                  │                         │
                  ├─────────────────────────┤
                  │         [Actions]        │  ← Footer (optional)
                  └─────────────────────────┘
                           ◇
                     (arrow indicator)
                        [Trigger]
```

- Anchored to a trigger element (button, icon, text, etc.)
- Arrow indicator points from the popup toward the trigger
- Can appear above, below, left, or right of the trigger
- Optional header with title and close button
- Optional footer with action buttons
- Invisible buffer zone prevents accidental closure on hover triggers

---

## Container

| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `var(--bg-primary)` (#FFFFFF) | `bg-[var(--bg-primary)]` |
| Border | 1px solid `var(--border-secondary)` (#CCD6DC) | `border border-[var(--border-secondary)]` |
| Border radius | 8px | `rounded-lg` |
| Box shadow | Large elevation | `shadow-2xl` |
| Max height | 440px | `max-h-[440px]` |
| Overflow | Auto (scrollable) | `overflow-auto` |
| Position | Absolute | `absolute` |
| Z-index | 50 | `z-50` |

---

## Sizes (Width)

| Size | Width | Tailwind |
|------|-------|----------|
| 0 (Auto) | max-content | `w-max` |
| XS | 192px | `w-[192px]` |
| SM | 224px | `w-[224px]` |
| MD (Default) | 256px | `w-[256px]` |
| LG | 300px | `w-[300px]` |
| XL | 384px | `w-[384px]` |

---

## Gap (Inner Padding)

| Gap Size | Padding | Tailwind |
|----------|---------|----------|
| XS | 8px | `p-2` |
| SM | 12px | `p-3` |
| MD (Default) | 16px | `p-4` |
| LG | 24px | `p-6` |
| XL | 32px | `p-8` |

---

## Arrow Indicator

The arrow is a rotated diamond shape pointing from the popup toward the trigger:

| Property | Value |
|----------|-------|
| Size | 10px × 10px |
| Shape | Square rotated 45° |
| Background | `var(--bg-primary)` (#FFFFFF) |
| Border | 1px solid `var(--border-secondary)` (#CCD6DC) (top and left sides only) |
| Offset | -6px from popup edge |

### Arrow Rotation by Position

| Popup Position | Arrow Rotation | Arrow Location |
|----------------|---------------|----------------|
| Above trigger | 225° | Bottom center of popup |
| Below trigger | 45° | Top center of popup |
| Left of trigger | 135° | Right center of popup |
| Right of trigger | 315° | Left center of popup |

---

## Animation

| Property | Value |
|----------|-------|
| Type | Slide in + fade |
| Duration | 200ms |
| Easing | ease-out |
| Distance | 12px (from direction) |
| Opacity | 0 → 1 |

### Slide Direction (matches popup position)

| Popup Position | Slide From |
|----------------|------------|
| Above trigger | Slides up from bottom |
| Below trigger | Slides down from top |
| Left of trigger | Slides left from right |
| Right of trigger | Slides right from left |

---

## Header (Optional)

| Property | Value | Tailwind |
|----------|-------|----------|
| Border bottom | 1px solid `var(--border-tertiary)` (#E8ECF1) | `border-b border-[var(--border-tertiary)]` |
| Padding | 12px vertical, 16px horizontal | `py-3 px-4` |
| Margin | -16px left/right, -16px top (pulls to popup edges) | `-mx-4 -mt-4 mb-4` |
| Display | Flex, space-between, centered | `flex items-center justify-between` |
| Border radius (top) | 6px top-left, 6px top-right | `rounded-t-md` |

### Header Title
| Property | Value | Tailwind |
|----------|-------|----------|
| Typography | 18px / 28px / weight 600 (title-xl) | `text-lg leading-7` + `style={{ fontWeight: 600 }}` |
| Color | `var(--text-primary)` (#353E5A) | `text-[var(--text-primary)]` |

### Header Close Button
| Property | Value | Tailwind |
|----------|-------|----------|
| Size | 24px | `w-6 h-6` |
| Icon | X (close) | `X` |
| Color | `var(--fg-tertiary)` (#6B748E) | `text-[var(--fg-tertiary)]` |
| Cursor | Pointer | `cursor-pointer` |
| Hover color | `var(--fg-primary)` (#353E5A) | `hover:text-[var(--fg-primary)]` |

---

## Footer (Optional)

| Property | Value | Tailwind |
|----------|-------|----------|
| Border top | 1px solid `var(--border-tertiary)` (#E8ECF1) | `border-t border-[var(--border-tertiary)]` |
| Padding | 12px vertical, 16px horizontal | `py-3 px-4` |
| Margin | -16px left/right, -16px bottom (pulls to popup edges) | `-mx-4 -mb-4 mt-4` |
| Display | Flex, end-aligned | `flex justify-end` |
| Gap | 12px | `gap-3` |
| Border radius (bottom) | 6px bottom-left, 6px bottom-right | `rounded-b-md` |

---

## Trigger Types

### Click Trigger
- Popup opens on click, closes on click outside or trigger re-click
- A transparent backdrop overlay captures outside clicks
- Backdrop: `position: fixed, inset: 0, z-index: 40, bg: transparent`

### Hover Trigger
- Popup opens after hovering trigger for 50ms
- Popup closes 200ms after mouse leaves both trigger and popup
- Invisible buffer zone (20px) between trigger and popup prevents accidental closure

### Manual Trigger
- Open/close is controlled programmatically
- No automatic dismiss behavior

---

## Buffer Zone (Hover Trigger)

| Property | Value |
|----------|-------|
| Size | 20px invisible area |
| Position | Between trigger and popup |
| Background | Transparent |
| Pointer events | All (captures mouse) |
| Purpose | Prevents popup from closing when moving mouse from trigger to popup |

---

## Implementation Pattern for Lovable

```jsx
{/* === Popup — Below trigger, MD size, MD gap === */}
<div className="relative inline-block">
  {/* Trigger */}
  <button
    onClick={() => setOpen(!open)}
    className="...trigger-styles..."
  >
    Open Popup
  </button>

  {/* Backdrop (click trigger only) */}
  {open && (
    <div
      className="fixed inset-0 z-40"
      onClick={() => setOpen(false)}
    />
  )}

  {/* Popup */}
  {open && (
    <div className="
      absolute top-full left-1/2 -translate-x-1/2 mt-2.5
      w-[256px] p-4
      bg-white rounded-lg
      border border-[var(--border-secondary)]
      shadow-2xl
      max-h-[440px] overflow-auto
      z-50
      animate-in fade-in slide-in-from-top-3 duration-200 ease-out
    ">
      {/* Arrow — pointing up (popup below trigger) */}
      <div
        className="absolute -top-[6px] left-1/2 -translate-x-1/2 w-2.5 h-2.5 bg-white border-l border-t border-[var(--border-secondary)]"
        style={{ transform: 'translateX(-50%) rotate(45deg)' }}
      />

      {/* Body content */}
      <p className="text-sm leading-5 text-[var(--text-secondary)]">
        Popup content goes here.
      </p>
    </div>
  )}
</div>


{/* === Popup — With Header and Footer === */}
<div className="relative inline-block">
  <button onClick={() => setOpen(!open)} className="...trigger-styles...">
    Settings
  </button>

  {open && (
    <div
      className="fixed inset-0 z-40"
      onClick={() => setOpen(false)}
    />
  )}

  {open && (
    <div className="
      absolute top-full left-0 mt-2.5
      w-[300px] p-4
      bg-white rounded-lg
      border border-[var(--border-secondary)]
      shadow-2xl
      max-h-[440px] overflow-auto
      z-50
    ">
      {/* Header — negative margins pull to edges */}
      <div className="
        -mx-4 -mt-4 mb-4
        flex items-center justify-between
        py-3 px-4
        border-b border-[var(--border-tertiary)]
        rounded-t-md
      ">
        <span
          className="text-lg leading-7 text-[var(--text-primary)]"
          style={{ fontWeight: 600 }}
        >
          Settings
        </span>
        <button
          className="text-[var(--fg-tertiary)] hover:text-[var(--fg-primary)] cursor-pointer"
          onClick={() => setOpen(false)}
        >
          <X className="w-6 h-6" />
        </button>
      </div>

      {/* Body */}
      <div className="space-y-3">
        <p className="text-sm leading-5 text-[var(--text-secondary)]">
          Configure your preferences below.
        </p>
        {/* ...form fields, options, etc... */}
      </div>

      {/* Footer — negative margins pull to edges */}
      <div className="
        -mx-4 -mb-4 mt-4
        flex justify-end gap-3
        py-3 px-4
        border-t border-[var(--border-tertiary)]
        rounded-b-md
      ">
        <button
          className="
            h-8 py-2 px-3 rounded-md
            bg-[var(--bg-secondary)] text-[var(--text-primary)]
            border border-[var(--fg-disabled)]
            text-sm leading-5
            hover:bg-[var(--bg-secondary_hover)]
            transition-all duration-150 ease-out
          "
          style={{ fontWeight: 600 }}
          onClick={() => setOpen(false)}
        >
          Cancel
        </button>
        <button
          className="
            h-8 py-2 px-3 rounded-md
            bg-[var(--bg-brand-solid)] text-white
            text-sm leading-5
            hover:bg-[var(--bg-brand-solid_hover)]
            transition-all duration-150 ease-out
          "
          style={{ fontWeight: 600 }}
        >
          Save
        </button>
      </div>
    </div>
  )}
</div>


{/* === Popup — Above trigger, XS gap === */}
<div className="relative inline-block">
  <button onClick={() => setOpen(!open)} className="...trigger-styles...">
    Info
  </button>

  {open && (
    <div className="
      absolute bottom-full left-1/2 -translate-x-1/2 mb-2.5
      w-[192px] p-2
      bg-white rounded-lg
      border border-[var(--border-secondary)]
      shadow-2xl
      z-50
    ">
      {/* Arrow — pointing down (popup above trigger) */}
      <div
        className="absolute -bottom-[6px] left-1/2 -translate-x-1/2 w-2.5 h-2.5 bg-white border-r border-b border-[var(--border-secondary)]"
        style={{ transform: 'translateX(-50%) rotate(45deg)' }}
      />

      <p className="text-xs leading-[18px] text-[var(--text-secondary)]">
        Small info popup.
      </p>
    </div>
  )}
</div>


{/* === Popup — XL size, LG gap, no arrow === */}
<div className="relative inline-block">
  <button onClick={() => setOpen(!open)} className="...trigger-styles...">
    Menu
  </button>

  {open && (
    <div
      className="fixed inset-0 z-40"
      onClick={() => setOpen(false)}
    />
  )}

  {open && (
    <div className="
      absolute top-full right-0 mt-2
      w-[384px] p-6
      bg-white rounded-lg
      border border-[var(--border-secondary)]
      shadow-2xl
      max-h-[440px] overflow-auto
      z-50
    ">
      {/* Content */}
      <div className="space-y-4">
        <p className="text-sm leading-5 text-[var(--text-primary)]">Large popup content</p>
      </div>
    </div>
  )}
</div>
```

### Size Quick Reference

| Size | Width | Use Case |
|------|-------|----------|
| 0 (Auto) | max-content | Tooltips, small menus |
| XS (192px) | Narrow popups | Quick info, mini menus |
| SM (224px) | Small popups | Short forms, selectors |
| MD (256px) | Default | General purpose popups |
| LG (300px) | Large popups | Confirmation dialogs, settings |
| XL (384px) | Extra large | Complex forms, rich content |

### Key Implementation Rules

1. **Container uses 8px border-radius** (`rounded-lg`) with `var(--border-secondary)` (#CCD6DC) border and `shadow-2xl`
2. **Max height is 440px** — content scrolls when exceeding this
3. **Default size is MD (256px)** and default gap is MD (16px padding)
4. **Arrow is 10px × 10px** diamond — white background with partial border matching popup
5. **Arrow offset is -6px** from the popup edge
6. **Arrow rotation changes per position** — border sides visible must face away from popup
7. **Click triggers use a transparent fixed backdrop** (`z-40`) for outside-click detection
8. **Hover triggers use 50ms show delay, 200ms hide delay** with 20px buffer zone
9. **Header uses negative margins** (`-mx-4 -mt-4`) to span full popup width despite padding
10. **Footer uses negative margins** (`-mx-4 -mb-4`) similarly, with `border-t` separator
11. **Header/footer border radius is 6px** (`rounded-t-md` / `rounded-b-md`) — slightly less than container
12. **Animation slides 12px** from the anchor direction, fading in over 200ms
13. **Header title is 18px/28px/600**, close icon is 24px in `var(--fg-tertiary)` (#6B748E)
14. **Footer buttons align right** (`justify-end`) with 12px gap
