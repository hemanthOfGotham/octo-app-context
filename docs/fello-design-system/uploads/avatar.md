# Avatar Component — Strict Specification

When building any avatar or user profile indicator, follow these specifications exactly. Do NOT use default Tailwind avatar styles, shadcn avatars, or any other avatar system.

> **TOKEN RULE:** Every colour in this spec is expressed as a CSS variable token. When writing Tailwind classes use the `bg-[var(--token)]` / `text-[var(--token)]` / `border-[var(--token)]` syntax. In inline styles use `'var(--token)'`. In prose and tables the original hex is kept only as a parenthetical annotation, e.g. `var(--bg-secondary)` (#F1F4F7).

---

## Avatar Anatomy

An avatar is an **inline-flex, circular** element that displays one of three content types:

| Type | Content | Example |
|------|---------|---------|
| **text** | 1–2 uppercase initials extracted from a name | "John Doe" → "JD" |
| **icon** | A centered icon | User icon, placeholder icon |
| **image** | A photo/image filling the circle | Profile photo URL |

---

## Sizes

There are 5 avatar sizes. Use these EXACT dimensions.

| Size | Dimensions | Typography | Icon Size |
|------|-----------|------------|-----------|
| **XS** | 32px × 32px | 10px / 16px / weight 500 | 20px |
| **SM** | 40px × 40px | 12px / 18px / weight 500 | 24px |
| **MD** | 48px × 48px (default) | 14px / 20px / weight 500 | 28px |
| **LG** | 56px × 56px | 16px / 24px / weight 500 | 32px |
| **XL** | 64px × 64px | 16px / 24px / weight 500 | 32px |

---

## Visual Spec

### Common Properties (all sizes, all types)

| Property | Value | Tailwind |
|----------|-------|----------|
| Display | inline-flex | `inline-flex` |
| Align | center horizontally + vertically | `items-center justify-center` |
| Shape | Fully circular | `rounded-full` |
| Overflow | Hidden (clips content to circle) | `overflow-hidden` |
| Text align | Center | `text-center` |
| Background | `var(--bg-secondary)` (#F1F4F7) | `bg-[var(--bg-secondary)]` |

### Text & Icon Types — Border

| Property | Value | Tailwind |
|----------|-------|----------|
| Border | 1px solid `var(--border-secondary)` (#CCD6DC) | `border border-[var(--border-secondary)]` |

### Image Type — No Border
Images have no border. The photo fills the circle edge-to-edge.

### Image Sizing

| Property | Value | Tailwind |
|----------|-------|----------|
| Width | 100% | `w-full` |
| Height | 100% | `h-full` |
| Object fit | cover | `object-cover` |

### Hover / Focus States

| State | Effect |
|-------|--------|
| Hover | `box-shadow: 0 1px 2px 0 rgba(34,42,63,0.05)` (`var(--shadow-xs)`) |
| Focus-visible | Same `var(--shadow-xs)` |

---

## Initials Logic (Text Type)

When displaying text content, extract initials using this logic:

1. Split the string by spaces
2. Take the first character of the **first word** → uppercase
3. If there's a second word, take the first character of the **second word** → uppercase
4. Display 1–2 characters maximum

| Input | Output |
|-------|--------|
| "John Doe" | "JD" |
| "Alice" | "A" |
| "Mary Ann Smith" | "MA" |
| "" (empty) | "" |

---

## Compact Avatar (Small Inline Variant)

For tight spaces (inside list items, inline mentions), use the compact avatar:

| Size | Dimensions | Typography | Icon Size |
|------|-----------|------------|-----------|
| **XS** | 12px × 12px | 10px / 16px / weight 400 | 8px |
| **SM** | 16px × 16px | 10px / 16px / weight 400 | 10px |
| **MD** | 20px × 20px | 10px / 16px / weight 400 | 12px |
| **LG** | 24px × 24px | 12px / 18px / weight 400 | 14px |
| **XL** | 28px × 28px | 12px / 18px / weight 600 | 16px |

**Key difference:** Compact avatar shows only the **first character** of the text (not full initials).

Same visual properties: circular, `var(--bg-secondary)` (#F1F4F7) background, 1px `var(--border-secondary)` (#CCD6DC) border for text/icon, no border for image.

---

## Implementation Pattern for Lovable

```jsx
{/* === Text Avatar — MD (default) === */}
<div className="
  inline-flex items-center justify-center
  w-12 h-12
  rounded-full overflow-hidden
  bg-[var(--bg-secondary)]
  border border-[var(--border-secondary)]
  text-sm leading-5 font-medium
  text-center
  hover:shadow-sm
  transition-shadow duration-150 ease-out
">
  JD
</div>


{/* === Image Avatar — LG === */}
<div className="
  inline-flex items-center justify-center
  w-14 h-14
  rounded-full overflow-hidden
  bg-[var(--bg-secondary)]
  hover:shadow-sm
  transition-shadow duration-150 ease-out
">
  <img
    src="/path/to/profile.jpg"
    alt="John Doe"
    className="w-full h-full object-cover"
  />
</div>


{/* === Icon Avatar — SM === */}
<div className="
  inline-flex items-center justify-center
  w-10 h-10
  rounded-full overflow-hidden
  bg-[var(--bg-secondary)]
  border border-[var(--border-secondary)]
  text-xs leading-[18px] font-medium
  hover:shadow-sm
  transition-shadow duration-150 ease-out
">
  <UserIcon className="w-6 h-6" />
</div>


{/* === All 5 sizes side by side (text type) === */}
<div className="flex items-center gap-3">
  {/* XS - 32px */}
  <div className="inline-flex items-center justify-center w-8 h-8 rounded-full overflow-hidden bg-[var(--bg-secondary)] border border-[var(--border-secondary)] text-[10px] leading-[16px] font-medium">
    JD
  </div>

  {/* SM - 40px */}
  <div className="inline-flex items-center justify-center w-10 h-10 rounded-full overflow-hidden bg-[var(--bg-secondary)] border border-[var(--border-secondary)] text-xs leading-[18px] font-medium">
    JD
  </div>

  {/* MD - 48px (default) */}
  <div className="inline-flex items-center justify-center w-12 h-12 rounded-full overflow-hidden bg-[var(--bg-secondary)] border border-[var(--border-secondary)] text-sm leading-5 font-medium">
    JD
  </div>

  {/* LG - 56px */}
  <div className="inline-flex items-center justify-center w-14 h-14 rounded-full overflow-hidden bg-[var(--bg-secondary)] border border-[var(--border-secondary)] text-base leading-6 font-medium">
    JD
  </div>

  {/* XL - 64px */}
  <div className="inline-flex items-center justify-center w-16 h-16 rounded-full overflow-hidden bg-[var(--bg-secondary)] border border-[var(--border-secondary)] text-base leading-6 font-medium">
    JD
  </div>
</div>


{/* === Compact Avatar — MD (20px, inline usage) === */}
<span className="
  inline-flex items-center justify-center
  w-5 h-5
  rounded-full overflow-hidden
  bg-[var(--bg-secondary)]
  border border-[var(--border-secondary)]
  text-[10px] leading-[16px] font-normal
">
  J
</span>
```

### Tailwind Size Quick Reference

| Size | Tailwind Width/Height |
|------|-----------------------|
| XS (32px) | `w-8 h-8` |
| SM (40px) | `w-10 h-10` |
| MD (48px) | `w-12 h-12` |
| LG (56px) | `w-14 h-14` |
| XL (64px) | `w-16 h-16` |

### Compact Size Quick Reference

| Size | Tailwind Width/Height |
|------|-----------------------|
| XS (12px) | `w-3 h-3` |
| SM (16px) | `w-4 h-4` |
| MD (20px) | `w-5 h-5` |
| LG (24px) | `w-6 h-6` |
| XL (28px) | `w-7 h-7` |

---

## Key Implementation Rules

1. **Avatars are ALWAYS circular** — `rounded-full overflow-hidden`, no exceptions
2. **Background is always `var(--bg-secondary)`** (#F1F4F7) — never white, never gray-100, never transparent
3. **Border is 1px solid `var(--border-secondary)`** (#CCD6DC) for text and icon types ONLY — image avatars have NO border
4. **Images use `object-cover`** to fill the circle while maintaining aspect ratio
5. **Text type shows uppercase initials** — first letter of first two words, max 2 characters
6. **Compact avatar shows only the first character**, not full initials
7. **Font weight is 500** (`font-medium`) for all standard avatar text
8. **XL uses the same typography as LG** (16px) — NOT a larger size
9. **Hover adds `var(--shadow-xs)`** — subtle `0 1px 2px 0 rgba(34,42,63,0.05)`
10. **No status indicators or online/offline dots** are built into the avatar — those are separate components if needed
