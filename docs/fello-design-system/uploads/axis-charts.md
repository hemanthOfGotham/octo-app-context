# Axis Charts (Bar, Area, Line) — Strict Specification

When building bar charts, area charts, or line charts, follow these specifications exactly. All three chart types share common axis configuration, grid styling, and color palettes.

**Library:** Use [Recharts](https://recharts.org/) (React charting library) — NOT ApexCharts, Chart.js, or D3 directly. Recharts provides declarative React components that match these specifications.

> **TOKEN RULE:** Every color in this spec is expressed as a CSS variable token. In Tailwind classes use `bg-[var(--token)]` / `text-[var(--token)]`. In inline styles use `'var(--token)'`. In prose and tables, show `var(--token)` (#hex) for reference. **Exception:** Chart palette arrays (CHART_TRACK_COLORS, CHART_AI_TRACK_COLORS, CHART_COLUMN_FILL_GRADIENT_COLORS) keep raw hex values because they are passed programmatically to SVG/canvas renderers.

---

## Color Palettes

### Default Series Colors (CHART_TRACK_COLORS)

Used for multi-series charts when `gradient` is OFF:

| Index | Hex | Name | Usage |
|-------|-----|------|-------|
| 0 | `var(--chart-series-0)` (#FF725C) | Primary 500 | First series |
| 1 | `var(--chart-series-1)` (#0DC1C4) | Secondary 500 | Second series |
| 2 | `var(--chart-series-2)` (#6EAEF7) | Blue 500 | Third series |
| 3 | `var(--chart-series-3)` (#6B748E) | Primary Text 500 | Fourth series |
| 4 | `var(--chart-series-4)` (#AB91ED) | Purple 500 | Fifth series |

### AI Gradient Colors (CHART_AI_TRACK_COLORS)

Used for gradient-enabled area and line charts:

| Index | Hex | Name |
|-------|-----|------|
| 0 | `var(--chart-ai-0)` (#F17E99) | AI 09 |
| 1 | `var(--chart-ai-1)` (#26A9DD) | AI 01 |
| 2 | `var(--chart-ai-2)` (#5188F1) | AI 03 |
| 3 | `var(--chart-ai-3)` (#8D6AE6) | AI 05 |
| 4 | `var(--chart-ai-4)` (#D363B3) | AI 07 |

### Bar Chart Gradient Fill Colors (CHART_COLUMN_FILL_GRADIENT_COLORS)

Used for bar charts when `gradient` is ON — each series gets a top→bottom gradient pair:

| Index | Top Color (0%) | Bottom Color (67%) | Description |
|-------|---------------|-------------------|-------------|
| 0 | `var(--chart-gradient-0-from)` (#FDE7E3) | `var(--chart-gradient-0-to)` (#FFF9F8) | Primary light gradient |
| 1 | `var(--chart-gradient-1-from)` (#CAFBFB) | `var(--chart-gradient-1-to)` (#F9FFFF) | Secondary light gradient |
| 2 | `var(--chart-gradient-2-from)` (#E7F1FE) | `var(--chart-gradient-2-to)` (#F6FAFF) | Blue light gradient |
| 3 | `var(--chart-gradient-3-from)` (#E2E4EB) | `var(--chart-gradient-3-to)` (#F8F8FA) | Gray light gradient |
| 4 | `var(--chart-gradient-4-from)` (#F2EDFC) | `var(--chart-gradient-4-to)` (#FAFAFF) | Purple light gradient |

### Single-Series Variant Colors

| Variant | Color |
|---------|-------|
| `primary` | `var(--fg-brand-primary)` (#FF725C) |
| `secondary` | `var(--chart-series-1)` (#0DC1C4) |

---

## Shared Configuration (All Axis Charts)

### Typography

| Element | Font | Size | Weight | Color |
|---------|------|------|--------|-------|
| X-axis labels | Inter, Arial, sans-serif | 10px | 500 | `var(--text-tertiary)` (#6B748E) |
| Y-axis labels | Inter, Arial, sans-serif | 10px | 500 | `var(--text-tertiary)` (#6B748E) |
| Legend text | Inter, Arial, sans-serif | 12px | 500 | `var(--text-primary)` (#353E5A) |
| Legend markers | — | — | — | Circle shape, series color |

### Grid

| Property | Value |
|----------|-------|
| X-axis grid lines | Hidden |
| Y-axis grid lines | Visible, dashed (`strokeDasharray="4"`) |
| Grid line color | Use default light gray |
| Padding left | 15px |

### X-Axis

| Property | Value |
|----------|-------|
| Axis line | Visible |
| Tick marks | Hidden |
| Label rotation | 0 (horizontal) |
| Overlapping labels | Auto-hide |
| Crosshair stroke color | `var(--fg-primary)` (#353E5A) |

### Y-Axis

| Property | Value |
|----------|-------|
| Min value | `0` (default) |
| Zero label | Hidden (empty string when value is 0) |
| Nice scale | Optional (configurable) |
| Logarithmic | Optional (configurable) |

### Markers (when enabled)

| Property | Value |
|----------|-------|
| Size | 4px radius |
| Fill color | `var(--bg-primary)` (#FFFFFF) |
| Stroke width | 2px |
| Stroke color | Series color (or `var(--fg-brand-primary)` (#FF725C) in gradient mode) |
| Shape | Circle |
| Hover size offset | +2px |

### Legend (when enabled)

| Property | Value |
|----------|-------|
| Font size | 12px |
| Font weight | 500 |
| Font family | Inter, Arial, sans-serif |
| Label color | `var(--text-primary)` (#353E5A) |
| Marker shape | Circle |
| Show for single series | Yes |

### Tooltip

- Custom tooltip with header and body
- Header shows x-axis label
- Body shows series name, color marker, and value
- Supports custom `tooltipHeaderFormatter` and `tooltipValueFormatter`

---

## 1. Bar Chart

### Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `series` | `{ name: string; data: number[] }[]` | Required | Array of named data series |
| `labels` | `string[]` | `[]` | X-axis category labels |
| `gradient` | `boolean` | `false` | Enable vertical gradient fill |
| `stacked` | `boolean` | `false` | Stack bars on top of each other |
| `horizontal` | `boolean` | `false` | Horizontal bar orientation |
| `showLegend` | `boolean` | `false` | Show legend below chart |
| `variant` | `'primary' \| 'secondary'` | — | Color variant (single series) |
| `showMarker` | `boolean` | `false` | Show data markers |
| `min` | `number` | `0` | Y-axis minimum |
| `logarithmic` | `boolean` | `false` | Logarithmic Y-axis |
| `forceNiceScale` | `boolean` | `false` | Force rounded Y-axis intervals |

### Visual Specifications

| Property | Value |
|----------|-------|
| Bar border radius | 0px (square corners) |
| Bar stroke width | 3px (matches bar color) |
| Stacked horizontal stroke | 1px, transparent |
| Default mode | Grouped (side-by-side) |

### Gradient Fill (Bar)

When `gradient` is enabled, each bar fills with a **vertical linear gradient** (top to bottom):

- **Stop 1** (offset 0%): Lighter color from `CHART_COLUMN_FILL_GRADIENT_COLORS[seriesIndex][0]`
- **Stop 2** (offset 67%): Even lighter color from `CHART_COLUMN_FILL_GRADIENT_COLORS[seriesIndex][1]`

The bar **stroke** (outline) uses the solid color from `CHART_TRACK_COLORS[seriesIndex]`.

### Stacked Mode

- Colors are **reversed** for proper visual layering (last series renders on top)
- Total value shown at top of stacked bar

### Implementation (Recharts)

```jsx
import { BarChart, Bar, XAxis, YAxis, CartesianGrid, Tooltip, Legend, ResponsiveContainer } from 'recharts';

const CHART_TRACK_COLORS = ['var(--chart-series-0)', 'var(--chart-series-1)', 'var(--chart-series-2)', 'var(--chart-series-3)', 'var(--chart-series-4)'];

const GRADIENT_FILLS = [
  ['var(--chart-gradient-0-from)', 'var(--chart-gradient-0-to)'],
  ['var(--chart-gradient-1-from)', 'var(--chart-gradient-1-to)'],
  ['var(--chart-gradient-2-from)', 'var(--chart-gradient-2-to)'],
  ['var(--chart-gradient-3-from)', 'var(--chart-gradient-3-to)'],
  ['var(--chart-gradient-4-from)', 'var(--chart-gradient-4-to)'],
];

function MyBarChart({ series, labels, gradient = false, stacked = false, showLegend = false, variant }) {
  // Transform series data into Recharts format
  const data = labels.map((label, i) => {
    const point = { name: label };
    series.forEach(s => { point[s.name] = s.data[i]; });
    return point;
  });

  const getColor = (index) => {
    if (variant && series.length === 1) {
      return variant === 'primary' ? 'var(--chart-series-0)' : 'var(--chart-series-1)';
    }
    return CHART_TRACK_COLORS[index % CHART_TRACK_COLORS.length];
  };

  return (
    <ResponsiveContainer width="100%" height={350}>
      <BarChart data={data} margin={{ top: 0, right: 0, bottom: 0, left: 15 }}>
        {/* Gradient definitions */}
        {gradient && (
          <defs>
            {series.map((s, i) => (
              <linearGradient key={i} id={`barGrad-${i}`} x1="0" y1="0" x2="0" y2="1">
                <stop offset="0%" stopColor={GRADIENT_FILLS[i % GRADIENT_FILLS.length][0]} />
                <stop offset="67%" stopColor={GRADIENT_FILLS[i % GRADIENT_FILLS.length][1]} />
              </linearGradient>
            ))}
          </defs>
        )}

        <CartesianGrid strokeDasharray="4" vertical={false} />
        <XAxis
          dataKey="name"
          axisLine={true}
          tickLine={false}
          tick={{ fontSize: 10, fontWeight: 500, fill: 'var(--text-tertiary)', fontFamily: 'Inter, Arial, sans-serif' }}
        />
        <YAxis
          axisLine={false}
          tickLine={false}
          tick={{ fontSize: 10, fontWeight: 500, fill: 'var(--text-tertiary)', fontFamily: 'Inter, Arial, sans-serif' }}
          tickFormatter={(v) => v === 0 ? '' : v}
        />
        <Tooltip />
        {showLegend && <Legend iconType="circle" />}

        {series.map((s, i) => (
          <Bar
            key={s.name}
            dataKey={s.name}
            fill={gradient ? `url(#barGrad-${i})` : getColor(i)}
            stroke={getColor(i)}
            strokeWidth={stacked ? 1 : 3}
            stackId={stacked ? 'stack' : undefined}
            radius={0}
          />
        ))}
      </BarChart>
    </ResponsiveContainer>
  );
}
```

---

## 2. Area Chart

### Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `series` | `{ name: string; data: number[] }[]` | Required | Array of named data series |
| `labels` | `string[]` | `[]` | X-axis category labels |
| `gradient` | `boolean` | `false` | Enable gradient fill |
| `showLegend` | `boolean` | `false` | Show legend |
| `variant` | `'primary' \| 'secondary'` | — | Color variant (single series) |
| `showMarker` | `boolean` | `false` | Show data point markers |
| `min` | `number` | `0` | Y-axis minimum |
| `logarithmic` | `boolean` | `false` | Logarithmic Y-axis |
| `forceNiceScale` | `boolean` | `false` | Force nice scale |

### Visual Specifications

| Property | Value |
|----------|-------|
| Stroke curve | Straight (linear interpolation) |
| Stroke width | 2px |
| Fill opacity (no gradient) | Solid with low opacity |

### Gradient Fill (Area)

When `gradient` is enabled:

- **Stroke**: Uses an SVG linear gradient across the AI colors
- **Area fill**: Diagonal gradient from opaque (1.0) to transparent (0.0)
- **AI gradient stops** (applied to stroke):
  - 0% → `var(--chart-ai-0)` (#F17E99)
  - 20% → `var(--chart-ai-1)` (#26A9DD)
  - 40% → `var(--chart-ai-2)` (#5188F1)
  - 60% → `var(--chart-ai-3)` (#8D6AE6)
  - 80% → `var(--chart-ai-4)` (#D363B3)
- **Area fill opacity**: Fades from 0.1 to 0

### Markers (Area)

When `showMarker` is enabled:
- White fill `var(--bg-primary)` (#FFFFFF), 4px size
- 2px stroke in `var(--fg-brand-primary)` (#FF725C) (gradient mode) or series color
- Circle shape

### Implementation (Recharts)

```jsx
import { AreaChart, Area, XAxis, YAxis, CartesianGrid, Tooltip, Legend, ResponsiveContainer } from 'recharts';

const CHART_TRACK_COLORS = ['var(--chart-series-0)', 'var(--chart-series-1)', 'var(--chart-series-2)', 'var(--chart-series-3)', 'var(--chart-series-4)'];
const CHART_AI_TRACK_COLORS = ['var(--chart-ai-0)', 'var(--chart-ai-1)', 'var(--chart-ai-2)', 'var(--chart-ai-3)', 'var(--chart-ai-4)'];

function MyAreaChart({ series, labels, gradient = false, showLegend = false, showMarker = false, variant }) {
  const data = labels.map((label, i) => {
    const point = { name: label };
    series.forEach(s => { point[s.name] = s.data[i]; });
    return point;
  });

  const getColor = (index) => {
    if (variant && series.length === 1) {
      return variant === 'primary' ? 'var(--chart-series-0)' : 'var(--chart-series-1)';
    }
    return CHART_TRACK_COLORS[index % CHART_TRACK_COLORS.length];
  };

  return (
    <ResponsiveContainer width="100%" height={350}>
      <AreaChart data={data} margin={{ top: 0, right: 0, bottom: 0, left: 15 }}>
        <defs>
          {gradient && (
            <linearGradient id="areaStrokeGrad" x1="0" y1="0" x2="1" y2="1">
              <stop offset="0%" stopColor="var(--chart-ai-0)" />
              <stop offset="20%" stopColor="var(--chart-ai-1)" />
              <stop offset="40%" stopColor="var(--chart-ai-2)" />
              <stop offset="60%" stopColor="var(--chart-ai-3)" />
              <stop offset="80%" stopColor="var(--chart-ai-4)" />
            </linearGradient>
          )}
          {series.map((s, i) => (
            <linearGradient key={i} id={`areaFill-${i}`} x1="0" y1="0" x2="0" y2="1">
              <stop offset="0%" stopColor={gradient ? CHART_AI_TRACK_COLORS[i % 5] : getColor(i)} stopOpacity={0.1} />
              <stop offset="100%" stopColor={gradient ? CHART_AI_TRACK_COLORS[i % 5] : getColor(i)} stopOpacity={0} />
            </linearGradient>
          ))}
        </defs>

        <CartesianGrid strokeDasharray="4" vertical={false} />
        <XAxis
          dataKey="name"
          axisLine={true}
          tickLine={false}
          tick={{ fontSize: 10, fontWeight: 500, fill: 'var(--text-tertiary)', fontFamily: 'Inter, Arial, sans-serif' }}
        />
        <YAxis
          axisLine={false}
          tickLine={false}
          tick={{ fontSize: 10, fontWeight: 500, fill: 'var(--text-tertiary)', fontFamily: 'Inter, Arial, sans-serif' }}
          tickFormatter={(v) => v === 0 ? '' : v}
        />
        <Tooltip />
        {showLegend && <Legend iconType="circle" />}

        {series.map((s, i) => (
          <Area
            key={s.name}
            type="linear"
            dataKey={s.name}
            stroke={gradient ? 'url(#areaStrokeGrad)' : getColor(i)}
            strokeWidth={2}
            fill={`url(#areaFill-${i})`}
            dot={showMarker ? {
              fill: 'var(--bg-primary)',
              stroke: gradient ? 'var(--chart-series-0)' : getColor(i),
              strokeWidth: 2,
              r: 4
            } : false}
            activeDot={showMarker ? { r: 6 } : undefined}
          />
        ))}
      </AreaChart>
    </ResponsiveContainer>
  );
}
```

---

## 3. Line Chart

### Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `series` | `{ name: string; data: number[] }[]` | Required | Array of named data series |
| `labels` | `string[]` | `[]` | X-axis category labels |
| `gradient` | `boolean` | `false` | Enable gradient stroke |
| `showLegend` | `boolean` | `false` | Show legend |
| `variant` | `'primary' \| 'secondary'` | — | Color variant (single series) |
| `showMarker` | `boolean` | `false` | Show data point markers |
| `min` | `number` | `0` | Y-axis minimum |
| `logarithmic` | `boolean` | `false` | Logarithmic Y-axis |
| `forceNiceScale` | `boolean` | `false` | Force nice scale |

### Visual Specifications

| Property | Value |
|----------|-------|
| Stroke curve | Straight (linear) |
| Stroke width | 2px |
| Fill | None (line only, no area fill) |

### Gradient Stroke (Line)

When `gradient` is enabled:
- Line stroke uses gradient from `CHART_TRACK_COLORS` with 0.7 opacity
- Gradient stops at: 0%, 20%, 40%, 60%, 80%

### Markers (Line)

Same as Area chart markers — white fill, 2px stroke, 4px circle.

### Implementation (Recharts)

```jsx
import { LineChart, Line, XAxis, YAxis, CartesianGrid, Tooltip, Legend, ResponsiveContainer } from 'recharts';

const CHART_TRACK_COLORS = ['var(--chart-series-0)', 'var(--chart-series-1)', 'var(--chart-series-2)', 'var(--chart-series-3)', 'var(--chart-series-4)'];

function MyLineChart({ series, labels, gradient = false, showLegend = false, showMarker = false, variant }) {
  const data = labels.map((label, i) => {
    const point = { name: label };
    series.forEach(s => { point[s.name] = s.data[i]; });
    return point;
  });

  const getColor = (index) => {
    if (variant && series.length === 1) {
      return variant === 'primary' ? 'var(--chart-series-0)' : 'var(--chart-series-1)';
    }
    return CHART_TRACK_COLORS[index % CHART_TRACK_COLORS.length];
  };

  return (
    <ResponsiveContainer width="100%" height={350}>
      <LineChart data={data} margin={{ top: 0, right: 0, bottom: 0, left: 15 }}>
        {gradient && (
          <defs>
            <linearGradient id="lineStrokeGrad" x1="0" y1="0" x2="1" y2="1">
              <stop offset="0%" stopColor="var(--chart-series-0)" stopOpacity={0.7} />
              <stop offset="20%" stopColor="var(--chart-series-1)" stopOpacity={0.7} />
              <stop offset="40%" stopColor="var(--chart-series-2)" stopOpacity={0.7} />
              <stop offset="60%" stopColor="var(--chart-series-3)" stopOpacity={0.7} />
              <stop offset="80%" stopColor="var(--chart-series-4)" stopOpacity={0.7} />
            </linearGradient>
          </defs>
        )}

        <CartesianGrid strokeDasharray="4" vertical={false} />
        <XAxis
          dataKey="name"
          axisLine={true}
          tickLine={false}
          tick={{ fontSize: 10, fontWeight: 500, fill: 'var(--text-tertiary)', fontFamily: 'Inter, Arial, sans-serif' }}
        />
        <YAxis
          axisLine={false}
          tickLine={false}
          tick={{ fontSize: 10, fontWeight: 500, fill: 'var(--text-tertiary)', fontFamily: 'Inter, Arial, sans-serif' }}
          tickFormatter={(v) => v === 0 ? '' : v}
        />
        <Tooltip />
        {showLegend && <Legend iconType="circle" />}

        {series.map((s, i) => (
          <Line
            key={s.name}
            type="linear"
            dataKey={s.name}
            stroke={gradient ? 'url(#lineStrokeGrad)' : getColor(i)}
            strokeWidth={2}
            dot={showMarker ? {
              fill: 'var(--bg-primary)',
              stroke: gradient ? 'var(--chart-series-0)' : getColor(i),
              strokeWidth: 2,
              r: 4
            } : false}
            activeDot={showMarker ? { r: 6 } : undefined}
          />
        ))}
      </LineChart>
    </ResponsiveContainer>
  );
}
```

---

## Key Differences Between Chart Types

| Feature | Bar Chart | Area Chart | Line Chart |
|---------|-----------|------------|------------|
| Fill | Solid or vertical gradient | Opacity-faded area fill | No fill (line only) |
| Gradient colors | `CHART_COLUMN_FILL_GRADIENT_COLORS` (pastel pairs) | `CHART_AI_TRACK_COLORS` (vibrant) | `CHART_TRACK_COLORS` at 0.7 opacity |
| Default stroke width | 3px | 2px | 2px |
| Stacked mode | Yes | No | No |
| Horizontal mode | Yes | No | No |
| Curve type | N/A | Straight (linear) | Straight (linear) |
| Bar border radius | 0px (square) | N/A | N/A |

---

## Data Format

All axis charts accept the same series format:

```typescript
type Series = {
  name: string;    // Series display name (used in legend and tooltip)
  data: number[];  // Array of numeric values, one per label
}[];

// Example:
const series = [
  { name: 'Revenue', data: [30, 40, 45, 50, 49, 60, 70, 91] },
  { name: 'Expenses', data: [20, 30, 35, 40, 39, 50, 55, 70] },
];
const labels = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug'];
```

---

## CRITICAL Rules

1. **Curve type is ALWAYS straight** (`type="linear"` in Recharts) — never smooth, monotone, or natural. Both area and line charts use straight line segments.
2. **Y-axis zero label is hidden** — when the value is 0, the formatter returns an empty string.
3. **Vertical grid lines are ALWAYS hidden** — only horizontal (Y-axis) grid lines are shown, and they are dashed (`strokeDasharray="4"`).
4. **X-axis tick marks are hidden** — only the axis line is shown, no ticks.
5. **Bar corners are square** — border radius is 0px, NOT rounded.
6. **Font is Inter** — all axis labels, legends, and tooltips use `Inter, Arial, sans-serif` at weight 500.
7. **Axis label color is `var(--text-tertiary)` (#6B748E)** — NOT `var(--text-primary)` (#353E5A), NOT black.
8. **Legend label color is `var(--text-primary)` (#353E5A)** — different from axis labels.
9. **Marker fill is `var(--bg-primary)` (#FFFFFF)** with a 2px colored stroke — NOT solid colored circles.
10. **Bar chart gradient is a VERTICAL fill gradient** using pastel colors (`CHART_COLUMN_FILL_GRADIENT_COLORS`) — NOT the AI track colors.
11. **Area chart gradient uses AI track colors** (`CHART_AI_TRACK_COLORS`) for the stroke, with the area fill fading from 0.1 opacity to 0.
12. **Series colors cycle through the 5-color palette** — if there are more than 5 series, colors repeat from index 0.
13. **Single-series variant overrides palette** — when `variant` is set and there is exactly 1 series, use `var(--fg-brand-primary)` (#FF725C) (primary) or `#0DC1C4` (secondary) instead of the palette.
14. **Stacked bar colors are reversed** — the array order is reversed so the last series appears on top with the first color.
15. **Charts MUST be responsive** — always wrap in a responsive container that fills available width.
