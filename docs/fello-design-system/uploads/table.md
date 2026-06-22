# Table Component — Strict Specification

When building any data table, grid, or tabular data display, follow these specifications exactly. Do NOT use default shadcn Table, Tailwind tables, or any other table system.

> **TOKEN RULE:** Every color in this spec is expressed as a CSS variable token.
> Never hard-code a raw hex value — always use the `var(--*)` form.
> In inline styles use the string `'var(--token)'`; in Tailwind classes use `bg-[var(--bg-token)]`, `text-[var(--text-token)]`, etc.
> The parenthetical `(#hex)` after each token is for reference only.

---

## Structure

```html
<div className="block overflow-auto relative h-full"
  style={{
    border: '1px solid var(--border-secondary)',  /* (#CCD6DC) */
    borderRadius: '8px',
  }}
>
  <table style={{ display: 'table', tableLayout: 'fixed', minWidth: '100%', width: '100%', borderCollapse: 'collapse', outline: '1px solid var(--border-tertiary)' /* (#E8ECF1) */ }}>
    <thead style={{ display: 'table-header-group', position: 'relative', zIndex: 3 }}>
      <tr style={{ display: 'table-row', position: 'sticky', top: 0, zIndex: 3 }}>
        <th>{/* header cell */}</th>
      </tr>
    </thead>
    <tbody style={{ display: 'table-row-group' }}>
      <tr style={{ display: 'table-row' }}>
        <td>{/* data cell */}</td>
      </tr>
    </tbody>
  </table>
</div>
```

---

## Outer Container

```jsx
style={{
  display: 'block',
  overflow: 'auto',
  border: '1px solid var(--border-secondary)',   // (#CCD6DC)
  borderRadius: '8px',                           // radius-4
  height: '100%',
  position: 'relative',
  color: 'var(--text-primary)',                   // (#353E5A)
  fontFamily: 'Inter, sans-serif',
  fontSize: '14px',                      // paragraph-md
  lineHeight: '20px',
  fontWeight: 400,
}}
```

---

## Header Row

```jsx
// <tr> in <thead>
style={{
  display: 'table-row',
  position: 'sticky',
  top: 0,
  zIndex: 3,
}}
```

### Header Cell

```jsx
<th style={{
  fontFamily: 'Inter, sans-serif',
  fontSize: '14px',                      // caption-md
  lineHeight: '20px',
  fontWeight: 500,
  color: 'var(--text-secondary)',                 // (#495883)
  backgroundColor: 'var(--bg-secondary)',         // (#F1F4F7)
  padding: '6px 16px',                            // spacing(1.5) spacing(4)
  outline: '1px solid var(--border-tertiary)',     // (#E8ECF1)
  height: '36px',                        // 2.25rem
  textAlign: 'left',
  position: 'relative',
}}>
```

**Header cell hover:**
```jsx
// On hover:
style={{ backgroundColor: 'var(--bg-tertiary)' }}  // (#E8ECF1)
```

---

## Data Row

```jsx
// Each <tr> after the first gets a top border on its cells
<tr style={{ display: 'table-row' }}>
```

### Data Cell

```jsx
<td style={{
  display: 'table-cell',
  padding: '6px 16px',                  // spacing(1.5) spacing(4)
  height: '64px',                        // 4rem (regular)
  wordBreak: 'break-word',
  backgroundColor: 'var(--bg-primary)',            // (#FFFFFF)
  verticalAlign: 'middle',
  position: 'relative',
  zIndex: 1,
  borderTop: '1px solid var(--border-tertiary)',   // (#E8ECF1) (skip first row)
}}>
```

