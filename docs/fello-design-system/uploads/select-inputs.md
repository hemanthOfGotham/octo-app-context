# Select Input Components — Strict Specification

This spec covers **Dropdown**, **Autocomplete**, and the shared **Option** component. All use the Form Field + Input Wrapper system defined in `form-field.md`. Read that spec first — it defines the label, error/hint, border states, sizes, and shared input styling.

> **TOKEN RULE:** Every colour in this file is expressed as a CSS custom-property token — `var(--token)` — with the raw hex in parentheses only as a comment. When writing Tailwind classes use the `[var(--token)]` syntax (e.g. `bg-[var(--bg-secondary)]`). Never hard-code a hex value.

Do NOT use shadcn `<Select>`, `<Combobox>`, `<Command>`, or default HTML `<select>`. Build from scratch using these specs.

---

## 1. Option Component (Shared)

Used by both Dropdown and Autocomplete. Each selectable item in a dropdown panel is an Option.

### Option Structure

```jsx
<div
  className="block"
  style={{ padding: '2px 4px', background: 'var(--bg-primary)', display: optionHidden ? 'none' : 'block' }}
>
  <div
    className={`inline-flex justify-between items-center w-full cursor-pointer`}
    style={{
      gap: '8px',
      borderRadius: '4px',   // radius-2
      color: disabled ? 'var(--text-disabled)' : 'var(--text-primary)',
      ...sizeStyles,
      ...hoverActiveStyles,
    }}
  >
    <div className="inline-flex items-center gap-2 flex-1">
      {showCheckbox && <Checkbox checked={selected} disabled={disabled} />}
      <div className="break-words w-full">{children}</div>
    </div>

    {/* Check icon (single-select only, when selected) */}
    {selected && !showCheckbox && (
      <CheckIcon style={{ color: 'var(--fg-brand-secondary)', fontSize: iconFontSize }} />
    )}
  </div>
</div>
```

### Option Sizes

| Size | Typography | Padding | Check Icon Size |
|------|-----------|---------|-----------------|
| **Small** | caption-md: 14px / 20px / 500 | 8px 12px | title-lg: 18px |
| **Medium** | caption-md: 14px / 20px / 500 | 8px 12px | title-lg: 18px |
| **Large** | caption-lg: 16px / 24px / 500 | 10px 12px | title-xl: 18px |

### Option States

