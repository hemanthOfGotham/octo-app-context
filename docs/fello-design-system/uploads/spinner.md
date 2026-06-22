# Spinner Component — Strict Specification

When building any loading spinner, progress indicator, or loading animation, follow these specifications exactly. Do NOT use default Tailwind spinners, shadcn loaders, or any other spinner system.

> **TOKEN RULE:** Every color in this spec is expressed as a CSS variable token. In Tailwind classes use the pattern `stroke-[var(--token)]`. In inline styles use `'var(--token)'`. In prose and tables, show `var(--token)` (#hex) for reference.

---

## Spinner Anatomy

A spinner is an SVG-based circular loading indicator with two parts:

```
[Track circle — full ring, light gray]
[Fill arc — 120° arc, brand color, rotating]
```

- The track is a near-complete circle (359.99°) in light gray
- The fill is a 120° arc in the brand primary color
- The fill arc continuously rotates around the center
- Both use `stroke-linecap: round` for smooth endpoints

---

## Sizes

There are 5 spinner sizes:

| Size | Dimension | Stroke Width | Tailwind Dimension |
|------|-----------|-------------|-------------------|
| **xs** | 16px × 16px | 1px | `w-4 h-4` |
| **sm** | 26px × 26px | 1.5px | `w-[26px] h-[26px]` |
| **md** (default) | 40px × 40px | 2px | `w-10 h-10` |
| **lg** | 48px × 48px | 2.5px | `w-12 h-12` |
| **xl** | 56px × 56px | 3px | `w-14 h-14` |

---

## Colors

| Element | Color | Hex | Tailwind |
|---------|-------|-----|----------|
| Track (background circle) | bg-tertiary | `var(--bg-tertiary)` (#E8ECF1) | `stroke-[var(--bg-tertiary)]` |
| Fill (animated arc) | fg-brand-primary | `var(--fg-brand-primary)` (#FF725C) | `stroke-[var(--fg-brand-primary)]` |

---

## SVG Structure

The spinner uses SVG `<path>` elements to draw arcs:

| Property | Value |
|----------|-------|
| Track arc | 359.99° (nearly full circle) |
| Fill arc | 120° (one-third of the circle) |
| Stroke linecap | `round` |
| Fill | `none` (transparent — only strokes are visible) |
| Radius | `(size - strokeWidth) / 2` |
| Center | `size / 2` (both cx and cy) |

---

## Animation

| Property | Value |
|----------|-------|
| Type | Continuous rotation (`animateTransform type="rotate"`) |
| Duration | 2 seconds per full rotation |
| Repeat | Infinite |
| Easing | Linear (constant speed) |
| Rotation center | Center of the SVG (`size/2, size/2`) |

---

## Implementation Pattern for Lovable

```jsx
{/* === Spinner — Medium (40px, default) === */}
<svg
  className="w-10 h-10 animate-spin"
  style={{ animationDuration: '2s', animationTimingFunction: 'linear' }}
  viewBox="0 0 40 40"
  fill="none"
>
  {/* Track (background circle) */}
  <circle
    cx="20"
    cy="20"
    r="19"
    stroke="var(--bg-tertiary)"
    strokeWidth="2"
    strokeLinecap="round"
  />
  {/* Fill arc (120° rotating segment) */}
  <path
    d="M20 1 A19 19 0 0 1 36.45 29.5"
    stroke="var(--fg-brand-primary)"
    strokeWidth="2"
    strokeLinecap="round"
  />
</svg>


{/* === Spinner — XS (16px) === */}
<svg
  className="w-4 h-4 animate-spin"
  style={{ animationDuration: '2s', animationTimingFunction: 'linear' }}
  viewBox="0 0 16 16"
  fill="none"
>
  <circle
    cx="8"
    cy="8"
    r="7.5"
    stroke="var(--bg-tertiary)"
    strokeWidth="1"
    strokeLinecap="round"
  />
  <path
    d="M8 0.5 A7.5 7.5 0 0 1 14.5 12"
    stroke="var(--fg-brand-primary)"
    strokeWidth="1"
    strokeLinecap="round"
  />
</svg>


{/* === Spinner — SM (26px) === */}
<svg
  className="w-[26px] h-[26px] animate-spin"
  style={{ animationDuration: '2s', animationTimingFunction: 'linear' }}
  viewBox="0 0 26 26"
  fill="none"
>
  <circle
    cx="13"
    cy="13"
    r="12.25"
    stroke="var(--bg-tertiary)"
    strokeWidth="1.5"
    strokeLinecap="round"
  />
  <path
    d="M13 0.75 A12.25 12.25 0 0 1 23.6 19.125"
    stroke="var(--fg-brand-primary)"
    strokeWidth="1.5"
    strokeLinecap="round"
  />
</svg>


{/* === Spinner — LG (48px) === */}
<svg
  className="w-12 h-12 animate-spin"
  style={{ animationDuration: '2s', animationTimingFunction: 'linear' }}
  viewBox="0 0 48 48"
  fill="none"
>
  <circle
    cx="24"
    cy="24"
    r="22.75"
    stroke="var(--bg-tertiary)"
    strokeWidth="2.5"
    strokeLinecap="round"
  />
  <path
    d="M24 1.25 A22.75 22.75 0 0 1 43.7 35.375"
    stroke="var(--fg-brand-primary)"
    strokeWidth="2.5"
    strokeLinecap="round"
  />
</svg>


{/* === Spinner — XL (56px) === */}
<svg
  className="w-14 h-14 animate-spin"
  style={{ animationDuration: '2s', animationTimingFunction: 'linear' }}
  viewBox="0 0 56 56"
  fill="none"
>
  <circle
    cx="28"
    cy="28"
    r="26.5"
    stroke="var(--bg-tertiary)"
    strokeWidth="3"
    strokeLinecap="round"
  />
  <path
    d="M28 1.5 A26.5 26.5 0 0 1 50.95 41.25"
    stroke="var(--fg-brand-primary)"
    strokeWidth="3"
    strokeLinecap="round"
  />
</svg>


{/* === Reusable Spinner Component Pattern === */}
{(() => {
  const sizes = {
    xs:  { size: 16, stroke: 1 },
    sm:  { size: 26, stroke: 1.5 },
    md:  { size: 40, stroke: 2 },
    lg:  { size: 48, stroke: 2.5 },
    xl:  { size: 56, stroke: 3 },
  };
  const { size, stroke } = sizes['md']; // change size key as needed
  const center = size / 2;
  const radius = (size - stroke) / 2;

  // 120° arc endpoint (using parametric circle equations)
  const endX = center + radius * Math.sin((120 * Math.PI) / 180);
  const endY = center - radius * Math.cos((120 * Math.PI) / 180);

  return (
    <svg
      width={size}
      height={size}
      viewBox={`0 0 ${size} ${size}`}
      fill="none"
      className="animate-spin"
      style={{ animationDuration: '2s', animationTimingFunction: 'linear' }}
    >
      <circle
        cx={center}
        cy={center}
        r={radius}
        stroke="var(--bg-tertiary)"
        strokeWidth={stroke}
        strokeLinecap="round"
      />
      <path
        d={`M${center} ${stroke / 2} A${radius} ${radius} 0 0 1 ${endX.toFixed(2)} ${endY.toFixed(2)}`}
        stroke="var(--fg-brand-primary)"
        strokeWidth={stroke}
        strokeLinecap="round"
      />
    </svg>
  );
})()}
```

### Spinner Size Quick Reference

| Size | Pixels | Stroke | Radius | Center |
|------|--------|--------|--------|--------|
| xs | 16 | 1 | 7.5 | 8 |
| sm | 26 | 1.5 | 12.25 | 13 |
| md | 40 | 2 | 19 | 20 |
| lg | 48 | 2.5 | 22.75 | 24 |
| xl | 56 | 3 | 26.5 | 28 |

### Key Implementation Rules

1. **Spinner uses SVG**, NOT CSS borders or pseudo-elements
2. **Track color is always `var(--bg-tertiary)` (#E8ECF1)**, fill is always `var(--fg-brand-primary)` (#FF725C)
3. **Fill arc is exactly 120°** — one-third of the circle
4. **Rotation is 2 seconds** per full turn, linear easing, infinite repeat
5. **Stroke linecap is `round`** for both track and fill — gives smooth rounded endpoints
6. **Radius = (size - strokeWidth) / 2** — ensures the stroke fits inside the viewBox
7. **Stroke width scales with size**: 1px (xs) → 1.5px (sm) → 2px (md) → 2.5px (lg) → 3px (xl)
8. **SVG fill is `none`** — only strokes are visible, the interior is transparent
9. **Use Tailwind's `animate-spin`** with overridden duration (`2s`) and timing (`linear`)
10. **Spinner is purely presentational** — no ARIA role needed, but can be wrapped in a container with `role="status"` and `aria-label="Loading"` for accessibility