**Row hover:** All cells in row → `backgroundColor: 'var(--bg-secondary)'` (#F1F4F7)

---

## Compact Row

Apply `[compact]` attribute for reduced height rows:

```jsx
// Compact cell
style={{
  height: '40px',                        // 2.5rem
  paddingTop: '2px',                     // spacing(0.5)
  paddingBottom: '2px',
  paddingLeft: '16px',
  paddingRight: '16px',
}}
```

---

## Size Reference

| Element | Height | Padding (V × H) |
|---------|--------|-----------------|
| Header row | 36px | 6px × 16px |
| Regular data row | 64px | 6px × 16px |
| Compact data row | 40px | 2px × 16px |

---

## Colors

| Element | Property | Token | Reference |
|---------|----------|-------|-----------|
| Container border | border | `var(--border-secondary)` | (#CCD6DC) |
| Cell outline | outline | `var(--border-tertiary)` | (#E8ECF1) |
| Row separator | border-top | `var(--border-tertiary)` | (#E8ECF1) |
| Header bg | background | `var(--bg-secondary)` | (#F1F4F7) |
| Header bg hover | background | `var(--bg-tertiary)` | (#E8ECF1) |
| Header text | color | `var(--text-secondary)` | (#495883) |
| Cell bg | background | `var(--bg-primary)` | (#FFFFFF) |
| Cell text | color | `var(--text-primary)` | (#353E5A) |
| Row hover bg | background | `var(--bg-secondary)` | (#F1F4F7) |
| Column resizing bg | background | `var(--bg-brand-secondary_alt)` | (#E7FDFD) |
| Secondary row bg | background | `var(--bg-secondary_subtle)` | (#F7F9FA) |
| Sticky column shadow | box-shadow | `var(--shadow-sm)` | (#E8ECF1) |

---

## Typography

| Element | Size | Line Height | Weight | Family |
|---------|------|-------------|--------|--------|
| Header cell | 14px | 20px | 500 (caption-md) | content |
| Data cell | 14px | 20px | 400 (paragraph-md) | content |

---

## Sticky Header

The header row is sticky by default:

```jsx
style={{
  position: 'sticky',
  top: 0,
  zIndex: 3,
}}
```

---

## Sticky Columns

Sticky columns remain fixed during horizontal scroll:

```jsx
// Sticky cell
style={{
  position: 'sticky',
  left: 0,   // or calculated offset for multiple sticky columns
  zIndex: 2,
}}

// Right edge shadow (pseudo-element)
// Add this after the last sticky column cell:
<div style={{
  position: 'absolute',
  right: 0,
  top: 0,
  height: '100%',
  width: 0,
  boxShadow: 'var(--shadow-sm)',   // (#E8ECF1) — 1px 0 5px 1px var(--border-tertiary), 0 0 0 0.4px var(--border-tertiary)
}} />
```

---

## Column Resizing

Header cells can have a draggable resize handle:

- Min width: 150px (default)
- Max width: 2000px (default)
- During resize: header cell gets `backgroundColor: 'var(--bg-brand-secondary_alt)'` (#E7FDFD)
- Body gets `user-select: none` during drag

---

## Secondary (Expandable) Rows

For hierarchical data with parent/child rows:

**Secondary row cell:**
```jsx
style={{
  backgroundColor: 'var(--bg-secondary_subtle)',      // (#F7F9FA)
  position: 'relative',
}}
```

**First cell of secondary row — indent + connector line:**
```jsx
// First cell gets extra left padding
style={{ paddingLeft: '44px' }}

// Connector line (::before pseudo-element on first cell)
<div style={{
  position: 'absolute',
  left: '24px',
  width: '8px',
  top: '-50%',
  height: 'calc(64px + 4px)',          // row height + 4px
  borderLeft: '1px solid var(--border-secondary)',    // (#CCD6DC)
  borderBottom: '1px solid var(--border-secondary)', // (#CCD6DC)
  borderBottomLeftRadius: '4px',         // radius-2
}} />
```

**Secondary row hover:** `backgroundColor: 'var(--bg-secondary)'` (#F1F4F7)

---

## Loading State

When `isLoading`:

```jsx
{/* Overlay covering the table */}
<div style={{
  position: 'absolute',
  height: 'calc(100% - 2px)',
  width: 'calc(100% - 2px)',
  top: '1px',
  left: '1px',
  zIndex: 2,
  background: 'var(--bg-primary)',   // (#FFFFFF)
  display: 'flex',
  justifyContent: 'center',
  alignItems: 'center',
}}>
  <Spinner size="md" />
</div>
```

- Table overflow set to `hidden`
- All data rows hidden
- Scroll shadows hidden

---

## Empty State

When no data rows exist, render a full-width empty state (use the Empty State component from `empty-state.md`). Table overflow set to `hidden` when empty state is shown.

---

## Scrollable Table

For tables wider than the container, add `width: max-content` to the table element and let the outer container scroll horizontally.

---

## Scroll Shadows (MANDATORY)

**Every table MUST have scroll shadows** from `scroll-shadow.md`. The table's outer container is a scrollable area, so wrap it in a `position: relative; overflow: hidden` parent and add scroll shadow overlays on all four edges (top, bottom, left, right).

```jsx
{/* Table wrapper with scroll shadows */}
<div className="relative overflow-hidden" style={{ height: '100%' }}>
  {/* Top scroll shadow — visible when scrolled down */}
  {showTopShadow && (
    <div
      className="absolute top-0 left-0 w-full z-10 pointer-events-none"
      style={{
        height: '12px',
        opacity: 0.1,
        background: 'linear-gradient(180deg, rgba(0, 0, 0, 0.06) 0%, rgba(146, 152, 169, 0.04) 40%, transparent 100%)',  // (#000000, #9298A9)
      }}
    />
  )}

  {/* Bottom scroll shadow — visible when more content below */}
  {showBottomShadow && (
    <div
      className="absolute bottom-0 left-0 w-full z-10 pointer-events-none"
      style={{
        height: '12px',
        opacity: 0.1,
        background: 'linear-gradient(0deg, rgba(0, 0, 0, 0.06) 0%, rgba(146, 152, 169, 0.04) 40%, transparent 100%)',  // (#000000, #9298A9)
      }}
    />
  )}

  {/* Left scroll shadow — visible when scrolled right */}
  {showLeftShadow && (
    <div
      className="absolute top-0 left-0 h-full z-10 pointer-events-none"
      style={{
        width: '12px',
        opacity: 0.1,
        background: 'linear-gradient(90deg, rgba(0, 0, 0, 0.06) 0%, rgba(146, 152, 169, 0.04) 40%, transparent 100%)',  // (#000000, #9298A9)
      }}
    />
  )}

  {/* Right scroll shadow — visible when more content to the right */}
  {showRightShadow && (
    <div
      className="absolute top-0 right-0 h-full z-10 pointer-events-none"
      style={{
        width: '12px',
        opacity: 0.1,
        background: 'linear-gradient(270deg, rgba(0, 0, 0, 0.06) 0%, rgba(146, 152, 169, 0.04) 40%, transparent 100%)',  // (#000000, #9298A9)
      }}
    />
  )}

  {/* The actual scrollable table container */}
  <div
    ref={scrollRef}
    className="overflow-auto h-full"
    style={{
      border: '1px solid var(--border-secondary)',  // (#CCD6DC)
      borderRadius: '8px',
      position: 'relative',
    }}
    onScroll={checkOverflow}
  >
    <table>...</table>
  </div>
</div>
```

**Overflow check — shadows ONLY appear when content overflows:**
```jsx
const scrollRef = useRef(null);
const checkOverflow = useCallback(() => {
  const el = scrollRef.current;
  if (!el) return;
  const overflowsV = el.scrollHeight > el.clientHeight;
  const overflowsH = el.scrollWidth > el.clientWidth;
  setShowTopShadow(overflowsV && el.scrollTop > 1);
  setShowBottomShadow(overflowsV && el.scrollTop < el.scrollHeight - el.clientHeight - 1);
  setShowLeftShadow(overflowsH && el.scrollLeft > 1);
  setShowRightShadow(overflowsH && el.scrollLeft < el.scrollWidth - el.clientWidth - 1);
}, []);

// Re-check on mount and whenever data changes
useEffect(() => { checkOverflow(); }, [data, checkOverflow]);
```

**Scroll shadow rules for tables:**
- Shadow thickness: **12px**
- Opacity: **10%** (0.1)
- Gradient: `rgba(0, 0, 0, 0.06)` (#000000) → `rgba(146, 152, 169, 0.04)` (#9298A9) at 40% → transparent
- `pointer-events: none`, `z-index: 10`
- Hide ALL shadows when table is in loading or empty state
- **Shadows ONLY appear when content actually overflows.** If all rows fit without scrolling, NO shadows should be shown. Always check `scrollHeight > clientHeight` (vertical) and `scrollWidth > clientWidth` (horizontal) before showing any shadow.
- Re-check overflow on mount and after data changes (not just on scroll)

---

## CRITICAL Rules

1. **NEVER use shadcn `<Table>`** or Tailwind table utilities. Build from raw `<table>`, `<thead>`, `<tbody>`, `<tr>`, `<th>`, `<td>`.
2. **Container border-radius is 8px** (radius-4), matching the dropdown popup panel.
3. **Container border is `var(--border-secondary)`** (#CCD6DC), cell outlines are `var(--border-tertiary)` (#E8ECF1).
4. **Header is always sticky** — `position: sticky; top: 0; z-index: 3`.
5. **Regular row height is 64px**, compact is 40px. Headers are always 36px.
6. **Row hover applies to all cells** — use a hover state on `<tr>` that changes all child cell backgrounds to `var(--bg-secondary)` (#F1F4F7).
7. **Table layout is always `fixed`** — column widths must be explicit.
8. **border-collapse: collapse** — this is required for the outline borders to work correctly.
9. **Header typography is caption-md (500 weight)**, body text is paragraph-md (400 weight). Both are 14px.
10. **Loading overlay has z-index: 2** and covers the entire table content area with a centered spinner.
11. **ALWAYS add scroll shadows** (see Scroll Shadows section above). Every table must have scroll shadow overlays on all four edges. Use the gradient pattern from `scroll-shadow.md`. NEVER skip scroll shadows — they are a core visual feature of the table.
12. **When used inside a Grid**, the table MUST be paired with the Paginator component from `paginator.md` — NEVER use a generic "Previous/Next" or "Showing X of Y" text pagination. Use the exact teal page-number buttons spec.
