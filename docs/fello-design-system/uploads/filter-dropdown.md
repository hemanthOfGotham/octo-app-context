# Filter Dropdown — Strict Specification

> **⚠️ CROSS-REFERENCE — REQUIRED COMPONENT SPEC:**
> When `multiSelect` is enabled, each option MUST use the **Checkbox component** from `checkbox.md`. Do NOT use a native HTML `<input type="checkbox">` or any default/shadcn checkbox. The checkbox must follow the exact box styling, colors, and SVG check icon defined in that spec.

When building a filter dropdown (inline dropdown filter for selecting one or more options from a list), follow these specifications exactly. The filter dropdown uses a hyperlink-style teal trigger button (same pattern as the filter date picker) with a dropdown panel of selectable options.

> **TOKEN RULE:** Every color in this spec is expressed as a CSS variable token. In Tailwind classes use `bg-[var(--token)]` / `text-[var(--token)]`. In inline styles use `'var(--token)'`. In prose and tables, show `var(--token)` (#hex) for reference.

---

## Architecture

```
Inline Trigger:
┌──────────────────────────────────────┐
│  Status  Active ▾                    │  ← Single select
│  ↑ Label  ↑ Hyperlink button (teal)  │
└──────────────────────────────────────┘

┌──────────────────────────────────────┐
│  Categories  Design (3) ▾            │  ← Multi-select with count
│              ↑ First selected  ↑ count
└──────────────────────────────────────┘

Dropdown Panel:
┌──────────────────────────┐
│  🔍 Search               │  ← Optional search bar
├──────────────────────────┤
│  ☐ Design                │  ← Multi-select option (checkbox)
│  ☑ Development      ✓    │  ← Selected option (teal check)
│  ☐ Marketing             │
│  ──────────────────────  │  ← Separator
│  ☐ Other                 │
└──────────────────────────┘
```

---

## Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `label` | `string` | — | Optional label before trigger |
| `placeholder` | `string` | `'Select'` | Placeholder when no selection |
| `size` | `'small' \| 'medium'` | `'medium'` | Controls trigger and option sizing |
| `popupSize` | `'xs' \| 'sm' \| 'md' \| 'lg' \| 'xl'` | — | Dropdown panel width |
| `disabled` | `boolean` | `false` | Disables the dropdown |
| `showSearchBar` | `boolean` | `false` | Shows search input in dropdown |
| `multiSelect` | `boolean` | `false` | Enables multi-select with checkboxes |
| `value` | `T \| T[]` | — | Current selection |
| `options` | Array | — | List of options (children) |

---

## Inline Trigger

### Container

| Property | Value | Tailwind |
|----------|-------|----------|
| Display | inline-flex | `inline-flex` |
| Align items | center | `items-center` |
| Color | `var(--text-secondary)` (#495883) | `text-[var(--text-secondary)]` |

### Label

| Property | Value | Tailwind |
|----------|-------|----------|
| White space | nowrap | `whitespace-nowrap` |
| Right margin | 6px | `mr-1.5` |
| Color | Inherits `var(--text-secondary)` (#495883) from container | — |

**Label typography by size:**

| Size | Font Size | Line Height | Weight |
|------|-----------|-------------|--------|
| Small | 12px | 18px | 500 |
| Medium | 14px | 20px | 500 |

### Trigger Button (Hyperlink Style)

Identical to the filter date picker trigger — a borderless hyperlink button in teal.

| Property | Value |
|----------|-------|
| Background | transparent |
| Padding | 0 |
| Height | auto |
| Color | `var(--text-brand-secondary)` (#098486) |
| Cursor | pointer |
| Border | none |
| Text decoration | underline (transparent by default) |
| Hover/Focus | underline becomes visible (inherits teal color) |
| Transition | text-decoration-color 150ms ease-out |

**Button typography by size:**

| Size | Font Size | Line Height | Weight |
|------|-----------|-------------|--------|
| SM | 12px | 18px | 600 |
| MD | 14px | 20px | 600 |

### Button Label Display

| Mode | Display |
|------|---------|
| No selection | Shows placeholder text |
| Single select | Shows selected option's display text |
| Multi-select (1 selected) | Shows the one selected option's text |
| Multi-select (2+ selected) | Shows first selected option's text + ` (N)` count |

### Count + Chevron

The count indicator and chevron icon appear after the button, sharing the same teal color.

| Property | SM | MD |
|----------|----|----|
| Color | `var(--text-brand-secondary)` (#098486) | `var(--text-brand-secondary)` (#098486) |
| Font size | 12px | 14px |
| Font weight | 600 | 600 |
| Chevron icon size | 16px | 20px |
| Gap from button | 4px | 4px |
| Disabled color | `var(--bg-brand-secondary-solid_hover)` (#0BA5A7) at 50% opacity | `var(--bg-brand-secondary-solid_hover)` (#0BA5A7) at 50% opacity |

---

## Dropdown Panel

### Panel Container

| Property | Value |
|----------|-------|
| Background | `var(--bg-primary)` (#FFFFFF) |
| Border | 1px solid `var(--border-secondary)` (#CCD6DC) |
| Border radius | 8px |
| Padding top | 2px |
| Min width | 160px |
| Max height | 400px |
| Overflow | auto (scrollable) |
| Box shadow | `var(--shadow-lg)` — `0 4px 6px -2px #4958830a, 0 12px 16px -4px #49588329` |
| Animation | Slide down 12px, 200ms |
| Offset from trigger | 4px below |

### Panel Width Sizes

| Size | Width |
|------|-------|
| `xs` | 192px |
| `sm` | 224px |
| `md` | 256px |
| `lg` | 300px |
| `xl` | 384px |

---

## Search Bar (Optional)

When `showSearchBar` is enabled, a search input appears at the top of the dropdown.

| Property | Value |
|----------|-------|
| Container padding | 4px horizontal, 2px vertical |
| Input size | small (32px height) |
| Leading icon | Search (🔍) |
| Placeholder | "Search" |
| Width | 100% |
| Total height with padding | 36px |

The options container below the search bar adjusts: `height: calc(100% - 36px); overflow: auto`

---

## Option Item

### Option Container

| Property | Value | Tailwind |
|----------|-------|----------|
| Padding (outer) | 2px 4px | `px-1 py-0.5` |
| Background | `var(--bg-primary)` (#FFFFFF) | `bg-[var(--bg-primary)]` |

### Option Row

| Property | Value | Tailwind |
|----------|-------|----------|
| Display | inline-flex | `inline-flex` |
| Justify | space-between | `justify-between` |
| Align | center | `items-center` |
| Width | 100% | `w-full` |
| Border radius | 4px | `rounded` |
| Cursor | pointer | `cursor-pointer` |
| Gap | 8px | `gap-2` |
| Color | `var(--text-primary)` (#353E5A) | `text-[var(--text-primary)]` |

### Option Typography by Size

| Size | Font Size | Line Height | Weight | Padding |
|------|-----------|-------------|--------|---------|
| Small / Medium | 14px | 20px | 500 | 8px 12px |
| Large | 16px | 24px | 500 | 10px 12px |

### Option States

| State | Background | Text Color |
|-------|------------|------------|
| Default | transparent | `var(--text-primary)` (#353E5A) |
| Hover | `var(--bg-secondary)` (#F1F4F7) | `var(--text-primary)` (#353E5A) |
| Focus / Active (keyboard) | `var(--bg-secondary)` (#F1F4F7) | `var(--text-primary)` (#353E5A) |
| Active + Hover | `var(--bg-secondary_hover)` (#E8ECF1) | `var(--text-primary)` (#353E5A) |
| Disabled | transparent | `var(--text-disabled)` (#8EA1AF) |

### Selected Option Checkmark

When an option is selected (single-select mode), a teal checkmark icon appears on the right.

| Property | Value |
|----------|-------|
| Icon | Check (✓) |
| Color | `var(--fg-brand-secondary)` (#098486) |
| Size (SM/MD) | 16px |
| Size (LG) | 18px |

### Multi-Select Checkbox

When `multiSelect` is enabled, each option shows a **custom checkbox** on the left instead of a checkmark on the right. You MUST use the checkbox from `checkbox.md` — do NOT use a native HTML checkbox.

**Checkbox in dropdown options uses Small size (no label):**

| Property | Value |
|----------|-------|
| Size | Small (16×16px box) |
| Label | None (icon-only, no text label on the checkbox itself) |
| Border radius | 4px |
| Unchecked border | 1px solid `var(--fg-disabled)` (#8EA1AF) |
| Unchecked background | `var(--bg-primary)` (#FFFFFF) |
| Checked background | `var(--bg-brand-secondary-solid)` (#098486) |
| Checked border | `var(--bg-brand-secondary-solid)` (#098486) |
| Check icon | White SVG checkmark (10×8px) inside the box |
| Indeterminate | White dash (12×2px) inside the box |
| Disabled + unchecked | Border `var(--border-secondary)` (#CCD6DC), background `var(--bg-secondary_subtle)` (#F7F9FA) |
| Disabled + checked | Background `var(--bg-brand-secondary-solid_hover)` (#0BA5A7) at 50% opacity |

**Checkbox JSX for dropdown option (from checkbox spec):**

```jsx
{/* Unchecked */}
<div
  className="w-4 h-4 rounded border shrink-0 flex items-center justify-center"
  style={{ borderColor: 'var(--fg-disabled)', backgroundColor: 'var(--bg-primary)' }}
/>

{/* Checked */}
<div
  className="w-4 h-4 rounded shrink-0 flex items-center justify-center"
  style={{ backgroundColor: 'var(--bg-brand-secondary-solid)', border: '1px solid var(--bg-brand-secondary-solid)' }}
>
  <svg width="10" height="8" viewBox="0 0 10 8" fill="none">
    <path d="M1 4.11111L3.42857 6.5L9 1" stroke="white" strokeWidth="1.5" strokeLinecap="round" strokeLinejoin="round" />
  </svg>
</div>
```

---

## Color Reference

| Token | Hex | Usage |
|-------|-----|-------|
| `var(--bg-primary)` | (#FFFFFF) | Panel bg, option bg |
| `var(--text-primary)` | (#353E5A) | Option text, month header |
| `var(--text-secondary)` | (#495883) | Label color |
| `var(--text-brand-secondary)` | (#098486) | Trigger button text, count, chevron, checkmark |
| `var(--bg-brand-secondary-solid_hover)` at 50% opacity | Disabled chevron |
| `var(--text-disabled)` | (#8EA1AF) | Disabled option text |
| `var(--bg-secondary)` | (#F1F4F7) | Option hover/focus |
| `var(--bg-secondary_hover)` | (#E8ECF1) | Active option hover |
| `var(--border-secondary)` | (#CCD6DC) | Panel border |

---

## Implementation

```jsx
import { useState, useRef, useEffect } from 'react';
import { ChevronDown, Check, Search } from 'lucide-react';

function FilterDropdown({
  label,
  placeholder = 'Select',
  size = 'medium',
  popupSize = 'md',
  disabled = false,
  showSearchBar = false,
  multiSelect = false,
  options = [],
  value,
  onChange,
}) {
  const [isOpen, setIsOpen] = useState(false);
  const [search, setSearch] = useState('');
  const containerRef = useRef(null);

  // Resolve selected items
  const selected = multiSelect
    ? (Array.isArray(value) ? value : [])
    : (value != null ? [value] : []);

  const selectedOptions = options.filter((o) => selected.includes(o.value));

  // Display value
  const displayValue = selectedOptions.length > 0
    ? selectedOptions[0].label
    : null;

  const count = selectedOptions.length;

  // Filter options by search
  const filtered = search
    ? options.filter((o) => o.label.toLowerCase().includes(search.toLowerCase()))
    : options;

  // Size configs
  const isSmall = size === 'small';
  const labelClass = isSmall ? 'text-xs leading-[18px]' : 'text-sm leading-5';
  const buttonClass = isSmall ? 'text-xs leading-[18px]' : 'text-sm leading-5';
  const chevronSize = isSmall ? 16 : 20;
  const optionPadding = 'py-2 px-3'; // 8px 12px for sm/md
  const optionText = 'text-sm leading-5'; // 14px for sm/md

  const popupWidths = { xs: 192, sm: 224, md: 256, lg: 300, xl: 384 };
  const panelWidth = popupWidths[popupSize] || 256;

  // Handle selection
  const handleSelect = (optionValue) => {
    if (multiSelect) {
      const newValue = selected.includes(optionValue)
        ? selected.filter((v) => v !== optionValue)
        : [...selected, optionValue];
      onChange?.(newValue);
    } else {
      onChange?.(optionValue);
      setIsOpen(false);
    }
  };

  // Close on outside click
  useEffect(() => {
    const handler = (e) => {
      if (containerRef.current && !containerRef.current.contains(e.target)) {
        setIsOpen(false);
        setSearch('');
      }
    };
    document.addEventListener('mousedown', handler);
    return () => document.removeEventListener('mousedown', handler);
  }, []);

  return (
    <div ref={containerRef} className="inline-flex items-center relative">
      {/* Label */}
      {label && (
        <span
          className={`whitespace-nowrap font-medium mr-1.5 ${labelClass}`}
          style={{ color: 'var(--text-secondary)', fontFamily: 'Inter, Arial, sans-serif' }}
        >
          {label}
        </span>
      )}

      {/* Trigger */}
      <div
        className={`flex items-center gap-1 cursor-pointer ${disabled ? 'pointer-events-none' : ''}`}
        onClick={() => !disabled && setIsOpen(!isOpen)}
      >
        {/* Hyperlink button */}
        <button
          type="button"
          disabled={disabled}
          className={`bg-transparent p-0 h-auto border-none font-semibold ${buttonClass}`}
          style={{
            color: 'var(--text-brand-secondary)',
            cursor: 'pointer',
            fontFamily: 'Inter, Arial, sans-serif',
          }}
        >
          <span className="underline decoration-transparent hover:decoration-current transition-colors duration-150 ease-out">
            {displayValue || placeholder}
          </span>
        </button>

        {/* Count + Chevron */}
        <span
          className={`flex items-center shrink-0 font-semibold ${isSmall ? 'text-xs leading-[18px]' : 'text-sm leading-5'}`}
          style={{ color: disabled ? '#0BA5A780' : 'var(--text-brand-secondary)' }}
        >
          {multiSelect && count > 1 && <span>&nbsp;({count})</span>}
          <ChevronDown size={chevronSize} />
        </span>
      </div>

      {/* Dropdown Panel */}
      {isOpen && (
        <div
          className="absolute top-full left-0 mt-1 z-50"
          style={{
            width: `${panelWidth}px`,
            minWidth: '160px',
            maxHeight: '400px',
            backgroundColor: 'var(--bg-primary)',
            border: '1px solid var(--border-secondary)',
            borderRadius: '8px',
            boxShadow: 'var(--shadow-lg)',
            paddingTop: '2px',
            overflow: 'auto',
            fontFamily: 'Inter, Arial, sans-serif',
            animation: 'slideDown 200ms ease-out',
          }}
        >
          {/* Search bar */}
          {showSearchBar && (
            <div className="px-1 py-0.5">
              <div
                className="flex items-center gap-2 w-full rounded px-3"
                style={{
                  height: '32px',
                  border: '1px solid var(--border-secondary)',
                  borderRadius: '4px',
                  backgroundColor: 'var(--bg-primary)',
                }}
              >
                <Search size={16} style={{ color: 'var(--fg-tertiary)', flexShrink: 0 }} />
                <input
                  type="text"
                  placeholder="Search"
                  className="flex-1 border-none outline-none bg-transparent text-sm"
                  style={{ color: 'var(--text-primary)', fontFamily: 'Inter, Arial, sans-serif' }}
                  value={search}
                  onChange={(e) => setSearch(e.target.value)}
                  autoFocus
                />
              </div>
            </div>
          )}

          {/* Options */}
          <div
            className="pb-0.5"
            style={showSearchBar ? { height: 'calc(100% - 36px)', overflow: 'auto' } : undefined}
          >
            {filtered.length === 0 ? (
              <div className="py-4 px-3 text-center text-sm" style={{ color: 'var(--text-tertiary)' }}>
                No Results Found
              </div>
            ) : (
              filtered.map((option) => {
                const isSelected = selected.includes(option.value);
                return (
                  <div key={option.value} className="px-1 py-0.5">
                    <div
                      className={`
                        inline-flex items-center justify-between w-full gap-2 rounded cursor-pointer
                        ${optionPadding}
                        hover:bg-[var(--bg-secondary)]
                        ${option.disabled ? 'pointer-events-none' : ''}
                      `}
                      style={{
                        color: option.disabled ? 'var(--text-disabled)' : 'var(--text-primary)',
                        fontSize: '14px',
                        lineHeight: '20px',
                        fontWeight: 500,
                        borderRadius: '4px',
                      }}
                      onClick={() => !option.disabled && handleSelect(option.value)}
                    >
                      {/* Option content */}
                      <div className="flex items-center gap-2 flex-1 min-w-0">
                        {/* Multi-select checkbox — uses checkbox.md spec */}
                        {/* Do NOT use native <input type="checkbox"> — use custom styled div */}
                        {multiSelect && (
                          isSelected ? (
                            <div
                              className="w-4 h-4 rounded shrink-0 flex items-center justify-center"
                              style={{ backgroundColor: 'var(--bg-brand-secondary-solid)', border: '1px solid var(--bg-brand-secondary-solid)' }}
                            >
                              <svg width="10" height="8" viewBox="0 0 10 8" fill="none">
                                <path d="M1 4.11111L3.42857 6.5L9 1" stroke="white" strokeWidth="1.5" strokeLinecap="round" strokeLinejoin="round" />
                              </svg>
                            </div>
                          ) : (
                            <div
                              className="w-4 h-4 rounded shrink-0 flex items-center justify-center"
                              style={{ borderColor: 'var(--fg-disabled)', backgroundColor: 'var(--bg-primary)', border: '1px solid var(--fg-disabled)' }}
                            />
                          )
                        )}
                        <span className="break-words">{option.label}</span>
                      </div>

                      {/* Single-select checkmark */}
                      {!multiSelect && isSelected && (
                        <Check size={16} style={{ color: 'var(--fg-brand-secondary)', flexShrink: 0 }} />
                      )}
                    </div>
                  </div>
                );
              })
            )}
          </div>
        </div>
      )}
    </div>
  );
}

{/* Usage — Single Select */}
<FilterDropdown
  label="Status"
  placeholder="All"
  options={[
    { value: 'active', label: 'Active' },
    { value: 'inactive', label: 'Inactive' },
    { value: 'archived', label: 'Archived' },
  ]}
  value={status}
  onChange={setStatus}
/>

{/* Usage — Multi-Select with Search */}
<FilterDropdown
  label="Categories"
  placeholder="Select"
  multiSelect={true}
  showSearchBar={true}
  popupSize="lg"
  options={[
    { value: 'design', label: 'Design' },
    { value: 'development', label: 'Development' },
    { value: 'marketing', label: 'Marketing' },
    { value: 'sales', label: 'Sales' },
  ]}
  value={categories}
  onChange={setCategories}
/>
```

---

## CRITICAL Rules

1. **Trigger button is a teal hyperlink** — color `var(--text-brand-secondary)` (#098486), transparent background, no border, no padding. The underline is transparent by default and shows on hover/focus. Do NOT use a bordered or filled button.
2. **Label color is `var(--text-secondary)` (#495883)** — NOT the same teal as the button. The label is a separate span before the trigger.
3. **Multi-select count appears after the button text** — format is `(N)` where N is the total count. Only shows when 2+ items are selected. The count shares the teal `var(--text-brand-secondary)` (#098486) color.
4. **Dropdown panel border-radius is 8px** — NOT 4px (unlike the date picker panel which is 4px). The dropdown uses `radius(4)` = 4x2 = 8px.
5. **Dropdown panel border is `var(--border-secondary)` (#CCD6DC)** — NOT `var(--border-primary)` (#BDC8D3).
6. **Dropdown panel shadow is `var(--shadow-lg)`** — `0 4px 6px -2px #4958830a, 0 12px 16px -4px #49588329`. This is a LARGER shadow than the date picker (which uses shadow-md).
7. **Option border-radius is 4px** — each option row has `rounded` (4px) corners, inside a 2px outer padding container.
8. **Option hover background is `var(--bg-secondary)` (#F1F4F7)** — NOT `var(--bg-secondary_hover)` (#E8ECF1). The darker `var(--bg-secondary_hover)` (#E8ECF1) is only used when an active (keyboard-focused) option is ALSO hovered.
9. **Selected option checkmark is teal `var(--fg-brand-secondary)` (#098486)** — appears on the right side of the option row in single-select mode. In multi-select mode, a checkbox appears on the left instead.
10. **Chevron icon is ChevronDown** — NOT ChevronRight. It does NOT rotate on open (unlike expansion panels).
11. **Panel max-height is 400px** with overflow auto — the dropdown scrolls when content exceeds 400px.
12. **Search bar is 32px tall** with a search icon — when present, the options area adjusts height with `calc(100% - 36px)`.
13. **Option text color is `var(--text-primary)` (#353E5A)** — NOT `var(--text-secondary)` (#495883). Disabled options use `var(--text-disabled)` (#8EA1AF).
14. **Panel opening animation is 200ms slide-down** — slides down 12px from collapsed to open position.
15. **Font is Inter** — all text uses `Inter, Arial, sans-serif`.
16. **Multi-select checkboxes MUST use the custom checkbox from `checkbox.md`** — do NOT use native HTML `<input type="checkbox">`, shadcn checkbox, or any browser-default checkbox. The checkbox is a 16x16px styled `<div>` with: unchecked = `var(--bg-primary)` (#FFFFFF) bg + `var(--fg-disabled)` (#8EA1AF) border; checked = `var(--bg-brand-secondary-solid)` (#098486) bg + white SVG checkmark (10x8px). See the checkbox spec for exact SVG path and all states.
