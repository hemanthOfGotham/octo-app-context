# Image Card Component — Strict Specification

When building any image card, image thumbnail, selectable image tile, or image preview card, follow these specifications exactly. Do NOT use default Tailwind card styles or any other image card system.

> **TOKEN RULE:** Every colour in this spec is expressed as a CSS variable token. When writing Tailwind classes use the `bg-[var(--token)]` / `text-[var(--token)]` / `border-[var(--token)]` syntax. In inline styles use `'var(--token)'`. In prose and tables the original hex is kept only as a parenthetical annotation, e.g. `var(--bg-secondary)` (#F1F4F7).

---

## Image Card Anatomy

A card that displays an image with an optional hover overlay containing a label and action buttons:

```
Default (no hover):           On Hover:
┌──────────────────────┐      ┌──────────────────────┐
│                      │      │                      │
│                      │      │                      │
│      [Image]         │      │      [Image]         │
│                      │      │ ┌──────────────────┐ │
│                      │      │ │ Label   [btn][btn]│ │  ← Gradient overlay at bottom
└──────────────────────┘      └─┴──────────────────┴─┘
```

- Image fills the entire card
- Hover shows a gradient overlay at the bottom with label + action buttons
- Selected state adds a white inner border ring

---

## Container Specs

| Property | Value | Tailwind |
|----------|-------|----------|
| Display | Block | `block` |
| Background | `var(--bg-secondary)` (#F1F4F7) | `bg-[var(--bg-secondary)]` |
| Border | 1px solid `var(--border-secondary)` (#CCD6DC) | `border border-[var(--border-secondary)]` |
| Border radius | 8px | `rounded-lg` |
| Overflow | Hidden | `overflow-hidden` |
| Position | Relative | `relative` |
| Cursor | Pointer | `cursor-pointer` |
| Transition | box-shadow 150ms ease-out | `transition-shadow duration-150 ease-out` |

**Width/Height**: Set by the consumer — the image card fills its given container.

---

## States

### Default
| Property | Value |
|----------|-------|
| Border | 1px solid `var(--border-secondary)` (#CCD6DC) |
| Shadow | None |

### Hover
| Property | Value | Tailwind |
|----------|-------|----------|
| Border | 1px solid `var(--border-brand-secondary-solid)` (#0BA5A7) | `hover:border-[var(--border-brand-secondary-solid)]` |
| Shadow | Small shadow | `hover:shadow-sm` |
| Overlay | Visible (opacity: 1) | — |

### Selected
| Property | Value | Tailwind |
|----------|-------|----------|
| Border | 1px solid `var(--border-brand-secondary-solid)` (#0BA5A7) | `border-[var(--border-brand-secondary-solid)]` |
| Shadow | Small shadow | `shadow-sm` |

### Focus-Visible
| Property | Value | Tailwind |
|----------|-------|----------|
| Border | 1px solid `var(--border-brand-secondary-solid)` (#0BA5A7) | `focus-visible:border-[var(--border-brand-secondary-solid)]` |
| Shadow | Teal focus ring | `focus-visible:shadow-[0_0_0_4px_rgba(11,165,167,0.24),0_1px_2px_0_rgba(17,23,41,0.16)]` |
| Overlay | Visible | — |

---

## Image

| Property | Value | Tailwind |
|----------|-------|----------|
| Width | 100% | `w-full` |
| Height | 100% | `h-full` |
| Object fit (contain) | `object-contain` (default) | `object-contain` |
| Object fit (cover) | `object-cover` | `object-cover` |

---

## Overlay

The overlay appears on hover/focus and sits at the bottom of the card:

| Property | Value | Tailwind |
|----------|-------|----------|
| Position | Absolute, covers full card | `absolute inset-0` |
| Background | Gradient: transparent top → semi-transparent bottom | See below |
| Display | Flex column, justify-end | `flex flex-col justify-end` |
| Opacity (default) | 0 — hidden | `opacity-0` |
| Opacity (hover) | 1 — visible | `group-hover:opacity-100` |
| Transition | opacity 150ms ease-out | `transition-opacity duration-150 ease-out` |

**Gradient:**
```
background: linear-gradient(180deg, rgba(73,88,131,0) 0%, rgba(73,88,131,0.11) 62.5%, rgba(73,88,131,0.25) 100%)
```

### Overlay Content Area
| Property | Value | Tailwind |
|----------|-------|----------|
| Layout | Flex, space-between, centered | `flex justify-between items-center` |
| Padding | 10px | `p-2.5` |
| Gap | 8px | `gap-2` |

### Label
| Property | Value | Tailwind |
|----------|-------|----------|
| Typography | 12px / 18px / weight 600 (title-sm) | `text-xs leading-[18px]` + `style={{ fontWeight: 600 }}` |
| Color | `var(--text-white)` (#FFFFFF) | `text-white` |
| Overflow | Truncated | `truncate` |

### Action Buttons
| Property | Value | Tailwind |
|----------|-------|----------|
| Layout | Flex, no shrink | `flex gap-1 shrink-0` |
| Gap | 4px | `gap-1` |

---

## Selected State Ring

When selected, a white inner border ring appears:

| Property | Value | Tailwind |
|----------|-------|----------|
| Position | Absolute, full coverage | `absolute inset-0` |
| Border | 2px solid `var(--bg-primary)` (#FFFFFF) | `border-2 border-white` |
| Border radius | 6px (slightly less than container) | `rounded-md` |
| Pointer events | None | `pointer-events-none` |

---

## Always Show Overlay

When `alwaysShowOverlay` is true, the overlay is permanently visible (opacity: 1) regardless of hover state.

---

## Implementation Pattern for Lovable

```jsx
{/* === Image Card — Default (hover to show overlay) === */}
<div className="
  relative block overflow-hidden
  rounded-lg bg-[var(--bg-secondary)]
  border border-[var(--border-secondary)]
  cursor-pointer group
  hover:border-[var(--border-brand-secondary-solid)] hover:shadow-sm
  focus-visible:border-[var(--border-brand-secondary-solid)]
  focus-visible:shadow-[0_0_0_4px_rgba(11,165,167,0.24),0_1px_2px_0_rgba(17,23,41,0.16)]
  transition-shadow duration-150 ease-out
  w-[200px] h-[160px]
"
  tabIndex={0}
>
  {/* Image */}
  <img
    src="/path/to/image.jpg"
    alt="Image card"
    className="w-full h-full object-contain"
  />

  {/* Overlay — visible on hover */}
  <div className="
    absolute inset-0
    flex flex-col justify-end
    opacity-0 group-hover:opacity-100 group-focus-visible:opacity-100
    transition-opacity duration-150 ease-out
  "
    style={{
      background: 'linear-gradient(180deg, rgba(73,88,131,0) 0%, rgba(73,88,131,0.11) 62.5%, rgba(73,88,131,0.25) 100%)'
    }}
  >
    <div className="flex justify-between items-center p-2.5 gap-2">
      {/* Label */}
      <span
        className="text-xs leading-[18px] text-white truncate"
        style={{ fontWeight: 600 }}
      >
        Background Image
      </span>
      {/* Action buttons */}
      <div className="flex gap-1 shrink-0">
        <button className="w-7 h-7 rounded bg-white/80 flex items-center justify-center hover:bg-white">
          <Edit className="w-3.5 h-3.5 text-[var(--fg-primary)]" />
        </button>
        <button className="w-7 h-7 rounded bg-white/80 flex items-center justify-center hover:bg-white">
          <Trash2 className="w-3.5 h-3.5 text-[var(--fg-primary)]" />
        </button>
      </div>
    </div>
  </div>
</div>


{/* === Image Card — Selected state === */}
<div className="
  relative block overflow-hidden
  rounded-lg bg-[var(--bg-secondary)]
  border border-[var(--border-brand-secondary-solid)]
  shadow-sm cursor-pointer group
  w-[200px] h-[160px]
">
  <img
    src="/path/to/image.jpg"
    alt="Selected image"
    className="w-full h-full object-cover"
  />

  {/* Selected ring */}
  <div className="absolute inset-0 border-2 border-white rounded-md pointer-events-none" />

  {/* Overlay */}
  <div className="
    absolute inset-0 flex flex-col justify-end
    opacity-0 group-hover:opacity-100
    transition-opacity duration-150 ease-out
  "
    style={{
      background: 'linear-gradient(180deg, rgba(73,88,131,0) 0%, rgba(73,88,131,0.11) 62.5%, rgba(73,88,131,0.25) 100%)'
    }}
  >
    <div className="flex justify-between items-center p-2.5 gap-2">
      <span className="text-xs leading-[18px] text-white truncate" style={{ fontWeight: 600 }}>
        Selected Image
      </span>
    </div>
  </div>
</div>


{/* === Image Card — Always show overlay === */}
<div className="
  relative block overflow-hidden
  rounded-lg bg-[var(--bg-secondary)]
  border border-[var(--border-secondary)]
  cursor-pointer
  hover:border-[var(--border-brand-secondary-solid)] hover:shadow-sm
  w-[200px] h-[160px]
">
  <img src="/path/to/image.jpg" alt="Image" className="w-full h-full object-contain" />

  {/* Overlay — always visible */}
  <div className="
    absolute inset-0 flex flex-col justify-end
  "
    style={{
      background: 'linear-gradient(180deg, rgba(73,88,131,0) 0%, rgba(73,88,131,0.11) 62.5%, rgba(73,88,131,0.25) 100%)'
    }}
  >
    <div className="flex justify-between items-center p-2.5 gap-2">
      <span className="text-xs leading-[18px] text-white truncate" style={{ fontWeight: 600 }}>
        Always Visible Label
      </span>
    </div>
  </div>
</div>
```

### Key Implementation Rules

1. **Image fills the entire card** — `w-full h-full` with `object-contain` (default) or `object-cover`
2. **Background is `var(--bg-secondary)`** (#F1F4F7) — visible when image hasn't loaded or has transparent areas
3. **Overlay is hidden by default** (opacity: 0), appears on hover/focus (opacity: 1)
4. **Overlay gradient goes from transparent (top) to semi-transparent blue-gray (bottom)**
5. **Label is white, 12px/18px/600** with truncation — positioned at the bottom-left of the overlay
6. **Action buttons sit at bottom-right** of the overlay with 4px gap between them
7. **Hover changes border to teal** `var(--border-brand-secondary-solid)` (#0BA5A7) and adds a small shadow
8. **Selected state adds a 2px white inner border ring** with 6px radius (pointer-events-none)
9. **Selected state also gets teal border** and small shadow (same as hover but persistent)
10. **Card dimensions are set by the consumer** — the component fills whatever size it's given
11. **Use `group` + `group-hover`** classes to trigger overlay visibility on card hover
12. **`alwaysShowOverlay`** makes the overlay permanently visible (no opacity transition needed)
