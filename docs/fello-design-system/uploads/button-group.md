# Button Group Component — Strict Specification

When building any grouped set of toggle buttons or segmented controls, follow these specifications exactly. Do NOT use default Tailwind button groups, shadcn toggle groups, or any other segmented control system.

> **TOKEN RULE:** Every colour in this file is expressed as a CSS custom-property token — e.g. `var(--bg-primary)` — with the original hex in parentheses for reference only. When implementing, always use the `var(--*)` form (in Tailwind: `bg-[var(--bg-primary)]`, `text-[var(--text-primary)]`, etc.). Never hard-code a raw hex value.

---

## Button Group Anatomy

A horizontal row of adjacent buttons sharing a single outer border. Buttons are visually connected — no gap between them — separated only by 1px internal dividers.

```
Neutral Variant (md):
+-------------------------------------------------------+
| [icon] Label  |  Label  |  [icon]                     |
+-------------------------------------------------------+
  ^ leading icon    ^ text only   ^ icon-only button

Primary Variant (md):
+-------------------------------------------------------+
| [icon] Label  |  Label  |  [icon]                     |
+-------------------------------------------------------+
  coral filled bg throughout
```

- Buttons sit flush against each other — no spacing/gap between them
- 1px vertical divider between each button (same color as outer border)
- Last button has no right divider
- Each button can have: text only, leading icon + text, text + trailing icon, or icon only
- Any button can be independently disabled

---

## Variants

Two visual variants:

| Variant | Use Case |
|---------|----------|
| **Neutral** | Default. Gray background, dark text. For toggling views, modes, or filters. |
| **Primary** | Emphasis. Coral/brand background, white text. For primary segmented actions. |

---

## Sizes

| Size | Container Height | Button Padding | Icon-Only Padding | Typography | Icon Size |
|------|-----------------|----------------|-------------------|------------|-----------|
| **md** (default) | 40px | 10px 16px (`py-2.5 px-4`) | 10px (`p-2.5`) | title-md: 14px / 20px / 600 | 20px (`w-5 h-5`) |
| **sm** | 32px | 8px 12px (`py-2 px-3`) | 8px (`p-2`) | title-sm: 12px / 18px / 600 | 16px (`w-4 h-4`) |

---

## Container (Outer Wrapper)

| Property | Value | Tailwind / CSS |
|----------|-------|----------------|
| Display | Inline-flex, centered | `inline-flex items-center` |
| Width | Fit content | `w-fit` |
| Border radius | 8px | `rounded-lg` |
| Border | 1px solid (color varies by variant) | `border` |
| Overflow | Hidden | `overflow-hidden` |

### Container Border Color by Variant

