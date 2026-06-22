# Paginator Component — Strict Specification

When building any pagination control, page navigator, or paged list navigation, follow these specifications exactly. Do NOT use default Tailwind pagination, shadcn pagination, or any other pagination system.

> **TOKEN RULE:** Every colour in this file is expressed as a CSS custom-property token — e.g. `var(--bg-primary)` — with the original hex in parentheses for reference only. When implementing, always use the `var(--*)` form (in Tailwind: `bg-[var(--bg-primary)]`, `text-[var(--text-primary)]`, etc.). Never hard-code a raw hex value.

---

## Paginator Anatomy

A horizontal bar with a page size selector on the left and page navigation **centered** across the full width:

```
Default Variant:
+--------------------------------------------------------------------------------+
|  25 per page v             < Previous   1  2  [3]  4  5  ...  12   Next >      |
|  ^ link button                       ^ centered page nav                       |
|  (absolute left)                                                               |
+--------------------------------------------------------------------------------+

Compact Variant:
+--------------------------------------------------------------------------------+
|  25 per page v                  <   1  2  [3]  4  5   >                        |
|  ^ link button                  ^ centered, icon-only prev/next                |
+--------------------------------------------------------------------------------+
```

- **Page size selector** on the left — a **secondary link button** (see `button.md` hyperlink/secondary variant) with a ChevronDown icon. Clicking opens a **list box** (see `list-box.md`) with options: 25, 50, 100. It is NOT a native `<select>` element.
- **Page navigation** (Previous + page numbers + Next) is **always centered** in the full width of the paginator bar — NOT pushed to the right
- Current page has a distinct gray background
- Previous/Next buttons can be text+icon (default) or icon-only (compact)
- Ellipsis (`...`) appears when pages are truncated

---

## Container

| Property | Value | Tailwind / CSS |
|----------|-------|----------------|
| Position | Relative | `relative` |
| Display | Flex row | `flex items-center` |
| Padding | 6px vertical | `py-1.5` |
| Width | Full width | `w-full` |

**Layout rule — two layers:**
- **Page size selector** is positioned `absolute left-0` — pinned to the left edge without affecting centering
- **Page navigation** (`<nav>`) uses `flex-1 flex justify-center` — this centers the page buttons across the FULL container width

**IMPORTANT:** Do NOT use `justify-between`. The page navigation must be centered, not right-aligned.

---

## Page Size Selector

A **secondary link button** (from `button.md`) on the left side. It is NOT a native `<select>` — it is a button that opens a list box popover.

### Trigger Button Styling

