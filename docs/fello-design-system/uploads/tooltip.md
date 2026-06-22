# Tooltip Component — Strict Specification

When building any tooltip, hover popup, or informational overlay, follow these specifications exactly. Do NOT use default Tailwind tooltip styles, shadcn tooltips, or any other tooltip system.

> **TOKEN RULE:** Every color in this spec is expressed as a CSS variable token. In Tailwind classes use `bg-[var(--token)]` / `text-[var(--token)]`. In inline styles use `'var(--token)'`. In prose and tables, show `var(--token)` (#hex) for reference.

---

## Tooltip Anatomy

A tooltip is a small floating overlay attached to a trigger element:

```
         ┌──────────────────────┐
         │  [Optional Title]    │
         │  [Description text]  │
         └──────────┬───────────┘
                    ▼
            [Trigger Element]
```

- Contains an optional title and a required description
- Has a small arrow/pointer that points toward the trigger element
- Appears on hover (with delay) or keyboard focus

---

## Fill Modes

There are 2 visual styles:

### Dark (Default)
| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `var(--tooltip-bg-primary-dark)` (#052033) | `bg-[var(--tooltip-bg-primary-dark)]` |
| Text color | `var(--text-white)` (#FFFFFF) | `text-[var(--text-white)]` |
| Arrow color | Same as background `var(--tooltip-bg-primary-dark)` (#052033) | — |

### Light
| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `var(--bg-primary)` (#FFFFFF) | `bg-[var(--bg-primary)]` |
| Text color | `var(--text-primary)` (#353E5A) | `text-[var(--text-primary)]` |
| Arrow color | Same as background `var(--bg-primary)` (#FFFFFF) | — |

---

## Sizing

| Property | Default | Large (> 250 chars) |
|----------|---------|---------------------|
| Max width | 200px | 350px |

- If the description text exceeds 250 characters, the tooltip automatically widens to 350px max
- Width is `fit-content` up to the max-width

---

## Layout Spec

| Property | Value | Tailwind |
|----------|-------|----------|
| Display | `block` | `block` |
| Position | `relative` (for arrow positioning) | `relative` |
| Padding | 8px vertical, 12px horizontal | `py-2 px-3` |
| Border radius | 8px | `rounded-lg` |
| Shadow | `var(--shadow-lg)` | `shadow-lg` |
| Word break | `break-word` | `break-words` |

---

## Typography

### Title (optional)
| Property | Value | Tailwind |
|----------|-------|----------|
| Size | 12px / 18px / weight 600 | `text-xs leading-[18px]` with `style={{ fontWeight: 600 }}` |
| Margin bottom | 4px (`mb-1`) | `mb-1` |
| Display | Block (own line) | — |

### Description
| Property | Value | Tailwind |
|----------|-------|----------|
| Size | 12px / 18px / weight 400 | `text-xs leading-[18px] font-normal` |
| Display | Block | — |

---

## Arrow / Pointer

The arrow is a small rotated square that creates a triangular pointer toward the trigger:

| Property | Value |
|----------|-------|
| Size | 12px × 12px |
| Shape | Rotated 45° square with 1px border-radius |
| Color | Matches tooltip background (dark or light) |
| Offset from edge | -4px (overlaps tooltip edge by 4px) |

### Arrow Position by Tooltip Placement

| Tooltip Position | Arrow Location | Arrow Rotation |
|-----------------|---------------|----------------|
| **above** (tooltip is above trigger) | Bottom center of tooltip | 225° |
| **below** (tooltip is below trigger) | Top center of tooltip | 45° |
| **left** (tooltip is left of trigger) | Right center of tooltip | 135° |
| **right** (tooltip is right of trigger) | Left center of tooltip | 315° |

- Arrow position is dynamically adjusted to point at the trigger element center
- Uses CSS custom properties `--arrow-left-offset` and `--arrow-top-offset` for fine-tuning

---

## Positioning

| Property | Value |
|----------|-------|
| Offset from trigger | 10px gap between trigger and tooltip |
| Default position | Below the trigger, centered horizontally |
| Fallback | Repositions if not enough space (flips to opposite side) |
| Viewport margin | 8px minimum from viewport edges |

### Position Options
| Position | Tooltip placement |
|----------|-------------------|
| **above** | Centered above the trigger |
| **below** (default) | Centered below the trigger |
| **left** | Centered to the left of the trigger |
| **right** | Centered to the right of the trigger |

---

## Trigger Behavior

| Event | Action | Delay |
|-------|--------|-------|
| Mouse enter trigger | Show tooltip | 200ms delay |
| Mouse leave trigger | Start hide timer | 100ms delay |
| Mouse enter tooltip | Cancel hide timer (keep open) | — |
| Mouse leave tooltip | Hide tooltip | Immediate |
| Keyboard focus (focus-visible) | Show tooltip | Immediate |
| Keyboard blur | Hide tooltip | Immediate |
| Page scroll | Hide tooltip | Immediate |

- Tooltip stays open when user moves mouse from trigger to tooltip (100ms grace period)
- Only keyboard focus-visible triggers the tooltip (not mouse click focus)
- Trigger element gets `cursor: pointer` when tooltip is enabled

---

## Entrance Animations

| Position | Animation |
|----------|-----------|
| Above | Slide up (small distance) |
| Below | Slide down (small distance) |
| Left | Slide left (small distance) |
| Right | Slide right (small distance) |

- Animation speed: fast (~100-150ms)
- No exit animation — tooltip disappears instantly

---

## Accessibility

| Property | Value |
|----------|-------|
| Role | `tooltip` |
| Connection | Trigger element uses `aria-describedby` pointing to tooltip `id` |
| Keyboard | Shows on focus-visible, hides on blur |

---

## Implementation Pattern for Lovable

```jsx
{/* === Dark Tooltip — Below position (default) === */}
<div className="relative inline-block group">
  {/* Trigger element */}
  <span className="cursor-pointer">
    Hover me
  </span>

  {/* Tooltip — shown on hover */}
  <div
    role="tooltip"
    className="
      absolute top-full left-1/2 -translate-x-1/2 mt-2.5
      block relative
      py-2 px-3
      rounded-lg
      bg-[var(--tooltip-bg-primary-dark)] text-white
      text-xs leading-[18px] font-normal
      max-w-[200px] w-fit
      shadow-lg
      break-words
      z-50

      invisible group-hover:visible
      opacity-0 group-hover:opacity-100
      translate-y-1 group-hover:translate-y-0
      transition-all duration-150 ease-out
    "
  >
    {/* Arrow */}
    <div className="
      absolute -top-1 left-1/2 -translate-x-1/2
      w-3 h-3
      bg-[var(--tooltip-bg-primary-dark)]
      rotate-45
      rounded-[1px]
    " />
    {/* Content */}
    <p>This is the tooltip description text.</p>
  </div>
</div>


{/* === Dark Tooltip with Title — Below position === */}
<div className="relative inline-block group">
  <span className="cursor-pointer">
    Hover for details
  </span>

  <div
    role="tooltip"
    className="
      absolute top-full left-1/2 -translate-x-1/2 mt-2.5
      block
      py-2 px-3
      rounded-lg
      bg-[var(--tooltip-bg-primary-dark)] text-white
      text-xs leading-[18px] font-normal
      max-w-[200px] w-fit
      shadow-lg
      break-words
      z-50

      invisible group-hover:visible
      opacity-0 group-hover:opacity-100
      translate-y-1 group-hover:translate-y-0
      transition-all duration-150 ease-out
    "
  >
    <div className="
      absolute -top-1 left-1/2 -translate-x-1/2
      w-3 h-3
      bg-[var(--tooltip-bg-primary-dark)]
      rotate-45
      rounded-[1px]
    " />
    <p className="mb-1" style={{ fontWeight: 600 }}>Smart Content</p>
    <p>Smart Content is used for Emails and Postcards to update their value dynamically.</p>
  </div>
</div>


{/* === Light Tooltip — Right position === */}
<div className="relative inline-block group">
  <span className="cursor-pointer">
    ℹ
  </span>

  <div
    role="tooltip"
    className="
      absolute left-full top-1/2 -translate-y-1/2 ml-2.5
      block
      py-2 px-3
      rounded-lg
      bg-white text-[var(--text-primary)]
      text-xs leading-[18px] font-normal
      max-w-[200px] w-fit
      shadow-lg
      break-words
      z-50

      invisible group-hover:visible
      opacity-0 group-hover:opacity-100
      translate-x-1 group-hover:translate-x-0
      transition-all duration-150 ease-out
    "
  >
    <div className="
      absolute -left-1 top-1/2 -translate-y-1/2
      w-3 h-3
      bg-white
      rotate-45
      rounded-[1px]
    " />
    <p>This is a light tooltip positioned to the right.</p>
  </div>
</div>


{/* === Large Tooltip (long description > 250 chars) === */}
<div className="relative inline-block group">
  <span className="cursor-pointer">
    Learn more
  </span>

  <div
    role="tooltip"
    className="
      absolute top-full left-1/2 -translate-x-1/2 mt-2.5
      block
      py-2 px-3
      rounded-lg
      bg-[var(--tooltip-bg-primary-dark)] text-white
      text-xs leading-[18px] font-normal
      max-w-[350px] w-fit
      shadow-lg
      break-words
      z-50

      invisible group-hover:visible
      opacity-0 group-hover:opacity-100
      translate-y-1 group-hover:translate-y-0
      transition-all duration-150 ease-out
    "
  >
    <div className="
      absolute -top-1 left-1/2 -translate-x-1/2
      w-3 h-3
      bg-[var(--tooltip-bg-primary-dark)]
      rotate-45
      rounded-[1px]
    " />
    <p className="mb-1" style={{ fontWeight: 600 }}>Detailed Information</p>
    <p>This is a much longer description that exceeds 250 characters. When tooltips have long descriptions, the max-width automatically increases from 200px to 350px to provide a more comfortable reading experience without excessive line wrapping that would make the content hard to read.</p>
  </div>
</div>
```

### Key Implementation Rules

1. **Dark mode is the default** — `var(--tooltip-bg-primary-dark)` (#052033) background, white text
2. **Light mode uses white background** with `var(--text-primary)` (#353E5A) text
3. **Arrow matches the tooltip background token** — dark or white
4. **Arrow is a 12px rotated square** with 1px border-radius, offset -4px from tooltip edge
5. **Default max-width is 200px**, increases to 350px for descriptions longer than 250 characters
6. **Padding is 8px vertical, 12px horizontal** (`py-2 px-3`)
7. **Typography is `paragraph-sm`** — 12px / 18px / weight 400 for description
8. **Title uses weight 600** (via `style={{ fontWeight: 600 }}`), 12px size, with 4px bottom margin
9. **10px gap between trigger and tooltip** (use `mt-2.5`, `ml-2.5`, etc.)
10. **Shadow is always present** — `shadow-lg`
11. **Border radius is always 8px** (`rounded-lg`)
12. **Tooltip shows on hover with 200ms delay** and hides with 100ms delay — tooltip stays open if user moves mouse to it
13. **Keyboard focus-visible shows tooltip**, blur hides it
14. **Use `role="tooltip"`** and `aria-describedby` on trigger for accessibility
15. **Use `break-words`** for long text to prevent overflow
