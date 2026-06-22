# Grid Component — Strict Specification

When building any data grid, paginated data table, sortable/selectable data view, or server-driven tabular interface, follow these specifications exactly. Do NOT use shadcn DataTable, TanStack Table, or any other data grid library.

> **TOKEN RULE — MANDATORY:** Every color in this spec is expressed as a CSS custom-property token (with the raw hex shown only for reference in parentheses). When writing code, always use the `var(--token)` form — never hard-code a raw hex value.

**Note:** This component composes the Table (see `table.md`) and Paginator (see `paginator.md`). The Grid adds server-side data fetching, sorting, pagination, row selection, column resizing, and optional secondary (nested) rows on top of the base Table.

---

## Architecture

```
Grid (flex column container)
├── Selection Banner (shown when all current-page rows are selected)
│   ├── "All N rows on this page are selected."
│   ├── "Select all M records" hyperlink button (optional)
│   └── "Clear Selection" hyperlink button (when all-records selected)
├── Grid Container (flex-1, overflow-auto)
│   └── Scroll Shadow Wrapper (relative, overflow-hidden)
│       ├── Scroll Shadow overlays (top/bottom/left/right — see scroll-shadow spec)
│       └── Table (see table spec)
│           ├── Header Row (sticky)
│           │   ├── Checkbox column (if selectable)
│           │   └── Data columns (sortable / resizable / sticky)
│           ├── Primary Data Rows
│           │   ├── Checkbox cell (if selectable)
│           │   └── Data cells (default or custom template)
│           ├── Secondary Rows (if secondaryColumns provided)
│           │   └── Nested Table (per primary row with secondaryData)
│           │       └── Secondary cells (label + value stacked)
│           └── No Data Row (empty state)
└── Paginator (MANDATORY — see `paginator.md` — includes BOTH page size selector AND page buttons)
    ├── Left: Page size selector [25, 50, 100] (built into paginator)
    ├── Right: Previous / Next buttons (teal text `var(--text-brand-secondary)` (#098486))
    └── Right: Page number buttons (32×32px, current page has `var(--bg-secondary)` (#F1F4F7) bg)
```

**⚠️ THREE MANDATORY SUB-COMPONENTS:** Every grid MUST include:
1. **Table** — built from `table.md`
2. **Scroll Shadows** — on the table container, from `scroll-shadow.md`
3. **Paginator** — from `paginator.md` — this ALREADY includes the page size selector (25, 50, 100). Do NOT build a separate page size dropdown — it's part of the Paginator component.

NEVER substitute any of these with generic alternatives (no "Showing 1-25 of 100" text, no plain prev/next arrows, no missing scroll shadows).

---

## Grid Container

```jsx
<div className="flex flex-col" style={{ height: '100%' /* or a fixed height */ }}>
  {/* Selection banner — conditionally shown */}

  {/* Grid container with scroll shadows */}
  <div className="flex-1 relative overflow-hidden">
    {/* Scroll shadow overlays — see scroll-shadow.md */}
    {showTopShadow && (
      <div className="absolute top-0 left-0 w-full z-10 pointer-events-none"
        style={{ height: '12px', opacity: 0.1,
          background: 'linear-gradient(180deg, var(--shadow-start, #000) 0%, var(--shadow-md, #9298A9) 40%, transparent 100%)' }} />
    )}
    {showBottomShadow && (
      <div className="absolute bottom-0 left-0 w-full z-10 pointer-events-none"
        style={{ height: '12px', opacity: 0.1,
          background: 'linear-gradient(0deg, var(--shadow-start, #000) 0%, var(--shadow-md, #9298A9) 40%, transparent 100%)' }} />
    )}
    {showLeftShadow && (
      <div className="absolute top-0 left-0 h-full z-10 pointer-events-none"
        style={{ width: '12px', opacity: 0.1,
          background: 'linear-gradient(90deg, var(--shadow-start, #000) 0%, var(--shadow-md, #9298A9) 40%, transparent 100%)' }} />
    )}
    {showRightShadow && (
      <div className="absolute top-0 right-0 h-full z-10 pointer-events-none"
        style={{ width: '12px', opacity: 0.1,
          background: 'linear-gradient(270deg, var(--shadow-start, #000) 0%, var(--shadow-md, #9298A9) 40%, transparent 100%)' }} />
    )}

    {/* Scrollable table container — ref needed for overflow check */}
    <div ref={scrollRef} className="overflow-auto h-full" onScroll={checkOverflow}>
      {/* Table built per table spec */}
    </div>
  </div>

  {/* MANDATORY Paginator — use paginator.md */}
  {/* Teal page-number buttons, NOT a generic "Showing X of Y" text */}
  <Paginator
    totalItems={totalItems}
    pageSize={pageSize}
    currentPage={page}
    pageSizeOptions={[25, 50, 100]}
    disabled={loading}
    onPageChange={setPage}
    onPageSizeChange={setPageSize}
  />
</div>
```

