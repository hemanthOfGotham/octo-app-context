# Separator Component — Strict Specification

When building any divider line, horizontal rule, or content separator, follow these specifications exactly. Do NOT use `<hr>`, Tailwind's `divide-*` utilities, or shadcn Separator.

> **TOKEN RULE:** Every color in this spec is expressed as a CSS variable token. In Tailwind classes use `bg-[var(--token)]` / `text-[var(--token)]` / `border-[var(--token)]`. In inline styles use `'var(--token)'`. In prose and tables, show `var(--token)` (#hex) for reference.

---

## Structure

### Simple Line (no text/icons)

```jsx
<div
  className="flex items-center w-full"
  style={{
    margin: gapMargin,
  }}
>
  <div style={{
    flex: 1,
    borderTopWidth: '1px',
    borderTopStyle: borderStyle,
    borderTopColor: variantColor,
  }} />
</div>
```

### Line with Text/Icons

```jsx
<div
  className="flex items-center"
  style={{
    gap: '8px',
    margin: gapMargin,
    width: '100%',
  }}
>
  {/* Left line */}
  <div style={{ flex: 1, borderTopWidth: '1px', borderTopStyle: borderStyle, borderTopColor: variantColor }} />

  {/* Content */}
  <span className="flex items-center" style={{ gap: '4px', color: 'var(--text-primary)' }}>
    {leadingIcon && <Icon style={{ fontSize: '14px', lineHeight: '20px' }} />}
    {text && (
      <span style={{ fontFamily: 'Inter, sans-serif', fontSize: '12px', lineHeight: '18px', fontWeight: 600 }}>
        {text}
      </span>
    )}
    {trailingIcon && <Icon style={{ fontSize: '14px', lineHeight: '20px' }} />}
  </span>

  {/* Right line */}
  <div style={{ flex: 1, borderTopWidth: '1px', borderTopStyle: borderStyle, borderTopColor: variantColor }} />
</div>
```

---

## Inputs

| Input | Type | Default | Description |
|-------|------|---------|-------------|
| `text` | string | `''` | Optional text in center |
| `leadingIcon` | Icon | — | Optional icon before text |
| `trailingIcon` | Icon | — | Optional icon after text |
| `type` | `'solid'` \| `'dashed'` | `'solid'` | Border style |
| `variant` | `'primary'` \| `'secondary'` \| `'tertiary'` | `'secondary'` | Line color |
| `gap` | `'xs'`\|`'sm'`\|`'smd'`\|`'md'`\|`'lg'`\|`'xl'` | `'md'` | Spacing around separator |
| `direction` | `'horizontal'` \| `'vertical'` | `'horizontal'` | Orientation |

---

## Variant Colors

| Variant | Hex | Token |
|---------|-----|-------|
| `primary` | `var(--border-primary)` (#BDC8D3) | border-primary (SECONDARY_TEXT_300) |
| `secondary` | `var(--border-secondary)` (#CCD6DC) | border-secondary (SECONDARY_TEXT_200) |
| `tertiary` | `var(--border-tertiary)` (#E8ECF1) | border-tertiary (SECONDARY_TEXT_100) |

**Default variant:** `secondary` → `var(--border-secondary)` (#CCD6DC)

---

## Gap Spacing

For **horizontal** direction (margin top/bottom):

| Gap | Margin | Pixels |
|-----|--------|--------|
| `xs` | 4px 0 | spacing-1 |
| `sm` | 8px 0 | spacing-2 |
| `smd` | 12px 0 | spacing-3 |
| `md` | 16px 0 | spacing-4 (default) |
| `lg` | 24px 0 | spacing-6 |
| `xl` | 32px 0 | spacing-8 |

For **vertical** direction (margin left/right):

| Gap | Margin | Pixels |
|-----|--------|--------|
| `xs` | 0 4px | spacing-1 |
| `sm` | 0 8px | spacing-2 |
| `md` | 0 16px | spacing-4 |
| `lg` | 0 24px | spacing-6 |
| `xl` | 0 32px | spacing-8 |

Note: `smd` gap is only available for horizontal direction.

---

## Vertical Direction

For vertical separators, the border is applied on the left instead of top:

```jsx
<div
  className="flex flex-col self-stretch"
  style={{
    minHeight: '100%',
    margin: gapMargin,
    alignItems: 'center',
    gap: '8px',
  }}
>
  <div style={{
    flex: 1,
    borderLeftWidth: '1px',
    borderLeftStyle: borderStyle,
    borderLeftColor: variantColor,
  }} />
</div>
```

---

## Typography

| Element | Size | Line Height | Weight |
|---------|------|-------------|--------|
| Text | 12px | 18px | 600 (title-sm) |
| Icon | 14px | 20px | — (caption-md) |

Text color: `var(--text-primary)` (#353E5A)
Content gap (between icon and text): 4px (spacing-1)
Outer gap (between lines and content): 8px (spacing-2)

---

## Content Rendering Rule

- If **none** of `text`, `leadingIcon`, or `trailingIcon` are provided → render a single line only
- If **any** of them are provided → render two lines with content in between

---

## CRITICAL Rules

1. **Do NOT use `<hr>` elements** — build with `<div>` elements and CSS border properties.
2. **Default variant is `secondary` (`var(--border-secondary)` #CCD6DC)** — not primary or tertiary.
3. **Default gap is `md` (16px margin)** — not 8px or 0.
4. **Default type is `solid`** — not dashed.
5. **Border width is always 1px** — never 2px or thicker.
6. **Text uses title-sm typography** (12px/18px/600 weight) — this is a heading weight, NOT paragraph.
7. **The line uses `flex: 1`** to fill available space — when content is present, both lines share the remaining space equally.
8. **Vertical separators use `border-left`**, horizontal use `border-top`. Never use `border-bottom` or `border-right`.
