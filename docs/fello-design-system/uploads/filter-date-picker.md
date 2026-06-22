# Filter Date Picker — Strict Specification

When building a date range filter, follow these specifications exactly. The filter date picker is an inline label + hyperlink-style button that opens a calendar dropdown overlay for selecting a date range.

> **TOKEN RULE — DO NOT use raw hex colors.**
> Every color value in this spec is expressed as a CSS custom-property token.
> In Tailwind classes use the `[var(--token)]` syntax (e.g. `text-[var(--text-primary)]`).
> In inline `style` objects use the `'var(--token)'` syntax.
> The parenthetical hex after each token is for reference only — never emit it in code.

---

## Architecture

```
Inline Trigger:
┌──────────────────────────────────────────────────┐
│  Date Range  Feb 19, 2026 – Feb 25, 2026  ▾     │
│  ↑ Label     ↑ Hyperlink button (teal, underline on hover)
└──────────────────────────────────────────────────┘

Calendar Dropdown:
┌────────────────┬──────────────────────────────────────────────┐
│  Presets        │           ◀  February 2026  ▶              │
│                │  Mo  Tu  We  Th  Fr  Sa  Su                 │
│  Today         │  ──────────────────────────────             │
│  Last 7 Days ✓ │  26  27  28  29  30  31   1                │
│  Last 30 Days  │   2   3   4   5   6   7   8                │
│  This Month    │   9  10  11  12  13  14  15                │
│  Last Month    │  16  17  18 [19  20  21  22]               │
│  This Year     │  23  24 [25] 26  27  28   1                │
│  Last Year     │                                             │
│                │                      Cancel    Apply        │
└────────────────┴──────────────────────────────────────────────┘
      ↑ Sidebar              ↑ Calendar grid
```

---

## Inline Trigger

### Container

| Property | Value | Tailwind |
|----------|-------|----------|
| Display | inline-flex | `inline-flex` |
| Align items | center | `items-center` |
| Gap | 6px | `gap-1.5` |
| Position | relative | `relative` |

### Label