| State | Background | Text Color |
|-------|-----------|------------|
| Default | transparent | `var(--text-primary)` (#353E5A) |
| Hover (not disabled) | `var(--bg-secondary)` (#F1F4F7) | `var(--text-primary)` (#353E5A) |
| Active (keyboard highlight) | `var(--bg-secondary)` (#F1F4F7) | `var(--text-primary)` (#353E5A) |
| Active + Hover | `var(--bg-secondary_hover)` (#E8ECF1) | `var(--text-primary)` (#353E5A) |
| Disabled | transparent | `var(--text-disabled)` (#8EA1AF) |
| Selected | transparent (check icon shown) | `var(--text-primary)` (#353E5A) |

### Option Check Icon

- Color: `var(--fg-brand-secondary)` (#098486)
- Only visible when `selected=true` AND `showCheckbox=false`
- In multi-select mode, a checkbox replaces the check icon
- **The checkbox MUST use the exact Checkbox from `checkbox.md`** — same 16x16 box, same `var(--bg-brand-secondary-solid)` (#098486) teal fill, same `var(--border-primary)` (#BDC8D3) default border, same SVG icons. NEVER build a different checkbox.

### Option Content Alignment

Options support alignment: `start` (default), `center`, `end`

```jsx
// center alignment
style={{ justifyContent: 'center' }}

// end alignment
style={{ justifyContent: 'flex-end' }}
```

### Option Outer Padding

Each option has an outer container with `padding: 2px 4px` (spacing 0.5 / spacing 1). This provides consistent spacing between options in the list.

---

## 2. Dropdown Popup Panel (Shared)

The floating panel used by both Dropdown and Autocomplete to display options.

### Panel Structure

```jsx
<div
  className="flex flex-col"
  style={{
    background: 'var(--bg-primary)',
    border: '1px solid var(--border-secondary)',
    borderRadius: '8px',             // radius-4
    paddingTop: '2px',               // spacing-0.5
    width: '100%',
    minWidth: '160px',
    maxHeight: '400px',
    overflow: 'auto',
    boxShadow: 'var(--shadow-lg)',
    animation: 'slideDown 200ms ease-out',
  }}
  role="listbox"
>
  {children}
</div>
```

### Panel Sizes

By default, the panel matches the trigger element's width. When a fixed size is specified:

| Size | Width |
|------|-------|
| Auto | Matches trigger width |
| xs | 192px |
| sm | 224px |
| md | 256px |
| lg | 300px |
| xl | 384px |

### Panel Positioning

The panel appears 4px below the trigger element (default). Positioning fallback order:

1. Below trigger, left-aligned
2. Above trigger, left-aligned (if no space below)
3. Below trigger, right-aligned
4. Above trigger, right-aligned

When position is set to `top`, the order reverses (above first).

### Slide-Down Animation

```css
@keyframes slideDown {
  from {
    opacity: 0;
    transform: translateY(-12px);  /* animation-distance md = 12px */
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
/* Duration: 200ms, Timing: ease-out */
```

### Backdrop

A transparent backdrop covers the page behind the panel. Clicking it closes the dropdown.

---

## 3. Dropdown

A select control supporting single-select, multi-select, search, grouped options, and custom triggers.

### Dropdown Trigger Structure

```jsx
<FormFieldWrapper label={label} error={error} hint={hint} size={size}>
  <InputWrapper size={size} disabled={disabled} error={hasError}>
    {leadingIcon}
    {leadingAdornment}

    {/* Custom trigger OR default readonly input */}
    {customTrigger && hasValue ? (
      <div className="flex-1 cursor-pointer">{customTriggerContent}</div>
    ) : (
      <input
        className="flex-1 w-full outline-none bg-transparent border-none cursor-pointer"
        style={{ fontFamily: 'inherit', fontSize: 'inherit', lineHeight: 'inherit', fontWeight: 400, color: 'var(--text-primary)' }}
        readOnly
        placeholder={placeholder}
        value={displayValue}
        disabled={disabled}
      />
    )}

    <ChevronDownIcon className="cursor-pointer" />
    {trailingIcon}
    {trailingAdornment}
  </InputWrapper>
</FormFieldWrapper>

{/* Panel (visible when open) */}
{isOpen && (
  <DropdownPanel>
    {showSearchBar && <SearchBar />}
    <div style={{ maxHeight: '300px', overflowY: 'auto' }}>
      {groupedOptions}
      {options}
      {options.length === 0 && <EmptyState />}
    </div>
    {footer && <DropdownFooter>{footerContent}</DropdownFooter>}
  </DropdownPanel>
)}
```

### Dropdown Inputs

| Input | Type | Default | Description |
|-------|------|---------|-------------|
| `placeholder` | string | `''` | Placeholder when no selection |
| `size` | `'small'` \| `'medium'` \| `'large'` | `'medium'` | Control size |
| `disabled` | boolean | `false` | Disables the control |
| `multiSelect` | boolean | `false` | Enables multi-select mode |
| `showSearchBar` | boolean | `false` | Shows search input in panel |
| `popupSize` | `'xs'`\|`'sm'`\|`'md'`\|`'lg'`\|`'xl'` | auto | Fixed panel width |
| `popupPosition` | `'below'` \| `'top'` | `'below'` | Panel position preference |

### Single-Select Behavior

- Clicking an option selects it and closes the panel
- The selected option's display value appears in the trigger input
- A check icon appears to the right of the selected option in the list
- Only one option can be selected at a time

### Multi-Select Behavior

- Each option displays a checkbox on the left side
- Clicking an option toggles its selection (panel stays open)
- The trigger input shows comma-separated display values: `"Red, Green, Blue"`
- The panel includes a total count of selected items
- Value is an array of selected option values

```jsx
// Multi-select display
<input readOnly value="Red, Green, Blue" />  // multiSelectDisplayValue
```

### Search Bar

When `showSearchBar=true`, a search input appears at the top of the panel:

```jsx
<div style={{ paddingTop: '2px', paddingBottom: '2px', paddingLeft: '4px', paddingRight: '4px' }}>
  <TextfieldSmall
    placeholder="Search"
    size="small"
    leadingIcon={<SearchIcon />}
    value={searchText}
    onChange={handleSearch}
  />
</div>
```

**Default search behavior:** Case-insensitive substring matching on the option's display value.

```javascript
const defaultSearchPredicate = (searchText, option) => {
  return option.displayValue.toLowerCase().includes(searchText.toLowerCase());
};
```

Options that don't match are hidden (`display: none`). When the panel closes, search text resets to empty and all options are shown again.

**Options container with search:**
```jsx
// When search bar is shown, the options container adjusts height
style={{ height: 'calc(100% - 36px)', overflow: 'auto' }}
```

### Option Groups

Options can be organized into titled groups:

```jsx
<div role="group">
  {visibleOptionsCount > 0 && (
    <>
      <p style={{
        padding: '16px 16px 4px 16px',
        fontSize: '12px',
        lineHeight: '18px',
        fontWeight: 600,
        color: 'var(--text-tertiary)',
      }}>
        {groupTitle}
      </p>
    </>
  )}
  {groupOptions}
  {visibleOptionsCount > 0 && <Separator gap="xs" />}
</div>
```

- Group title uses `title-sm` typography (12px / 18px / 600 weight)
- Color: `var(--text-tertiary)` (#6B748E)
- Padding: 16px left/right, 16px top, 4px bottom
- A separator line appears below each group (except when group has no visible options)

### Dropdown Footer

```jsx
<div style={{
  paddingTop: '12px',   // py-3
  paddingBottom: '12px',
  paddingLeft: '16px',  // px-4
  paddingRight: '16px',
  display: 'block',
  borderTop: '1px solid var(--border-tertiary)',
}}>
  {footerContent}
</div>
```

### Custom Trigger

For custom trigger content (e.g., showing an avatar + name instead of plain text):

```jsx
<Dropdown>
  <DropdownTrigger>
    {/* Custom content — only shown when a value is selected */}
    <div className="flex items-center gap-2">
      <Avatar src={selected.avatar} size="xs" />
      <span>{selected.name}</span>
    </div>
  </DropdownTrigger>
  <Option value="1">Option 1</Option>
</Dropdown>
```

The custom trigger:
- Has `role="button"`, `aria-haspopup="listbox"`, `tabIndex={0}`
- Uses `flex-1 cursor-pointer` styling
- Only renders when `customTrigger` exists AND value is present
- When no value, the standard readonly input with placeholder is shown instead

### Keyboard Navigation

| Key | Action |
|-----|--------|
| **Arrow Down / Up** | Navigate options (wraps around) |
| **Enter / Space** | Open dropdown (if closed) or select highlighted option |
| **Escape** | Close dropdown |
| **Home / End** | Jump to first/last option |
| **Tab / PageUp / PageDown** | Prevented when dropdown is open |
| **Type-ahead** | Quick-find by first letter (only when search is disabled) |

Active option auto-scrolls into view with `scrollIntoView({ behavior: 'instant', block: 'center' })`.

---

## 4. Autocomplete

A headless suggestion panel that attaches to any text input. Unlike Dropdown, Autocomplete is NOT a form control — it does not manage its own value.

### Key Differences from Dropdown

| Aspect | Dropdown | Autocomplete |
|--------|----------|-------------|
| Is a form control? | Yes | No |
| Manages value? | Yes | No — parent controls value |
| Built-in search? | Optional | No — parent filters options |
| Multi-select? | Yes | No |
| Has its own trigger? | Yes (built-in input) | No — attaches to any input |
| Filtering logic? | Internal | External (parent component) |
| When does it open? | On click/keyboard | On focus |
| When does it close? | On selection/escape/backdrop | On selection/escape OR when options list becomes empty |

### Autocomplete Structure

```jsx
{/* Parent controls the text input and filtering */}
<TextInput
  value={searchText}
  onChange={(val) => {
    setSearchText(val);
    setFilteredOptions(allOptions.filter(o => o.includes(val)));
  }}
  ref={autocompleteRef}  // attaches autocomplete directive
/>

{/* Autocomplete panel — positioned below the input */}
{isOpen && filteredOptions.length > 0 && (
  <DropdownPanel origin={inputRef}>
    <div style={{ paddingBottom: '2px' }}>
      {filteredOptions.map(option => (
        <Option
          key={option}
          value={option}
          onSelect={() => {
            setSearchText(option.displayValue);
            closePanel();
          }}
        >
          {option}
        </Option>
      ))}
    </div>
  </DropdownPanel>
)}
```

### Autocomplete Behavior

1. **Opens** when the text input receives focus
2. **Closes** when:
   - An option is selected
   - Escape is pressed
   - Options list becomes empty (unless `showEmptyState=true`)
   - User clicks outside (backdrop click)
3. **Selection**: When an option is selected, its `displayValue` is passed back to the parent input
4. **No internal value management**: The parent is responsible for:
   - Filtering options based on input text
   - Updating the options list (add/remove `<Option>` elements)
   - Setting the input value when an option is selected

### Autocomplete Inputs

| Input | Type | Default | Description |
|-------|------|---------|-------------|
| `size` | `'small'` \| `'medium'` \| `'large'` | `'medium'` | Option sizes |
| `popupSize` | `'xs'`\|`'sm'`\|`'md'`\|`'lg'`\|`'xl'` | auto | Panel width |
| `showEmptyState` | boolean | `false` | Keep panel open when no options match |

### Autocomplete Keyboard Navigation

| Key | Action |
|-----|--------|
| **Arrow Down / Up / Left / Right** | Navigate options |
| **Enter** | Select highlighted option |
| **Escape** | Close panel |
| **Tab / PageUp / PageDown** | Prevented when panel is open |

### Empty State

When `showEmptyState=true` and no options match:
```jsx
<DropdownPanel>
  <EmptyState
    title="No results found"
    description="Try a different search term"
  />
</DropdownPanel>
```

---

## Panel Options Container

### Dropdown Options Container
```jsx
style={{
  maxHeight: '300px',
  overflowY: 'auto',
}}
```

### Autocomplete Options Container
```jsx
style={{
  paddingBottom: '2px',  // spacing-0.5
}}
```

Both containers sit inside the Dropdown Popup Panel which has its own `maxHeight: 400px`.

---

## Implementation Pattern for Lovable

```jsx
{/* === Complete Dropdown — Single-Select, Medium Size === */}
function Dropdown({ label, placeholder = 'Select', options, value, onChange, disabled }) {
  const [isOpen, setIsOpen] = React.useState(false);
  const [activeIndex, setActiveIndex] = React.useState(-1);

  const selectedOption = options.find(o => o.value === value);

  return (
    <div className="relative">
      {/* Label */}
      {label && (
        <label className="block mb-1.5" style={{
          fontSize: '14px', lineHeight: '20px', fontWeight: 500, color: 'var(--text-primary)',
        }}>{label}</label>
      )}

      {/* Trigger — styled like form field input wrapper */}
      <button
        type="button"
        onClick={() => !disabled && setIsOpen(!isOpen)}
        className="flex items-center w-full cursor-pointer"
        style={{
          height: '40px',
          padding: '10px 12px',
          borderRadius: '6px',
          border: `1px solid ${isOpen ? 'var(--bg-brand-secondary-solid)' : 'var(--border-secondary)'}`,
          background: disabled ? 'var(--bg-secondary_subtle)' : 'var(--bg-primary)',
          boxShadow: isOpen ? 'var(--ring-brand-secondary)' : 'none',
          fontSize: '14px', lineHeight: '20px', fontWeight: 400,
          color: selectedOption ? 'var(--text-primary)' : 'var(--text-disabled)',
          opacity: disabled ? 0.6 : 1,
          pointerEvents: disabled ? 'none' : 'auto',
        }}
      >
        <span className="flex-1 text-left truncate">
          {selectedOption ? selectedOption.label : placeholder}
        </span>
        <svg width="16" height="16" viewBox="0 0 16 16" fill="none" className="shrink-0 ml-2">
          <polyline points="4,6 8,10 12,6" stroke="var(--text-primary)" strokeWidth="1.5" strokeLinecap="round" strokeLinejoin="round" />
        </svg>
      </button>

      {/* Backdrop */}
      {isOpen && (
        <div className="fixed inset-0 z-40" onClick={() => setIsOpen(false)} />
      )}

      {/* Dropdown Panel */}
      {isOpen && (
        <div
          className="absolute left-0 w-full z-50 flex flex-col"
          style={{
            marginTop: '4px',
            background: 'var(--bg-primary)',
            border: '1px solid var(--border-secondary)',
            borderRadius: '8px',
            paddingTop: '2px',
            maxHeight: '400px',
            overflow: 'auto',
            boxShadow: 'var(--shadow-lg)',
            animation: 'slideDown 200ms ease-out',
          }}
          role="listbox"
        >
          {options.map((option, index) => (
            <div key={option.value} style={{ padding: '2px 4px' }}>
              <div
                role="option"
                aria-selected={option.value === value}
                className="inline-flex justify-between items-center w-full cursor-pointer"
                style={{
                  padding: '8px 12px',
                  borderRadius: '4px',
                  fontSize: '14px', lineHeight: '20px', fontWeight: 500,
                  color: option.disabled ? 'var(--text-disabled)' : 'var(--text-primary)',
                  background: index === activeIndex ? 'var(--bg-secondary)' : 'transparent',
                  pointerEvents: option.disabled ? 'none' : 'auto',
                }}
                onMouseEnter={() => setActiveIndex(index)}
                onMouseLeave={() => setActiveIndex(-1)}
                onClick={() => {
                  onChange(option.value);
                  setIsOpen(false);
                }}
              >
                <span className="flex-1 break-words">{option.label}</span>
                {option.value === value && (
                  <svg width="18" height="18" viewBox="0 0 18 18" fill="none" className="shrink-0 ml-2">
                    <polyline points="4,9 7,12 14,5" stroke="var(--fg-brand-secondary)" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" />
                  </svg>
                )}
              </div>
            </div>
          ))}
        </div>
      )}

      <style>{`
        @keyframes slideDown {
          from { opacity: 0; transform: translateY(-12px); }
          to { opacity: 1; transform: translateY(0); }
        }
      `}</style>
    </div>
  );
}
```

---

## CRITICAL Rules

1. **NEVER use `<select>` HTML element** or shadcn Select/Combobox. Build the dropdown from scratch with a custom panel.
2. **Dropdown is a form control** that manages its own value. **Autocomplete is NOT** — the parent manages filtering and value.
3. **Single-select closes on selection**. Multi-select stays open.
4. **Multi-select shows checkboxes** inside each option. Single-select shows a check icon for the selected option. Checkboxes MUST use `checkbox.md`.
5. **The dropdown panel uses border-radius 8px** (radius-4), NOT 6px. This is different from the input wrapper which uses 6px.
6. **Panel shadow is `var(--shadow-lg)`**.
7. **Panel border is `var(--border-secondary)` (#CCD6DC)**, same as the default input wrapper border.
8. **The chevron-down icon is always shown** in the dropdown trigger — it is not conditional.
9. **Search bar uses a Small-sized textfield** inside the dropdown panel, with a search leading icon.
10. **Group titles use title-sm typography** (12px / 18px / 600 weight) in `var(--text-tertiary)` (#6B748E) color, with 16px padding.
11. **Option outer padding is 2px 4px** — this is the gap between options, not the inner content padding.
12. **Option inner padding is 8px 12px** for small/medium and **10px 12px** for large.
13. **Active option (keyboard) and hovered option share the same highlight**: `var(--bg-secondary)` (#F1F4F7). Active+Hover uses the darker `var(--bg-secondary_hover)` (#E8ECF1).
14. **Panel animation is slideDown 200ms ease-out** with 12px translateY distance.
15. **Autocomplete opens on focus, closes when options become empty** (unless showEmptyState is true).
