# Non-Axis Charts (Gauge, Radial) — Strict Specification

When building gauge charts or radial/donut charts, follow these specifications exactly. Both chart types use a radial bar layout — the gauge is a half-circle (180°) and the radial is a full-circle (360°).

> **TOKEN RULE — MANDATORY**
> Never use raw hex color values in implementation code. Every color must reference a CSS variable token (`var(--token-name)`) via Tailwind arbitrary-value syntax (e.g., `text-[var(--text-primary)]`, `bg-[var(--bg-brand-primary)]`). Chart palette arrays (AI gradient colors, series colors) are the only exception — they remain as hex since they are consumed programmatically by SVG fills/strokes. Hex codes appear in this spec only as reference annotations in parentheses. See `design-tokens.md` for the full token list.

**Library:** Use [Recharts](https://recharts.org/) (React charting library) with `RadialBarChart` and `PieChart` components, or build with SVG directly. These charts do NOT have X/Y axes.

---

## Color Palettes

### Gauge AI Colors (RADIAL_CHART_AI_TRACK_COLORS)

Used for gauge chart gradient:

| Index | Hex | Name |
|-------|-----|------|
| 0 | `var(--chart-ai-00)` (#14BACB) | AI 00 |
| 1 | `var(--chart-ai-2)` (#5188F1) | AI 03 |
| 2 | `var(--chart-ai-06)` (#B066CD) | AI 06 |
| 3 | `var(--chart-ai-0)` (#F17E99) | AI 09 |
| 4 | `var(--chart-ai-10)` (#F59593) | AI 10 |

### Radial AI Colors (CHART_AI_TRACK_COLORS)

Used for radial chart series and legend dots:

| Index | Hex | Name |
|-------|-----|------|
| 0 | `var(--chart-ai-0)` (#F17E99) | AI 09 |
| 1 | `var(--chart-ai-1)` (#26A9DD) | AI 01 |
| 2 | `var(--chart-ai-2)` (#5188F1) | AI 03 |
| 3 | `var(--chart-ai-3)` (#8D6AE6) | AI 05 |
| 4 | `var(--chart-ai-4)` (#D363B3) | AI 07 |

### Default Series Colors (CHART_TRACK_COLORS)

Used for non-gradient radial charts:

| Index | Hex | Name |
|-------|-----|------|
| 0 | `var(--fg-brand-primary)` (#FF725C) | Primary 500 |
| 1 | `var(--chart-series-1)` (#0DC1C4) | Secondary 500 |
| 2 | `var(--chart-series-2)` (#6EAEF7) | Blue 500 |
| 3 | `var(--fg-tertiary)` (#6B748E) | Primary Text 500 |
| 4 | `var(--chart-series-4)` (#AB91ED) | Purple 500 |

---

## 1. Gauge Chart (Half-Circle)

A semi-circular gauge showing a single percentage value (0–100).

### Architecture

```
┌─────────────────────────────┐
│         ╭───────╮           │
│       ╱           ╲         │  ← 180° arc (gradient filled)
│     ╱               ╲       │
│    ╱                 ╲      │
│   ─────────────────────     │  ← Flat bottom
│          72%                │  ← Center value (heading-lg)
│        ▲ +5.2%              │  ← Optional badge (projected content)
└─────────────────────────────┘
```

### Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `value` | `number` | Required | Percentage value (0–100) |
| `hollowSize` | `number` | `45` | Center hollow as % of radius |
| `gradient` | `boolean` | `true` | Enable gradient arc |
| `displayValue` | `string` | — | Custom display text (defaults to `value`) |

### Gauge Geometry

| Property | Value |
|----------|-------|
| Arc start angle | -90° (9 o'clock) |
| Arc end angle | +90° (3 o'clock) |
| Total sweep | 180° (half circle) |
| Hollow size | 45% of radius (default) |
| Track stroke width | 100% |
| Track margin | 0 |

### Center Display Value

| Property | Value | Tailwind |
|----------|-------|----------|
| Font size | 32px | `text-[32px]` |
| Line height | 40px | `leading-[40px]` |
| Font weight | 600 | `font-semibold` |
| Color | `var(--text-primary)` (#353E5A) | `text-[var(--text-primary)]` |
| Position | Centered horizontally, at bottom of arc | `absolute bottom-[2px]` |

### Track Background

- The unfilled portion of the arc shows a light track
- Track colors use the same gradient colors at **0.1 opacity**
- Track stroke width: 100%

### Gradient Configuration

When `gradient` is ON (default), the arc fill uses a diagonal gradient with 5 color stops:

| Offset | Color | Opacity |
|--------|-------|---------|
| 0% | `var(--chart-ai-00)` (#14BACB) | 1.0 |
| 20% | `var(--chart-ai-2)` (#5188F1) | 1.0 |
| 40% | `var(--chart-ai-06)` (#B066CD) | 1.0 |
| 60% | `var(--chart-ai-0)` (#F17E99) | 1.0 |
| 80% | `var(--chart-ai-10)` (#F59593) | 1.0 |

### Content Below Value

The gauge supports projected content below the center value — typically a badge showing a trend indicator (e.g., "+5.2%"). This is passed as children.

### Implementation

```jsx
function GaugeChart({ value, hollowSize = 45, gradient = true, displayValue, children }) {
  const GAUGE_AI_COLORS = ['var(--chart-ai-00)', 'var(--chart-ai-2)', 'var(--chart-ai-06)', 'var(--chart-ai-0)', 'var(--chart-ai-10)'];
  const display = displayValue || `${value}%`;

  // SVG-based half-circle gauge
  const size = 200;
  const strokeWidth = size * (1 - hollowSize / 100) / 2;
  const radius = (size - strokeWidth) / 2;
  const circumference = Math.PI * radius; // Half circle
  const filledLength = (value / 100) * circumference;

  return (
    <div className="relative w-full" style={{ paddingBottom: '50%' /* half-circle aspect */ }}>
      <svg
        viewBox={`0 0 ${size} ${size / 2 + strokeWidth / 2}`}
        className="absolute inset-0 w-full h-full"
      >
        <defs>
          {gradient && (
            <linearGradient id="gaugeGrad" x1="0" y1="0" x2="1" y2="1">
              {GAUGE_AI_COLORS.map((color, i) => (
                <stop key={i} offset={`${i * 20}%`} stopColor={color} />
              ))}
            </linearGradient>
          )}
        </defs>

        {/* Track (background arc) */}
        <path
          d={`M ${strokeWidth / 2} ${size / 2} A ${radius} ${radius} 0 0 1 ${size - strokeWidth / 2} ${size / 2}`}
          fill="none"
          stroke={gradient ? GAUGE_AI_COLORS[0] : 'var(--bg-secondary_hover)' /* #E8ECF1 */}
          strokeWidth={strokeWidth}
          strokeLinecap="round"
          opacity={0.1}
        />

        {/* Filled arc */}
        <path
          d={`M ${strokeWidth / 2} ${size / 2} A ${radius} ${radius} 0 0 1 ${size - strokeWidth / 2} ${size / 2}`}
          fill="none"
          stroke={gradient ? 'url(#gaugeGrad)' : 'var(--bg-brand-solid)' /* #FF725C */}
          strokeWidth={strokeWidth}
          strokeLinecap="round"
          strokeDasharray={`${filledLength} ${circumference}`}
        />
      </svg>

      {/* Center value */}
      <div className="absolute bottom-[2px] left-0 right-0 flex flex-col items-center">
        <span className="text-[32px] leading-[40px] font-semibold"
          style={{ color: 'var(--text-primary)' /* #353E5A */, fontFamily: 'Inter, Arial, sans-serif' }}>
          {display}
        </span>
        {children}
      </div>
    </div>
  );
}

{/* Usage */}
<GaugeChart value={72}>
  <span className="text-xs text-green-500">▲ +5.2%</span>
</GaugeChart>
```

---

## 2. Radial Chart (Full-Circle / Donut)

A multi-ring concentric donut chart showing multiple values around a center total. Each series gets its own ring.

### Architecture

```
┌─────────────────────────────────────────────────┐
│                                                 │
│      ╭─── Ring 1 ───╮                           │
│    ╭─── Ring 2 ───╮  │     Legend Panel          │
│  ╭─── Ring 3 ───╮  │  │    ● Label 1    45%     │
│  │    Total      │  │  │    ● Label 2    30%     │
│  │     125       │  │  │    ● Label 3    25%     │
│  ╰───────────────╯  │  │                         │
│    ╰─────────────────╯  │                         │
│      ╰───────────────────╯                         │
│                                                 │
└─────────────────────────────────────────────────┘
```

### Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `series` | `number[]` | Required | Array of values (one per ring) |
| `labels` | `string[]` | `[]` | Labels for each series |
| `totalValue` | `number` | Required | Total value shown in center |
| `gradient` | `boolean` | `false` | Enable gradient ring fills |
| `hollowSize` | `number` | `80` | Center hollow as % of radius |
| `showMarker` | `boolean` | `false` | Show colored markers in tooltip |
| `showLegend` | `boolean` | `true` | Show legend panel |
| `legendPosition` | `'top' \| 'right' \| 'bottom' \| 'left'` | `'right'` | Legend placement |
| `valueInPercentage` | `boolean` | `false` | Append % to legend values |
| `enableTooltip` | `boolean` | `true` | Enable hover tooltips |
| `isNullState` | `boolean` | `false` | Show dashes for no data |
| `compactChart` | `boolean` | `false` | Show single ring (total only) |

### Radial Geometry

| Property | Value |
|----------|-------|
| Arc start angle | 0° (12 o'clock) |
| Arc end angle | 360° (full circle) |
| Total sweep | 360° |
| Hollow size | 80% of radius (default) |
| Track stroke width | 100% |
| Track opacity | 0.1 |
| Ring margin/gap | 6px between rings |

### Center Display

| Property | Value | Tailwind |
|----------|-------|----------|
| Font size | 36px | `text-[36px]` |
| Color | `var(--text-primary)` (#353E5A) | `text-[var(--text-primary)]` |
| Label text | "Total" | — |
| Null state | Shows `-` instead of number | — |

### Track Background

Each ring's unfilled track uses its own series color at **0.1 opacity**, creating a faint colored background track:

- Ring 1 track: `var(--chart-ai-0)` (#F17E99) at 10% opacity
- Ring 2 track: `var(--chart-ai-1)` (#26A9DD) at 10% opacity
- Ring 3 track: `var(--chart-ai-2)` (#5188F1) at 10% opacity
- Ring 4 track: `var(--chart-ai-3)` (#8D6AE6) at 10% opacity
- Ring 5 track: `var(--chart-ai-4)` (#D363B3) at 10% opacity

### Gradient Configuration

When `gradient` is ON:

| Offset | Color | Opacity |
|--------|-------|---------|
| 0% | `var(--chart-ai-0)` (#F17E99) | 1.0 |
| 20% | `var(--chart-ai-1)` (#26A9DD) | 1.0 |
| 40% | `var(--chart-ai-2)` (#5188F1) | 1.0 |
| 60% | `var(--chart-ai-3)` (#8D6AE6) | 1.0 |
| 80% | `var(--chart-ai-4)` (#D363B3) | 1.0 |

### Legend Panel

The legend sits beside (or above/below) the chart and lists each series with a colored dot, label, and value.

**Legend Container:**

| Property | Value | Tailwind |
|----------|-------|----------|
| Layout | Vertical flex column | `flex flex-col` |
| Gap between items | 10px | `gap-2.5` |
| Min width | 120px | `min-w-[120px]` |

**Legend Item:**

| Property | Value | Tailwind |
|----------|-------|----------|
| Layout | Horizontal flex row | `flex items-start` |
| Dot size | 10px circle | `w-2.5 h-2.5 rounded-full` |
| Dot margin-top | 4px (align with text) | `mt-1` |
| Dot margin-right | 6px | `mr-1.5` |
| Label font | 12px, weight 600 | `text-xs font-semibold` |
| Label color | `var(--text-primary)` (#353E5A) | `text-[var(--text-primary)]` |
| Value font | 12px, weight 400 | `text-xs font-normal` |
| Value color | `var(--text-primary)` (#353E5A) | `text-[var(--text-primary)]` |
| Label-value layout | Space between (label left, value right) | `justify-between flex-1` |

### Tooltip

When hovering on a ring:
- Shows the series label and value
- Optional colored marker dot before label
- Supports custom tooltip template via `toolTipTemplate`
- Appends `%` if `valueInPercentage` is true

### Implementation

```jsx
const CHART_AI_TRACK_COLORS = ['var(--chart-ai-0)', 'var(--chart-ai-1)', 'var(--chart-ai-2)', 'var(--chart-ai-3)', 'var(--chart-ai-4)'];

function RadialChart({
  series,
  labels = [],
  totalValue,
  gradient = false,
  hollowSize = 80,
  showLegend = true,
  legendPosition = 'right',
  valueInPercentage = false,
  isNullState = false,
}) {
  const colors = gradient ? CHART_AI_TRACK_COLORS : CHART_AI_TRACK_COLORS;
  const isHorizontal = legendPosition === 'left' || legendPosition === 'right';

  return (
    <div className={`flex ${isHorizontal ? 'flex-row' : 'flex-col'} items-center gap-6`}>
      {/* Chart */}
      {(legendPosition === 'right' || legendPosition === 'bottom') && (
        <div className="relative" style={{ width: 200, height: 200 }}>
          <svg viewBox="0 0 200 200" className="w-full h-full">
            <defs>
              {gradient && (
                <linearGradient id="radialGrad" x1="0" y1="0" x2="1" y2="1">
                  {CHART_AI_TRACK_COLORS.map((color, i) => (
                    <stop key={i} offset={`${i * 20}%`} stopColor={color} />
                  ))}
                </linearGradient>
              )}
            </defs>

            {series.map((value, i) => {
              const ringRadius = 90 - (i * 8); // 6px gap + stroke adjustments
              const strokeW = 8;
              const circ = 2 * Math.PI * ringRadius;
              const filled = (value / 100) * circ;
              return (
                <g key={i}>
                  {/* Track */}
                  <circle
                    cx="100" cy="100" r={ringRadius}
                    fill="none"
                    stroke={colors[i % colors.length]}
                    strokeWidth={strokeW}
                    opacity={0.1}
                  />
                  {/* Filled arc */}
                  <circle
                    cx="100" cy="100" r={ringRadius}
                    fill="none"
                    stroke={gradient ? 'url(#radialGrad)' : colors[i % colors.length]}
                    strokeWidth={strokeW}
                    strokeDasharray={`${filled} ${circ}`}
                    strokeLinecap="round"
                    transform="rotate(-90 100 100)"
                  />
                </g>
              );
            })}
          </svg>

          {/* Center value */}
          <div className="absolute inset-0 flex flex-col items-center justify-center">
            <span className="text-[36px] font-semibold" style={{ color: 'var(--text-primary)' /* #353E5A */ }}>
              {isNullState ? '-' : totalValue}
            </span>
            <span className="text-xs" style={{ color: 'var(--text-tertiary)' /* #6B748E */ }}>Total</span>
          </div>
        </div>
      )}

      {/* Legend */}
      {showLegend && (
        <div className="flex flex-col gap-2.5 min-w-[120px]">
          {labels.map((label, i) => (
            <div key={i} className="flex items-start">
              <div
                className="w-2.5 h-2.5 rounded-full mt-1 mr-1.5 shrink-0"
                style={{ backgroundColor: colors[i % colors.length] }}
              />
              <div className="flex justify-between flex-1 gap-4">
                <span className="text-xs font-semibold" style={{ color: 'var(--text-primary)' /* #353E5A */ }}>
                  {label}
                </span>
                <span className="text-xs font-normal" style={{ color: 'var(--text-primary)' /* #353E5A */ }}>
                  {isNullState ? '-' : series[i]}{valueInPercentage ? '%' : ''}
                </span>
              </div>
            </div>
          ))}
        </div>
      )}
    </div>
  );
}

{/* Usage */}
<RadialChart
  series={[75, 60, 45]}
  labels={['Leads', 'Contacts', 'Deals']}
  totalValue={180}
  gradient={true}
  showLegend={true}
  legendPosition="right"
/>
```

---

## Key Differences Between Gauge and Radial

| Feature | Gauge Chart | Radial Chart |
|---------|------------|--------------|
| Arc sweep | 180° (half circle) | 360° (full circle) |
| Start angle | -90° | 0° |
| Series count | Single value | Multiple rings |
| Hollow size default | 45% | 80% |
| Center value font | 32px / heading-lg | 36px |
| Center label | None (projected content) | "Total" |
| Legend | No (uses projected content) | Yes (side panel with dots) |
| Ring gap | N/A (single ring) | 6px between rings |
| Default gradient | ON | OFF |
| Gradient palette | `RADIAL_CHART_AI_TRACK_COLORS` | `CHART_AI_TRACK_COLORS` |

---

## CRITICAL Rules

1. **Gauge is a HALF circle (180°)** — starts at -90° (left), ends at +90° (right). NOT a full circle, NOT a 270° arc.
2. **Radial is a FULL circle (360°)** — starts at 0° (top), sweeps clockwise. NOT a semi-circle.
3. **Gauge center value is 32px, font-weight 600, color `var(--text-primary)` (#353E5A)** — uses the heading-lg typography. NOT 36px (that's radial).
4. **Radial center value is 36px, font-weight 600, color `var(--text-primary)` (#353E5A)** — NOT 32px (that's gauge).
5. **Track backgrounds use the SAME color as the filled arc but at 0.1 opacity** — NOT a generic gray track.
6. **Radial ring gap is 6px** — each concentric ring has 6px spacing between it and the next ring.
7. **Radial hollow size is 80%** — NOT 45% (that's gauge). The large hollow gives space for the center total.
8. **Gauge gradient uses `RADIAL_CHART_AI_TRACK_COLORS`** (starts with `var(--chart-ai-00)` #14BACB) — NOT `CHART_AI_TRACK_COLORS`.
9. **Radial gradient uses `CHART_AI_TRACK_COLORS`** (starts with `var(--chart-ai-0)` #F17E99) — NOT `RADIAL_CHART_AI_TRACK_COLORS`.
10. **Legend dots are 10px circles** — NOT 8px, NOT 12px. They align with text using `mt-1` (4px top margin).
11. **Legend label is font-weight 600, value is font-weight 400** — both at 12px size, both in `var(--text-primary)` (#353E5A) color.
12. **Legend position defaults to right** — legend panel appears to the right of the chart, vertically centered.
13. **Gauge gradient is ON by default** — radial gradient is OFF by default. Do not mix these defaults.
14. **Null state shows dashes (`-`)** — when `isNullState` is true, the center value and legend values show `-` instead of numbers.
15. **Font is Inter** — center display values and legend text all use `Inter, Arial, sans-serif`.