| Property | Value | Tailwind |
|----------|-------|----------|
| White space | nowrap | `whitespace-nowrap` |
| Color | `var(--text-secondary)` (#495883) | `text-[var(--text-secondary)]` |

**Label typography by size:**

| Size | Font Size | Line Height | Weight |
|------|-----------|-------------|--------|
| SM | 12px | 18px | 500 |
| MD | 14px | 20px | 500 |
| LG | 14px | 20px | 500 |

### Trigger Button (Hyperlink Style)

The button is a borderless hyperlink-style button in the **secondary** (teal) color variant.

| Property | Value | Tailwind |
|----------|-------|----------|
| Background | transparent | `bg-transparent` |
| Padding | 0 | `p-0` |
| Height | auto | `h-auto` |
| Color | `var(--text-brand-secondary)` (#098486) | `text-[var(--text-brand-secondary)]` |
| Cursor | pointer | `cursor-pointer` |
| Border | none | `border-none` |
| Text decoration | underline (transparent by default) | — |
| Hover/Focus | underline becomes visible (inherits color) | — |

**Button typography by size:**

| Size | Font Size | Line Height | Weight |
|------|-----------|-------------|--------|
| SM | 12px | 18px | 600 |
| MD | 14px | 20px | 600 |
| LG | 16px | 24px | 600 |

### Trailing Chevron Icon

A small down-arrow chevron icon appears after the button text.

| Property | Value |
|----------|-------|
| Icon | Chevron down (▾) |
| Color | Inherits button color `var(--fg-brand-secondary)` (#098486) |

### Display Format

Selected date range displays as: `MMM DD, YYYY – MMM DD, YYYY`

Example: `Feb 19, 2026 – Feb 25, 2026`

---

## Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `label` | `string` | `'Select'` | Label text before button |
| `placeholder` | `string` | `'Select'` | Placeholder when no date selected |
| `size` | `'sm' \| 'md' \| 'lg'` | `'md'` | Controls all sizing |
| `disabled` | `boolean` | `false` | Disables the date picker |
| `ranges` | `object` | — | Preset date ranges for sidebar |
| `singleDatePicker` | `boolean` | `false` | Single date vs range mode |

---

## Calendar Dropdown Panel

### Panel Container

| Property | Value |
|----------|-------|
| Background | `var(--bg-primary)` (#FFFFFF) |
| Border | 1px solid `var(--border-primary)` (#BDC8D3) |
| Border radius | 4px |
| Box shadow | `0 2px 4px -2px var(--shadow-md)` (#4958830a / #49588329) |
| Display | flex (when open) |
| Font | Inter, Arial, sans-serif |

### Positioning

Opens below the trigger (4px offset), with fallback positions:
1. Below, aligned left (default)
2. Above, aligned left
3. Below, aligned right
4. Above, aligned right

---

## Calendar Grid

### Month/Year Header

| Property | Value |
|----------|-------|
| Color | `var(--text-primary)` (#353E5A) |
| Font weight | 600 |
| Navigation | Left/right arrows for month navigation |

### Weekday Row

| Property | Value |
|----------|-------|
| Color | `var(--text-tertiary)` (#6B748E) |
| Border bottom | 1px solid `var(--border-tertiary)` (#E8ECF1) |

### Calendar Container Padding

| Area | Padding |
|------|---------|
| Top | 8px |
| Bottom | 64px (space for action buttons) |
| Left calendar left | 16px |
| Right calendar left + right | 16px each |
| Calendar table inner | 8px |

### Day Cell Dimensions

| Size | Cell Height | Cell Min Width | Selected Circle |
|------|-------------|----------------|-----------------|
| SM | 32px | 32px | 24×24px |
| MD | 40px | 40px | 32×32px |
| LG | 48px | 48px | 40×40px |

### Day Cell Typography

| Size | Font Size | Line Height | Weight |
|------|-----------|-------------|--------|
| SM | 12px | 18px | 500 |
| MD | 14px | 20px | 500 |
| LG | 16px | 24px | 500 |

---

## Day Cell States

### Default (Available)

| Property | Value |
|----------|-------|
| Color | `var(--text-secondary)` (#495883) |
| Border radius | 8px |
| Background | transparent |
| Hover background | `var(--bg-secondary)` (#F1F4F7) |

### Today (not selected, not in-range)

| Property | Value |
|----------|-------|
| Color | `var(--text-brand-secondary)` (#098486) |
| Background | `var(--bg-secondary)` (#F1F4F7) |
| Border radius | 8px |
| Hover background | `var(--bg-secondary_hover)` (#E8ECF1) |

### Selected (Active) — Start/End Date

The selected date renders a teal circle as a pseudo-element behind the date number.

| Property | Value |
|----------|-------|
| Cell background | `var(--bg-brand-secondary)` (#E7FDFD) |
| Circle background | `var(--fg-brand-secondary_alt)` (#0BA5A7) |
| Circle hover bg | `var(--fg-brand-secondary)` (#098486) |
| Circle border radius | fully rounded (pill) |
| Circle box shadow | `0 0 0 4px var(--ring-brand-secondary)` (#0BA5A73d) |
| Text color | `var(--text-white)` (#FFFFFF) |

### Single Date (Start = End, no range)

| Property | Value |
|----------|-------|
| Cell background | transparent (NOT teal bg) |
| Circle | Same teal circle as above |
| Text color | `var(--text-white)` (#FFFFFF) |

### In-Range (between start and end)

| Property | Value |
|----------|-------|
| Background | `var(--bg-brand-secondary)` (#E7FDFD) |
| Border radius | none (flat edges for continuous range band) |
| Hover background | `var(--bg-brand-secondary_alt)` (#CAFBFB) |

### Start Date (range)

| Property | Value |
|----------|-------|
| Left border radius | fully rounded (pill) |
| Right border radius | 0 |

### End Date (range)

| Property | Value |
|----------|-------|
| Left border radius | 0 |
| Right border radius | fully rounded (pill) |

### Off Dates (previous/next month days)

| Property | Value |
|----------|-------|
| Color | `var(--text-disabled)` (#8EA1AF) |

### Disabled Dates

| Property | Value |
|----------|-------|
| Color | `var(--text-disabled)` (#8EA1AF) |
| Cursor | not-allowed |

---

## Range Presets Sidebar

### Sidebar Container

| Property | Value |
|----------|-------|
| Padding | 16px |
| Border right | 1px solid `var(--border-primary)` (#BDC8D3) |

### Preset Options

Default presets:

| Label | Description |
|-------|-------------|
| Today | Start of today → end of today |
| Last 7 Days | 6 days ago → today |
| Last 30 Days | 29 days ago → today |
| This Month | Start of current month → end of current month |
| Last Month | Start of previous month → end of previous month |
| This Year | Start of current year → end of current year |
| Last Year | Start of previous year → end of previous year |

### Preset Button Styling

| Property | Value |
|----------|-------|
| Display | flex, space-between, center |
| Background | transparent |
| Margin | 2px 4px |
| Border radius | 4px |
| Border | none |
| Cursor | pointer |

**Preset button size variants:**

| Size | Font Size | Line Height | Weight | Padding | Min Width |
|------|-----------|-------------|--------|---------|-----------|
| SM | 12px | 18px | 500 | 4px 8px | 140px |
| MD | 14px | 20px | 500 | 6px 12px | 152px |
| LG | 14px | 20px | 500 | 8px 12px | 172px |

### Preset Button States

| State | Background |
|-------|------------|
| Default | transparent |
| Hover | `var(--bg-secondary)` (#F1F4F7) |
| Active (selected) | `var(--bg-secondary)` (#F1F4F7) |
| Active + hover | `var(--bg-secondary_hover)` (#E8ECF1) |

### Active Check Icon

The active preset shows a small teal checkmark on the right side.

| Property | Value |
|----------|-------|
| Icon | Checkmark (✓) |
| Color | `var(--fg-brand-secondary)` (#098486) |
| Size | 12×8px |
| Stroke width | 1.5px |

---

## Action Buttons (Cancel / Apply)

### Button Container

| Property | Value |
|----------|-------|
| Position | absolute |
| Right | 20px |
| Bottom | 20px |
| Display | flex |
| Gap | 16px |

### Button Styling

Both Cancel and Apply use the same base styling (outlined buttons):

| Property | Value |
|----------|-------|
| Background | `var(--bg-primary)` (#FFFFFF) |
| Color | `var(--text-primary)` (#353E5A) |
| Border | 1px solid `var(--border-secondary)` (#CCD6DC) |
| Border radius | 4px |
| Cursor | pointer |
| Text transform | none |

**Button size variants:**

| Size | Font Size | Line Height | Weight | Padding |
|------|-----------|-------------|--------|---------|
| SM | 12px | 18px | 600 | 8px 12px |
| MD | 12px | 18px | 600 | 8px 12px |
| LG | 14px | 20px | 600 | 10px 16px |

### Button States

| State | Effect |
|-------|--------|
| Hover | `box-shadow: 0 1px 2px 0 var(--shadow-sm)` (#49588314), bg `var(--bg-secondary)` (#F1F4F7) |
| Focus | `box-shadow: 0 0 0 4px var(--ring-brand-primary)` (#FF725C3d), bg `var(--bg-secondary)` (#F1F4F7) |

---

## Complete Color Reference

| Token | Hex | Usage |
|-------|-----|-------|
| `var(--bg-primary)` | #FFFFFF | Panel background, button bg |
| `var(--text-primary)` | #353E5A | Month header, action button text |
| `var(--text-secondary)` | #495883 | Label, default day text |
| `var(--text-tertiary)` | #6B748E | Weekday header text |
| `var(--text-brand-secondary)` | #098486 | Trigger button text, today date text |
| `var(--text-disabled)` | #8EA1AF | Disabled/off dates |
| `var(--bg-secondary)` | #F1F4F7 | Day hover, today bg, preset hover |
| `var(--bg-secondary_hover)` | #E8ECF1 | Today hover, active preset hover |
| `var(--bg-brand-secondary)` | #E7FDFD | In-range bg, selected day cell bg |
| `var(--bg-brand-secondary_alt)` | #CAFBFB | In-range hover bg |
| `var(--fg-brand-secondary_alt)` | #0BA5A7 | Selected day circle |
| `var(--fg-brand-secondary)` | #098486 | Selected day circle hover, check icon |
| `var(--border-primary)` | #BDC8D3 | Panel border, sidebar border |
| `var(--border-secondary)` | #CCD6DC | Action button border |
| `var(--border-tertiary)` | #E8ECF1 | Weekday header border |

---

## Implementation

```jsx
import { useState, useRef, useEffect } from 'react';
import { ChevronDown, ChevronLeft, ChevronRight, Check } from 'lucide-react';
import dayjs from 'dayjs';

const PRESETS = [
  { label: 'Today', start: dayjs().startOf('day'), end: dayjs().endOf('day') },
  { label: 'Last 7 Days', start: dayjs().subtract(6, 'day').startOf('day'), end: dayjs().endOf('day') },
  { label: 'Last 30 Days', start: dayjs().subtract(29, 'day').startOf('day'), end: dayjs().endOf('day') },
  { label: 'This Month', start: dayjs().startOf('month'), end: dayjs().endOf('month') },
  { label: 'Last Month', start: dayjs().subtract(1, 'month').startOf('month'), end: dayjs().subtract(1, 'month').endOf('month') },
  { label: 'This Year', start: dayjs().startOf('year'), end: dayjs().endOf('year') },
  { label: 'Last Year', start: dayjs().subtract(1, 'year').startOf('year'), end: dayjs().subtract(1, 'year').endOf('year') },
];

function FilterDatePicker({
  label = 'Select',
  placeholder = 'Select',
  size = 'md',
  disabled = false,
  showPresets = true,
  value,
  onChange,
}) {
  const [isOpen, setIsOpen] = useState(false);
  const [startDate, setStartDate] = useState(value?.startDate || null);
  const [endDate, setEndDate] = useState(value?.endDate || null);
  const [activePreset, setActivePreset] = useState(null);
  const containerRef = useRef(null);

  // Size configs
  const labelSize = {
    sm: 'text-xs leading-[18px]',
    md: 'text-sm leading-5',
    lg: 'text-sm leading-5',
  }[size];

  const buttonSize = {
    sm: 'text-xs leading-[18px]',
    md: 'text-sm leading-5',
    lg: 'text-base leading-6',
  }[size];

  const cellSize = {
    sm: { cell: 'h-8 min-w-[32px] text-xs leading-[18px]', circle: 'w-6 h-6' },
    md: { cell: 'h-10 min-w-[40px] text-sm leading-5', circle: 'w-8 h-8' },
    lg: { cell: 'h-12 min-w-[48px] text-base leading-6', circle: 'w-10 h-10' },
  }[size];

  const displayValue = startDate && endDate
    ? `${startDate.format('MMM DD, YYYY')} – ${endDate.format('MMM DD, YYYY')}`
    : null;

  return (
    <div ref={containerRef} className="inline-flex items-center gap-1.5 relative">
      {/* Label */}
      <span
        className={`whitespace-nowrap font-medium ${labelSize}`}
        style={{ color: 'var(--text-secondary)', fontFamily: 'Inter, Arial, sans-serif' }}
      >
        {label}
      </span>

      {/* Trigger button */}
      <button
        type="button"
        disabled={disabled}
        className={`
          bg-transparent p-0 h-auto border-none cursor-pointer
          font-semibold ${buttonSize}
          disabled:opacity-50 disabled:cursor-not-allowed
        `}
        style={{ color: 'var(--text-brand-secondary)', fontFamily: 'Inter, Arial, sans-serif' }}
        onClick={() => !disabled && setIsOpen(!isOpen)}
      >
        <span className="underline decoration-transparent hover:decoration-current transition-colors">
          {displayValue || placeholder}
        </span>
        <ChevronDown size={16} className="inline-block ml-1" style={{ color: 'var(--fg-brand-secondary)' }} />
      </button>

      {/* Dropdown panel */}
      {isOpen && (
        <>
          {/* Backdrop */}
          <div className="fixed inset-0 z-40" onClick={() => setIsOpen(false)} />

          {/* Calendar panel */}
          <div
            className="absolute top-full left-0 mt-1 z-50 flex"
            style={{
              backgroundColor: 'var(--bg-primary)',
              border: '1px solid var(--border-primary)',
              borderRadius: '4px',
              boxShadow: '0 2px 4px -2px var(--shadow-md-1), 0 4px 8px -2px var(--shadow-md-2)',
              fontFamily: 'Inter, Arial, sans-serif',
            }}
          >
            {/* Presets sidebar */}
            {showPresets && (
              <div style={{ padding: '16px', borderRight: '1px solid var(--border-primary)' }}>
                <div className="flex flex-col">
                  {PRESETS.map((preset) => (
                    <button
                      key={preset.label}
                      type="button"
                      className="flex items-center justify-between border-none cursor-pointer"
                      style={{
                        margin: '2px 4px',
                        borderRadius: '4px',
                        padding: size === 'sm' ? '4px 8px' : size === 'lg' ? '8px 12px' : '6px 12px',
                        minWidth: size === 'sm' ? '140px' : size === 'lg' ? '172px' : '152px',
                        fontSize: size === 'sm' ? '12px' : '14px',
                        lineHeight: size === 'sm' ? '18px' : '20px',
                        fontWeight: 500,
                        color: 'var(--text-primary)',
                        backgroundColor: activePreset === preset.label ? 'var(--bg-secondary)' : 'transparent',
                      }}
                      onClick={() => {
                        setActivePreset(preset.label);
                        setStartDate(preset.start);
                        setEndDate(preset.end);
                      }}
                      onMouseEnter={(e) => {
                        if (activePreset !== preset.label) e.currentTarget.style.backgroundColor = 'var(--bg-secondary)';
                      }}
                      onMouseLeave={(e) => {
                        if (activePreset !== preset.label) e.currentTarget.style.backgroundColor = 'transparent';
                      }}
                    >
                      {preset.label}
                      {activePreset === preset.label && (
                        <Check size={12} style={{ color: 'var(--fg-brand-secondary)' }} strokeWidth={1.5} />
                      )}
                    </button>
                  ))}
                </div>
              </div>
            )}

            {/* Calendar area */}
            <div style={{ padding: '8px 16px 64px 16px', position: 'relative' }}>
              {/* Month/Year header */}
              <div className="flex items-center justify-between" style={{ padding: '8px' }}>
                <button type="button" className="p-1 rounded hover:bg-[var(--bg-secondary)]">
                  <ChevronLeft size={16} style={{ color: 'var(--fg-primary)' }} />
                </button>
                <span style={{ color: 'var(--text-primary)', fontWeight: 600, fontSize: '14px' }}>
                  February 2026
                </span>
                <button type="button" className="p-1 rounded hover:bg-[var(--bg-secondary)]">
                  <ChevronRight size={16} style={{ color: 'var(--fg-primary)' }} />
                </button>
              </div>

              {/* Calendar grid */}
              <table style={{ borderSpacing: 0, borderCollapse: 'separate', padding: '8px' }}>
                <thead>
                  <tr>
                    {['Mo', 'Tu', 'We', 'Th', 'Fr', 'Sa', 'Su'].map((d) => (
                      <th
                        key={d}
                        className={`text-center font-medium ${cellSize.cell}`}
                        style={{ color: 'var(--text-tertiary)', borderBottom: '1px solid var(--border-tertiary)' }}
                      >
                        {d}
                      </th>
                    ))}
                  </tr>
                </thead>
                <tbody>
                  {/* Day cells — render based on state */}
                  {/* Default day cell */}
                  <td
                    className={`text-center font-medium cursor-pointer ${cellSize.cell}`}
                    style={{ color: 'var(--text-secondary)', borderRadius: '8px', verticalAlign: 'middle' }}
                  >
                    15
                  </td>

                  {/* Today cell (not selected) */}
                  <td
                    className={`text-center font-medium cursor-pointer ${cellSize.cell}`}
                    style={{
                      color: 'var(--text-brand-secondary)',
                      backgroundColor: 'var(--bg-secondary)',
                      borderRadius: '8px',
                      verticalAlign: 'middle',
                    }}
                  >
                    25
                  </td>

                  {/* Selected (active) day cell */}
                  <td
                    className={`text-center font-medium cursor-pointer relative ${cellSize.cell}`}
                    style={{ backgroundColor: 'var(--bg-brand-secondary)', verticalAlign: 'middle' }}
                  >
                    {/* Teal circle behind text */}
                    <div
                      className={`absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 rounded-full ${cellSize.circle}`}
                      style={{
                        backgroundColor: 'var(--fg-brand-secondary_alt)',
                        boxShadow: '0 0 0 4px var(--ring-brand-secondary), 0 1px 2px 0 var(--shadow-sm)',
                        zIndex: 0,
                      }}
                    />
                    <span className="relative z-10" style={{ color: 'var(--text-white)' }}>19</span>
                  </td>

                  {/* In-range day cell */}
                  <td
                    className={`text-center font-medium cursor-pointer ${cellSize.cell}`}
                    style={{
                      color: 'var(--text-secondary)',
                      backgroundColor: 'var(--bg-brand-secondary)',
                      verticalAlign: 'middle',
                    }}
                  >
                    20
                  </td>

                  {/* Start date (range) — left-rounded */}
                  <td
                    className={`text-center font-medium cursor-pointer relative ${cellSize.cell}`}
                    style={{
                      backgroundColor: 'var(--bg-brand-secondary)',
                      borderRadius: '9999px 0 0 9999px',
                      verticalAlign: 'middle',
                    }}
                  >
                    <div
                      className={`absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 rounded-full ${cellSize.circle}`}
                      style={{
                        backgroundColor: 'var(--fg-brand-secondary_alt)',
                        boxShadow: '0 0 0 4px var(--ring-brand-secondary), 0 1px 2px 0 var(--shadow-sm)',
                        zIndex: 0,
                      }}
                    />
                    <span className="relative z-10" style={{ color: 'var(--text-white)' }}>19</span>
                  </td>
                </tbody>
              </table>

              {/* Action buttons */}
              <div
                className="flex gap-4"
                style={{ position: 'absolute', right: '20px', bottom: '20px' }}
              >
                <button
                  type="button"
                  className="font-semibold"
                  style={{
                    backgroundColor: 'var(--bg-primary)',
                    color: 'var(--text-primary)',
                    border: '1px solid var(--border-secondary)',
                    borderRadius: '4px',
                    padding: size === 'lg' ? '10px 16px' : '8px 12px',
                    fontSize: size === 'lg' ? '14px' : '12px',
                    lineHeight: size === 'lg' ? '20px' : '18px',
                    fontWeight: 600,
                    cursor: 'pointer',
                  }}
                  onClick={() => setIsOpen(false)}
                >
                  Cancel
                </button>
                <button
                  type="button"
                  className="font-semibold"
                  style={{
                    backgroundColor: 'var(--bg-primary)',
                    color: 'var(--text-primary)',
                    border: '1px solid var(--border-secondary)',
                    borderRadius: '4px',
                    padding: size === 'lg' ? '10px 16px' : '8px 12px',
                    fontSize: size === 'lg' ? '14px' : '12px',
                    lineHeight: size === 'lg' ? '20px' : '18px',
                    fontWeight: 600,
                    cursor: 'pointer',
                  }}
                  onClick={() => {
                    onChange?.({ startDate, endDate });
                    setIsOpen(false);
                  }}
                >
                  Apply
                </button>
              </div>
            </div>
          </div>
        </>
      )}
    </div>
  );
}
```

---

## CRITICAL Rules

1. **Trigger button is teal hyperlink style** — color `var(--text-brand-secondary)` (#098486), transparent background, no border, no padding. The underline is transparent by default and becomes visible on hover/focus. Do NOT use a bordered button or filled button.
2. **Panel border-radius is 4px** — NOT 8px, NOT 12px.
3. **Panel border is `var(--border-primary)`** (#BDC8D3) — NOT `var(--border-tertiary)` (#E8ECF1), NOT `var(--border-secondary)` (#CCD6DC).
4. **Selected day circle is `var(--fg-brand-secondary_alt)`** (#0BA5A7) with focus ring `0 0 0 4px var(--ring-brand-secondary)` (#0BA5A73d) — the circle is rendered as an absolutely-positioned element behind the day number. Text on selected day is `var(--text-white)` (#FFFFFF).
5. **In-range background is `var(--bg-brand-secondary)`** (#E7FDFD) — a very light teal. NOT white, NOT `var(--bg-secondary)` (#F1F4F7).
6. **Start date has left-pill border-radius** (`9999px 0 0 9999px`), end date has right-pill (`0 9999px 9999px 0`). In-range cells have NO border radius (flat continuous band).
7. **Today marker** — shows `var(--text-brand-secondary)` (#098486) text color on `var(--bg-secondary)` (#F1F4F7) background with 8px border-radius. NOT a circle, NOT a dot.
8. **Day cell default text color is `var(--text-secondary)`** (#495883) — NOT `var(--text-primary)` (#353E5A).
9. **Weekday header text is `var(--text-tertiary)`** (#6B748E) with bottom border `var(--border-tertiary)` (#E8ECF1).
10. **Action buttons are outlined** — `var(--bg-primary)` (#FFFFFF) background, `var(--border-secondary)` (#CCD6DC) border, positioned absolutely at bottom-right (20px from edge). Both Cancel and Apply look identical.
11. **Preset sidebar uses `var(--border-primary)` (#BDC8D3) right border** — same color as the panel border. Active preset shows `var(--bg-secondary)` (#F1F4F7) background and a teal checkmark `var(--fg-brand-secondary)` (#098486).
12. **Preset button border-radius is 4px** — NOT 8px.
13. **SM and MD action buttons both use 12px/18px/600 typography** — they are identical. Only LG uses 14px/20px/600.
14. **Font is Inter** — all text uses `Inter, Arial, sans-serif` at weight 500 (calendar) or 600 (buttons, titles).
15. **Calendar bottom padding is 64px** — this reserves space for the absolutely-positioned action buttons. Do NOT overlap content with buttons.