| Variant | Border Color |
|---------|-------------|
| Neutral | `var(--border-secondary-solid)` (#8EA1AF) |
| Primary | `var(--border-brand-solid)` (#FF725C) |

---

## Individual Button Styling

Each button inside the group:

| Property | Value | Tailwind / CSS |
|----------|-------|----------------|
| Display | Inline-flex, centered | `inline-flex items-center justify-center` |
| Gap | 8px (between icon and text) | `gap-2` |
| Cursor | Pointer | `cursor-pointer` |
| Outline | None | `outline-none` |
| Transition | background-color 150ms | `transition-colors duration-150` |
| Right border | 1px solid (divider between buttons) | `border-r` |
| Last button right border | None | `last:border-r-0` |

### Typography by Size

| Size | Font Size | Line Height | Weight | Tailwind |
|------|-----------|-------------|--------|----------|
| md | 14px | 20px | 600 | `text-sm leading-5 font-semibold` |
| sm | 12px | 18px | 600 | `text-xs leading-[18px]` + `style={{ fontWeight: 600 }}` |

---

## Neutral Variant — Button States

| State | Background | Text Color | Border (divider) |
|-------|-----------|------------|------------------|
| Default | `var(--bg-secondary)` (#F1F4F7) | `var(--text-primary)` (#353E5A) | `var(--border-secondary-solid)` (#8EA1AF) |
| Hover | `var(--bg-secondary_hover)` (#E8ECF1) | `var(--text-primary)` (#353E5A) | same |
| Disabled | `var(--bg-disabled)` (#E8ECF1) | `var(--text-disabled)` (#8EA1AF) | same |

Tailwind:
```
bg-[var(--bg-secondary)]
text-[var(--text-primary)]
border-r border-r-[var(--border-secondary-solid)]
hover:bg-[var(--bg-secondary_hover)]
disabled:bg-[var(--bg-disabled)] disabled:text-[var(--text-disabled)] disabled:cursor-not-allowed
```

---

## Primary Variant — Button States

| State | Background | Text Color | Border (divider) |
|-------|-----------|------------|------------------|
| Default | `var(--bg-brand-solid)` (#FF725C) | `var(--text-primary_on-brand)` (#FFFFFF) | `var(--border-brand)` (#FFAA9D) |
| Hover | `var(--bg-brand-solid_hover)` (#FF8E7D) | `var(--text-primary_on-brand)` (#FFFFFF) | same |
| Disabled | `var(--bg-brand-solid-disabled)` (#FF725C5c) | `var(--text-primary_on-brand)` (#FFFFFF) | same |

Tailwind:
```
bg-[var(--bg-brand-solid)]
text-[var(--text-primary_on-brand)]
border-r border-r-[var(--border-brand)]
hover:bg-[var(--bg-brand-solid_hover)]
disabled:bg-[var(--bg-brand-solid-disabled)] disabled:cursor-not-allowed
```

---

## Icon Handling

- **Leading icon:** Placed before the button label text
- **Trailing icon:** Placed after the button label text
- **Icon-only mode:** Only the leading icon is shown, label text and trailing icon are hidden. Uses square padding (equal on all sides).
- Icon color: Inherits from the button's text color (`currentColor`)

---

## Keyboard Navigation

The button group supports arrow-key navigation between buttons:
- **ArrowRight / ArrowDown** — move focus to next button
- **ArrowLeft / ArrowUp** — move focus to previous button
- The container has `role="group"` for accessibility

---

## Implementation Pattern for Lovable

```jsx
{/* === Button Group — Neutral, Size md === */}
<div
  className="inline-flex items-center w-fit overflow-hidden"
  role="group"
  style={{
    border: '1px solid var(--border-secondary-solid)',
    borderRadius: '8px',
    height: '40px',
  }}
>
  {/* Button with leading icon + text */}
  <button
    className="
      inline-flex items-center justify-center gap-2
      text-sm leading-5 font-semibold
      bg-[var(--bg-secondary)] text-[var(--text-primary)]
      hover:bg-[var(--bg-secondary_hover)]
      transition-colors duration-150
      cursor-pointer outline-none
    "
    style={{
      padding: '10px 16px',
      borderRight: '1px solid var(--border-secondary-solid)',
    }}
  >
    <Edit className="w-5 h-5" />
    <span>With Icon</span>
  </button>

  {/* Text-only button (disabled) */}
  <button
    className="
      inline-flex items-center justify-center gap-2
      text-sm leading-5 font-semibold
      bg-[var(--bg-disabled)] text-[var(--text-disabled)]
      cursor-not-allowed outline-none
    "
    style={{
      padding: '10px 16px',
      borderRight: '1px solid var(--border-secondary-solid)',
    }}
    disabled
  >
    <span>Text</span>
  </button>

  {/* Icon-only button (last — no right border) */}
  <button
    className="
      inline-flex items-center justify-center
      bg-[var(--bg-secondary)] text-[var(--text-primary)]
      hover:bg-[var(--bg-secondary_hover)]
      transition-colors duration-150
      cursor-pointer outline-none
    "
    style={{ padding: '10px' }}
  >
    <Copy className="w-5 h-5" />
  </button>
</div>


{/* === Button Group — Primary, Size md === */}
<div
  className="inline-flex items-center w-fit overflow-hidden"
  role="group"
  style={{
    border: '1px solid var(--border-brand-solid)',
    borderRadius: '8px',
    height: '40px',
  }}
>
  <button
    className="
      inline-flex items-center justify-center gap-2
      text-sm leading-5 font-semibold
      bg-[var(--bg-brand-solid)] text-[var(--text-primary_on-brand)]
      hover:bg-[var(--bg-brand-solid_hover)]
      transition-colors duration-150
      cursor-pointer outline-none
    "
    style={{
      padding: '10px 16px',
      borderRight: '1px solid var(--border-brand)',
    }}
  >
    <Edit className="w-5 h-5" />
    <span>With Icon</span>
  </button>

  <button
    className="
      inline-flex items-center justify-center gap-2
      text-sm leading-5 font-semibold
      bg-[var(--bg-brand-solid)] text-[var(--text-primary_on-brand)]
      hover:bg-[var(--bg-brand-solid_hover)]
      transition-colors duration-150
      cursor-pointer outline-none
    "
    style={{
      padding: '10px 16px',
      borderRight: '1px solid var(--border-brand)',
    }}
  >
    <span>Text</span>
  </button>

  <button
    className="
      inline-flex items-center justify-center
      bg-[var(--bg-brand-solid)] text-[var(--text-primary_on-brand)]
      hover:bg-[var(--bg-brand-solid_hover)]
      transition-colors duration-150
      cursor-pointer outline-none
    "
    style={{ padding: '10px' }}
  >
    <Copy className="w-5 h-5" />
  </button>
</div>


{/* === Button Group — Neutral, Size sm === */}
<div
  className="inline-flex items-center w-fit overflow-hidden"
  role="group"
  style={{
    border: '1px solid var(--border-secondary-solid)',
    borderRadius: '8px',
    height: '32px',
  }}
>
  <button
    className="
      inline-flex items-center justify-center gap-2
      text-xs leading-[18px]
      bg-[var(--bg-secondary)] text-[var(--text-primary)]
      hover:bg-[var(--bg-secondary_hover)]
      transition-colors duration-150
      cursor-pointer outline-none
    "
    style={{
      fontWeight: 600,
      padding: '8px 12px',
      borderRight: '1px solid var(--border-secondary-solid)',
    }}
  >
    <Edit className="w-4 h-4" />
    <span>With Icon</span>
  </button>

  <button
    className="
      inline-flex items-center justify-center gap-2
      text-xs leading-[18px]
      bg-[var(--bg-secondary)] text-[var(--text-primary)]
      hover:bg-[var(--bg-secondary_hover)]
      transition-colors duration-150
      cursor-pointer outline-none
    "
    style={{
      fontWeight: 600,
      padding: '8px 12px',
      borderRight: '1px solid var(--border-secondary-solid)',
    }}
  >
    <span>Text</span>
  </button>

  <button
    className="
      inline-flex items-center justify-center
      bg-[var(--bg-secondary)] text-[var(--text-primary)]
      hover:bg-[var(--bg-secondary_hover)]
      transition-colors duration-150
      cursor-pointer outline-none
    "
    style={{ fontWeight: 600, padding: '8px' }}
  >
    <Copy className="w-4 h-4" />
  </button>
</div>
```

### Key Implementation Rules

1. **Container uses `overflow: hidden`** — this clips button corners to match the container's 8px border-radius. Do NOT put `border-radius` on individual buttons.
2. **Border radius is 8px** — Use `style={{ borderRadius: '8px' }}` to prevent Tailwind from overriding.
3. **Buttons must fill container height** — use `height: 100%` on each button so the background covers the full container height with no gaps.
4. **Dividers are 1px solid** matching the container border color — use `borderRight` on every button except the last one.
5. **No gap between buttons** — buttons are flush. Do NOT add `gap-*` on the container.
6. **Width is `fit-content`** — the group shrinks to fit its buttons, NOT full width.
7. **Typography uses `font-semibold` (600)** — for sm size, use inline `style={{ fontWeight: 600 }}` since Tailwind `font-semibold` may not apply on `<button>` elements.
8. **Icon-only buttons use square padding** — equal padding on all sides (10px for md, 8px for sm).
9. **Disabled buttons** don't change their divider color — only background and text color change.
10. **Primary variant text is white** `var(--text-primary_on-brand)` — NOT dark text.
11. **Primary variant divider uses `var(--border-brand)`** (#FFAA9D, lighter coral) — NOT the same as the outer border `var(--border-brand-solid)` (#FF725C).
