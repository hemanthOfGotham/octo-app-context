# Scroll Shadow Component — Strict Specification

When building any scrollable container that needs visual scroll indicators, gradient edge shadows, or overflow hints, follow these specifications exactly. Do NOT use default Tailwind scroll shadows or any other scroll indicator system.

> **TOKEN RULE:** Every colour in this spec is expressed as a CSS variable token or Tailwind palette name. When writing Tailwind classes use the `bg-[var(--token)]` / `text-[var(--token)]` / `border-[var(--token)]` syntax. In inline styles use `'var(--token)'`. In prose and tables the original hex is kept only as a parenthetical annotation, e.g. `var(--fg-quaternary)` (#9298A9).

---

## Scroll Shadow Anatomy

Gradient shadow overlays that appear at the edges of a scrollable container to indicate more content is available:

```
     ┌─ top shadow (when scrolled down) ─┐
     ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓
     ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
   ┌─────────────────────────────────────┐
   │                                     │
   │        Scrollable Content           │
   │                                     │
   └─────────────────────────────────────┘
     ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
     ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓
     └─ bottom shadow (when not at end) ─┘
```

- Shadows appear only when content is scrollable in that direction
- Shadows fade in/out with animation
- Supports all four directions: top, bottom, left, right
- Non-interactive (pointer-events: none)

---

## Shadow Bar Dimensions

| Direction | Width | Height |
|-----------|-------|--------|
| Top | 100% | 12px |
| Bottom | 100% | 12px |
| Left | 12px | 100% |
| Right | 12px | 100% |

---

## Shadow Styling

| Property | Value | Tailwind |
|----------|-------|----------|
| Position | Absolute | `absolute` |
| Z-index | 10 | `z-10` |
| Opacity | 10% | `opacity-10` |
| Pointer events | None | `pointer-events-none` |
| Display | Block | `block` |

---

## Shadow Positions

| Direction | Position | Tailwind |
|-----------|----------|----------|
| Top | top: 0, left: 0 | `top-0 left-0` |
| Bottom | bottom: 0, left: 0 | `bottom-0 left-0` |
| Left | top: 0, left: 0 | `top-0 left-0` |
| Right | top: 0, right: 0 | `top-0 right-0` |

---

## Gradient Definitions

All gradients use the same color stops:
- **0%**: `black` (#000000 — standard CSS)
- **40%**: `var(--fg-quaternary)` (#9298A9)
- **100%**: `transparent`

### Per Direction

| Direction | Gradient Angle | CSS |
|-----------|---------------|-----|
| Top | 180deg (top → bottom) | `linear-gradient(180deg, black 0%, var(--fg-quaternary) 40%, transparent 100%)` |
| Bottom | 0deg (bottom → top) | `linear-gradient(0deg, black 0%, var(--fg-quaternary) 40%, transparent 100%)` |
| Left | 90deg (left → right) | `linear-gradient(90deg, black 0%, var(--fg-quaternary) 40%, transparent 100%)` |
| Right | 270deg (right → left) | `linear-gradient(270deg, black 0%, var(--fg-quaternary) 40%, transparent 100%)` |

---

## Animation

| Property | Value |
|----------|-------|
| Enter duration | 300ms |
| Enter easing | ease-in |
| Enter: opacity | 0 → 10% |
| Exit duration | 300ms |
| Exit easing | ease-out |
| Exit: opacity | 10% → 0 |

---

## Visibility Logic

| Condition | Shadow Shown |
|-----------|-------------|
| Scroll offset > 1px from top | Top shadow visible |
| Scroll offset > 1px from bottom | Bottom shadow visible |
| Scroll offset > 1px from left | Left shadow visible |
| Scroll offset > 1px from right | Right shadow visible |
| At edge (offset ≤ 0) | Shadow hidden |

---

## Configuration

| Property | Default | Description |
|----------|---------|-------------|
| shadowPositions | `['top', 'bottom']` | Which edges to show shadows |

Supported positions: `'top'`, `'bottom'`, `'left'`, `'right'` — any combination.

---

## Implementation Pattern for Lovable

```jsx
{/* === Scroll Shadow — Vertical (Top + Bottom) === */}
<div className="relative overflow-hidden rounded-lg h-[400px]">
  {/* Top shadow — shown when scrolled down */}
  {showTopShadow && (
    <div
      className="
        absolute top-0 left-0 w-full h-3
        z-10 pointer-events-none opacity-10
        animate-in fade-in duration-300 ease-in
      "
      style={{
        background: 'linear-gradient(180deg, black 0%, var(--fg-quaternary) 40%, transparent 100%)'
      }}
    />
  )}

  {/* Scrollable content */}
  <div
    className="overflow-auto h-full"
    onScroll={(e) => {
      const el = e.currentTarget;
      setShowTopShadow(el.scrollTop > 1);
      setShowBottomShadow(
        el.scrollTop < el.scrollHeight - el.clientHeight - 1
      );
    }}
  >
    {/* ... content ... */}
  </div>

  {/* Bottom shadow — shown when more content below */}
  {showBottomShadow && (
    <div
      className="
        absolute bottom-0 left-0 w-full h-3
        z-10 pointer-events-none opacity-10
        animate-in fade-in duration-300 ease-in
      "
      style={{
        background: 'linear-gradient(0deg, black 0%, var(--fg-quaternary) 40%, transparent 100%)'
      }}
    />
  )}
</div>


{/* === Scroll Shadow — Horizontal (Left + Right) === */}
<div className="relative overflow-hidden rounded-lg w-[500px]">
  {/* Left shadow */}
  {showLeftShadow && (
    <div
      className="
        absolute top-0 left-0 w-3 h-full
        z-10 pointer-events-none opacity-10
      "
      style={{
        background: 'linear-gradient(90deg, black 0%, var(--fg-quaternary) 40%, transparent 100%)'
      }}
    />
  )}

  {/* Scrollable content */}
  <div
    className="overflow-x-auto h-full flex"
    onScroll={(e) => {
      const el = e.currentTarget;
      setShowLeftShadow(el.scrollLeft > 1);
      setShowRightShadow(
        el.scrollLeft < el.scrollWidth - el.clientWidth - 1
      );
    }}
  >
    {/* ... horizontal content ... */}
  </div>

  {/* Right shadow */}
  {showRightShadow && (
    <div
      className="
        absolute top-0 right-0 w-3 h-full
        z-10 pointer-events-none opacity-10
      "
      style={{
        background: 'linear-gradient(270deg, black 0%, var(--fg-quaternary) 40%, transparent 100%)'
      }}
    />
  )}
</div>


{/* === Scroll Shadow — All Four Directions === */}
<div className="relative overflow-hidden rounded-lg w-[400px] h-[300px]">
  {showTopShadow && (
    <div className="absolute top-0 left-0 w-full h-3 z-10 pointer-events-none opacity-10"
      style={{ background: 'linear-gradient(180deg, black 0%, var(--fg-quaternary) 40%, transparent 100%)' }}
    />
  )}
  {showBottomShadow && (
    <div className="absolute bottom-0 left-0 w-full h-3 z-10 pointer-events-none opacity-10"
      style={{ background: 'linear-gradient(0deg, black 0%, var(--fg-quaternary) 40%, transparent 100%)' }}
    />
  )}
  {showLeftShadow && (
    <div className="absolute top-0 left-0 w-3 h-full z-10 pointer-events-none opacity-10"
      style={{ background: 'linear-gradient(90deg, black 0%, var(--fg-quaternary) 40%, transparent 100%)' }}
    />
  )}
  {showRightShadow && (
    <div className="absolute top-0 right-0 w-3 h-full z-10 pointer-events-none opacity-10"
      style={{ background: 'linear-gradient(270deg, black 0%, var(--fg-quaternary) 40%, transparent 100%)' }}
    />
  )}

  <div className="overflow-auto h-full w-full" onScroll={handleScroll}>
    {/* ... content that overflows both directions ... */}
  </div>
</div>
```

### Key Implementation Rules

1. **Shadow thickness is 12px** (`h-3` for horizontal, `w-3` for vertical) — always
2. **Base opacity is 10%** (`opacity-10`) — shadows are subtle, not heavy
3. **Gradient goes from black → gray → transparent** — creates a natural edge fade
4. **Gradient mid-stop is at 40%** using `var(--fg-quaternary)` (#9298A9)
5. **Pointer events are `none`** — shadows never interfere with scrolling or clicking
6. **Z-index is 10** — sits above content but below modals/popups
7. **Shadow appears when scroll offset > 1px** — tiny threshold to avoid false triggers
8. **Shadow disappears at edge** — when fully scrolled to that direction
9. **Container must be `position: relative`** with `overflow: hidden` for the shadows to clip correctly
10. **Animation fades in over 300ms** with ease-in, fades out with ease-out
11. **Default positions are `['top', 'bottom']`** — vertical scroll is most common
12. **Use `onScroll` handler** in React to track scroll position and toggle shadow visibility
13. **Shadows ONLY appear when content actually overflows.** If content fits without scrolling, NO shadows should be visible. Use a `useEffect` + `useRef` to check overflow on mount and after data changes:
```jsx
const scrollRef = useRef(null);
const checkOverflow = useCallback(() => {
  const el = scrollRef.current;
  if (!el) return;
  setShowTopShadow(el.scrollTop > 1);
  setShowBottomShadow(el.scrollHeight > el.clientHeight && el.scrollTop < el.scrollHeight - el.clientHeight - 1);
  setShowLeftShadow(el.scrollLeft > 1);
  setShowRightShadow(el.scrollWidth > el.clientWidth && el.scrollLeft < el.scrollWidth - el.clientWidth - 1);
}, []);

// Check on mount and whenever data/content changes
useEffect(() => { checkOverflow(); }, [data, checkOverflow]);

// Also check on scroll
<div ref={scrollRef} onScroll={checkOverflow} className="overflow-auto h-full">
```
**Key:** The bottom/right shadows must check `scrollHeight > clientHeight` / `scrollWidth > clientWidth` BEFORE checking scroll offset. If content doesn't overflow, the shadow must NOT appear even if offset math would evaluate to true.
14. **Horizontal shadows** use the same 12px dimension and gradient pattern, just rotated
