# Callout Component — Strict Specification

When building any callout, alert, or inline message, follow these specifications exactly. Do NOT use default Tailwind alert styles, shadcn alerts, or any other alert system.

> **TOKEN RULE:** Every color in the code examples below uses CSS variable tokens or Tailwind palette names — never raw hex. When implementing, copy the token references exactly. See `design-tokens.md` for definitions.

---

## Callout Anatomy

A callout is a flex container with this structure:

### Basic Type (icon-based)
```
[leading icon]  [heading + text (inline)]  [trailing button]  [close button]
                [bottom button]
```

### Detailed Type (bar-based)
```
[vertical bar]  [heading]                                     [close button]
                [text]
                [trailing button]
                [bottom button]
```

---

## Two Layout Types

### Basic
- Leading element: **Icon** (optional, 20px)
- Text layout: heading and description are **inline** (same row, flex-wrap)
- Padding: 10px vertical, 16px horizontal (`py-2.5 px-4`)
- Has a **1px border** around the entire callout (color varies by variant)

### Detailed
- Leading element: **Vertical bar** (2px wide, full height, rounded)
- Text layout: heading and description are **stacked** (column direction)
- Padding: 12px all sides (`p-3`)
- **No border** — only background color

---

## Variants — Complete Color Spec

There are 7 variants. Each has background, border (basic only), and icon/bar color.

### Primary

