# Rating Component — Strict Specification

When building any star rating, review rating, or score input, follow these specifications exactly. Do NOT use default Tailwind rating components, shadcn rating systems, or any other star rating system.

> **TOKEN RULE:** Every color in this spec is expressed as a CSS variable token. In Tailwind classes use `bg-[var(--token)]` / `text-[var(--token)]` / `border-[var(--token)]`. In inline styles use `'var(--token)'`. In prose and tables, show `var(--token)` (#hex) for reference.

---

## Rating Anatomy

A horizontal row of star icons, filled from left to right based on the rating value:

```
  ★ ★ ★ ★ ☆     ← 4 out of 5 stars
  filled  unfilled
```

- Stars are SVG icons in a horizontal flex row
- Filled stars show solid yellow/amber color
- Unfilled stars show only the outline (stroke)
- Interactive: hover previews the rating, click selects it
- Readonly mode disables all interactions

---

## Sizes

| Size | Star Dimension | Tailwind |
|------|---------------|----------|
| XS | 16px | `w-4 h-4` |
| MD (Default) | 32px | `w-8 h-8` |

---

## Colors

| State | Property | Color | Hex |
|-------|----------|-------|-----|
| Unfilled | Stroke | border-warning | `var(--border-warning)` (#FFC400) |
| Unfilled | Fill | None | `transparent` |
| Filled | Fill | border-warning | `var(--border-warning)` (#FFC400) |
| Filled | Stroke | border-warning | `var(--border-warning)` (#FFC400) |
| Hover preview | Fill | border-warning | `var(--border-warning)` (#FFC400) |

> **Note:** The star color is amber/gold (`var(--border-warning)` (#FFC400)) — NOT the brand primary orange.

---

## Layout

| Property | Value | Tailwind |
|----------|-------|----------|
| Display | Flex row | `flex` |
| Gap | 4px | `gap-1` |
| Alignment | Center | `items-center` |

---

## Star SVG

Each star is a 5-pointed star SVG with the following path:

```
viewBox="0 0 32 32"
```

| Property | Value |
|----------|-------|
| Stroke | `var(--border-warning)` (#FFC400) |
| Stroke width | 1.5 (scales with viewBox) |
| Fill (unfilled) | `none` |
| Fill (filled) | `var(--border-warning)` (#FFC400) |

---

## States

### Default (Interactive)
- Stars are clickable buttons
- Cursor: pointer
- Hover fills the hovered star and all stars to its left

### Hover Effect
- Uses CSS sibling selector: hovered button and all preceding buttons get filled
- Only active when NOT in readonly mode

### Readonly
- Buttons are disabled
- No hover effects
- Rating is display-only
- No cursor change

---

## Options

| Property | Default | Description |
|----------|---------|-------------|
| size | MD | XS or MD |
| maxRating | 5 | Total number of stars (1–10) |
| rating | 0 | Currently selected value (0 = no selection) |
| readonly | false | Disables all interactions |

---

## Accessibility

| Property | Value |
|----------|-------|
| Each star | `<button type="button">` |
| aria-label | `"{n} out of {max} stars"` |
| Disabled (readonly) | `disabled` attribute on buttons |
| Keyboard | Tab to navigate, Enter/Space to select |

---

## Implementation Pattern for Lovable

```jsx
{/* === Rating — MD, Interactive, 4/5 selected === */}
<div className="flex gap-1 items-center">
  {[1, 2, 3, 4, 5].map((star) => (
    <button
      key={star}
      type="button"
      aria-label={`${star} out of 5 stars`}
      onClick={() => setRating(star)}
      onMouseEnter={() => setHoverRating(star)}
      onMouseLeave={() => setHoverRating(0)}
      className="cursor-pointer focus:outline-none"
    >
      <svg
        width="32"
        height="32"
        viewBox="0 0 32 32"
        xmlns="http://www.w3.org/2000/svg"
      >
        <path
          d="M15.0488 4.54492C15.3482 3.62376 16.6518 3.62376 16.9512 4.54492L18.7803 10.1738C19.048 10.9977 19.8154 11.5555 20.6816 11.5557H26.6006C27.569 11.5558 27.9716 12.7948 27.1885 13.3643L22.4004 16.8438C21.6995 17.353 21.4063 18.2551 21.6738 19.0791L23.5029 24.708C23.8023 25.6293 22.7476 26.3954 21.9639 25.8262L17.1758 22.3477C16.4748 21.8384 15.5252 21.8384 14.8242 22.3477L10.0361 25.8262C9.25244 26.3954 8.19773 25.6293 8.49707 24.708L10.3262 19.0791C10.5937 18.2551 10.3005 17.353 9.59961 16.8438L4.81152 13.3643C4.02845 12.7948 4.43102 11.5558 5.39941 11.5557H11.3184C12.1846 11.5555 12.952 10.9977 13.2197 10.1738L15.0488 4.54492Z"
          stroke="var(--border-warning)"
          strokeWidth="1.5"
          fill={star <= (hoverRating || rating) ? 'var(--border-warning)' : 'none'}
          className="transition-colors duration-150"
        />
      </svg>
    </button>
  ))}
</div>


{/* === Rating — XS, Readonly, 3/5 === */}
<div className="flex gap-1 items-center">
  {[1, 2, 3, 4, 5].map((star) => (
    <svg
      key={star}
      width="16"
      height="16"
      viewBox="0 0 32 32"
      xmlns="http://www.w3.org/2000/svg"
    >
      <path
        d="M15.0488 4.54492C15.3482 3.62376 16.6518 3.62376 16.9512 4.54492L18.7803 10.1738C19.048 10.9977 19.8154 11.5555 20.6816 11.5557H26.6006C27.569 11.5558 27.9716 12.7948 27.1885 13.3643L22.4004 16.8438C21.6995 17.353 21.4063 18.2551 21.6738 19.0791L23.5029 24.708C23.8023 25.6293 22.7476 26.3954 21.9639 25.8262L17.1758 22.3477C16.4748 21.8384 15.5252 21.8384 14.8242 22.3477L10.0361 25.8262C9.25244 26.3954 8.19773 25.6293 8.49707 24.708L10.3262 19.0791C10.5937 18.2551 10.3005 17.353 9.59961 16.8438L4.81152 13.3643C4.02845 12.7948 4.43102 11.5558 5.39941 11.5557H11.3184C12.1846 11.5555 12.952 10.9977 13.2197 10.1738L15.0488 4.54492Z"
        stroke="var(--border-warning)"
        strokeWidth="1.5"
        fill={star <= 3 ? 'var(--border-warning)' : 'none'}
      />
    </svg>
  ))}
</div>


{/* === Rating — MD, 0/5 (empty) === */}
<div className="flex gap-1 items-center">
  {[1, 2, 3, 4, 5].map((star) => (
    <button
      key={star}
      type="button"
      aria-label={`${star} out of 5 stars`}
      onClick={() => setRating(star)}
      className="cursor-pointer focus:outline-none"
    >
      <svg width="32" height="32" viewBox="0 0 32 32" xmlns="http://www.w3.org/2000/svg">
        <path
          d="M15.0488 4.54492C15.3482 3.62376 16.6518 3.62376 16.9512 4.54492L18.7803 10.1738C19.048 10.9977 19.8154 11.5555 20.6816 11.5557H26.6006C27.569 11.5558 27.9716 12.7948 27.1885 13.3643L22.4004 16.8438C21.6995 17.353 21.4063 18.2551 21.6738 19.0791L23.5029 24.708C23.8023 25.6293 22.7476 26.3954 21.9639 25.8262L17.1758 22.3477C16.4748 21.8384 15.5252 21.8384 14.8242 22.3477L10.0361 25.8262C9.25244 26.3954 8.19773 25.6293 8.49707 24.708L10.3262 19.0791C10.5937 18.2551 10.3005 17.353 9.59961 16.8438L4.81152 13.3643C4.02845 12.7948 4.43102 11.5558 5.39941 11.5557H11.3184C12.1846 11.5555 12.952 10.9977 13.2197 10.1738L15.0488 4.54492Z"
          stroke="var(--border-warning)"
          strokeWidth="1.5"
          fill="none"
        />
      </svg>
    </button>
  ))}
</div>
```

### Key Implementation Rules

1. **Star color is `var(--border-warning)` (#FFC400) (amber/gold)** — uses `border-warning` semantic token, NOT brand orange
2. **Two sizes only**: XS (16px) and MD (32px) — no SM/LG/XL variants
3. **SVG viewBox is always `0 0 32 32`** — size controlled by width/height attributes
4. **Stroke width is 1.5** — consistent across both sizes
5. **Filled = stroke + fill both `var(--border-warning)` (#FFC400)**; Unfilled = stroke `var(--border-warning)` (#FFC400), fill `none`
6. **Gap between stars is 4px** (`gap-1`)
7. **Default maxRating is 5** — supports custom values (e.g., 10)
8. **Hover previews rating** — fills hovered star and all stars to its left
9. **Hover requires state**: track `hoverRating` separately, use `hoverRating || rating` for display
10. **Readonly mode: disable buttons**, no hover, display-only (use `<svg>` directly without `<button>`)
11. **Each star is a `<button>` with aria-label** — e.g., "3 out of 5 stars"
12. **No half-star support** — integer ratings only
13. **Rating 0 means no filled stars** — all outlines only
14. **Use the exact SVG path** provided — it creates the precise 5-pointed star shape
15. **Transition on fill color** — 150ms for smooth hover effect
