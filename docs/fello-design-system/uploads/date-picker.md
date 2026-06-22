# Date Picker Component — Strict Specification

When building any date picker, date range selector, or calendar input, follow these specifications exactly. Do NOT use shadcn Calendar/DatePicker, Tailwind date utilities, or default HTML `<input type="date">`. Build a custom calendar from scratch using these specs.

> **TOKEN RULE:** Every color in the code examples below uses CSS variable tokens or Tailwind palette names — never raw hex. When implementing, copy the token references exactly. See `design-tokens.md` for definitions.

---

## Architecture

```
Date Picker
├── Trigger — input field (inside Form Field wrapper) or filter button
├── Calendar Panel — floating overlay
│   ├── Navigation header (month/year + arrows)
│   ├── Day-of-week header row
│   ├── Day cells grid (6 rows × 7 columns)
│   └── Action buttons (Cancel / Apply for range mode)
└── Range Presets sidebar (optional, for predefined ranges)
```

---

## Usage Modes

### 1. Input Date Picker

Renders inside a Form Field wrapper (see `form-field.md`). The input displays the selected date(s) as text, and clicking opens the calendar overlay.

```jsx
<FormField label="Select Date" trailingIcon="calendar" size={size}>
  <input readOnly value={displayValue} onClick={openCalendar} placeholder="Select" />
</FormField>
```

### 2. Filter Date Picker

A compact inline trigger with a label and hyperlink-style button:

```jsx
<div className="inline-flex items-center gap-1.5" style={{ color: 'var(--text-secondary)' }}>
  <span style={{ whiteSpace: 'nowrap', ...captionTypography }}>{label}</span>
  <button onClick={openCalendar}>
    {selectedValue || placeholder}
    <ChevronDownIcon />
  </button>
</div>
```

---

## Calendar Panel (Overlay)

### Container

```jsx
style={{
  background: 'var(--bg-primary)',
  borderRadius: '8px',
  border: '1px solid var(--border-secondary)',
  boxShadow: 'var(--shadow-lg)',
  overflow: 'hidden',
  display: 'flex',
}}
```

### Size Variants

| Size | Cell Dimensions | Selected Circle | Typography | Range Button Min-Width | Range Button Padding |
|------|----------------|----------------|------------|----------------------|---------------------|
| **Small** | 32px × 32px | 24px × 24px | caption-sm: 12px/18px/500 | 140px | 4px 8px |
| **Medium** | 40px × 40px | 32px × 32px | caption-md: 14px/20px/500 | 152px | 6px 12px |
| **Large** | 48px × 48px | 40px × 40px | caption-lg: 16px/24px/500 | 172px | 8px 12px |

---

## Day Cell Structure

```jsx
<td
  style={{
    width: cellSize,
    height: cellSize,
    minWidth: cellSize,
    textAlign: 'center',
    cursor: 'pointer',
    borderRadius: '0',           // cells are square by default
    position: 'relative',
    ...captionTypography,
  }}
>
  {/* Selected circle (::before pseudo) */}
  {isSelected && (
    <div style={{
      position: 'absolute',
      top: '50%',
      left: '50%',
      transform: 'translate(-50%, -50%)',
      width: selectedCircleSize,
      height: selectedCircleSize,
      borderRadius: '9999px',
      background: 'var(--bg-brand-secondary-solid_hover)',     // fg-brand-secondary-alt
      zIndex: 0,
    }} />
  )}
  <span style={{ position: 'relative', zIndex: 1, color: isSelected ? 'var(--text-white)' : 'var(--text-primary)' }}>
    {dayNumber}
  </span>
</td>
```

---

## Day Cell States