| Property | Value |
|----------|-------|
| Background | `var(--bg-brand-primary)` (#FDF1EF) |
| Border (basic) | 1px solid `var(--border-brand)` (#FFAA9D) |
| Icon / Bar color | `var(--fg-brand-primary)` (#FF725C) |
| Text color | `var(--text-primary)` (#353E5A) — inherited |
| Default icon | ✓ checkCircleFill |

### Secondary

| Property | Value |
|----------|-------|
| Background | `var(--bg-brand-secondary)` (#E7FDFD) |
| Border (basic) | 1px solid `var(--border-brand-secondary)` (#46E6E8) |
| Icon / Bar color | `var(--fg-brand-secondary)` (#098486) |
| Text color | `var(--text-primary)` (#353E5A) — inherited |
| Default icon | ✓ checkCircleFill |

### Neutral

| Property | Value |
|----------|-------|
| Background | `var(--bg-secondary)` (#F1F4F7) |
| Border (basic) | 1px solid `var(--border-secondary)` (#CCD6DC) |
| Icon / Bar color | `var(--fg-primary)` (#353E5A) |
| Text color | `var(--text-primary)` (#353E5A) — inherited |
| Default icon | ✓ checkCircleFill |

### Warning

| Property | Value |
|----------|-------|
| Background | `var(--bg-warning-primary)` (#FFFAE6) |
| Border (basic) | 1px solid `var(--border-warning)` (#FFC400) |
| Icon / Bar color | `var(--fg-warning-primary)` (#F08400) |
| Text color | `var(--text-primary)` (#353E5A) — inherited |
| Default icon | ⚠ alertTriangleFill |

### Danger

| Property | Value |
|----------|-------|
| Background | `var(--bg-error-primary)` (#FFEBE6) |
| Border (basic) | 1px solid `var(--border-error)` (#FF7452) |
| Icon / Bar color | `var(--fg-error-primary)` (#F02C00) |
| Text color | `var(--text-primary)` (#353E5A) — inherited |
| Default icon | ✕ closeFill |

### Success

| Property | Value |
|----------|-------|
| Background | `var(--bg-success-primary)` (#E3FCEF) |
| Border (basic) | 1px solid `var(--border-success)` (#57D9A3) |
| Icon / Bar color | `var(--fg-success-primary)` (#31A172) |
| Text color | `var(--text-primary)` (#353E5A) — inherited |
| Default icon | ✓ checkCircleFill |

### Info

| Property | Value |
|----------|-------|
| Background | `var(--bg-info-primary)` (#F0F7FF) |
| Border (basic) | 1px solid `var(--border-info)` (#B6D6FB) |
| Icon / Bar color | `var(--fg-info-primary)` (#3D93F5) |
| Text color | `var(--text-primary)` (#353E5A) — inherited |
| Default icon | ℹ infoFill |

---

## Typography

> **CRITICAL: Basic and Detailed types use DIFFERENT font weights for headings. Do NOT mix them up.**

### Basic Type — Heading is `font-medium` (500), NOT semibold
| Element | Typography | Classes |
|---------|-----------|---------|
| Heading | 14px / 20px / **weight 500** | `text-sm leading-5 font-medium` |
| Description | 14px / 20px / **weight 500**, secondary color | `text-sm leading-5 font-medium text-[var(--text-secondary)]` |

- Basic heading and description use the **same font weight** (500 / `font-medium`)
- They appear **inline** in the same row (flex-wrap), separated by a 4px gap
- The only visual difference is the description has a lighter text color (`var(--text-secondary)` (#495883))

### Detailed Type — Heading is weight 600, Description is weight 400
| Element | Typography | Classes |
|---------|-----------|---------|
| Heading | 14px / 20px / **weight 600** | `text-sm leading-5` with `style={{ fontWeight: 600 }}` |
| Description | 14px / 20px / **weight 400** | `text-sm leading-5 font-normal` |

- Detailed heading uses **exact numeric weight 600** — always set via inline `style={{ fontWeight: 600 }}` to ensure precision. Do NOT use Tailwind's `font-semibold` or `font-bold` classes.
- Description uses weight 400 (`font-normal`)
- They are **stacked** vertically (flex-column)

### Quick Reference — Font Weights by Type
| | Basic Type | Detailed Type |
|---|---|---|
| **Heading** | `font-medium` (500) | `style={{ fontWeight: 600 }}` — exact 600, not font-bold |
| **Description** | `font-medium` (500) | `font-normal` (400) |

---

## Leading Element

### Basic Type — Icon
- Size: 20px × 20px
- Color: variant-specific (see color table above)
- Margin-right: 6px (`mr-1.5`)
- Only shown when `showLeadingIcon` is true
- Uses a default icon per variant (can be overridden with custom icon)

**Default icons by variant:**
| Variant | Icon |
|---------|------|
| primary | ✓ Check circle (filled) |
| secondary | ✓ Check circle (filled) |
| neutral | ✓ Check circle (filled) |
| success | ✓ Check circle (filled) |
| info | ℹ Info circle (filled) |
| danger | ✕ Close circle (filled) |
| warning | ⚠ Alert triangle (filled) |

### Detailed Type — Vertical Bar
- Width: 2px
- Height: stretches to full callout height (`align-self: stretch`)
- Border-radius: 9999px (fully rounded)
- Color: variant-specific (same as icon color)
- Margin-right: 12px (`mr-3`)

---

## Close Button

- Positioned at the far right of the callout
- Margin-left: 8px (`ml-2`)
- Rendered as an icon-only button:
  - Size: XS (24px × 24px)
  - Variant: neutral
  - Type: text (no background)
  - Icon: ✕ close
- Only shown when `showCloseButton` is true
- Emits a close event on click

---

## Action Buttons

Callouts support two button placement slots:

### Trailing Button (inline)
- Placed inline, to the right of the text content
- Gap: 4px from text content
- **Must be:** secondary variant, hyperlink type, md size

### Bottom Button (stacked)
- Placed below the text content
- Adds 4px margin-bottom to the content area above it
- When present, callout switches to `align-items: flex-start` (top-aligned)
- **Must be:** secondary variant, hyperlink type, md size

Button styling for callout actions:
- Text color: `var(--text-brand-secondary)` (#098486)
- No background, no border
- Underline on hover
- Size: md (14px font, 40px height)

---

## Multi-line Behavior

When the text content exceeds a single line (height > 20px):
- Callout switches to `align-items: flex-start` (top-aligned)
- Content container also top-aligns
- This prevents the icon/bar from being vertically centered against tall content

---

## Container Specs

| Property | Value |
|----------|-------|
| Display | `flex` |
| Align items | `center` (single line) or `flex-start` (multi-line / bottom button) |
| Border radius | 8px (`rounded-lg`) |
| Text color | `var(--text-primary)` (#353E5A) — body text color |

### Basic Type
| Property | Value |
|----------|-------|
| Padding | 10px top/bottom, 16px left/right |
| Border | 1px solid (variant-specific color) |
| Background | variant-specific light color |

### Detailed Type
| Property | Value |
|----------|-------|
| Padding | 12px all sides |
| Border | none |
| Background | variant-specific light color |

---

## Implementation Pattern for Lovable

```jsx
{/* === BASIC TYPE — Success variant with icon === */}
<div className="
  flex items-center
  rounded-lg
  py-2.5 px-4
  bg-[var(--bg-success-primary)]
  border border-[var(--border-success)]
  text-[var(--text-primary)]
">
  {/* Leading icon */}
  <CheckCircleIcon className="w-5 h-5 text-[var(--fg-success-primary)] mr-1.5 shrink-0" />

  {/* Content */}
  <div className="flex-1">
    <div className="flex justify-between gap-1">
      <div className="flex items-center flex-wrap gap-1">
        <span className="text-sm leading-5 font-medium">
          Operation successful
        </span>
        <span className="text-sm leading-5 font-medium text-[var(--text-secondary)]">
          Your changes have been saved.
        </span>
      </div>
    </div>
  </div>

  {/* Close button */}
  <button className="
    ml-2 p-1 rounded
    inline-flex items-center justify-center
    text-[var(--fg-primary)]
    hover:bg-[var(--bg-secondary_hover)]
    cursor-pointer border-none outline-none
    transition-[background-color] duration-150 ease-out
  " aria-label="Close">
    <XIcon className="w-4 h-4" />
  </button>
</div>


{/* === BASIC TYPE — Warning with trailing action button === */}
<div className="
  flex items-center
  rounded-lg
  py-2.5 px-4
  bg-[var(--bg-warning-primary)]
  border border-[var(--border-warning)]
  text-[var(--text-primary)]
">
  <AlertTriangleIcon className="w-5 h-5 text-[var(--fg-warning-primary)] mr-1.5 shrink-0" />

  <div className="flex-1">
    <div className="flex justify-between gap-1">
      <div className="flex items-center flex-wrap gap-1">
        <span className="text-sm leading-5 font-medium">
          Subscription expiring soon
        </span>
        <span className="text-sm leading-5 font-medium text-[var(--text-secondary)]">
          Your plan expires in 3 days.
        </span>
      </div>

      {/* Trailing action button */}
      <button className="
        text-[var(--text-brand-secondary)] text-sm leading-5 font-semibold
        underline decoration-transparent
        hover:decoration-[var(--text-brand-secondary)]
        bg-transparent border-none cursor-pointer
        whitespace-nowrap shrink-0
        transition-all duration-150 ease-out
      ">
        Renew Now
      </button>
    </div>
  </div>
</div>


{/* === DETAILED TYPE — Danger variant === */}
{/* IMPORTANT: Detailed heading uses font-semibold (600), NOT font-bold (700). Description uses font-normal (400). */}
<div className="
  flex items-start
  rounded-lg
  p-3
  bg-[var(--bg-error-primary)]
  text-[var(--text-primary)]
">
  {/* Vertical bar */}
  <div className="w-0.5 self-stretch rounded-full bg-[var(--bg-error-solid)] mr-3 shrink-0" />

  {/* Content */}
  <div className="flex-1">
    <div className="flex justify-between gap-1">
      <div className="flex flex-col">
        {/* Heading: exact weight 600 via inline style — do NOT use font-bold or font-semibold class */}
        <span className="text-sm leading-5" style={{ fontWeight: 600 }}>
          Payment failed
        </span>
        {/* Description: font-normal (400) — do NOT use font-medium */}
        <span className="text-sm leading-5 font-normal">
          We couldn't process your last payment. Please update your billing information.
        </span>
      </div>
    </div>

    {/* Bottom action button */}
    <div className="mt-1">
      <button className="
        text-[var(--text-brand-secondary)] text-sm leading-5 font-semibold
        underline decoration-transparent
        hover:decoration-[var(--text-brand-secondary)]
        bg-transparent border-none cursor-pointer p-0
        transition-all duration-150 ease-out
      ">
        Update Billing
      </button>
    </div>
  </div>

  {/* Close button */}
  <button className="
    ml-2 p-1 rounded
    inline-flex items-center justify-center
    text-[var(--fg-primary)]
    hover:bg-[var(--bg-secondary_hover)]
    cursor-pointer border-none outline-none shrink-0
    transition-[background-color] duration-150 ease-out
  " aria-label="Close">
    <XIcon className="w-4 h-4" />
  </button>
</div>


{/* === BASIC TYPE — Info, no icon, no close button (minimal) === */}
<div className="
  flex items-center
  rounded-lg
  py-2.5 px-4
  bg-[var(--bg-info-primary)]
  border border-[var(--border-info)]
  text-[var(--text-primary)]
">
  <div className="flex-1">
    <div className="flex items-center flex-wrap gap-1">
      <span className="text-sm leading-5 font-medium">
        Tip:
      </span>
      <span className="text-sm leading-5 font-medium text-[var(--text-secondary)]">
        You can drag and drop files to upload them.
      </span>
    </div>
  </div>
</div>
```

---

### Variant Quick-Reference for Code (all 7 variants)

Use this table to swap tokens when building different variants:

| Variant | Background token | Border token | Icon/Bar token |
|---------|-----------------|--------------|----------------|
| primary | `var(--bg-brand-primary)` | `var(--border-brand)` | `var(--fg-brand-primary)` |
| secondary | `var(--bg-brand-secondary)` | `secondary-300` | `var(--fg-brand-secondary)` |
| neutral | `var(--bg-secondary)` | `var(--border-secondary)` | `var(--fg-primary)` |
| warning | `var(--bg-warning-primary)` | `var(--border-warning)` | `var(--fg-warning-primary)` |
| danger | `var(--bg-error-primary)` | `var(--border-error)` | `var(--fg-error-primary)` |
| success | `var(--bg-success-primary)` | `var(--border-success)` | `var(--fg-success-primary)` |
| info | `var(--bg-info-primary)` | `var(--border-info)` | `var(--fg-info-primary)` |

---

### Key Implementation Rules

1. **Basic type has a border**, detailed type does NOT
2. **Basic type icon is 20px**, positioned with 6px right margin
3. **Detailed type bar is 2px wide**, stretches full height, positioned with 12px right margin
4. **Border radius is always 8px** (`rounded-lg`)
5. **Body text color is always `var(--text-primary)`** — the variant only changes bg, border, and icon/bar color
6. **Basic heading and description are inline** (same row with flex-wrap); **detailed heading and description are stacked** (column)
7. **CRITICAL — Basic heading = `font-medium` (500), Detailed heading = exact weight 600 via `style={{ fontWeight: 600 }}`.** Do NOT use `font-semibold` or `font-bold` Tailwind classes for detailed headings — always use inline `style={{ fontWeight: 600 }}` for precise weight. Basic type heading uses `font-medium` (500).
8. **Action buttons MUST be secondary hyperlink style** — text color `var(--text-brand-secondary)`, underline on hover
9. **Close button is always an XS neutral text button** (24px, icon-only)
10. **Multi-line content switches to `items-start`** (top-aligned) instead of `items-center`
11. **Detailed type uses `items-start` by default** because heading + text are stacked
12. **Use the correct default icon per variant** — check for success/primary/secondary/neutral, info for info, alert-triangle for warning, close/x for danger
13. **NEVER use raw hex values** — always use the CSS variable token or Tailwind palette name from the variant quick-reference table above