---

## Column Configuration

Each column is defined as an object:

```ts
interface GridColumn {
  id: string;            // unique column key
  label: string;         // header text
  width?: string;        // CSS width e.g. '200px', '30%'
  sortable?: boolean;    // enables sort header
  sticky?: boolean;      // sticky left column
  dataAccessKey?: string; // dot-path key to access nested data e.g. 'user.name'
  custom?: boolean;      // true = column template provided via children, not auto-generated
}
```

**Non-custom columns** are auto-rendered using `dataAccessKey` to read the value from the row object. If the value is empty/null and the column has a label, show `'-'` as placeholder.

**Custom columns** (`custom: true`) are excluded from auto-generation. The consumer provides the column template via children (see Custom Columns section below).

---

## Data Source Pattern

The grid expects a function, NOT a static array:

```ts
type GridDataSource = (params: GridParams) => Promise<{ data: RowType[]; totalItems: number }>;

interface GridParams {
  pageNumber: number;    // 1-based
  pageSize: number;      // 25, 50, or 100
  sortBy: string;        // column id or ''
  sortDirection: 'asc' | 'desc';
}
```

The grid calls this function whenever sort, page, or page size changes. Implement in React as a callback that fetches data:

```jsx
const fetchData = useCallback(async ({ pageNumber, pageSize, sortBy, sortDirection }) => {
  const res = await fetch(`/api/items?page=${pageNumber}&size=${pageSize}&sort=${sortBy}&dir=${sortDirection}`);
  const json = await res.json();
  return { data: json.items, totalItems: json.total };
}, []);
```

### Page Reset Rules
- Page resets to **1** when: sort changes, data source changes, or page size changes.
- Page is preserved when: only the page number itself changes.

### Page Size Options
Default: `[25, 50, 100]` — the page size selector is built into the Paginator component (see `paginator.md`). Do NOT build a separate dropdown.

---

## Selection System

When selection is enabled, a checkbox column is prepended to the table.

