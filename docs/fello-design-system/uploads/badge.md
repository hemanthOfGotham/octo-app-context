# Badge Component — Strict Specification

When building any badge, tag, or status indicator, follow these specifications exactly. Do NOT use default Tailwind badge styles, shadcn badges, or any other badge system.

> **TOKEN RULE:** Every colour in this file is expressed as a CSS variable token.
> In **tables / prose** write `var(--token)` (#HEX).
> In **Tailwind classes** write `bg-[var(--bg-token)]`, `text-[var(--text-token)]`, `border-[var(--border-token)]`, etc.
> In **inline styles** write `'var(--token)'`.
> Do NOT use raw hex values outside the parenthetical annotation.
> AI gradient colours are kept as-is or use `var(--ai-XX)` tokens.

---

## Badge Anatomy

A badge is an inline-flex element structured as:

```
[leading element] [text] [trailing element]
```

- Layout: `inline-flex items-center justify-center`
- Gap: 4px (`gap-1`) — except small size which uses 2px (`gap-0.5`)
- Height: `fit-content`
- Leading element: none, dot indicator, or icon
- Trailing element: none, icon, or close button
- In **icon-only** mode: only the leading icon is shown, no text, no trailing

---

## Sizes

There are 3 badge sizes. Use these EXACT dimensions.

### Small
- Typography: 10px / 16px / font-weight 500 (`text-[10px] leading-[16px] font-medium`)
- Padding: 0px vertical, 6px horizontal (`px-1.5 py-0`)
- Min-width: 16px
- Gap: 2px (`gap-0.5`)
- Icon size: 14px / 20px / font-weight 500
- Icon-only dimensions: 16px x 16px, no padding
- Dot size: 6px x 6px

### Medium (Default)
- Typography: 12px / 18px / font-weight 500 (`text-xs leading-[18px] font-medium`)
- Padding: 1px vertical, 6px horizontal (`px-1.5 py-px`)
- Min-width: 20px
- Gap: 4px (`gap-1`)
- Icon size: 16px / 24px / font-weight 500
- Icon-only dimensions: 20px x 20px, no padding
- Dot size: 8px x 8px

### Large
- Typography: 14px / 20px / font-weight 500 (`text-sm leading-5 font-medium`)
- Padding: 2px vertical, 6px horizontal (`px-1.5 py-0.5`)
- Min-width: 24px
- Gap: 4px (`gap-1`)
- Icon size: 18px / 28px / font-weight 600
- Icon-only dimensions: 24px x 24px, no padding
- Dot size: 8px x 8px

---

## Shapes

| Shape | Border Radius | Tailwind | Use For |
|-------|--------------|----------|---------|
| **block** | 4px | `rounded` | Default — standard rectangular badges |
| **rounded** | 9999px | `rounded-full` | Pill-shaped badges, counts, tags |

---

## Fill Types

Each badge type has two fill variants:

| Fill | Description | Text Color |
|------|-------------|------------|
| **dark** | Solid colored background | White text |
| **light** | Soft/tinted background | Colored text matching the type |

---

## Types x Fill — Complete Color Spec

### Primary

| Fill | Background | Text | Dot Color |
|------|-----------|------|-----------|
| Dark | `var(--bg-brand-solid)` (#FF725C) | `var(--text-white)` (#FFFFFF) | — |
| Light | `var(--bg-brand-primary)` (#FDF1EF) | `var(--text-brand-primary)` (#FF725C) | `var(--fg-brand-primary)` (#FF725C) |

### Secondary

| Fill | Background | Text | Dot Color |
|------|-----------|------|-----------|
| Dark | `var(--bg-brand-secondary-solid)` (#098486) | `var(--text-white)` (#FFFFFF) | — |
| Light | `var(--bg-brand-secondary)` (#E7FDFD) | `var(--text-brand-secondary)` (#098486) | `var(--fg-brand-secondary)` (#098486) |

### Grey

| Fill | Background | Text | Dot Color |
|------|-----------|------|-----------|
| Dark | `var(--bg-primary-solid)` (#353E5A) | `var(--text-white)` (#FFFFFF) | — |
| Light | `var(--bg-tertiary)` (#E8ECF1) | `var(--text-primary)` (#353E5A) | `var(--fg-disabled)` (#8EA1AF) |

### Warning

| Fill | Background | Text | Dot Color |
|------|-----------|------|-----------|
| Dark | `var(--bg-warning-solid)` (#F08400) | `var(--text-white)` (#FFFFFF) | — |
| Light | `var(--bg-warning-primary)` (#FFFAE6) | `var(--text-warning-primary)` (#F08400) | `var(--fg-warning-secondary)` (#FF991F) |

### Danger

| Fill | Background | Text | Dot Color |
|------|-----------|------|-----------|
| Dark | `var(--bg-error-solid)` (#F02C00) | `var(--text-white)` (#FFFFFF) | — |
| Light | `var(--bg-error-primary)` (#FFEBE6) | `var(--text-error-primary)` (#F02C00) | `var(--fg-error-secondary)` (#FF431B) |

### Success

| Fill | Background | Text | Dot Color |
|------|-----------|------|-----------|
| Dark | `var(--bg-success-solid)` (#31A172) | `var(--text-white)` (#FFFFFF) | — |
| Light | `var(--bg-success-primary)` (#E3FCEF) | `var(--text-success-primary)` (#31A172) | `var(--fg-success-secondary)` (#36B37E) |

### Info

| Fill | Background | Text | Dot Color |
|------|-----------|------|-----------|
| Dark | `var(--bg-info-solid)` (#3D93F5) | `var(--text-white)` (#FFFFFF) | — |
| Light | `var(--bg-info-primary)` (#F0F7FF) | `var(--text-info-primary)` (#3D93F5) | `var(--fg-info-secondary)` (#6EAEF7) |

### Magic

| Fill | Background | Text | Dot Color |
|------|-----------|------|-----------|
| Dark | `var(--bg-magic-solid)` (#8D6AE7) | `var(--text-white)` (#FFFFFF) | — |
| Light | `var(--bg-magic-primary)` (#F4F3FF) | `var(--text-magic-primary)` (#8D6AE7) | `var(--fg-magic-secondary)` (#AB91ED) |

### AI

| Fill | Background | Text | Dot Color |
|------|-----------|------|-----------|
| Dark | `var(--ai-gradient)` (linear-gradient(90deg, #14BACB, #3898F0, #8D6AE6, #EC67A0, #F59593)) | `var(--text-white)` (#FFFFFF) | — |
| Light | `var(--ai-gradient-light)` (linear-gradient(98.42deg, #F1F4F7 3.79%, #F4F3FF 51.25%, #FDF1EF 98.71%)) | AI gradient text (transparent text with gradient background-clip) | `var(--ai-00)` (#14BACB) |

### Custom
- Background: provided via `customBackgroundColor` prop
- Text: provided via `customForegroundColor` prop
- Use when none of the built-in types match

---

## Leading Element Types

### None (Default)
No leading element displayed.

### Dot
A small circular dot indicator before the text.

| Badge Size | Dot Size |
|-----------|----------|
| Small | 6px x 6px |
| Medium | 8px x 8px |
| Large | 8px x 8px |

- The dot is a circle (`rounded-full`) with `background-color: currentColor`
- In **dark** fill: dot is white (inherits from parent)
- In **light** fill: dot uses the type-specific dot color from the table above
- Dot has `flex-shrink: 0` to prevent shrinking

### Icon
A small icon before the text. Icon inherits the text color.

---

## Trailing Element Types

### None (Default)
No trailing element displayed.

### Icon
A small icon after the text. Icon inherits the text color.

### Close Button
A close (x) icon that acts as a button:
- `cursor: pointer`
- Keyboard accessible (`tabindex="0"`, responds to Enter key)
- Emits a close event on click

---

## Icon-Only Mode

When `iconOnly` is true:
- Only the leading icon is displayed
- No text, no trailing element
- Padding: 0 (no padding)
- Width = Height (square) — matches size dimensions
- Icon is centered

| Size | Dimensions |
|------|-----------|
| Small | 16px x 16px |
| Medium | 20px x 20px |
| Large | 24px x 24px |

---

## Status Dot Component

The status dot is also used independently outside badges. Here is its complete spec:

### Sizes

| Size | Dimensions |
|------|-----------|
| XS | 6px x 6px |
| SM | 8px x 8px |
| MD | 10px x 10px |
| LG | 12px x 12px |
| XL | 14px x 14px |

### Variants

| Variant | Color |
|---------|-------|
| primary | `var(--fg-brand-primary)` (#FF725C) |
| secondary | `var(--fg-brand-secondary)` (#098486) |
| success | `var(--fg-success-secondary)` (#36B37E) |
| info | `var(--fg-info-secondary)` (#6EAEF7) |
| warning | `var(--fg-warning-secondary)` (#FF991F) |
| error | `var(--fg-error-secondary)` (#FF431B) |
| disabled | `var(--fg-disabled)` (#8EA1AF) |
| white | `var(--fg-white)` (#FFFFFF) |
| inherit | inherits from parent |

### Status Dot Structure
- `display: inline-flex`, `items-center`, `justify-center`
- `border-radius: 9999px` (fully circular)
- Background rendered as `currentColor` (uses the color property)

---

## Usage Guidelines

### When to Use Which Type
| Type | Use For |
|------|---------|
| **primary** | Brand-related status, primary category labels |
| **secondary** | Secondary category, alternative brand labels |
| **grey** | Neutral/default status, generic labels, counts |
| **success** | Completed, active, approved, online status |
| **warning** | Pending, attention needed, expiring soon |
| **danger** | Error, failed, rejected, critical |
| **info** | Informational, new, updated, notifications |
| **magic** | Premium, pro, special features |
| **ai** | AI-generated, AI-powered, smart features |

### When to Use Which Fill
| Fill | Use For |
|------|---------|
| **dark** | High emphasis — important status, counts, key labels |
| **light** | Low emphasis — secondary status, inline tags, subtle indicators |

### When to Use Which Shape
| Shape | Use For |
|-------|---------|
| **block** | Default badges, status labels, categories |
| **rounded** | Counts, notification numbers, pills, tags |

---

## Implementation Pattern for Lovable

```jsx
{/* Dark Primary Badge — Medium, Block shape */}
<span className="
  inline-flex items-center justify-center gap-1
  px-1.5 py-px
  min-w-[20px]
  rounded
  bg-[var(--bg-brand-solid)] text-[var(--text-white)]
  text-xs leading-[18px] font-medium
">
  Active
</span>

{/* Light Success Badge — Medium, Rounded with dot */}
<span className="
  inline-flex items-center justify-center gap-1
  px-1.5 py-px
  min-w-[20px]
  rounded-full
  bg-[var(--bg-success-primary)] text-[var(--text-success-primary)]
  text-xs leading-[18px] font-medium
">
  <span className="w-2 h-2 rounded-full bg-[var(--fg-success-primary)] shrink-0" />
  Completed
</span>

{/* Dark Grey Badge — Small, Block */}
<span className="
  inline-flex items-center justify-center gap-0.5
  px-1.5 py-0
  min-w-[16px]
  rounded
  bg-[var(--bg-primary-solid)] text-[var(--text-white)]
  text-[10px] leading-[16px] font-medium
">
  12
</span>

{/* Light Danger Badge — Large, Rounded with icon */}
<span className="
  inline-flex items-center justify-center gap-1
  px-1.5 py-0.5
  min-w-[24px]
  rounded-full
  bg-[var(--bg-error-primary)] text-[var(--text-error-primary)]
  text-sm leading-5 font-medium
">
  <AlertIcon className="w-[18px] h-[18px]" />
  Failed
</span>

{/* AI Badge — Dark fill */}
<span className="
  inline-flex items-center justify-center gap-1
  px-1.5 py-px
  min-w-[20px]
  rounded-full
  text-[var(--text-white)]
  text-xs leading-[18px] font-medium
" style={{ background: 'var(--ai-gradient)' }}>
  AI Generated
</span>

{/* Icon-only badge — Medium, Primary Dark */}
<span className="
  inline-flex items-center justify-center
  w-5 h-5
  rounded
  bg-[var(--bg-brand-solid)] text-[var(--text-white)]
">
  <StarIcon className="w-4 h-4" />
</span>

{/* Status dot — standalone */}
<span className="inline-flex w-2 h-2 rounded-full bg-[var(--fg-success-primary)]" />
```

### Key Implementation Rules
1. **ALWAYS use `inline-flex items-center justify-center`** for badge layout
2. **Padding is always 6px horizontal** (`px-1.5`) — vertical varies by size
3. **Font weight is always 500** (`font-medium`) for badge text
4. **Dark fill = white text**, **Light fill = colored text** matching the type
5. **Dot indicator** is a simple circle div with `rounded-full` and `bg-currentColor` or explicit color
6. **Dot has `shrink-0`** to prevent it from shrinking
7. **Icon-only badges have NO padding** and are square (width = height)
8. **Block shape = `rounded`** (4px), **Rounded shape = `rounded-full`** (9999px)
9. **min-width** ensures badges don't collapse: 16px (small), 20px (medium), 24px (large)
10. **AI badges** need inline `style` for gradient backgrounds since Tailwind doesn't support multi-stop gradients natively
