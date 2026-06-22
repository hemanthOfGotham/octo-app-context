# Slider Component — Strict Specification

When building any range slider, value slider, or draggable track control, follow these specifications exactly. Do NOT use shadcn Slider, Tailwind range inputs, or default HTML `<input type="range">`.

> **TOKEN RULE:** Every color in this spec is expressed as a CSS variable token. In Tailwind classes use `bg-[var(--token)]` / `text-[var(--token)]` / `border-[var(--token)]`. In inline styles use `'var(--token)'`. In prose and tables, show `var(--token)` (#hex) for reference.

---

## Structure

```jsx
<div className="flex flex-col gap-1">
  {/* Label row (optional) */}
  {label && (
    <div className="flex items-center gap-1">
      <p style={{ fontFamily: 'Inter, sans-serif', fontSize: '12px', lineHeight: '18px', fontWeight: 500 }}>
        {label}
      </p>
      {helpText && (
        <InfoIcon style={{ fontSize: '16px', color: 'var(--fg-quaternary)' }} title={helpText} />
      )}
    </div>
  )}

  {/* Slider track row */}
  <div className="flex flex-1 items-center gap-3">
    {/* Text value (left/right alignment) */}
    {textAlignment === 'left' && (
      <p style={{ fontFamily: 'Inter, sans-serif', fontSize: '16px', lineHeight: '24px', fontWeight: 600, minWidth: '40px' }}>
        {textFormatter(value)}
      </p>
    )}

    {/* Track container */}
    <div className="flex-1 relative" style={{ margin: '10px 0' }}>
      {/* Track background */}
      <div
        className="w-full cursor-pointer"
        style={{
          height: '6px',
          borderRadius: '4px',
          backgroundColor: 'var(--bg-tertiary)',    // bg-tertiary
        }}
        onClick={handleTrackClick}
      />

      {/* Track fill */}
      <div
        style={{
          position: 'absolute',
          top: 0,
          left: 0,
          height: '6px',
          borderRadius: '4px',
          width: `${percentage}%`,
          backgroundColor: variantFillColor,
          pointerEvents: 'none',
        }}
      />

      {/* Thumb */}
      {!disabled && (
        <div
          role="slider"
          tabIndex={0}
          aria-valuemin={min}
          aria-valuemax={max}
          aria-valuenow={value}
          style={{
            position: 'absolute',
            top: '50%',
            left: `${percentage}%`,
            transform: 'translate(-50%, -50%)',
            width: '24px',
            height: '24px',
            borderRadius: '9999px',
            background: 'var(--bg-primary)',
            border: `3px solid ${variantBorderColor}`,
            cursor: 'pointer',
          }}
          onMouseDown={startDragging}
          onKeyDown={handleKeyDown}
        />
      )}
    </div>

    {textAlignment === 'right' && (
      <p style={{ fontFamily: 'Inter, sans-serif', fontSize: '16px', lineHeight: '24px', fontWeight: 600, minWidth: '40px', textAlign: 'right' }}>
        {textFormatter(value)}
      </p>
    )}
  </div>
</div>
```

---

## Inputs

| Input | Type | Default | Description |
|-------|------|---------|-------------|
| `min` | number | `0` | Minimum value |
| `max` | number | `100` | Maximum value |
| `step` | number | `1` | Step increment |
| `value` | number | `0` | Current value |
| `variant` | `'primary'` \| `'secondary'` | `'primary'` | Color theme |
| `textAlignment` | `'left'` \| `'right'` \| `'none'` | `'left'` | Value text position |
| `label` | string | — | Optional label above slider |
| `helpText` | string | — | Tooltip on info icon beside label |
| `disabled` | boolean | `false` | Disables slider |
| `textFormatter` | `(value: number) => string` | `String(value)` | Custom format for display value |

---

## Dimensions

| Element | Value |
|---------|-------|
| Track height | 6px |
| Track border-radius | 4px (radius-2) |
| Thumb width/height | 24px |
| Thumb border-radius | full (circle) |
| Thumb border width | 3px |
| Track margin (vertical) | 10px (my-2.5) |
| Text min-width | 40px |
| Gap between text and track | 12px (gap-3) |

---

## Variant Colors

### Primary Variant

| State | Thumb Border | Fill Color |
|-------|-------------|-----------|
| Default | `var(--fg-brand-primary)` (#FF725C) | `#FF8E7D` (fg-brand-primary-alt) |
| Hover | `var(--fg-brand-primary_alt)` (#FF8E7D) | `var(--fg-brand-primary_alt)` (#FF8E7D) |
| Active/Focus | `var(--fg-brand-primary_alt)` (#FF8E7D) + focus ring | `var(--fg-brand-primary_alt)` (#FF8E7D) |

**Primary focus ring:**
```jsx
boxShadow: 'var(--ring-brand-primary)'  // focus-brand-xs
```

### Secondary Variant

| State | Thumb Border | Fill Color |
|-------|-------------|-----------|
| Default | `var(--fg-brand-secondary)` (#098486) | `var(--fg-brand-secondary_alt)` (#0BA5A7) |
| Hover | `var(--fg-brand-secondary_alt)` (#0BA5A7) | `#0BA5A7` |
| Active/Focus | `var(--fg-brand-secondary_alt)` (#0BA5A7) + focus ring | `var(--fg-brand-secondary_alt)` (#0BA5A7) |

**Secondary focus ring:**
```jsx
boxShadow: 'var(--ring-brand-secondary)'  // focus-brand-secondary-xs
```

### Track Background

Always `var(--bg-tertiary)` (#E8ECF1) for the unfilled portion.

---

## Interaction Behavior

### Track Click
Clicking on the track moves the thumb to the clicked position, clamped to the nearest step.

### Thumb Drag
- `mousedown` on thumb starts drag
- `mousemove` on document updates value continuously
- `mouseup` on document ends drag
- Value is clamped to min/max and snapped to step

### Keyboard Navigation
| Key | Action |
|-----|--------|
| Arrow Right | Increment by `step` |
| Arrow Left | Decrement by `step` |

### Step Clamping

```javascript
const clampToStep = (value, step, min, max) => {
  const stepped = Math.round(value / step) * step;
  return Math.min(Math.max(stepped, min), max);
};
```

### Percentage Calculation

```javascript
const getPercentage = (value, min, max) => {
  return ((value - min) / (max - min)) * 100;
};
```

---

## Disabled State

When disabled:
- Thumb is completely removed (not rendered)
- Track remains visible but non-interactive
- No hover effects

---

## Label Typography

| Element | Size | Line Height | Weight |
|---------|------|-------------|--------|
| Label | 12px | 18px | 500 (caption-sm) |
| Value text | 16px | 24px | 600 (title-md) |
| Help icon | 16px | — | — |

Help icon color: `var(--fg-quaternary)` (#9298A9)

---

## CRITICAL Rules

1. **Do NOT use `<input type="range">`** — build a custom slider with div elements, mouse events, and keyboard handlers.
2. **Track height is 6px** — not 4px or 8px.
3. **Thumb is 24px with 3px border** — making the visible circle diameter 24px, inner white area 18px.
4. **Fill color is the `_alt` variant** (`var(--fg-brand-primary_alt)` #FF8E7D for primary, `var(--fg-brand-secondary_alt)` #0BA5A7 for secondary), NOT the base brand color.
5. **Thumb border color is the base brand color** (`var(--fg-brand-primary)` #FF725C for primary, `var(--fg-brand-secondary)` #098486 for secondary) in default state.
6. **Step clamping uses Math.round** — not floor or ceil.
7. **When disabled, the thumb is removed entirely** — don't just grey it out.
8. **Text value uses title-md** (16px/24px/600 weight heading font), NOT paragraph text.
9. **Single-value slider only** — no range/dual-thumb support.