| Property | Value | Tailwind |
|----------|-------|----------|
| Display | Inline-flex, centered | `inline-flex items-center` |
| Gap | 4px (between text and chevron) | `gap-1` |
| Background | Transparent | `bg-transparent` |
| Border | None | `border-none` |
| Padding | 0 | `p-0` |
| Text color | `var(--text-brand-secondary)` (#098486) | `text-[var(--text-brand-secondary)]` |
| Typography | 14px / 20px / weight 600 | `text-sm leading-5 font-semibold` |
| Text decoration | Underline offset 2px, transparent by default | `underline-offset-2 decoration-transparent` |
| Hover decoration | Underline becomes visible in teal | `hover:decoration-[var(--text-brand-secondary)]` |
| Cursor | Pointer | `cursor-pointer` |
| Outline | None | `outline-none` |
| Icon | ChevronDown, 16px | `w-4 h-4` |
| Icon color | Same teal — `var(--text-brand-secondary)` (#098486) | inherited |

### Label Format
`{value} per page` — e.g., "25 per page", "50 per page", "100 per page"

### Dropdown Popover Panel

When clicked, the trigger button opens a dropdown panel identical to the **filter-dropdown** (see `filter-dropdown.md`). Key styling:

| Property | Value |
|----------|-------|
| Background | `var(--bg-primary)` (#FFFFFF) |
| Border | 1px solid `var(--border-secondary)` (#CCD6DC) |
| Border radius | 8px |
| Shadow | `var(--shadow-lg)` — `0 4px 6px -2px #4958830a, 0 12px 16px -4px #49588329` |
| Padding top | 2px |
| Min width | 160px |
| Offset from trigger | 4px below (`top: calc(100% + 4px)`) |
| Overflow | auto |
| Item outer padding | 2px 4px (`px-1 py-0.5`) |
| Item inner padding | 8px 12px |
| Item border radius | 4px |
| Item font | 14px / 20px / weight 500 |
| Item text color | `var(--text-primary)` (#353E5A) |
| Item hover bg | `var(--bg-secondary)` (#F1F4F7) |
| Selected item bg | `var(--bg-secondary)` (#F1F4F7) |
| Selected + hover bg | `var(--bg-secondary_hover)` (#E8ECF1) |
| Selected checkmark | `var(--fg-brand-secondary)` (#098486) teal checkmark, 16px, right side |
| Transition | `background 150ms ease-out` |

### Page Size Options
Default options: `[25, 50, 100]`

### Behavior
- Changing page size resets the current page to **1**
- The list box closes after selection
- The trigger button label updates to reflect the new selection
- **When inside a container with `overflow: hidden`** (e.g., Grid), the dropdown must open **upward** (`bottom: calc(100% + 4px)` instead of `top`) to avoid being clipped

---

## Page Buttons

| Property | Value | Tailwind |
|----------|-------|----------|
| Width | 32px | `w-8` |
| Height | 32px | `h-8` |
| Border radius | 6px | `rounded-md` |
| Display | Flex, centered | `flex items-center justify-center` |
| Typography | 12px / 18px / weight 600 (title-sm) | `text-xs leading-[18px]` + `style={{ fontWeight: 600 }}` |
| Cursor | Pointer | `cursor-pointer` |
| Transition | background-color, box-shadow 150ms ease-out | `transition-all duration-150 ease-out` |

### Page Button States

#### Default (Non-Current Page)
| Property | Value | Tailwind |
|----------|-------|----------|
| Background | Transparent | `bg-transparent` |
| Text color | `var(--text-brand-secondary)` (#098486) | `text-[var(--text-brand-secondary)]` |
| Hover bg | `var(--bg-brand-secondary)` (#E7FDFD) | `hover:bg-[var(--bg-brand-secondary)]` |
| Hover shadow | var(--shadow-xs) | `hover:shadow-sm` |
| Focus ring | `var(--ring-brand-secondary)` — `0 0 0 4px rgba(11,165,167,0.24), 0 1px 2px 0 rgba(17,23,41,0.16)` | `focus-visible:shadow-[0_0_0_4px_rgba(11,165,167,0.24),0_1px_2px_0_rgba(17,23,41,0.16)]` |

#### Current Page
| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `var(--bg-secondary)` (#F1F4F7) | `bg-[var(--bg-secondary)]` |
| Text color | `var(--text-primary)` (#353E5A) | `text-[var(--text-primary)]` |
| Cursor | Default (not clickable) | `cursor-default` |
| Pointer events | None | `pointer-events-none` |

#### Disabled (Prev/Next when at boundary)
| Property | Value | Tailwind |
|----------|-------|----------|
| Opacity | 40% | `opacity-40` |
| Cursor | Not-allowed | `cursor-not-allowed` |
| Pointer events | None | `pointer-events-none` |

---

## Previous / Next Buttons

### Default Variant (Text + Icon)

| Property | Value | Tailwind |
|----------|-------|----------|
| Display | Inline-flex, centered | `inline-flex items-center` |
| Gap | 4px | `gap-1` |
| Height | 32px | `h-8` |
| Padding | 8px horizontal | `px-2` |
| Border radius | 6px | `rounded-md` |
| Typography | 12px / 18px / weight 600 | `text-xs leading-[18px]` + `style={{ fontWeight: 600 }}` |
| Text color | `var(--text-brand-secondary)` (#098486) | `text-[var(--text-brand-secondary)]` |
| Hover bg | `var(--bg-brand-secondary)` (#E7FDFD) | `hover:bg-[var(--bg-brand-secondary)]` |
| Icon size | 16px | `w-4 h-4` |
| Icon | ChevronLeft / ChevronRight | — |

### Compact Variant (Icon Only)

| Property | Value | Tailwind |
|----------|-------|----------|
| Width | 32px | `w-8` |
| Height | 32px | `h-8` |
| Display | Flex, centered | `flex items-center justify-center` |
| Border radius | 6px | `rounded-md` |
| Text color | `var(--text-brand-secondary)` (#098486) | `text-[var(--text-brand-secondary)]` |
| Hover bg | `var(--bg-brand-secondary)` (#E7FDFD) | `hover:bg-[var(--bg-brand-secondary)]` |
| Icon size | 16px | `w-4 h-4` |

---

## Ellipsis

| Property | Value | Tailwind |
|----------|-------|----------|
| Width | 32px | `w-8` |
| Height | 32px | `h-8` |
| Display | Flex, centered | `flex items-center justify-center` |
| Text | `...` | — |
| Typography | 12px / 18px / weight 600 | `text-xs leading-[18px]` + `style={{ fontWeight: 600 }}` |
| Color | `var(--text-brand-secondary)` (#098486) | `text-[var(--text-brand-secondary)]` |
| Cursor | Default | `cursor-default` |

---

## Page Truncation Logic

### Default Variant — Max 9 Visible Page Buttons

When total pages exceed 9, truncation applies:
- Always show first page and last page
- Show up to 5 pages around the current page
- Use ellipsis (`...`) for gaps

Examples (total = 20 pages):
- Current = 1: `[1] 2 3 4 5 6 7 ... 20`
- Current = 5: `1 ... 3 4 [5] 6 7 ... 20`
- Current = 10: `1 ... 8 9 [10] 11 12 ... 20`
- Current = 20: `1 ... 14 15 16 17 18 19 [20]`

### Compact Variant — Max 5 Visible Page Buttons

When total pages exceed 5, truncation applies:
- Always show first page and last page
- Show up to 3 pages around the current page
- Use ellipsis for gaps

Examples (total = 20 pages):
- Current = 1: `[1] 2 3 ... 20`
- Current = 5: `1 ... [5] ... 20`
- Current = 20: `1 ... 18 19 [20]`

---

## Implementation Pattern for Lovable

```jsx
{/* === Paginator — Default Variant (FULL COMPONENT) === */}
{/* IMPORTANT: Page nav is CENTERED. Page size selector is absolute-left. */}
<div className="relative flex items-center w-full py-1.5">
  {/* LEFT: Page Size Selector — secondary link button, absolute positioned */}
  <div className="absolute left-0">
    <button
      className="
        inline-flex items-center gap-1
        bg-transparent border-none p-0
        text-[var(--text-brand-secondary)]
        text-sm leading-5 font-semibold
        underline-offset-2 decoration-transparent
        hover:decoration-[var(--text-brand-secondary)]
        cursor-pointer outline-none
      "
      onClick={() => setShowPageSizeMenu(!showPageSizeMenu)}
    >
      <span>{pageSize} per page</span>
      <ChevronDown className="w-4 h-4" />
    </button>

    {/* Dropdown panel — matches filter-dropdown.md */}
    {showPageSizeMenu && (
      <div
        className="absolute top-full left-0 mt-1 bg-[var(--bg-primary)] z-50 overflow-auto"
        style={{
          border: '1px solid var(--border-secondary)',
          borderRadius: '8px',
          boxShadow: '0 4px 6px -2px #4958830a, 0 12px 16px -4px #49588329',
          paddingTop: '2px',
          minWidth: '160px',
        }}
      >
        {[25, 50, 100].map((size) => (
          <div
            key={size}
            className={`
              inline-flex items-center justify-between gap-2
              cursor-pointer w-full
              text-sm leading-5 font-medium
              text-[var(--text-primary)]
              transition-[background] duration-150 ease-out
              ${pageSize === size
                ? 'bg-[var(--bg-secondary)] hover:bg-[var(--bg-secondary_hover)]'
                : 'hover:bg-[var(--bg-secondary)]'
              }
            `}
            style={{ borderRadius: '4px', padding: '8px 12px', margin: '2px 4px', width: 'calc(100% - 8px)' }}
            onClick={() => { setPageSize(size); setPage(1); setShowPageSizeMenu(false); }}
          >
            <span>{size} per page</span>
            {pageSize === size && (
              <Check className="w-4 h-4 text-[var(--fg-brand-secondary)]" />
            )}
          </div>
        ))}
      </div>
    )}
  </div>

  {/* CENTER: Page Navigation Buttons — centered in full width */}
  <nav className="flex-1 flex items-center justify-center gap-1" aria-label="Pagination">
    {/* Previous button — text + icon */}
    <button
      className="
        inline-flex items-center gap-1
        h-8 px-2 rounded-md
        text-[var(--text-brand-secondary)] bg-transparent
        hover:bg-[var(--bg-brand-secondary)]
        transition-all duration-150 ease-out
        cursor-pointer outline-none
        focus-visible:shadow-[0_0_0_4px_rgba(11,165,167,0.24),0_1px_2px_0_rgba(17,23,41,0.16)]
      "
      style={{ fontWeight: 600 }}
    >
      <ChevronLeft className="w-4 h-4" />
      <span className="text-xs leading-[18px]">Previous</span>
    </button>

    {/* Page 1 */}
    <button
      className="
        w-8 h-8 rounded-md flex items-center justify-center
        text-xs leading-[18px]
        text-[var(--text-brand-secondary)] bg-transparent
        hover:bg-[var(--bg-brand-secondary)] hover:shadow-sm
        transition-all duration-150 ease-out cursor-pointer outline-none
      "
      style={{ fontWeight: 600 }}
    >
      1
    </button>

    {/* Ellipsis */}
    <span className="w-8 h-8 flex items-center justify-center text-xs leading-[18px] text-[var(--text-brand-secondary)] cursor-default"
      style={{ fontWeight: 600 }}>...</span>

    <button className="w-8 h-8 rounded-md flex items-center justify-center text-xs leading-[18px]
      text-[var(--text-brand-secondary)] bg-transparent hover:bg-[var(--bg-brand-secondary)] hover:shadow-sm
      transition-all duration-150 ease-out cursor-pointer outline-none"
      style={{ fontWeight: 600 }}>4</button>

    {/* Current page (5) — non-interactive, gray bg */}
    <span className="w-8 h-8 rounded-md flex items-center justify-center text-xs leading-[18px]
      bg-[var(--bg-secondary)] text-[var(--text-primary)] cursor-default pointer-events-none"
      style={{ fontWeight: 600 }} aria-current="page">5</span>

    <button className="w-8 h-8 rounded-md flex items-center justify-center text-xs leading-[18px]
      text-[var(--text-brand-secondary)] bg-transparent hover:bg-[var(--bg-brand-secondary)] hover:shadow-sm
      transition-all duration-150 ease-out cursor-pointer outline-none"
      style={{ fontWeight: 600 }}>6</button>

    <span className="w-8 h-8 flex items-center justify-center text-xs leading-[18px] text-[var(--text-brand-secondary)] cursor-default"
      style={{ fontWeight: 600 }}>...</span>

    <button className="w-8 h-8 rounded-md flex items-center justify-center text-xs leading-[18px]
      text-[var(--text-brand-secondary)] bg-transparent hover:bg-[var(--bg-brand-secondary)] hover:shadow-sm
      transition-all duration-150 ease-out cursor-pointer outline-none"
      style={{ fontWeight: 600 }}>20</button>

    {/* Next button — text + icon */}
    <button
      className="
        inline-flex items-center gap-1
        h-8 px-2 rounded-md
        text-[var(--text-brand-secondary)] bg-transparent
        hover:bg-[var(--bg-brand-secondary)]
        transition-all duration-150 ease-out
        cursor-pointer outline-none
        focus-visible:shadow-[0_0_0_4px_rgba(11,165,167,0.24),0_1px_2px_0_rgba(17,23,41,0.16)]
      "
      style={{ fontWeight: 600 }}
    >
      <span className="text-xs leading-[18px]">Next</span>
      <ChevronRight className="w-4 h-4" />
    </button>
  </nav>
</div>


{/* === Paginator — Compact Variant (FULL COMPONENT) === */}
<div className="relative flex items-center w-full py-1.5">
  {/* LEFT: Page Size Selector — same link button, absolute left */}
  <div className="absolute left-0">
    <button className="inline-flex items-center gap-1 bg-transparent border-none p-0
      text-[var(--text-brand-secondary)] text-sm leading-5 font-semibold
      underline-offset-2 decoration-transparent hover:decoration-[var(--text-brand-secondary)]
      cursor-pointer outline-none">
      <span>{pageSize} per page</span>
      <ChevronDown className="w-4 h-4" />
    </button>
    {/* List box popover — same as default variant */}
  </div>

  {/* CENTER: Compact Page Navigation — icon-only prev/next */}
  <nav className="flex-1 flex items-center justify-center gap-1" aria-label="Pagination">
    <button className="w-8 h-8 rounded-md flex items-center justify-center
      text-[var(--text-brand-secondary)] bg-transparent hover:bg-[var(--bg-brand-secondary)]
      transition-all duration-150 ease-out cursor-pointer outline-none
      focus-visible:shadow-[0_0_0_4px_rgba(11,165,167,0.24),0_1px_2px_0_rgba(17,23,41,0.16)]">
      <ChevronLeft className="w-4 h-4" />
    </button>

    <span className="w-8 h-8 rounded-md flex items-center justify-center text-xs leading-[18px]
      bg-[var(--bg-secondary)] text-[var(--text-primary)] cursor-default pointer-events-none"
      style={{ fontWeight: 600 }} aria-current="page">1</span>

    <button className="w-8 h-8 rounded-md flex items-center justify-center text-xs leading-[18px]
      text-[var(--text-brand-secondary)] bg-transparent hover:bg-[var(--bg-brand-secondary)] hover:shadow-sm
      transition-all duration-150 ease-out cursor-pointer outline-none"
      style={{ fontWeight: 600 }}>2</button>

    <button className="w-8 h-8 rounded-md flex items-center justify-center text-xs leading-[18px]
      text-[var(--text-brand-secondary)] bg-transparent hover:bg-[var(--bg-brand-secondary)] hover:shadow-sm
      transition-all duration-150 ease-out cursor-pointer outline-none"
      style={{ fontWeight: 600 }}>3</button>

    <span className="w-8 h-8 flex items-center justify-center text-xs leading-[18px] text-[var(--text-brand-secondary)] cursor-default"
      style={{ fontWeight: 600 }}>...</span>

    <button className="w-8 h-8 rounded-md flex items-center justify-center text-xs leading-[18px]
      text-[var(--text-brand-secondary)] bg-transparent hover:bg-[var(--bg-brand-secondary)] hover:shadow-sm
      transition-all duration-150 ease-out cursor-pointer outline-none"
      style={{ fontWeight: 600 }}>10</button>

    <button className="w-8 h-8 rounded-md flex items-center justify-center
      text-[var(--text-brand-secondary)] bg-transparent hover:bg-[var(--bg-brand-secondary)]
      transition-all duration-150 ease-out cursor-pointer outline-none
      focus-visible:shadow-[0_0_0_4px_rgba(11,165,167,0.24),0_1px_2px_0_rgba(17,23,41,0.16)]">
      <ChevronRight className="w-4 h-4" />
    </button>
  </nav>
</div>


{/* === Previous button — disabled (at first page) === */}
<button
  className="
    inline-flex items-center gap-1
    h-8 px-2 rounded-md
    text-[var(--text-brand-secondary)] bg-transparent
    opacity-40 cursor-not-allowed pointer-events-none
  "
  style={{ fontWeight: 600 }}
  disabled
>
  <ChevronLeft className="w-4 h-4" />
  <span className="text-xs leading-[18px]">Previous</span>
</button>
```

### Key Implementation Rules

1. **Page navigation is CENTERED** — use `flex-1 flex justify-center` on the `<nav>`. Do NOT use `justify-between` on the container.
2. **Page size selector is absolute-left** — positioned with `absolute left-0` so it doesn't affect centering.
3. **Page size selector is a secondary link button** — transparent bg, teal text, no border, underline on hover. It is NOT a `<select>` element.
4. **Clicking page size opens a list box** (popover) — use styling from `list-box.md`. Selected item has teal checkmark.
5. **Page buttons are 32px x 32px** with 6px border-radius (`rounded-md`)
6. **Typography is 12px/18px/600** (title-sm) for page buttons — use inline `style={{ fontWeight: 600 }}`
7. **Page size button typography is 14px/20px/600** (text-sm font-semibold) — from the secondary link button spec
8. **Default text color is teal** `var(--text-brand-secondary)` (#098486) — NOT gray or black
9. **Current page has gray background** `var(--bg-secondary)` (#F1F4F7) with dark text `var(--text-primary)` (#353E5A) — NOT teal
10. **Current page is non-interactive** — `cursor-default pointer-events-none`, use `<span>` not `<button>`
11. **Hover background is light teal** `var(--bg-brand-secondary)` (#E7FDFD) with shadow-sm
12. **Focus ring is teal** — `var(--ring-brand-secondary)` (`0 0 0 4px rgba(11,165,167,0.24)` + small shadow)
13. **Gap between all buttons is 4px** (`gap-1`)
14. **Default variant**: text "Previous"/"Next" with chevron icons, max 9 visible pages
15. **Compact variant**: icon-only prev/next, max 5 visible pages
16. **Disabled prev/next at boundaries** — 40% opacity, cursor-not-allowed
17. **Ellipsis is non-interactive** — rendered as `<span>`, same teal color, `cursor-default`
18. **Container has 6px vertical padding** (`py-1.5`)
19. **Use `aria-current="page"`** on the current page element for accessibility
20. **Use `<nav>` with `aria-label="Pagination"`** for the page buttons wrapper