### Default
- Background: transparent
- Text color: `var(--text-primary)` (#353E5A)

### Today (not selected)
- Background: `var(--bg-secondary)` (#F1F4F7)
- Border-radius: 9999px (full circle)

### Hover
- Background: `var(--bg-secondary)` (#F1F4F7)

### Selected (start/end date)
- Circle background: `var(--bg-brand-secondary-solid_hover)` (#0BA5A7) (fg-brand-secondary-alt / SECONDARY_600)
- Text color: `var(--text-white)` (#FFFFFF)
- Circle size: see Size Variants table

### In-Range (between start and end)
- Background: `var(--bg-brand-secondary)` (SECONDARY_50)
- Text color: `var(--text-primary)` (#353E5A)

### Off/Disabled (outside current month)
- Opacity or hidden
- Non-interactive

---

## Navigation Header

```jsx
<div className="flex items-center justify-between" style={{ padding: '8px' }}>
  <button onClick={prevMonth}>
    <ChevronLeftIcon style={{ fontSize: '16px' }} />
  </button>
  <span style={{ fontFamily: 'Inter, sans-serif', fontSize: '14px', lineHeight: '20px', fontWeight: 500 }}>
    {monthName} {year}
  </span>
  <button onClick={nextMonth}>
    <ChevronRightIcon style={{ fontSize: '16px' }} />
  </button>
</div>
```

---

## Range Presets Sidebar

When predefined ranges are provided, a sidebar appears to the left of the calendar:

```jsx
<div style={{ display: 'flex', flexDirection: 'column', gap: '4px', padding: '12px 8px' }}>
  {ranges.map(range => (
    <button
      key={range.label}
      style={{
        ...captionTypography,
        padding: rangeButtonPadding,
        minWidth: rangeButtonMinWidth,
        borderRadius: '4px',
        background: isActive ? 'var(--bg-secondary)' : 'transparent',
        color: 'var(--text-primary)',
        cursor: 'pointer',
        textAlign: 'left',
      }}
    >
      {range.label}
    </button>
  ))}
</div>
```

### Preset Hover/Active
- Hover: `background: var(--bg-secondary)` (#F1F4F7)
- Active + Hover: `background: var(--bg-secondary_hover)` (#E8ECF1)
- Active check: teal check icon `var(--fg-brand-secondary)` (#098486) (SECONDARY_700) appears after text

### Common Presets
- Today
- Yesterday
- Last 7 Days
- Last 30 Days
- This Month
- Last Month

---

## Action Buttons (Range Mode)

For range selection, Cancel and Apply buttons appear below the calendar:

| Size | Typography | Padding |
|------|-----------|---------|
| Small | title-sm: 12px/18px/600 | 8px 12px |
| Medium | title-sm: 12px/18px/600 | 8px 12px |
| Large | title-md: 14px/20px/600 | 10px 16px |

---

## Display Format

Selected date text format: `MMM DD, YYYY` (e.g., "Feb 19, 2026")
Range format: `MMM DD, YYYY - MMM DD, YYYY` (e.g., "Feb 01, 2026 - Feb 19, 2026")

---

## Inputs

| Input | Type | Default | Description |
|-------|------|---------|-------------|
| `size` | `'small'` \| `'medium'` \| `'large'` | `'medium'` | Controls cell dimensions and typography |
| `singleDatePicker` | boolean | `false` | Single date vs range selection |
| `showClearButton` | boolean | `false` | Shows clear button to reset value |
| `linkedCalendars` | boolean | `false` | Links month navigation between two calendars |
| `ranges` | object | — | Predefined range options (key-value pairs) |
| `showRangeLabelOnInput` | boolean | `false` | Show range label instead of date text |
| `alwaysShowCalendars` | boolean | `false` | Keep calendars visible alongside ranges |
| `placeholder` | string | `'Select'` | Placeholder text |
| `disabled` | boolean | `false` | Disables the picker |

---

## Color Reference

| Token | Value | Usage |
|-------|-------|-------|
| `var(--text-primary)` | (#353E5A) | Default day text, header text |
| `var(--text-secondary)` | (#495883) | Filter label text |
| `var(--bg-brand-secondary-solid_hover)` | (#0BA5A7) | Selected day circle |
| `var(--fg-brand-secondary)` | (#098486) | Active preset check icon |
| `var(--bg-primary)` | (#FFFFFF) | Panel background |
| `var(--bg-secondary)` | (#F1F4F7) | Hover, today highlight, active preset |
| `var(--bg-secondary_hover)` | (#E8ECF1) | Active preset + hover |
| `var(--bg-brand-secondary)` | (SECONDARY_50) | In-range day background |
| `var(--border-secondary)` | (#CCD6DC) | Panel border |
| `var(--shadow-lg)` | — | Panel shadow |

---

## Implementation Pattern for Lovable

```jsx
function DatePicker({ value, onChange, placeholder = 'Select', size = 'medium' }) {
  const [isOpen, setIsOpen] = React.useState(false);
  const [currentMonth, setCurrentMonth] = React.useState(value ? new Date(value) : new Date());

  const cellSizes = { small: 32, medium: 40, large: 48 };
  const circleSizes = { small: 24, medium: 32, large: 40 };
  const cellSize = cellSizes[size];
  const circleSize = circleSizes[size];
  const fontSize = size === 'small' ? '12px' : size === 'large' ? '16px' : '14px';

  const daysInMonth = new Date(currentMonth.getFullYear(), currentMonth.getMonth() + 1, 0).getDate();
  const firstDayOfWeek = new Date(currentMonth.getFullYear(), currentMonth.getMonth(), 1).getDay();
  const today = new Date();

  const formatDate = (d) => {
    const months = ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'];
    return `${months[d.getMonth()]} ${String(d.getDate()).padStart(2,'0')}, ${d.getFullYear()}`;
  };

  const isToday = (day) => {
    return today.getFullYear() === currentMonth.getFullYear()
      && today.getMonth() === currentMonth.getMonth()
      && today.getDate() === day;
  };

  const isSelected = (day) => {
    if (!value) return false;
    const sel = new Date(value);
    return sel.getFullYear() === currentMonth.getFullYear()
      && sel.getMonth() === currentMonth.getMonth()
      && sel.getDate() === day;
  };

  const weeks = [];
  let dayCounter = 1;
  for (let w = 0; w < 6; w++) {
    const week = [];
    for (let d = 0; d < 7; d++) {
      if ((w === 0 && d < firstDayOfWeek) || dayCounter > daysInMonth) {
        week.push(null);
      } else {
        week.push(dayCounter++);
      }
    }
    weeks.push(week);
    if (dayCounter > daysInMonth) break;
  }

  return (
    <div className="relative inline-block">
      {/* Trigger — form field input */}
      <button
        type="button"
        onClick={() => setIsOpen(!isOpen)}
        className="flex items-center cursor-pointer"
        style={{
          height: size === 'small' ? '32px' : size === 'large' ? '48px' : '40px',
          padding: '0 12px',
          borderRadius: '6px',
          border: `1px solid ${isOpen ? 'var(--border-brand-secondary)' : 'var(--border-secondary)'}`,
          background: 'var(--bg-primary)',
          boxShadow: isOpen ? 'var(--ring-brand-secondary), var(--shadow-xs)' : 'none',
          fontSize, fontWeight: 400, color: value ? 'var(--text-primary)' : 'var(--text-disabled)',
          gap: '8px', minWidth: '180px',
        }}
      >
        <span className="flex-1 text-left truncate">{value ? formatDate(new Date(value)) : placeholder}</span>
        <svg width="16" height="16" viewBox="0 0 16 16" fill="none" className="shrink-0">
          <rect x="2" y="3" width="12" height="11" rx="1.5" stroke="var(--fg-primary)" strokeWidth="1.2" fill="none" />
          <line x1="5" y1="1.5" x2="5" y2="4.5" stroke="var(--fg-primary)" strokeWidth="1.2" strokeLinecap="round" />
          <line x1="11" y1="1.5" x2="11" y2="4.5" stroke="var(--fg-primary)" strokeWidth="1.2" strokeLinecap="round" />
          <line x1="2" y1="7" x2="14" y2="7" stroke="var(--fg-primary)" strokeWidth="1.2" />
        </svg>
      </button>

      {/* Backdrop */}
      {isOpen && <div className="fixed inset-0 z-40" onClick={() => setIsOpen(false)} />}

      {/* Calendar Panel */}
      {isOpen && (
        <div
          className="absolute left-0 z-50"
          style={{
            marginTop: '4px',
            background: 'var(--bg-primary)',
            borderRadius: '8px',
            border: '1px solid var(--border-secondary)',
            boxShadow: 'var(--shadow-lg)',
            overflow: 'hidden',
            padding: '8px',
          }}
        >
          {/* Navigation header */}
          <div className="flex items-center justify-between" style={{ padding: '8px', marginBottom: '4px' }}>
            <button
              className="flex items-center justify-center w-8 h-8 rounded-md cursor-pointer border-none bg-transparent hover:bg-[var(--bg-secondary)] transition-colors duration-150"
              onClick={() => setCurrentMonth(new Date(currentMonth.getFullYear(), currentMonth.getMonth() - 1))}
            >
              <svg width="16" height="16" viewBox="0 0 16 16" fill="none">
                <polyline points="10,3 5,8 10,13" stroke="var(--fg-primary)" strokeWidth="1.5" strokeLinecap="round" strokeLinejoin="round" />
              </svg>
            </button>
            <span style={{ fontSize: '14px', lineHeight: '20px', fontWeight: 500, color: 'var(--text-primary)' }}>
              {currentMonth.toLocaleString('en', { month: 'long' })} {currentMonth.getFullYear()}
            </span>
            <button
              className="flex items-center justify-center w-8 h-8 rounded-md cursor-pointer border-none bg-transparent hover:bg-[var(--bg-secondary)] transition-colors duration-150"
              onClick={() => setCurrentMonth(new Date(currentMonth.getFullYear(), currentMonth.getMonth() + 1))}
            >
              <svg width="16" height="16" viewBox="0 0 16 16" fill="none">
                <polyline points="6,3 11,8 6,13" stroke="var(--fg-primary)" strokeWidth="1.5" strokeLinecap="round" strokeLinejoin="round" />
              </svg>
            </button>
          </div>

          {/* Day-of-week header */}
          <table style={{ borderCollapse: 'collapse' }}>
            <thead>
              <tr>
                {['Su','Mo','Tu','We','Th','Fr','Sa'].map(day => (
                  <th key={day} style={{
                    width: `${cellSize}px`, height: `${cellSize}px`,
                    textAlign: 'center', fontSize: '12px', fontWeight: 500,
                    color: 'var(--text-tertiary)',
                  }}>{day}</th>
                ))}
              </tr>
            </thead>
            <tbody>
              {weeks.map((week, wi) => (
                <tr key={wi}>
                  {week.map((day, di) => (
                    <td
                      key={di}
                      onClick={() => {
                        if (day) {
                          onChange(new Date(currentMonth.getFullYear(), currentMonth.getMonth(), day));
                          setIsOpen(false);
                        }
                      }}
                      style={{
                        width: `${cellSize}px`, height: `${cellSize}px`,
                        textAlign: 'center', cursor: day ? 'pointer' : 'default',
                        position: 'relative', fontSize, fontWeight: 500,
                      }}
                    >
                      {day && (
                        <>
                          {/* Selected circle */}
                          {isSelected(day) && (
                            <div style={{
                              position: 'absolute', top: '50%', left: '50%',
                              transform: 'translate(-50%, -50%)',
                              width: `${circleSize}px`, height: `${circleSize}px`,
                              borderRadius: '9999px', background: 'var(--bg-brand-secondary-solid_hover)', zIndex: 0,
                            }} />
                          )}
                          {/* Today highlight */}
                          {isToday(day) && !isSelected(day) && (
                            <div style={{
                              position: 'absolute', top: '50%', left: '50%',
                              transform: 'translate(-50%, -50%)',
                              width: `${circleSize}px`, height: `${circleSize}px`,
                              borderRadius: '9999px', background: 'var(--bg-secondary)', zIndex: 0,
                            }} />
                          )}
                          <span style={{
                            position: 'relative', zIndex: 1,
                            color: isSelected(day) ? 'var(--text-white)' : 'var(--text-primary)',
                          }}>{day}</span>
                        </>
                      )}
                    </td>
                  ))}
                </tr>
              ))}
            </tbody>
          </table>
        </div>
      )}
    </div>
  );
}
```

---

## CRITICAL Rules

1. **Selected day uses a circle with `var(--bg-brand-secondary-solid_hover)` (#0BA5A7)** (SECONDARY_600 / fg-brand-secondary-alt) — NOT the orange primary brand color.
2. **Cell sizes are 32px/40px/48px** for small/medium/large — the selected circle is 8px smaller than the cell.
3. **Panel border-radius is 8px** (radius-4) with 1px solid `var(--border-secondary)` (#CCD6DC) border.
4. **In-range background uses the light teal `SECONDARY_50`** — a very subtle teal, not the solid brand color.
5. **Range preset active indicator uses teal `var(--fg-brand-secondary)` (#098486)** (SECONDARY_700) for the check mark.
6. **Today highlight uses `var(--bg-secondary)` (#F1F4F7)** (bg-primary-hover) — NOT a border, it's a background fill on the cell.
7. **The calendar overlay uses `var(--shadow-lg)`** — not shadow-sm or shadow-md.
8. **Filter date picker uses hyperlink-style button** with chevron-down icon, inline layout with label.
9. **Display format is `MMM DD, YYYY`** — three-letter month abbreviation with comma.
10. **The panel is a CDK-style overlay** positioned below the trigger with smart repositioning (flip to above if no space below).
