# Progress Circle — Strict Specification

When building a progress circle (a circular SVG-based progress indicator showing a percentage value), follow these specifications exactly.

> **TOKEN RULE — mandatory for every colour value in this file.**
> Never use a raw hex colour in code output. Always use the design-system CSS variable via `var(--token)`.
> In **tables** write: `var(--token)` (#HEX). In **Tailwind classes** write: `bg-[var(--bg-token)]`, `text-[var(--text-token)]`, etc.
> In **inline styles** write: `style={{ color: 'var(--text-primary)' }}`. In **prose** write: `var(--token)` (#HEX).

---

## Architecture

```
Standard Mode:                         Compact Mode:
    ┌─────────────────┐                    ┌───┐
    │    ← arrow icon │                    │   │  ← Thin ring, no label
    │   ╭─────────╮   │                    │86%│  ← Value only (or hidden for XS)
    │   │  Label  │   │                    │   │
    │   │   86%   │   │                    └───┘
    │   ╰─────────╯   │
    │                  │
    └─────────────────┘
    Track (gray ring) with colored fill arc
```

---

## Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `value` | `number \| undefined` | Required | Progress percentage (0–100). Shows `-` when undefined |
| `size` | `'xs' \| 'sm' \| 'md' \| 'lg' \| 'xl'` | `'md'` | Size variant |
| `variant` | `'primary' \| 'secondary' \| 'ai'` | `'primary'` | Color variant |
| `label` | `string` | — | Optional label text above the value (hidden in compact mode) |
| `hidePercentage` | `boolean` | `false` | Hides the `%` symbol |
| `hideText` | `boolean` | `false` | Hides all text (value + label) |
| `compact` | `boolean` | `false` | Uses compact sizing (much smaller, thinner ring) |

---

## Sizes

### Standard Mode

| Size | Circle Diameter | Stroke Width | Arrow Icon Size |
|------|----------------|--------------|-----------------|
| XS | 102px | 6px | 6px |
| SM | 144px | 8px | 8px |
| MD | 180px | 10px | 10px |
| LG | 220px | 12px | 12px |
| XL | 260px | 16px | 16px |

### Compact Mode

| Size | Circle Diameter | Stroke Width |
|------|----------------|--------------|
| XS | 24px | 2px |
| SM | 32px | 2px |
| MD | 40px | 3px |
| LG | 48px | 3px |
| XL | 56px | 4px |

> **Note:** Compact XS hides the value text entirely (too small to display).

---

## Typography

### Standard Mode — Value Text

| Size | Typography | Font Size | Line Height | Weight |
|------|-----------|-----------|-------------|--------|
| XS | title, xl | 18px | 28px | 600 |
| SM | heading, sm | 20px | 28px | 600 |
| MD | heading, md | 24px | 32px | 600 |
| LG | heading, lg | 32px | 40px | 600 |
| XL | heading, xl | 40px | 52px | 600 |

### Standard Mode — Label Text

| Size | Typography | Font Size | Line Height | Weight |
|------|-----------|-----------|-------------|--------|
| XS | paragraph, xs | 10px | 16px | 400 |
| SM | paragraph, sm | 12px | 18px | 400 |
| MD | paragraph, md | 14px | 20px | 400 |
| LG | paragraph, lg | 16px | 24px | 400 |
| XL | paragraph, lg | 16px | 24px | 400 |

### Compact Mode — Value Text

| Size | Typography | Font Size | Line Height | Weight |
|------|-----------|-----------|-------------|--------|
| XS | *(hidden)* | — | — | — |
| SM | caption, xs | 10px | 16px | 500 |
| MD | caption, xs | 10px | 16px | 500 |
| LG | caption, sm | 12px | 18px | 500 |
| XL | caption, md | 14px | 20px | 500 |

---

## Variants

### Primary

| Element | Color Token | Hex |
|---------|------------|-----|
| Track fill | `var(--fg-brand-primary)` | (#FF725C) (PRIMARY_500) |
| Track background | `var(--bg-tertiary)` | (#E8ECF1) (SECONDARY_TEXT_100) |
| Zero-fill dot | `var(--fg-brand-primary)` | (#FF725C) |

### Secondary

| Element | Color Token | Hex |
|---------|------------|-----|
| Track fill | `var(--fg-brand-secondary)` | (#098486) (SECONDARY_700) |
| Track background | `var(--bg-tertiary)` | (#E8ECF1) (SECONDARY_TEXT_100) |
| Zero-fill dot | `var(--fg-brand-secondary)` | (#098486) |

### AI (Gradient)

| Element | Color | Hex |
|---------|-------|-----|
| Track fill | linear gradient | 5-stop gradient (see below) |
| Track background | teal at 10% opacity | `var(--chart-series-1)` (#0DC1C4) at opacity 0.1 |
| Zero-fill dot | teal | `var(--chart-series-1)` (#0DC1C4) |

#### AI Gradient Stops

| Offset | Color |
|--------|-------|
| 0% | `var(--chart-ai-1)` (#26A9DD) |
| 20% | `var(--chart-ai-2)` (#5188F1) |
| 40% | `var(--chart-ai-3)` (#8D6AE6) |
| 60% | `var(--chart-ai-4)` (#D363B3) |
| 80% | `var(--chart-ai-0)` (#F17E99) |

---

## Layout

### Container (Host)

| Property | Value | Tailwind / CSS |
|----------|-------|----------------|
| Display | block | `block` |
| Position | relative | `relative` |
| Width | circle diameter | Dynamic from size map |
| Height | circle diameter | Same as width |

### Text Area (Center)

| Property | Value | Tailwind / CSS |
|----------|-------|----------------|
| Position | absolute, centered | `absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2` |
| Display | flex column | `flex flex-col items-center` |
| Z-index | above SVG | `z-[1]` |
| Width | max-content | `w-max` |

### Value Text

| Property | Value |
|----------|-------|
| Color | `var(--text-primary)` (#353E5A) |
| Display | flex, align center |
| Content | `{value}%` or `-` when undefined |

### Label Text

| Property | Value |
|----------|-------|
| Color | `var(--text-tertiary)` (#6B748E) |
| Display | above value |
| Visibility | hidden in compact mode |

### Arrow Icon (Standard Mode Only)

| Property | Value |
|----------|-------|
| Position | absolute, top center |
| Icon | right-arrow-fill |
| Color | `var(--fg-white)` (#FFFFFF) |
| Size | matches stroke width per size |
| Z-index | 1 (above track) |
| Hidden in | compact mode |

---

## SVG Track

### Track Background (Gray Ring)

| Property | Value |
|----------|-------|
| Element | `<path>` with full 359.99° arc |
| Fill | none |
| Stroke width | from size map |
| Stroke color (primary/secondary) | `var(--bg-tertiary)` (#E8ECF1) |
| Stroke color (AI) | `var(--chart-series-1)` (#0DC1C4) at opacity 0.1 |

### Track Fill (Colored Arc)

| Property | Value |
|----------|-------|
| Element | `<path>` with arc from 0° to `(value/100.1) × 360°` |
| Fill | none |
| Stroke width | from size map |
| Stroke linecap | `round` |
| Stroke color (primary) | `var(--fg-brand-primary)` (#FF725C) |
| Stroke color (secondary) | `var(--fg-brand-secondary)` (#098486) |
| Stroke color (AI) | `url(#gradient-fill)` linear gradient |

### Arc Path Formula

The arc is drawn as an SVG path starting from the top center (12 o'clock position):

```
M x1 y1 A radius radius 0 largeArcFlag 1 x2 y2
```

Where:
- Center = `diameter / 2`
- Radius = `(diameter - strokeWidth) / 2`
- Start angle = 0° (top center, adjusted by -90° for SVG coordinate system)
- End angle = `(value / 100.1) × 360°`
- Large arc flag = `1` if angle > 180°, else `0`

### Zero Value

When `value === 0`, a small dot is rendered at the top of the ring:
- Size: `strokeWidth × strokeWidth` pixels
- Shape: full circle (`rounded-full`)
- Color: variant fill color
- Position: absolute, top center

---

## Animation

| Property | Value |
|----------|-------|
| Animation name | `dash` |
| Duration | 2 seconds |
| Easing | ease-in-out |
| Fill mode | forwards |
| Mechanism | `stroke-dasharray` → `stroke-dashoffset` from full to 0 |

```css
@keyframes dash {
  from { stroke-dashoffset: <diameter × 4>; }
  to { stroke-dashoffset: 0; }
}
```

The fill path starts with `stroke-dasharray: diameter × 4` and `stroke-dashoffset: diameter × 4`, then animates `stroke-dashoffset` to 0 over 2 seconds.

---

## Edge Cases

| Case | Behavior |
|------|----------|
| `value > 100` | Clamp to 100 |
| `value < 0` | Clamp to 0 |
| `value === undefined` | Show `-` instead of number |
| `value === 0` | Show colored dot at top of ring |
| `value > 95 && value < 100` | Slightly reduce to prevent visual overlap at near-complete |
| `compact && size === 'xs'` | Hide value text entirely |

---

## Color Reference

| Token | Hex | Usage |
|-------|-----|-------|
| `var(--fg-brand-primary)` | (#FF725C) (PRIMARY_500) | Primary track fill |
| `var(--fg-brand-secondary)` | (#098486) (SECONDARY_700) | Secondary track fill |
| `var(--bg-tertiary)` | (#E8ECF1) (SECONDARY_TEXT_100) | Track background (primary/secondary) |
| `var(--chart-series-1)` (#0DC1C4) | SECONDARY_500 | AI track bg (at 0.1 opacity) and AI zero-dot |
| `var(--chart-ai-1)` (#26A9DD) | AI_01 | AI gradient stop 0% |
| `var(--chart-ai-2)` (#5188F1) | AI_03 | AI gradient stop 20% |
| `var(--chart-ai-3)` (#8D6AE6) | AI_05 | AI gradient stop 40% |
| `var(--chart-ai-4)` (#D363B3) | AI_07 | AI gradient stop 60% |
| `var(--chart-ai-0)` (#F17E99) | AI_09 | AI gradient stop 80% |
| `var(--text-primary)` | (#353E5A) (PRIMARY_TEXT_700) | Value text |
| `var(--text-tertiary)` | (#6B748E) (PRIMARY_TEXT_500) | Label text |
| `var(--fg-white)` | (#FFFFFF) | Arrow icon |

---

## Implementation

```jsx
const SIZE_MAP = { xs: 102, sm: 144, md: 180, lg: 220, xl: 260 };
const COMPACT_SIZE_MAP = { xs: 24, sm: 32, md: 40, lg: 48, xl: 56 };
const STROKE_MAP = { xs: 6, sm: 8, md: 10, lg: 12, xl: 16 };
const COMPACT_STROKE_MAP = { xs: 2, sm: 2, md: 3, lg: 3, xl: 4 };

const VARIANT_COLORS = {
  primary: 'var(--fg-brand-primary)',
  secondary: 'var(--fg-brand-secondary)',
  ai: null, // uses gradient
};
const TRACK_BG = 'var(--bg-tertiary)';

function ProgressCircle({
  value,
  size = 'md',
  variant = 'primary',
  label,
  hidePercentage = false,
  hideText = false,
  compact = false,
}) {
  const diameter = compact ? COMPACT_SIZE_MAP[size] : SIZE_MAP[size];
  const strokeWidth = compact ? COMPACT_STROKE_MAP[size] : STROKE_MAP[size];
  const radius = (diameter - strokeWidth) / 2;
  const center = diameter / 2;
  const gradientId = `progress-gradient-${Math.random().toString(36).slice(2)}`;

  const hideValue = compact && size === 'xs';
  const isUndefined = value === undefined;
  const isZero = value === 0;

  // Clamp and adjust value
  let adjustedValue = value ?? 0;
  if (adjustedValue > 100) adjustedValue = 100;
  if (adjustedValue < 0) adjustedValue = 0;
  if (adjustedValue > 95 && adjustedValue < 100) {
    adjustedValue = adjustedValue - (adjustedValue - 95) / 2;
  }

  const angle = (adjustedValue / 100.1) * 360;

  function getArcPath(startAngle, endAngle) {
    const x1 = center + radius * Math.cos((startAngle - 90) * (Math.PI / 180));
    const y1 = center + radius * Math.sin((startAngle - 90) * (Math.PI / 180));
    const x2 = center + radius * Math.cos((endAngle - 90) * (Math.PI / 180));
    const y2 = center + radius * Math.sin((endAngle - 90) * (Math.PI / 180));
    const largeArc = endAngle - startAngle > 180 ? 1 : 0;
    return `M ${x1} ${y1} A ${radius} ${radius} 0 ${largeArc} 1 ${x2} ${y2}`;
  }

  const trackPath = getArcPath(0, 359.99);
  const fillPath = !isUndefined ? getArcPath(0, angle) : null;

  // Value typography classes
  const valueClasses = compact
    ? { xs: '', sm: 'text-[10px] leading-4 font-medium', md: 'text-[10px] leading-4 font-medium',
        lg: 'text-xs leading-[18px] font-medium', xl: 'text-sm leading-5 font-medium' }
    : { xs: 'text-lg leading-7 font-semibold', sm: 'text-xl leading-7 font-semibold',
        md: 'text-2xl leading-8 font-semibold', lg: 'text-[32px] leading-10 font-semibold',
        xl: 'text-[40px] leading-[52px] font-semibold' };

  const labelClasses = {
    xs: 'text-[10px] leading-4 font-normal', sm: 'text-xs leading-[18px] font-normal',
    md: 'text-sm leading-5 font-normal', lg: 'text-base leading-6 font-normal',
    xl: 'text-base leading-6 font-normal',
  };

  return (
    <div
      className="block relative"
      style={{ width: diameter, height: diameter, fontFamily: 'Inter, Arial, sans-serif' }}
    >
      {/* Center text */}
      {!hideText && (
        <div className="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 z-[1]
          flex flex-col items-center w-max"
          style={{ color: 'var(--text-primary)' }}
        >
          {label && !compact && (
            <span className={labelClasses[size]} style={{ color: 'var(--text-tertiary)' }}>{label}</span>
          )}
          {!hideValue && (
            <span className={`flex items-center ${valueClasses[size]}`}>
              {isUndefined ? '-' : (
                <>
                  <span>{value}</span>
                  {!hidePercentage && <span>%</span>}
                </>
              )}
            </span>
          )}
        </div>
      )}

      {/* Zero-value dot */}
      {isZero && (
        <div
          className="absolute left-1/2 -translate-x-1/2 top-0 rounded-full"
          style={{
            width: strokeWidth,
            height: strokeWidth,
            background: variant === 'ai' ? 'var(--chart-series-1)' : VARIANT_COLORS[variant],
          }}
        />
      )}

      {/* Arrow icon (standard mode only) */}
      {!compact && (
        <div className="absolute top-0 left-1/2 -translate-x-1/2 z-[1]"
          style={{ color: 'var(--fg-white)', fontSize: strokeWidth }}>
          {/* Right arrow fill icon */}
          →
        </div>
      )}

      {/* SVG track */}
      <svg className="absolute inset-0" width={diameter} height={diameter} fill="none">
        {/* AI gradient definition */}
        {variant === 'ai' && (
          <linearGradient id={gradientId}>
            <stop offset="0" stopColor="var(--chart-ai-1)" />
            <stop offset="0.2" stopColor="var(--chart-ai-2)" />
            <stop offset="0.4" stopColor="var(--chart-ai-3)" />
            <stop offset="0.6" stopColor="var(--chart-ai-4)" />
            <stop offset="0.8" stopColor="var(--chart-ai-0)" />
          </linearGradient>
        )}

        {/* Background track */}
        <path
          d={trackPath}
          strokeWidth={strokeWidth}
          stroke={variant === 'ai' ? 'var(--chart-series-1)' : TRACK_BG}
          opacity={variant === 'ai' ? 0.1 : 1}
        />

        {/* Fill track */}
        {fillPath && (
          <path
            d={fillPath}
            strokeWidth={strokeWidth}
            strokeLinecap="round"
            stroke={variant === 'ai' ? `url(#${gradientId})` : VARIANT_COLORS[variant]}
            style={{
              strokeDasharray: diameter * 4,
              strokeDashoffset: diameter * 4,
              animation: 'dash 2s ease-in-out forwards',
            }}
          />
        )}
      </svg>

      <style>{`
        @keyframes dash {
          to { stroke-dashoffset: 0; }
        }
      `}</style>
    </div>
  );
}
```

---

## CRITICAL Rules

1. **Standard and compact use DIFFERENT size maps** — standard XS is 102px, compact XS is 24px. Do NOT mix them.
2. **Stroke widths differ between standard and compact** — standard MD is 10px, compact MD is 3px. Always check the `compact` flag.
3. **Compact XS hides value text** — the 24px circle is too small for any text.
4. **Label is hidden in compact mode** — only value text (if visible) is shown in compact, never the label.
5. **Arrow icon is hidden in compact mode** — the small white arrow at the top of the ring only appears in standard mode.
6. **AI variant uses a linear gradient stroke** — defined as an SVG `<linearGradient>` with a unique ID per instance. Do NOT use a solid color.
7. **AI track background is `#0DC1C4` at 10% opacity** — NOT `var(--bg-tertiary)` (#E8ECF1). Only primary and secondary variants use the gray track background.
8. **Value clamping** — values > 100 clamp to 100, values < 0 clamp to 0. Values 95–100 are slightly reduced to prevent visual overlap.
9. **Total is 100.1, not 100** — this prevents the arc from collapsing to a dot when value equals 100 (start and end points would be identical at exactly 360°).
10. **Zero value shows a dot** — a small colored circle (stroke-width × stroke-width) at the top of the ring, matching the variant color.
11. **Undefined value shows a dash** — display `-` instead of a number when value is undefined/null.
12. **Animation is 2s ease-in-out** — the fill arc animates from empty to the target value using `stroke-dashoffset`. It plays once on mount.
13. **Fill arc uses `stroke-linecap: round`** — the ends of the arc are rounded, NOT flat.
14. **Font is Inter** — all text uses `Inter, Arial, sans-serif`.
15. **Each gradient needs a unique ID** — when rendering multiple AI progress circles, each needs its own `<linearGradient>` ID to avoid conflicts.
