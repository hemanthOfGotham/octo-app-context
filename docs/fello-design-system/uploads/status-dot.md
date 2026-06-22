# Status Dot Component — Strict Specification

When building any status indicator dot, online/offline indicator, or colored circle marker, follow these specifications exactly. Do NOT use Tailwind's rounded-full with arbitrary background colors.

> **TOKEN RULE:** Every color in this spec is expressed as a CSS variable token. In Tailwind classes use `bg-[var(--token)]` / `text-[var(--token)]` / `border-[var(--token)]`. In inline styles use `'var(--token)'`. In prose and tables, show `var(--token)` (#hex) for reference.

---

## Structure

A simple SVG circle that inherits its color from the parent element's CSS `color` property.

```jsx
<span
  className="inline-flex justify-center items-center"
  style={{
    borderRadius: '9999px',
    width: sizePixels,
    height: sizePixels,
    color: variantColor,
  }}
>
  <svg height="100%" width="100%" viewBox="0 0 100 100">
    <circle cx="50" cy="50" r="50" fill="currentColor" />
  </svg>
</span>
```

---

## Size Variants

| Size | Pixels |
|------|--------|
| `xs` | 6px |
| `sm` | 8px |
| `md` | **10px** (default) |
| `lg` | 12px |
| `xl` | 14px |

---

## Color Variants

| Variant | Hex | Token |
|---------|-----|-------|
| `primary` | `var(--fg-brand-primary)` (#FF725C) | fg-brand-primary (PRIMARY_500) |
| `secondary` | `var(--fg-brand-secondary)` (#098486) | fg-brand-secondary (SECONDARY_700) |
| `white` | `var(--fg-white)` (#FFFFFF) | fg-white |
| `info` | `var(--fg-info-primary)` (#3D93F5) | fg-info-primary (BLUE_600) |
| `success` | `var(--fg-success-primary)` (#31A172) | fg-success-primary (SUCCESS_600) |
| `warning` | `var(--fg-warning-primary)` (#F08400) | fg-warning-primary (WARNING_600) |
| `error` | `var(--fg-error-primary)` (#F02C00) | fg-error-primary (ERROR_600) |
| `disabled` | `var(--fg-disabled)` (#8EA1AF) | fg-disabled (SECONDARY_TEXT_500) |
| `inherit` | `inherit` | Inherits from parent |

**Default variant:** `info` (renders as `var(--fg-info-primary)` #3D93F5)

---

## Behavior

- **Purely presentational** — no interactions, no hover states, no animations, no pulsing
- **No border or outline ring** — solid filled circle only
- **Display: inline-flex** — flows inline with text

---

## Usage Examples

```jsx
{/* Default (info, md) */}
<StatusDot />

{/* Success, small */}
<StatusDot variant="success" size="sm" />

{/* Error, large */}
<StatusDot variant="error" size="lg" />

{/* Inline with text */}
<div className="flex items-center gap-2">
  <StatusDot variant="success" size="sm" />
  <span>Online</span>
</div>
```

---

## CRITICAL Rules

1. **Use an SVG circle with `fill="currentColor"`** — this allows the color to be set via CSS `color` property on the parent.
2. **Default variant is `info` (`var(--fg-info-primary)` #3D93F5)** — not success or primary.
3. **Default size is `md` (10px)** — not 8px or 12px.
4. **No animations** — this is a static indicator. Do NOT add pulse or glow effects.
5. **No border or ring** — solid circle only. Do NOT add outline or box-shadow.
6. **The `inherit` variant** inherits color from the parent element — useful for matching surrounding text color.
