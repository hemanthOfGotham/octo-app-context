# Animated Border Component — Strict Specification

When building any animated gradient border, AI-themed border animation, or rotating gradient outline, follow these specifications exactly. This uses an SVG mask + conic-gradient + requestAnimationFrame approach.

> **TOKEN RULE:** Every colour in this spec is expressed as a CSS variable token. When writing Tailwind classes use the `bg-[var(--token)]` / `text-[var(--token)]` / `border-[var(--token)]` syntax. In inline styles use `'var(--token)'`. In prose and tables the original hex is kept only as a parenthetical annotation, e.g. `var(--ai-00)` (#14BACB).

---

## Architecture

```
Animated Border Wrapper
├── Child content (rendered normally)
├── SVG Container (positioned behind content)
│   ├── <defs> → <mask> with animated dash path
│   └── <foreignObject> masked by the path
│       └── Conic gradient div (the visible color)
└── Shadow layer (blurred gradient glow behind)
```

---

## Usage

The animated border conditionally wraps child content:

```jsx
{isAnimating ? (
  <div style={{ position: 'relative', display: 'block', zIndex: 0, pointerEvents: 'none' }}>
    {children}
    <SVGBorderAnimation />
    <ShadowLayer />
  </div>
) : (
  children
)}
```

When `isAnimating` is `false`, the child content renders directly with no wrapper.

---

## SVG Border Animation Structure

```jsx
<div
  style={{
    position: 'absolute',
    inset: '-1px',              // extends 1px beyond host on all sides
    zIndex: -1,
  }}
>
  <svg height="100%" width="100%" viewBox={`0 0 ${width} ${height}`} style={{ overflow: 'visible' }}>
    <defs>
      <mask id={uniqueId}>
        <path
          ref={pathRef}
          fill="none"
          stroke="white"
          d={roundedRectPathData}
          strokeWidth={1.5}
        />
      </mask>
    </defs>

    <foreignObject
      height="100%"
      width="100%"
      x="0"
      y="0"
      mask={`url(#${uniqueId})`}
    >
      <div style={{
        width: '100%',
        height: '100%',
        background: conicGradient,
        transform: 'rotateY(180deg)',     // mirrors horizontally
      }} />
    </foreignObject>
  </svg>
</div>
```

---

## AI Color Palette

The animated border uses the AI gradient color palette:

| Token | Hex | Description |
|-------|-----|-------------|
| `var(--ai-00)` | (#14BACB) | Cyan/teal |
| `var(--ai-01)` | (#26A9DD) | Sky blue |
| `var(--ai-02)` | (#3898F0) | Blue |
| `var(--ai-03)` | (#5188F1) | Medium blue |
| `var(--ai-04)` | (#6F79EC) | Blue-purple |
| `var(--ai-05)` | (#8D6AE6) | Purple |
| `var(--ai-06)` | (#B066CD) | Magenta-purple |
| `var(--ai-07)` | (#D363B3) | Magenta-pink |
| `var(--ai-08)` | (#EC67A0) | Pink |
| `var(--ai-09)` | (#F17E99) | Salmon pink |
| `var(--ai-10)` | (#F59593) | Peach/coral |

---

## Conic Gradient (Border Color)

```css
background: conic-gradient(
  from 0deg,
  var(--ai-00) 8.79%,     /* (#14BACB) */
  var(--ai-03) 17.52%,    /* (#5188F1) */
  var(--ai-06) 41.25%,    /* (#B066CD) */
  var(--ai-09) 74.98%,    /* (#F17E99) */
  var(--ai-10) 80.71%,    /* (#F59593) */
  var(--ai-00) 95.99%     /* (#14BACB) — seamless loop */
);
transform: rotateY(180deg);
```

The `rotateY(180deg)` mirrors the gradient horizontally for the correct visual effect combined with the counter-clockwise animation.

---

## Shadow Layer

```jsx
<div
  style={{
    borderRadius: '10px',           // radius-5
    opacity: 0.2,
    background: 'linear-gradient(43.33deg, var(--ai-00) 0%, var(--ai-02) 25%, var(--ai-05) 50%, var(--ai-08) 75%, var(--ai-10) 100%)',
    filter: 'blur(4px)',
    position: 'absolute',
    inset: 0,
    zIndex: -1,
  }}
/>
```

The shadow gradient (`bg-ai-solid-asc`) uses stops at `var(--ai-00)`, `var(--ai-02)`, `var(--ai-05)`, `var(--ai-08)`, `var(--ai-10)`.

---

## Animation Logic

### Initialization
1. Measure the container's `clientWidth` and `clientHeight`.
2. Calculate perimeter: `p = (width + height) * 2`.
3. Set animation duration based on size:
   - Perimeter > 1500px → **2 seconds** (faster for large containers)
   - Perimeter ≤ 1500px → **3 seconds** (default)
4. Generate the rounded-rect SVG path.
5. Measure actual path length via `getTotalLength()`.
6. Calculate visible segment: `segmentLength = Math.min(200, pathLength / 4)`.
7. Set `strokeDasharray = '{segmentLength} {gapLength}'`.

### Animation Loop (requestAnimationFrame)

```javascript
function animate(timestamp) {
  if (startTime === null) startTime = timestamp;

  const elapsed = timestamp - startTime;
  const durationMs = animationDuration * 1000;  // 3000 or 2000
  const progress = (elapsed / durationMs) % 1;  // 0 to 1, loops
  const offset = pathLength * progress;

  // Counter-clockwise: negative offset
  pathElement.style.strokeDashoffset = `-${offset}`;

  requestAnimationFrame(animate);
}
```

### Key Properties

| Property | Value |
|----------|-------|
| Corner radius | 8px |
| Stroke width | 1.5px |
| Shadow border-radius | 10px (radius-5) |
| Shadow opacity | 0.2 |
| Shadow blur | 4px |
| SVG container inset | -1px (extends beyond host) |
| Visible segment length | min(200, pathLength / 4) |
| Rotation direction | Counter-clockwise |

---

## Duration by Container Size

| Perimeter | Duration | Example Container |
|-----------|----------|-------------------|
| ≤ 1500px | 3s | 300×300 (p=1200) |
| > 1500px | 2s | 400×400 (p=1600) |

Perimeter formula: `(clientWidth + clientHeight) × 2`

---

## Rounded Rect Path Generation

The SVG path is generated dynamically based on container dimensions:

```javascript
function generateRoundedRectPath(w, h, r, sw) {
  const inset = sw / 2;        // keep stroke inside viewbox
  const innerW = w - sw;
  const innerH = h - sw;
  r = Math.min(r, innerW / 2, innerH / 2);  // clamp radius

  // Generate M, H, A, V, A, H, A, V, A, Z path commands
  // Starting from top-left (after first corner arc)
  // Going: right → down → left → up (clockwise path)
}
```

---

## Resize Handling

A `ResizeObserver` watches the container. On any size change:
1. Re-measures dimensions.
2. Re-generates the SVG path.
3. Re-calculates path length and dash properties.
4. Restarts the animation.

---

## Inputs

| Input | Type | Default | Description |
|-------|------|---------|-------------|
| `isAnimating` | boolean | `true` | When false, renders content without animation wrapper |

---

## CRITICAL Rules

1. **Uses SVG mask + foreignObject + conic-gradient** — NOT CSS border-image or canvas. The animated dash path in the mask reveals portions of the conic gradient.
2. **Rotation is counter-clockwise** — achieved by using negative `strokeDashoffset` values.
3. **Corner radius is 8px** for the SVG path, shadow border-radius is 10px (radius-5).
4. **Stroke width is 1.5px** — NOT 1px or 2px.
5. **Duration switches at perimeter > 1500px** — 3s default, 2s for large containers.
6. **Visible segment is ~25% of path length**, capped at 200px max.
7. **Shadow uses 0.2 opacity with 4px blur** — subtle glow, not heavy.
8. **The SVG container extends 1px beyond the host** (`inset: -1px`) — this prevents border gaps at edges.
9. **When `isAnimating` is false**, the wrapper is completely removed — children render directly without any extra DOM.
10. **Each instance gets a unique ID** (`fello-animated-border-{n}`) for the SVG mask reference — prevents conflicts with multiple animated borders on the same page.
11. **The conic gradient uses `transform: rotateY(180deg)`** — this horizontal mirror is essential for the correct visual direction.
