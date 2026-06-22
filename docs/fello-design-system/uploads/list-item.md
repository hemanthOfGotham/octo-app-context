# List Item & List Box — Strict Specification

When building any selectable list, menu list, option list, or grouped list of items, follow these specifications exactly. Do NOT use shadcn `<Command>`, `<Listbox>`, or default HTML `<select>`. Build from scratch using these specs.

> **TOKEN RULE:** Every color in this file is expressed as a CSS variable token. In code use `bg-[var(--bg-token)]` / `text-[var(--text-token)]` / `border-[var(--border-token)]` classes. In inline styles use `'var(--token)'`. The parenthetical hex is for reference only — never use raw hex in production code.

**Note:** This component is different from the Option component (used inside Dropdowns/Autocomplete — see `select-inputs.md`). List Item is richer — it supports icons, avatars, buttons, supporting text, and danger variants.

---

## Architecture

```
List Box (behavioral wrapper — manages selection + keyboard)
└── List Item (visual component — renders content)
    ├── Checkbox (auto-shown in multi-select)
    ├── Leading content (icon or avatar)
    ├── Text column
    │   ├── Primary text
    │   └── Supporting text
    ├── Check icon (auto-shown in single-select when selected)
    └── Trailing content (icon, button, or text)
```

---

## List Item Structure

```jsx
<div
  style={{
    padding: outerPadding,
    background: 'var(--bg-primary)',  /* #FFFFFF */
    display: 'block',
  }}
>
  <div
    className="flex items-center gap-2 w-full"
    style={{
      padding: innerPadding,
      borderRadius: itemBorderRadius,
      color: variant === 'danger' ? 'var(--text-error-primary)' : 'var(--text-primary)',  /* #F02C00 / #353E5A */
      transition: 'background-color 150ms ease-out',
      cursor: 'pointer',
      ...typographyStyle,
      ...stateBackground,
    }}
  >
    {/* Checkbox — only in multi-select mode */}
    {multiSelect && <Checkbox checked={selected} disabled={disabled} />}

    {/* Leading icon or avatar */}
    {leadingIcon && <Icon style={{ fontSize: iconFontSize }} />}
    {leadingAvatar && <Avatar />}

    {/* Text column */}
    <div className="flex flex-col flex-1 text-left">
      <p>{text}</p>
      {supportingText && (
        <div style={{ fontFamily: 'Inter, sans-serif', fontSize: '12px', lineHeight: '18px', fontWeight: 400, color: 'var(--text-tertiary)' /* #6B748E */ }}>
          {supportingText}
        </div>
      )}
    </div>

    {/* Check icon — single-select only, when selected */}
    {selected && !multiSelect && (
      <CheckIcon style={{ color: 'var(--fg-brand-secondary)' /* #098486 */ }} />
    )}

    {/* Trailing content: icon, button, or text */}
    {trailingIcon && <Icon style={{ fontSize: '16px' }} />}
    {trailingButton}
    {trailingText}
  </div>
</div>
```

---

## Size Variants

### Large (LG)

| Property | Value |
|----------|-------|
| Typography | caption-lg: 16px / 24px / 500 |
| Inner padding | 10px 12px |
| Border radius | 4px (radius-2) |
| Icon font size | 24px |
| Outer padding | 2px 4px |

### Medium (MD) — Default

| Property | Value |
|----------|-------|
| Typography | caption-md: 14px / 20px / 500 |
| Inner padding | 8px 12px |
| Border radius | 4px (radius-2) |
| Icon font size | 20px |
| Outer padding | 2px 4px |

### Small (SM)

| Property | Value |
|----------|-------|
| Typography | caption-md: 14px / 20px / 500 |
| Inner padding | 6px 16px |
| Border radius | 8px (radius-4) |
| Icon font size | 20px |
| Outer padding | 0px |

---

## Color Variants

### Primary (Default)