**IMPORTANT:** The checkboxes in this column MUST use the exact Checkbox component from `checkbox.md` — same 16×16 box, same `var(--bg-brand-secondary-solid)` (#098486) teal fill, same `var(--border-primary)` (#BDC8D3) default border, same SVG icons. NEVER build a different checkbox.

### Checkbox Column

| Property | Value |
|----------|-------|
| Column width | **40px** fixed |
| Sticky | optional (via `selectionSticky`) |
| Next cell padding-left | **0** (padding removed from the cell immediately after checkbox column) |
| Checkbox style | Standalone (no label) — see `checkbox.md` |

### Header Checkbox States

| State | Checked | Indeterminate |
|-------|---------|---------------|
| No rows selected | ✗ | ✗ |
| Some rows on page selected | ✗ | ✓ |
| All rows on current page selected | ✓ | ✗ |

Clicking the header checkbox toggles: if all current-page rows are selected → clear all; otherwise → select all on current page.

### Row Checkbox
- Each row gets an independent checkbox
- `checkIfItemFrozen` callback can disable specific row checkboxes
- Click on checkbox does NOT propagate to row click

### Selection Banner

Shown only when **all rows on the current page** are selected:

```jsx
{/* Banner — all current page rows selected, but not all records */}
<p style={{
  fontSize: '14px',          // paragraph-md
  lineHeight: '20px',
  fontWeight: 400,
  color: 'var(--text-primary)',          // (#353E5A)
  marginBottom: '8px',       // mb-2
}}>
  All {currentPageItems} rows on this page are selected.
  {currentPageItems < totalItems && (
    <button style={{
      /* hyperlink button, secondary variant — see button spec */
      marginLeft: '6px',     // ml-1.5
      color: 'var(--text-brand-secondary)',      // (#098486) hyperlink
      background: 'none',
      border: 'none',
      cursor: 'pointer',
      textDecoration: 'underline',
      fontSize: '14px',
      fontWeight: 500,
    }}>
      Select all {totalItems.toLocaleString()} records
    </button>
  )}
</p>

{/* Banner — all records selected */}
<p style={{ /* same paragraph-md style */ }}>
  All {totalItems.toLocaleString()} records selected.
  <button style={{ /* same hyperlink style, marginLeft: 6px */ }}>
    Clear Selection
  </button>
</p>
```

---

## Sorting

Sortable columns (where `sortable: true`) display a sort indicator in the header cell. Use the sort header pattern from the table spec.

- Clicking a sortable header cycles: none → ascending → descending → none
- Sort change resets page to 1
- Only one column can be sorted at a time

---

## Column Resizing

Non-sticky columns are resizable by dragging the right edge of the header cell.

- **Min width:** 150px (default)
- **Max width:** 2000px (default)
- During resize: header cell gets `backgroundColor: 'var(--bg-brand-secondary_alt)'` (#E7FDFD)
- Body gets `user-select: none` during drag

---

## Secondary (Nested) Rows

When `secondaryColumns` is provided, each primary row that has a `secondaryData` array renders an expandable nested table below it.

### Secondary Row Anatomy

```
Primary Row (64px or 40px compact)
└── Secondary Row(s) — one per item in secondaryData
    ├── Empty placeholder cell (if selectable — aligns with checkbox column)
    └── Data cells — each shows:
        ├── Column label (12px, `var(--text-tertiary)` (#6B748E), weight 400)
        └── Cell value (14px, `var(--text-primary)` (#353E5A), weight 400)
```

### Secondary Row Cell — Default Template

```jsx
<td style={{
  backgroundColor: 'var(--bg-secondary_subtle)',  // (#F7F9FA)
  verticalAlign: 'middle',
  padding: '6px 16px',
}}>
  <p style={{
    fontSize: '12px',          // paragraph-sm
    lineHeight: '18px',
    fontWeight: 400,
    color: 'var(--text-tertiary)',          // (#6B748E)
    overflow: 'hidden',
    textOverflow: 'ellipsis',
    whiteSpace: 'nowrap',
  }}>
    {column.label}
  </p>
  <p style={{
    fontSize: '14px',          // paragraph-md
    lineHeight: '20px',
    fontWeight: 400,
    color: 'var(--text-primary)',          // (#353E5A)
    overflow: 'hidden',
    textOverflow: 'ellipsis',
    whiteSpace: 'nowrap',
  }}>
    {cellValue || (column.label ? '-' : '')}
  </p>
</td>
```

### Secondary Row Expanded Background

When the secondary row has fewer columns than the primary row (common), the row gets a full-width background overlay:

```jsx
style={{
  backgroundColor: 'var(--bg-secondary_subtle)',           // (#F7F9FA)
  borderTop: '1px solid var(--border-tertiary)',       // (#E8ECF1)
  maxHeight: '64px',                     // normal row
  // maxHeight: '40px'                   // compact row
}}
// On hover:
style={{ backgroundColor: 'var(--bg-secondary)' }}  // (#F1F4F7)
```

### Hierarchy Connector Lines

Secondary rows show L-shaped connector lines from the parent row (see table spec for full connector styling):
- Left offset: **24px**
- Width: **8px**
- Height: **68px** (64px row + 4px extension)
- Border: `1px solid var(--border-secondary)` (#CCD6DC) on left and bottom edges
- Corner radius: **4px** (bottom-left)
- First secondary cell gets **44px** left padding

---

## Compact Rows

Both primary and secondary rows support compact mode:

| Mode | Row Height | Padding V | Padding H |
|------|-----------|-----------|-----------|
| Normal | 64px | 6px | 16px |
| Compact | 40px | 2px | 16px |

Apply `primaryRowCompact` for primary rows, `secondaryRowCompact` for secondary rows. These are independent — you can mix normal primary with compact secondary.

---

## Loading State

While data is being fetched (`isLoading`):
- A white overlay covers the entire table content area (see table spec loading overlay)
- The paginator is disabled
- Previous data rows are hidden
- Skeleton loaders can optionally be shown

---

## Empty / No Data State

When the data source returns zero items, render a full-width empty state inside the table body. Use the Empty State component (see `empty-state.md`):

```jsx
<EmptyState
  title="No Results Found"
  description="Try adjusting your search or filters."
  imageSrc="/path/to/illustration.svg"
/>
```

---

## Custom Column Templates

Columns with `custom: true` must have their cell templates provided as children. The template receives `{ element, columnDef }`:

```jsx
{/* Example: custom "Created" column with sticky + sortable */}
{columns.find(c => c.id === 'created' && c.custom) && (
  <th style={{ position: 'sticky', left: 0, width: '400px', /* header cell styles */ }}>
    Created At {/* with sort indicator if sortable */}
  </th>
  // ...
  <td style={{ position: 'sticky', left: 0 }}>
    {formatDate(row.created)}
  </td>
)}
```

---

## Complete Implementation Pattern

```jsx
function DataGrid({ fetchData, columns, secondaryColumns, selectable }) {
  const [data, setData] = useState([]);
  const [totalItems, setTotalItems] = useState(0);
  const [loading, setLoading] = useState(true);
  const [page, setPage] = useState(1);
  const [pageSize, setPageSize] = useState(25);
  const [sortBy, setSortBy] = useState('');
  const [sortDir, setSortDir] = useState('asc');
  const [selectedRows, setSelectedRows] = useState(new Set());
  const [allRecordsSelected, setAllRecordsSelected] = useState(false);

  // Scroll shadow state — shadows ONLY appear when content overflows
  const scrollRef = useRef(null);
  const [showTopShadow, setShowTopShadow] = useState(false);
  const [showBottomShadow, setShowBottomShadow] = useState(false);
  const [showLeftShadow, setShowLeftShadow] = useState(false);
  const [showRightShadow, setShowRightShadow] = useState(false);

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

  // Re-check overflow on mount and after data changes
  useEffect(() => { checkOverflow(); }, [data, checkOverflow]);

  // Fetch data on sort/page/pageSize change
  useEffect(() => {
    setLoading(true);
    fetchData({ pageNumber: page, pageSize, sortBy, sortDirection: sortDir })
      .then(({ data, totalItems }) => {
        setData(data);
        setTotalItems(totalItems);
        setLoading(false);
      });
  }, [page, pageSize, sortBy, sortDir, fetchData]);

  // Reset page to 1 when sort or pageSize changes
  useEffect(() => { setPage(1); }, [sortBy, sortDir, pageSize]);

  const isCurrentPageSelected = selectedRows.size > 0
    && selectedRows.size === data.length;
  const totalPages = Math.ceil(totalItems / pageSize);

  return (
    <div className="flex flex-col" style={{ height: '100%' }}>
      {/* Selection Banner */}
      {isCurrentPageSelected && (
        <p style={{
          fontSize: '14px', lineHeight: '20px', fontWeight: 400,
          color: 'var(--text-primary)', marginBottom: '8px',
        }}>
          {allRecordsSelected
            ? `All ${totalItems.toLocaleString()} records selected.`
            : `All ${data.length} rows on this page are selected.`
          }
          {/* Hyperlink button — Select all / Clear selection */}
        </p>
      )}

      {/* Grid Container with MANDATORY scroll shadows */}
      <div className="flex-1 relative overflow-hidden">
        {/* Scroll shadows — from scroll-shadow.md */}
        {!loading && showTopShadow && (
          <div className="absolute top-0 left-0 w-full z-10 pointer-events-none"
            style={{ height: '12px', opacity: 0.1,
              background: 'linear-gradient(180deg, var(--shadow-start, #000) 0%, var(--shadow-md, #9298A9) 40%, transparent 100%)' }} />
        )}
        {!loading && showBottomShadow && (
          <div className="absolute bottom-0 left-0 w-full z-10 pointer-events-none"
            style={{ height: '12px', opacity: 0.1,
              background: 'linear-gradient(0deg, var(--shadow-start, #000) 0%, var(--shadow-md, #9298A9) 40%, transparent 100%)' }} />
        )}
        {!loading && showLeftShadow && (
          <div className="absolute top-0 left-0 h-full z-10 pointer-events-none"
            style={{ width: '12px', opacity: 0.1,
              background: 'linear-gradient(90deg, var(--shadow-start, #000) 0%, var(--shadow-md, #9298A9) 40%, transparent 100%)' }} />
        )}
        {!loading && showRightShadow && (
          <div className="absolute top-0 right-0 h-full z-10 pointer-events-none"
            style={{ width: '12px', opacity: 0.1,
              background: 'linear-gradient(270deg, var(--shadow-start, #000) 0%, var(--shadow-md, #9298A9) 40%, transparent 100%)' }} />
        )}

        {/* Scrollable table — built per table.md */}
        <div ref={scrollRef} className="overflow-auto h-full" onScroll={checkOverflow}
          style={{ border: '1px solid var(--border-secondary)', borderRadius: '8px' }}>
          <table style={{
            display: 'table', tableLayout: 'fixed',
            minWidth: '100%', width: '100%',
            borderCollapse: 'collapse',
            outline: '1px solid var(--border-tertiary)',
          }}>
            {/* thead / tbody per table spec */}
          </table>
        </div>
      </div>

      {/* MANDATORY Paginator — from paginator.md */}
      {/* Page size selector = absolute LEFT (link button), page nav = CENTERED */}
      {/* NEVER replace with "Showing X of Y" text or plain arrows */}
      <div className="relative flex items-center w-full py-1.5">
        {/* LEFT: Page Size Selector — secondary link button, absolute positioned */}
        <div className="absolute left-0">
          <button
            className="inline-flex items-center gap-1 bg-transparent border-none p-0
              text-[var(--text-brand-secondary)] text-sm leading-5 font-semibold
              underline-offset-2 decoration-transparent hover:decoration-[var(--text-brand-secondary)]
              cursor-pointer outline-none"
            onClick={() => setShowPageSizeMenu(!showPageSizeMenu)}
          >
            <span>{pageSize} per page</span>
            <ChevronDown className="w-4 h-4" />
          </button>
          {/* List box popover for page size — see list-box.md & paginator.md */}
        </div>

        {/* CENTER: Page Navigation Buttons — centered in full width */}
        <nav className="flex-1 flex items-center justify-center gap-1" aria-label="Pagination">
          {/* Previous button */}
          <button className="inline-flex items-center gap-1 h-8 px-2 rounded-md
            text-[var(--text-brand-secondary)] hover:bg-[var(--bg-brand-secondary)] transition-all duration-150 ease-out"
            style={{ fontWeight: 600 }}
            disabled={page === 1}
            onClick={() => setPage(p => p - 1)}>
            <ChevronLeft className="w-4 h-4" />
            <span className="text-xs leading-[18px]">Previous</span>
          </button>

          {/* Page number buttons — 32×32px each */}
          {getPageNumbers(page, totalPages).map((p) =>
            p === '...' ? (
              <span key={p} className="w-8 h-8 flex items-center justify-center
                text-xs leading-[18px] text-[var(--text-brand-secondary)] cursor-default"
                style={{ fontWeight: 600 }}>...</span>
            ) : p === page ? (
              <span key={p} className="w-8 h-8 rounded-md flex items-center justify-center
                text-xs leading-[18px] bg-[var(--bg-secondary)] text-[var(--text-primary)] cursor-default"
                style={{ fontWeight: 600 }} aria-current="page">{p}</span>
            ) : (
              <button key={p} className="w-8 h-8 rounded-md flex items-center justify-center
                text-xs leading-[18px] text-[var(--text-brand-secondary)] hover:bg-[var(--bg-brand-secondary)] hover:shadow-sm
                transition-all duration-150 ease-out cursor-pointer"
                style={{ fontWeight: 600 }}
                onClick={() => setPage(p)}>{p}</button>
            )
          )}

          {/* Next button */}
          <button className="inline-flex items-center gap-1 h-8 px-2 rounded-md
            text-[var(--text-brand-secondary)] hover:bg-[var(--bg-brand-secondary)] transition-all duration-150 ease-out"
            style={{ fontWeight: 600 }}
            disabled={page === totalPages}
            onClick={() => setPage(p => p + 1)}>
            <span className="text-xs leading-[18px]">Next</span>
            <ChevronRight className="w-4 h-4" />
          </button>
        </nav>
      </div>
    </div>
  );
}
```

---

## Colors Reference

| Element | Property | Hex | Token |
|---------|----------|-----|-------|
| Selection banner text | color | `var(--text-primary)` (#353E5A) | text-primary |
| Selection banner link | color | `var(--text-brand-secondary)` (#098486) | text-brand-secondary |
| Selection banner link marginLeft | | 6px | spacing(1.5) |
| Selection banner marginBottom | | 8px | spacing(2) |
| Selectable column divider (header) | background | `var(--bg-secondary)` (#F1F4F7) | bg-secondary |
| Secondary row cell bg | background | `var(--bg-secondary_subtle)` (#F7F9FA) | bg-secondary-subtle |
| Secondary row hover bg | background | `var(--bg-secondary)` (#F1F4F7) | bg-primary-hover |
| Secondary row expanded border-top | border | `var(--border-tertiary)` (#E8ECF1) | border-tertiary |
| Secondary cell label text | color | `var(--text-tertiary)` (#6B748E) | text-tertiary |
| Connector line | border | `var(--border-secondary)` (#CCD6DC) | border-secondary |
| Loading overlay bg | background | `var(--bg-primary)` (#FFFFFF) | white |

All other table colors (header, cells, borders, hover) — see `table.md`.

---

## Dimensions Reference

| Element | Value |
|---------|-------|
| Checkbox column width | 40px |
| Primary row height (normal) | 64px |
| Primary row height (compact) | 40px |
| Secondary row height (normal) | 64px |
| Secondary row height (compact) | 40px |
| Header row height | 36px |
| Cell padding (normal) | 6px 16px |
| Cell padding (compact) | 2px 16px |
| Secondary first cell padding-left | 44px |
| Connector line left | 24px |
| Connector line width | 8px |
| Connector line height | 68px |
| Connector corner radius | 4px |
| Table border-radius | 8px |
| Page size options | [25, 50, 100] |

---

## CRITICAL Rules

1. **Grid = Table + Scroll Shadows + Paginator + Data Fetching.** Always compose from all three sub-components. NEVER skip any of them.
2. **MANDATORY: Use the Paginator from `paginator.md`.** The paginator has teal `var(--text-brand-secondary)` (#098486) page-number buttons (32×32px), Previous/Next with chevron icons, ellipsis truncation, and current-page highlight `var(--bg-secondary)` (#F1F4F7) bg. NEVER replace it with a generic "Showing 1-25 of 100" text, plain prev/next arrows, or any other pagination pattern.
3. **MANDATORY: Use Scroll Shadows from `scroll-shadow.md`.** The table container MUST have gradient scroll shadows (12px thick, 10% opacity, black→`var(--shadow-md)` (#9298A9)→transparent) on all four edges. Shadows ONLY appear when content actually overflows — always check `scrollHeight > clientHeight` / `scrollWidth > clientWidth` before showing. Re-check on mount and after data changes, not just on scroll. NEVER skip scroll shadows.
4. **Data source is a FUNCTION, not a static array.** The grid calls it with `{ pageNumber, pageSize, sortBy, sortDirection }` and expects `{ data, totalItems }` in return.
5. **Page resets to 1** when sort, data source, or page size changes. Only page number changes preserve the current page.
6. **Checkbox column is always 40px wide** and prepended as the first column. The cell immediately after it has `padding-left: 0`. Checkboxes MUST use the spec from `checkbox.md`.
7. **Selection banner appears only when ALL rows on the current page are selected** — not when just some are selected.
8. **"Select all N records"** button (for server-side select-all) only appears when `currentPageItems < totalItems`.
9. **Default cell rendering** uses deep property access via `dataAccessKey` (supports dot-notation like `'user.name'`). Shows `'-'` for empty values when the column has a label.
10. **Secondary row default template** stacks the column label (paragraph-sm, text-tertiary) above the value (paragraph-md, text-primary).
11. **Compact and normal modes are independent** for primary and secondary rows — you can mix them.
12. **Loading disables the paginator** and shows a white overlay over the table content. Scroll shadows are hidden during loading.
13. **Empty state is projected inside the table** — use the Empty State component spec.
14. **Page size options are always `[25, 50, 100]`** unless explicitly overridden.
