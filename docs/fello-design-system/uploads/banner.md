# Banner Component — Strict Specification

When building any banner, notification bar, or top-level alert, follow these specifications exactly. Do NOT use default Tailwind alert/banner styles, shadcn banners, or any other banner system.

> **TOKEN RULE:** Every color in this spec is expressed as a CSS variable token. In Tailwind classes use `bg-[var(--token)]` / `text-[var(--token)]`. In inline styles use `'var(--token)'`. In prose and tables, show `var(--token)` (#hex) for reference.

---

## Banner Anatomy

A banner is a full-width flex container with this structure:

```
[icon]  [heading + text (inline) + leading button]  [trailing button]  [close button]
```

- Layout: `display: flex`, `align-items: center`, `gap: 12px` (`gap-3`)
- Padding: 8px vertical, 16px horizontal (`py-2 px-4`)
- Icon, content, buttons, and close button are in a single row

---

## Variants

There are 8 banner variants:

| Variant | Default Icon |
|---------|-------------|
| **primary** | ✓ Check circle (filled) |
| **secondary** | ✓ Check circle (filled) |
| **neutral** | ✓ Check circle (filled) |
| **success** | ✓ Check circle (filled) |
| **info** | ℹ Info circle (filled) |
| **danger** | ✕ Close circle (filled) |
| **warning** | ⚠ Alert triangle (filled) |
| **magic** | ✓ Check circle (filled) |

---

## Fill Types

Each variant has two fill modes:

| Fill | Description |
|------|-------------|
| **dark** | Solid colored background, white text and icons |
| **light** | Soft/tinted background, colored text and icons |

---

## Variants × Fill — Complete Color Spec

### Dark Fill (solid background, white text)

| Variant | Background | Text & Icon Color |
|---------|-----------|-------------------|
| primary | `var(--bg-brand-solid)` (#FF725C) | `var(--text-white)` (#FFFFFF) |
| secondary | `var(--bg-brand-secondary-solid)` (#098486) | `var(--text-white)` (#FFFFFF) |
| neutral | `var(--bg-primary-solid)` (#353E5A) | `var(--text-white)` (#FFFFFF) |
| warning | `var(--bg-warning-solid)` (#F08400) | `var(--text-white)` (#FFFFFF) |
| danger | `var(--bg-error-solid)` (#F02C00) | `var(--text-white)` (#FFFFFF) |
| success | `var(--bg-success-solid)` (#31A172) | `var(--text-white)` (#FFFFFF) |
| info | `var(--bg-info-solid)` (#3D93F5) | `var(--text-white)` (#FFFFFF) |
| magic | `var(--bg-magic-solid)` (#8D6AE7) | `var(--text-white)` (#FFFFFF) |

### Light Fill (tinted background, colored text)

| Variant | Background | Icon Color | Text Color |
|---------|-----------|------------|------------|
| primary | `var(--bg-brand-primary)` (#FDF1EF) | `var(--fg-brand-primary)` (#FF725C) | `var(--text-primary)` (#353E5A) |
| secondary | `var(--bg-brand-secondary)` (#E7FDFD) | `var(--fg-brand-secondary)` (#098486) | `var(--text-primary)` (#353E5A) |
| neutral | `var(--bg-secondary)` (#F1F4F7) | `var(--fg-primary)` (#353E5A) | `var(--text-primary)` (#353E5A) |
| warning | `var(--bg-warning-primary)` (#FFFAE6) | `var(--fg-warning-primary)` (#F08400) | `var(--text-primary)` (#353E5A) |
| danger | `var(--bg-error-primary)` (#FFEBE6) | `var(--fg-error-primary)` (#F02C00) | `var(--text-primary)` (#353E5A) |
| success | `var(--bg-success-primary)` (#E3FCEF) | `var(--fg-success-primary)` (#31A172) | `var(--text-primary)` (#353E5A) |
| info | `var(--bg-info-primary)` (#F0F7FF) | `var(--fg-info-primary)` (#3D93F5) | `var(--text-primary)` (#353E5A) |
| magic | `#F4F3FF` (purple-50) | `var(--fg-magic-primary)` (#8D6AE7) | `var(--text-primary)` (#353E5A) |

> **Key rule:** Dark fill → ALL text and icons are `var(--text-white)` (#FFFFFF). Light fill → text is `var(--text-primary)` (#353E5A), icon uses variant-specific color.

---

## Typography

| Element | Typography | Classes |
|---------|-----------|---------|
| Heading | 14px / 20px / weight 600 | `text-sm leading-5` with `style={{ fontWeight: 600 }}` |
| Text | 14px / 20px / weight 400 | `text-sm leading-5 font-normal` |

- Heading uses `title-md` = weight 600. Always set via `style={{ fontWeight: 600 }}` for precision.
- Text uses `paragraph-md` = weight 400 (`font-normal`).
- Both heading and text display **inline** (`display: inline`).
- Text has horizontal margin: 4px left and right (`mx-1`).

---

## Icon

- Size: 20px × 20px
- Color: variant-specific (see color table above)
- In dark fill: icon is white
- In light fill: icon uses the variant color
- Can be overridden with a custom icon

---

## Close Button

- Always present unless explicitly hidden
- Rendered as an icon-only button (XS size, 24px × 24px)
- **Dark fill:** white variant text button (white icon)
- **Light fill:** neutral variant text button (default icon color)
- Icon: ✕ close
- Emits a close event on click

---

## Action Buttons

Banners support two button placement slots:

### Leading Button (inline with text)
- Placed inline after the text content, inside the content container
- Part of the text flow

### Trailing Button (after content)
- Placed after the content container, before the close button
- Separate from the text content

---

## Multi-line Behavior

When text content exceeds a single line (height > 21px):
- Banner switches from `align-items: center` to `align-items: flex-start`
- This top-aligns the icon with the first line of text

---

## Container Specs

| Property | Value |
|----------|-------|
| Display | `flex` |
| Align items | `center` (single line) or `flex-start` (multi-line) |
| Gap | 12px (`gap-3`) |
| Padding | 8px vertical, 16px horizontal (`py-2 px-4`) |
| Width | Full width of parent container |

---

## Implementation Pattern for Lovable

```jsx
{/* === Dark Fill — Primary variant === */}
<div className="
  flex items-center gap-3
  py-2 px-4
  bg-[var(--bg-brand-solid)] text-[var(--text-white)]
  w-full
">
  {/* Icon */}
  <CheckCircleIcon className="w-5 h-5 shrink-0" />

  {/* Content */}
  <div className="flex-1">
    <span className="text-sm leading-5 inline" style={{ fontWeight: 600 }}>
      Banner Heading
    </span>
    <span className="text-sm leading-5 font-normal inline mx-1">
      Banner description text goes here.
    </span>
  </div>

  {/* Close button */}
  <button className="
    p-1 rounded
    inline-flex items-center justify-center
    text-white
    hover:bg-white/10
    cursor-pointer border-none outline-none shrink-0
    transition-[background-color] duration-150 ease-out
  " aria-label="Close">
    <XIcon className="w-4 h-4" />
  </button>
</div>


{/* === Light Fill — Warning variant === */}
<div className="
  flex items-center gap-3
  py-2 px-4
  bg-[var(--bg-warning-primary)] text-[var(--text-primary)]
  w-full
">
  {/* Icon — uses variant color in light fill */}
  <AlertTriangleIcon className="w-5 h-5 text-[var(--fg-warning-primary)] shrink-0" />

  {/* Content */}
  <div className="flex-1">
    <span className="text-sm leading-5 inline" style={{ fontWeight: 600 }}>
      Warning
    </span>
    <span className="text-sm leading-5 font-normal inline mx-1">
      Your subscription expires in 3 days.
    </span>
    {/* Inline leading button */}
    <button className="
      text-[var(--text-brand-secondary)] text-sm leading-5
      underline decoration-transparent hover:decoration-[var(--text-brand-secondary)]
      bg-transparent border-none cursor-pointer inline
      transition-all duration-150 ease-out
    " style={{ fontWeight: 600 }}>
      Renew Now
    </button>
  </div>

  {/* Close button — neutral variant in light fill */}
  <button className="
    p-1 rounded
    inline-flex items-center justify-center
    text-[var(--text-primary)]
    hover:bg-[var(--bg-secondary_hover)]
    cursor-pointer border-none outline-none shrink-0
    transition-[background-color] duration-150 ease-out
  " aria-label="Close">
    <XIcon className="w-4 h-4" />
  </button>
</div>


{/* === Dark Fill — Success variant with trailing button === */}
<div className="
  flex items-center gap-3
  py-2 px-4
  bg-[var(--bg-success-solid)] text-[var(--text-white)]
  w-full
">
  <CheckCircleIcon className="w-5 h-5 shrink-0" />

  <div className="flex-1">
    <span className="text-sm leading-5 inline" style={{ fontWeight: 600 }}>
      Success!
    </span>
    <span className="text-sm leading-5 font-normal inline mx-1">
      Your changes have been saved.
    </span>
  </div>

  {/* Trailing button */}
  <button className="
    text-white text-sm leading-5
    underline decoration-transparent hover:decoration-white
    bg-transparent border-none cursor-pointer shrink-0
    transition-all duration-150 ease-out
  " style={{ fontWeight: 600 }}>
    View Details
  </button>

  <button className="
    p-1 rounded
    inline-flex items-center justify-center
    text-white
    hover:bg-white/10
    cursor-pointer border-none outline-none shrink-0
    transition-[background-color] duration-150 ease-out
  " aria-label="Close">
    <XIcon className="w-4 h-4" />
  </button>
</div>
```

### Key Implementation Rules

1. **Dark fill = white text and icons for ALL elements**, light fill = `var(--text-primary)` (#353E5A) text + variant-colored icon
2. **Heading is `inline` with weight 600** (via `style={{ fontWeight: 600 }}`), text is `inline` with weight 400
3. **Text has 4px horizontal margin** (`mx-1`) to separate from heading and any buttons
4. **Gap between icon, content, and buttons is 12px** (`gap-3`)
5. **Padding is always 8px vertical, 16px horizontal** (`py-2 px-4`)
6. **Banner stretches full width** of its container
7. **Close button is always present** unless explicitly hidden — uses white variant (dark fill) or neutral variant (light fill)
8. **Multi-line content switches to `items-start`** (top-aligned) instead of `items-center`
9. **Icon is always 20px** and comes from the variant-specific default icon set
10. **Use the correct default icon per variant** — check for success/primary/secondary/neutral/magic, info for info, alert-triangle for warning, close/x for danger
