# Indicator Icon — Strict Specification

When building an indicator icon (a base icon with a small status indicator overlaid at the top-right corner), follow these specifications exactly.

> **TOKEN RULE:** Every colour in this spec is expressed as a CSS variable token. When writing Tailwind classes use the `bg-[var(--token)]` / `text-[var(--token)]` / `border-[var(--token)]` syntax. In inline styles use `'var(--token)'`. In prose and tables the original hex is kept only as a parenthetical annotation, e.g. `var(--fg-success-primary)` (#31A172).

---

## Architecture

```
┌──────────────┐
│              ●│  ← Indicator icon (top-right, overlapping edge)
│   BASE ICON  │
│              │
└──────────────┘
```

---

## Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `icon` | `string` | Required | The base icon name |
| `iconSize` | `number` | `16` | Base icon size in pixels |
| `variant` | `'success' \| 'warning' \| 'danger' \| 'neutral'` | `'success'` | Status variant for indicator color |
| `indicatorIcon` | `string` | — | Custom indicator icon (overrides variant default) |
| `hideIndicatorIcon` | `boolean` | `false` | Hides the indicator icon entirely |

---

## Layout

### Container (Host)

| Property | Value | Tailwind / CSS |
|----------|-------|----------------|
| Display | inline-flex | `inline-flex` |
| Position | relative | `relative` |

### Base Icon

| Property | Value | Tailwind / CSS |
|----------|-------|----------------|
| Size | `iconSize` prop (default 16px) | `text-[{iconSize}px]` |
| Color | inherits from parent | `text-current` |

### Indicator Icon

| Property | Value | Tailwind / CSS |
|----------|-------|----------------|
| Position | absolute | `absolute` |
| Top | 0 | `top-0` |
| Right | -10% of base icon size | `right-[-10%]` |
| Size | 50% of `iconSize` | Dynamic: `Math.ceil(iconSize / 2)` px |
| Background | `var(--bg-primary)` (#FFFFFF) | `bg-[var(--bg-primary)]` |
| Border radius | full circle | `rounded-full` |

---

## Variants

### Default Icons per Variant

| Variant | Default Icon | Description |
|---------|-------------|-------------|
| `success` | check-circle-fill | Filled circle with checkmark |
| `warning` | alert-circle-fill | Filled circle with exclamation |
| `danger` | close-fill | Filled circle with X |
| `neutral` | circle-fill | Filled solid circle |

### Variant Colors

| Variant | Color Token | Hex | Tailwind |
|---------|------------|-----|----------|
| `success` | `var(--fg-success-primary)` | (#31A172) | `text-[var(--fg-success-primary)]` |
| `warning` | `var(--fg-warning-primary)` | (#F08400) | `text-[var(--fg-warning-primary)]` |
| `danger` | `var(--fg-error-primary)` | (#F02C00) | `text-[var(--fg-error-primary)]` |
| `neutral` | `var(--fg-quinary)` | (#C5C9D7) | `text-[var(--fg-quinary)]` |

---

## Sizing Examples

| Base Icon Size | Indicator Size | Indicator Formula |
|---------------|----------------|-------------------|
| 16px | 8px | `Math.ceil(16/2)` |
| 20px | 10px | `Math.ceil(20/2)` |
| 24px | 12px | `Math.ceil(24/2)` |
| 32px | 16px | `Math.ceil(32/2)` |
| 48px | 24px | `Math.ceil(48/2)` |

---

## Color Reference

| Token | Hex | Usage |
|-------|-----|-------|
| `var(--fg-success-primary)` | (#31A172) | Success indicator |
| `var(--fg-warning-primary)` | (#F08400) | Warning indicator |
| `var(--fg-error-primary)` | (#F02C00) | Danger indicator |
| `var(--fg-quinary)` | (#C5C9D7) | Neutral indicator |
| `var(--bg-primary)` | (#FFFFFF) | Indicator background |

---

## Implementation

```jsx
const VARIANT_COLORS = {
  success: 'var(--fg-success-primary)',
  warning: 'var(--fg-warning-primary)',
  danger: 'var(--fg-error-primary)',
  neutral: 'var(--fg-quinary)',
};

const VARIANT_ICONS = {
  success: 'CheckCircleFill',   // ✓ in circle
  warning: 'AlertCircleFill',   // ! in circle
  danger: 'CloseFill',          // ✕ in circle
  neutral: 'CircleFill',        // solid circle
};

function IndicatorIcon({
  icon,
  iconSize = 16,
  variant = 'success',
  indicatorIcon,
  hideIndicatorIcon = false,
}) {
  const indicatorSize = Math.ceil(iconSize / 2);
  const indicatorColor = VARIANT_COLORS[variant];
  const resolvedIndicatorIcon = indicatorIcon || VARIANT_ICONS[variant];

  return (
    <span className="inline-flex relative" style={{ fontFamily: 'Inter, Arial, sans-serif' }}>
      {/* Base icon */}
      <span style={{ fontSize: iconSize }}>
        {/* Render base icon here */}
        <Icon name={icon} size={iconSize} />
      </span>

      {/* Indicator icon */}
      {!hideIndicatorIcon && (
        <span
          className="absolute top-0 rounded-full bg-[var(--bg-primary)] flex items-center justify-center"
          style={{
            right: '-10%',
            fontSize: indicatorSize,
            color: indicatorColor,
            width: indicatorSize,
            height: indicatorSize,
          }}
        >
          <Icon name={resolvedIndicatorIcon} size={indicatorSize} />
        </span>
      )}
    </span>
  );
}
```

---

## CRITICAL Rules

1. **Indicator size is always 50% of base icon size** — calculated as `Math.ceil(iconSize / 2)`. A 16px base icon gets an 8px indicator.
2. **Indicator is positioned at `top: 0; right: -10%`** — it slightly overflows the top-right corner of the base icon. Do NOT center it or place it inside the icon.
3. **Indicator has a `var(--bg-primary)` (#FFFFFF) circular background** — this creates a "cutout" effect so the indicator icon appears on a clean white circle overlapping the base icon.
4. **Border radius is fully circular** — `rounded-full` (10000px). The indicator is always a perfect circle.
5. **Success variant is the default** — if no `variant` is provided, use `success` (green `var(--fg-success-primary)` (#31A172)).
6. **Custom `indicatorIcon` overrides the variant default** — but the variant color still applies. Only the icon shape changes.
7. **`hideIndicatorIcon` completely removes the indicator** — the base icon renders alone with no overlay.
8. **No border on the indicator** — only the white background and icon color. Do NOT add a stroke or border.
9. **Container is `inline-flex relative`** — this ensures proper positioning of the absolute indicator relative to the base icon.
10. **Font is Inter** — all text uses `Inter, Arial, sans-serif`.