| State | Background | Text Color |
|-------|-----------|------------|
| Default | transparent | `var(--text-primary)` (#353E5A) |
| Hover / Focus | `var(--bg-secondary)` (#F1F4F7) | `var(--text-primary)` (#353E5A) |
| Selected | `var(--bg-secondary)` (#F1F4F7) | `var(--text-primary)` (#353E5A) |
| Selected + Hover | `var(--bg-secondary_hover)` (#E8ECF1) | `var(--text-primary)` (#353E5A) |
| Disabled | transparent | `var(--text-disabled)` (#8EA1AF) |
| Disabled + Selected | `var(--bg-secondary)` (#F1F4F7) | `var(--text-disabled)` (#8EA1AF) |

### Danger

| State | Background | Text Color |
|-------|-----------|------------|
| Default | transparent | `var(--text-error-primary)` (#F02C00) |
| Hover / Focus | `var(--bg-error-primary)` (#FFEBE6) | `var(--text-error-primary)` (#F02C00) |
| Disabled | transparent | `var(--text-disabled)` (#8EA1AF) |

---

## Selected State Indicators

### Single-Select Mode

When selected, a **check icon** (✓) appears on the right side:
- Color: `var(--fg-brand-secondary)` (#098486)
- Position: after trailing content slot

### Multi-Select Mode

When multi-select is active, a **checkbox** appears on the left side:
- **MUST use the exact Checkbox from `checkbox.md`** — same 16×16 box, same `var(--bg-brand-secondary-solid)` (#098486) teal fill, same `var(--border-primary)` (#BDC8D3) default border, same SVG checkmark icon. NEVER build a different checkbox.
- Before leading icon/avatar
- Checked state tracks selection
- Disabled state syncs with item disabled state
- No check icon on the right

---

## Supporting Text

Appears below the primary text in a flex column:

```jsx
<div style={{
  fontFamily: 'Inter, sans-serif',
  fontSize: '12px',
  lineHeight: '18px',
  fontWeight: 400,
  color: 'var(--text-tertiary)',  /* #6B748E */
}}>
  {supportingText}
</div>
```

Multiple supporting text lines can be stacked.

---

## List Item Group

Groups multiple list items under a titled section:

```jsx
<div role="group" style={{ background: 'var(--bg-primary)' /* #FFFFFF */, display: 'block' }}>
  <p style={{
    padding: groupTitlePadding,
    ...groupTitleTypography,
  }}>
    {groupTitle}
  </p>
  {children}
</div>
```

### Group Title by Size

| Size | Typography | Padding (V × H) |
|------|-----------|-----------------|
| **LG** | title-xl: 18px / 28px / 600 | 12px × 16px |
| **MD** | title-lg: 16px / 24px / 600 | 10px × 16px |
| **SM** | title-lg: 16px / 24px / 600 | 6px × 16px |

---

## List Box (Behavioral Wrapper)

The List Box is a container that adds selection management and keyboard navigation to a group of List Items. It has **no visual styling of its own** — it only provides behavior.

### Usage

```jsx
{/* Single Select */}
<div className="flex flex-col" role="listbox">
  <ListItem text="Option 1" selected={value === 'opt1'} onClick={() => setValue('opt1')} />
  <ListItem text="Option 2" selected={value === 'opt2'} onClick={() => setValue('opt2')} />
</div>

{/* Multi-Select */}
<div className="flex flex-col" role="listbox" aria-multiselectable="true">
  <ListItem text="Option 1" multiSelect selected={values.includes('opt1')} onClick={() => toggleValue('opt1')} />
  <ListItem text="Option 2" multiSelect selected={values.includes('opt2')} onClick={() => toggleValue('opt2')} />
</div>
```

### Selection Behavior

**Single-select (default):**
- Clicking an item selects it and deselects the previous selection
- Value is a single item: `T`

**Multi-select:**
- Clicking an item toggles its selection
- Value is an array: `T[]`
- Checkboxes shown on each item

### Keyboard Navigation

| Key | Action |
|-----|--------|
| **Arrow Down** | Focus next item |
| **Arrow Up** | Focus previous item |
| **Space** | Toggle selection (multi) or select (single) |
| **Enter** | Select/activate item |
| **Type-ahead** | Jump to item starting with typed character |

---

## Complete Color Reference

| Token | Usage |
|-------|-------|
| `var(--text-primary)` (#353E5A) | Default item text |
| `var(--text-tertiary)` (#6B748E) | Supporting text |
| `var(--text-disabled)` (#8EA1AF) | Disabled item text |
| `var(--text-error-primary)` (#F02C00) | Danger variant text |
| `var(--fg-brand-secondary)` (#098486) | Check icon (selected) |
| `var(--bg-primary)` (#FFFFFF) | Default background |
| `var(--bg-secondary)` (#F1F4F7) | Hover background |
| `var(--bg-secondary)` (#F1F4F7) | Selected background |
| `var(--bg-secondary_hover)` (#E8ECF1) | Selected + hover |
| `var(--bg-error-primary)` (#FFEBE6) | Danger hover |

---

## CRITICAL Rules

1. **List Item ≠ Option** — List Item is for standalone lists with rich content. Option is for dropdowns/autocomplete (see `select-inputs.md`).
2. **Multi-select shows checkboxes, single-select shows check icons** — never show both simultaneously. Checkboxes MUST use `checkbox.md`.
3. **Check icon color is teal `var(--fg-brand-secondary)` (#098486)**, NOT the brand primary orange.
4. **SM size uses 8px border-radius** (radius-4), while MD and LG use 4px (radius-2).
5. **SM outer padding is 0px** — the item has no outer spacing at small size.
6. **Danger variant only changes text color to `var(--text-error-primary)` (#F02C00)** — hover background changes to `#FFEBE6` (light red).
7. **Supporting text uses paragraph-sm** (12px/18px/400) in `var(--text-tertiary)` (#6B748E) — always below the primary text.
8. **Trailing icon font-size is always 16px** regardless of component size.
9. **Group title uses title-lg (16px/24px/600)** for MD and SM sizes, title-xl (18px/28px/600) for LG size.
10. **List Box has NO visual styling** — it only provides selection state management and keyboard behavior. All visual rendering comes from List Item.
