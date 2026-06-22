# Confirmation Popup Component — Strict Specification

When building any confirmation dialog, delete confirmation, action confirmation popup, or yes/no prompt, follow these specifications exactly. Do NOT use default Tailwind dialogs, shadcn alert dialogs, or any other confirmation system.

> **TOKEN RULE:** Every color in this spec is expressed as a CSS variable token. In Tailwind classes use `bg-[var(--token)]` / `text-[var(--token)]`. In inline styles use `'var(--token)'`. In prose and tables, show `var(--token)` (#hex) for reference.

---

## Confirmation Popup Anatomy

A small popup panel anchored to a trigger element (not a full-screen modal), containing a title, description, and two action buttons:

```
┌────────────────────────────────────┐
│                                    │
│  Are you sure?                     │  ← Title (18px/600)
│                                    │
│  This action cannot be undone.     │  ← Description (14px/500, tertiary color)
│                                    │
│  [Cancel]       [Confirm]          │  ← Two buttons
│                                    │
└────────────────────────────────────┘
         ▽ (arrow pointing to trigger)
```

- Uses the Popup component as its container (size LG = 300px)
- Positioned relative to a trigger element (default: above)
- Has an arrow indicator pointing to the trigger

---

## Container (from Popup)

| Property | Value | Tailwind |
|----------|-------|----------|
| Width | 300px (popup size LG) | `w-[300px]` |
| Padding | 16px | `p-4` |
| Max height | 440px | `max-h-[440px]` |
| Background | `var(--bg-primary)` (#FFFFFF) | `bg-[var(--bg-primary)]` |
| Border | 1px solid `var(--border-secondary)` (#CCD6DC) | `border border-[var(--border-secondary)]` |
| Border radius | 8px | `rounded-lg` |
| Box shadow | Large elevation | `shadow-2xl` |

---

## Title

| Property | Value | Tailwind |
|----------|-------|----------|
| Typography | 18px / 28px / weight 600 (title-xl) | `text-lg leading-7` + `style={{ fontWeight: 600 }}` |
| Color | `var(--text-primary)` (#353E5A) | `text-[var(--text-primary)]` |
| Margin bottom | 8px | `mb-2` |

---

## Description

| Property | Value | Tailwind |
|----------|-------|----------|
| Typography | 14px / 20px / weight 500 (caption-md) | `text-sm leading-5 font-medium` |
| Color | `var(--text-tertiary)` (#6B748E) | `text-[var(--text-tertiary)]` |
| Margin bottom | 24px | `mb-6` |

---

## Buttons

| Property | Value | Tailwind |
|----------|-------|----------|
| Layout | Flex row | `flex` |
| Gap | 12px | `gap-3` |

### Primary Button (Confirm — Danger)
| Property | Value | Tailwind |
|----------|-------|----------|
| Height | 32px | `h-8` |
| Padding | 8px vertical, 12px horizontal | `py-2 px-3` |
| Border radius | 6px | `rounded-md` |
| Background | `var(--bg-error-solid)` (#F02C00) | `bg-[var(--bg-error-solid)]` |
| Text color | `var(--text-white)` (#FFFFFF) | `text-[var(--text-white)]` |
| Typography | 14px / 20px / weight 600 | `text-sm leading-5` + `style={{ fontWeight: 600 }}` |
| Hover background | `var(--bg-error-solid_hover)` (#FF431B) | `hover:bg-[var(--bg-error-solid_hover)]` |
| Transition | background-color, box-shadow 150ms ease-out | `transition-all duration-150 ease-out` |

### Secondary Button (Cancel — Outline Neutral)
| Property | Value | Tailwind |
|----------|-------|----------|
| Height | 32px | `h-8` |
| Padding | 8px vertical, 12px horizontal | `py-2 px-3` |
| Border radius | 6px | `rounded-md` |
| Background | `var(--bg-secondary)` (#F1F4F7) | `bg-[var(--bg-secondary)]` |
| Border | 1px solid `var(--fg-disabled)` (#8EA1AF) | `border border-[var(--fg-disabled)]` |
| Text color | `var(--text-primary)` (#353E5A) | `text-[var(--text-primary)]` |
| Typography | 14px / 20px / weight 600 | `text-sm leading-5` + `style={{ fontWeight: 600 }}` |
| Hover background | `var(--bg-secondary_hover)` (#E8ECF1) | `hover:bg-[var(--bg-secondary_hover)]` |
| Transition | background-color, box-shadow 150ms ease-out | `transition-all duration-150 ease-out` |

---

## Arrow Indicator

| Property | Value |
|----------|-------|
| Size | 10px × 10px |
| Shape | Diamond (rotated square) |
| Background | White |
| Border | 1px solid `var(--border-secondary)` (#CCD6DC) (top and left only) |
| Position offset | -6px from popup edge |

---

## Animation

| Property | Value |
|----------|-------|
| Type | Slide in from direction |
| Duration | 200ms |
| Easing | ease-out |
| Distance | 12px |
| Opacity | 0 → 1 |

---

## Implementation Pattern for Lovable

```jsx
{/* === Confirmation Popup === */}
<div className="relative inline-block">
  {/* Trigger button */}
  <button onClick={() => setOpen(!open)} className="...trigger-styles...">
    Delete
  </button>

  {/* Popup */}
  {open && (
    <div className="
      absolute bottom-full left-1/2 -translate-x-1/2 mb-2.5
      w-[300px] p-4
      bg-[var(--bg-primary)] rounded-lg
      border border-[var(--border-secondary)]
      shadow-2xl
      z-50
      animate-in slide-in-from-bottom-3
    ">
      {/* Title */}
      <h3
        className="text-lg leading-7 text-[var(--text-primary)] mb-2"
        style={{ fontWeight: 600 }}
      >
        Delete this item?
      </h3>

      {/* Description */}
      <p className="text-sm leading-5 font-medium text-[var(--text-tertiary)] mb-6">
        This action cannot be undone. Are you sure you want to continue?
      </p>

      {/* Buttons */}
      <div className="flex gap-3">
        {/* Secondary — Cancel */}
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
          No
        </button>

        {/* Primary — Confirm (Danger) */}
        <button
          className="
            h-8 py-2 px-3 rounded-md
            bg-[var(--bg-error-solid)] text-[var(--text-white)]
            border-none
            text-sm leading-5
            hover:bg-[var(--bg-error-solid_hover)]
            transition-all duration-150 ease-out
          "
          style={{ fontWeight: 600 }}
          onClick={handleConfirm}
        >
          Yes
        </button>
      </div>
    </div>
  )}
</div>
```

### Key Implementation Rules

1. **This is a POPUP, not a modal** — it anchors to a trigger element, not centered on screen
2. **Width is 300px** with 16px padding and 8px border-radius
3. **Title uses 18px/28px/600**, description uses **14px/20px/500** in `var(--text-tertiary)` (#6B748E)
4. **Description has 24px margin-bottom** before the buttons (`mb-6`)
5. **Primary button is DANGER red** `var(--bg-error-solid)` (#F02C00) — this is a confirmation popup, default action is destructive
6. **Secondary button is outline neutral** — `var(--bg-secondary)` (#F1F4F7) background with `var(--fg-disabled)` (#8EA1AF) border
7. **Both buttons are small** (32px height, 6px border-radius)
8. **Buttons have 12px gap** between them (`gap-3`)
9. **Default labels are "Yes" and "No"** — customizable
10. **Animation slides in** from the popup position direction (200ms, ease-out)
11. **Arrow indicator** points from the popup toward the trigger element
