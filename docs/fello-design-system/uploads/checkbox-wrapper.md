# Checkbox Wrapper Component — Strict Specification

When building a card or content block with an overlaid checkbox (e.g., selectable image cards, selectable content tiles), follow these specifications exactly.

**Note:** This component uses the Checkbox from `checkbox.md` for the actual checkbox rendering. The wrapper is purely a positioning shell — it overlays a checkbox on top of projected content.

> **TOKEN RULE:** Every color in this spec is expressed as a CSS variable token. In Tailwind classes use `bg-[var(--token)]` / `text-[var(--token)]` / `border-[var(--token)]`. In inline styles use `'var(--token)'`. In prose and tables, show `var(--token)` (#hex) for reference.

---

## Architecture

```
Checkbox Wrapper (position: relative, display: block)
├── Checkbox (position: absolute, top: 8px, left: 8px)
└── Projected Content (the card, image, or block underneath)
```

---

## Structure

```jsx
<div
  style={{
    display: 'block',
    position: 'relative',
  }}
>
  {/* Checkbox — absolutely positioned in top-left corner */}
  <div
    style={{
      position: 'absolute',
      top: '8px',         // spacing-2
      left: '8px',        // spacing-2
      zIndex: 1,
    }}
  >
    <Checkbox checked={selected} onChange={onToggle} />
  </div>

  {/* Projected content — card, image, tile, etc. */}
  {children}
</div>
```

---

## Positioning

| Property | Value |
|----------|-------|
| Host display | `block` |
| Host position | `relative` |
| Checkbox position | `absolute` |
| Checkbox top | `8px` (spacing-2) |
| Checkbox left | `8px` (spacing-2) |
| Checkbox z-index | `1` |

---

## Behavior

- The wrapper has **no visual styling of its own** — no border, no background, no shadow
- It only provides absolute positioning context for the checkbox
- The checkbox sits on top of whatever content is projected inside
- Clicking the checkbox toggles selection WITHOUT triggering the underlying content's click handler
- The underlying content (card) remains fully interactive outside the checkbox area

---

## Implementation Pattern for Lovable

```jsx
{/* === Selectable Card with Checkbox Overlay === */}
<div className="relative block">
  {/* Checkbox overlay — top-left corner */}
  <div className="absolute top-2 left-2 z-10">
    <div
      className={`
        flex items-center justify-center shrink-0
        w-4 h-4 rounded
        border transition-all duration-150 ease-out cursor-pointer
        ${selected
          ? 'bg-[var(--bg-brand-secondary-solid)] border-[var(--border-brand-secondary-solid)] hover:bg-[var(--bg-brand-secondary-solid_hover)] hover:border-[var(--border-brand-secondary-solid)]'
          : 'bg-white border-[var(--border-primary)] hover:border-[var(--border-brand-secondary-solid)]'
        }
      `}
      tabIndex={0}
      role="checkbox"
      aria-checked={selected}
      onClick={(e) => { e.stopPropagation(); toggleSelect(); }}
    >
      {selected && (
        <svg width="10" height="8" viewBox="0 0 10 8" fill="none">
          <path d="M9 1L3.5 6.5L1 4" stroke="white" strokeWidth="1.5" strokeLinecap="round" strokeLinejoin="round" />
        </svg>
      )}
    </div>
  </div>

  {/* The actual card content underneath */}
  <div className="p-4 border border-[var(--border-secondary)] rounded-lg bg-white cursor-pointer hover:bg-[var(--bg-secondary)]"
    onClick={handleCardClick}>
    <img src={imageSrc} alt={title} className="w-full h-32 object-cover rounded" />
    <p className="text-sm font-medium text-[var(--text-primary)] mt-2">{title}</p>
  </div>
</div>


{/* === Grid of Selectable Cards === */}
<div className="grid grid-cols-3 gap-4">
  {items.map(item => (
    <div key={item.id} className="relative block">
      <div className="absolute top-2 left-2 z-10">
        {/* Checkbox from checkbox.md */}
        <div
          className={`
            flex items-center justify-center shrink-0
            w-4 h-4 rounded border cursor-pointer
            transition-all duration-150 ease-out
            ${selectedIds.has(item.id)
              ? 'bg-[var(--bg-brand-secondary-solid)] border-[var(--border-brand-secondary-solid)]'
              : 'bg-white border-[var(--border-primary)] hover:border-[var(--border-brand-secondary-solid)]'
            }
          `}
          role="checkbox"
          aria-checked={selectedIds.has(item.id)}
          onClick={(e) => { e.stopPropagation(); toggleItem(item.id); }}
        >
          {selectedIds.has(item.id) && (
            <svg width="10" height="8" viewBox="0 0 10 8" fill="none">
              <path d="M9 1L3.5 6.5L1 4" stroke="white" strokeWidth="1.5" strokeLinecap="round" strokeLinejoin="round" />
            </svg>
          )}
        </div>
      </div>
      <div className="p-3 border border-[var(--border-secondary)] rounded-lg bg-white">
        {item.content}
      </div>
    </div>
  ))}
</div>
```

---

## CRITICAL Rules

1. **The wrapper is ONLY a positioning shell** — no border, no background, no shadow, no padding.
2. **Checkbox position is ALWAYS `top: 8px, left: 8px`** — do not change this offset.
3. **Use `position: relative` on the wrapper** and `position: absolute` on the checkbox container.
4. **The checkbox MUST use the spec from `checkbox.md`** — same box styling, same states, same SVG icons.
5. **Use `e.stopPropagation()`** on the checkbox click to prevent triggering the underlying content's click handler.
6. **Z-index 1** (or higher, like z-10) ensures the checkbox sits above the projected content.
7. **The wrapper has `display: block`** — it is a block-level container, not inline.
