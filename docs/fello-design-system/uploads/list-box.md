# List Box — Strict Specification

> **CROSS-REFERENCE — REQUIRED COMPONENT SPEC:**
> When `multiSelect` is enabled, each list item MUST use the **Checkbox component** from `checkbox.md`. Do NOT use a native HTML checkbox. In single-select mode, selected items show a teal checkmark icon on the right.

When building a list box (a selectable list of options for single or multi-select), follow these specifications exactly. The list box is a vertical stack of list items, each clickable, with optional leading icons, sub-text, and selection indicators.

> **TOKEN RULE:** Every colour in this file is expressed as a CSS custom-property token — `var(--token)` — with the raw hex in parentheses only as a comment. When writing Tailwind classes use the `[var(--token)]` syntax (e.g. `bg-[var(--bg-secondary)]`). Never hard-code a hex value.

---

## Architecture

```
Single Select:
┌──────────────────────────────────────┐
│  [icon]  Option 1              ✓     │  ← Selected (teal checkmark)
│  [icon]  Option 2                    │
│  [icon]  Option 3 (disabled)         │  ← Disabled (muted text)
└──────────────────────────────────────┘

Multi Select:
┌──────────────────────────────────────┐
│  ☑  [icon]  Option 1                 │  ← Checked checkbox
│  ☐  [icon]  Option 2                 │  ← Unchecked checkbox
│  ☐  [icon]  Option 3                 │
└──────────────────────────────────────┘
```

---

## Props

### List Box Container

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `multiple` | `boolean` | `false` | Enables multi-select with checkboxes |
| `disabled` | `boolean` | `false` | Disables entire list box |
| `value` | `T \| T[]` | — | Selected value(s) |

### List Item

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `text` | `string` | Required | Main text content |
| `value` | `T` | Required | Option value |
| `size` | `'sm' \| 'md' \| 'lg'` | `'md'` | Size variant |
| `variant` | `'primary' \| 'danger'` | `'primary'` | Color variant |
| `disabled` | `boolean` | `false` | Disables this item |

---

## Sizes

### Small (SM)

| Element | Property | Value | Tailwind |
|---------|----------|-------|----------|
| Container padding | outer | 0 (no padding) | `p-0` |
| Item padding | — | 6px 16px | `py-1.5 px-4` |
| Item border-radius | — | 8px | `rounded-lg` |
| Text font | — | 14px / 20px / 500 | `text-sm leading-5 font-medium` |
| Icon size | — | 20px | `text-[20px]` |

### Medium (MD) — Default

| Element | Property | Value | Tailwind |
|---------|----------|-------|----------|
| Container padding | outer | 2px 4px | `px-1 py-0.5` |
| Item padding | — | 8px 12px | `py-2 px-3` |
| Item border-radius | — | 4px | `rounded` |
| Text font | — | 14px / 20px / 500 | `text-sm leading-5 font-medium` |
| Icon size | — | 20px | `text-[20px]` |

### Large (LG)

| Element | Property | Value | Tailwind |
|---------|----------|-------|----------|
| Container padding | outer | 2px 4px | `px-1 py-0.5` |
| Item padding | — | 10px 12px | `py-2.5 px-3` |
| Item border-radius | — | 4px | `rounded` |
| Text font | — | 16px / 24px / 500 | `text-base leading-6 font-medium` |
| Icon size | — | 24px | `text-[24px]` |

---

## List Item Layout

```
[Checkbox?] [Leading Icon?] [Text + Sub-text] [Checkmark? / Trailing Content?]
```

| Property | Value | Tailwind |
|----------|-------|----------|
| Display | inline-flex | `inline-flex` |
| Align | center | `items-center` |
| Justify | space-between | `justify-between` |
| Gap | 8px | `gap-2` |
| Width | 100% | `w-full` |
| Cursor | pointer | `cursor-pointer` |

### Text Area

| Element | Value |
|---------|-------|
| Main text | `var(--text-primary)` (#353E5A) color, weight from size |
| Sub-text | 12px / 18px / 400 (paragraph-sm), color `var(--text-tertiary)` (#6B748E) |
| Word break | break-word |

---

## States

| State | Background | Text Color |
|-------|------------|------------|
| Default | `var(--bg-primary)` (#FFFFFF) | `var(--text-primary)` (#353E5A) |
| Hover | `var(--bg-secondary)` (#F1F4F7) | `var(--text-primary)` (#353E5A) |
| Selected | `var(--bg-secondary)` (#F1F4F7) | `var(--text-primary)` (#353E5A) |
| Selected + Hover | `var(--bg-secondary_hover)` (#E8ECF1) | `var(--text-primary)` (#353E5A) |
| Disabled | transparent | `var(--text-disabled)` (#8EA1AF) |
| Disabled + Selected | `var(--bg-secondary)` (#F1F4F7) | `var(--text-disabled)` (#8EA1AF) |
| Danger variant | transparent | `var(--text-error-primary)` (#F02C00) |
| Danger + Hover | `var(--bg-error-primary)` (#FFEBE6) | `var(--text-error-primary)` (#F02C00) |

### Background transition

| Property | Value |
|----------|-------|
| Transition | background 150ms ease-out |

---

## Selection Indicators

### Single Select — Checkmark Icon

| Property | Value |
|----------|-------|
| Icon | Check |
| Color | `var(--fg-brand-secondary)` (#098486) |
| Position | Right side of item row |
| Size | Inherits from item size (20px for sm/md, 24px for lg) |

### Multi Select — Checkbox

Uses the checkbox from `checkbox.md` (Small size, 16x16px, no label). See that spec for exact styling.

---

## Item Group (Optional)

Items can be grouped with a title header.

### Group Title by Size

| Size | Padding | Font Size | Line Height | Weight |
|------|---------|-----------|-------------|--------|
| SM | 6px 16px | 16px | 24px | 600 |
| MD | 10px 16px | 16px | 24px | 600 |
| LG | 12px 16px | 18px | 28px | 600 |

---

## Color Reference

| Token | Hex | Usage |
|-------|-----|-------|
| `var(--bg-primary)` (#FFFFFF) | bg-primary | Default item background |
| `var(--bg-secondary)` (#F1F4F7) | bg-secondary | Hover, selected, disabled+selected |
| `var(--bg-secondary_hover)` (#E8ECF1) | bg-secondary-hover | Selected + hover |
| `var(--text-primary)` (#353E5A) | text-primary | Default text |
| `var(--text-tertiary)` (#6B748E) | text-tertiary | Sub-text |
| `var(--text-disabled)` (#8EA1AF) | text-disabled | Disabled text |
| `var(--fg-brand-secondary)` (#098486) | fg-brand-secondary | Selected checkmark |
| `var(--text-error-primary)` (#F02C00) | text-error | Danger variant text |
| `var(--bg-error-primary)` (#FFEBE6) | bg-error-primary | Danger hover background |

---

## Implementation

```jsx
function ListBox({ multiple = false, options = [], value, onChange, size = 'md' }) {
  const selected = multiple ? (Array.isArray(value) ? value : []) : (value != null ? [value] : []);

  const sizeConfig = {
    sm: { containerPad: '', itemPad: 'py-1.5 px-4', radius: 'rounded-lg', text: 'text-sm leading-5', icon: 20 },
    md: { containerPad: 'px-1 py-0.5', itemPad: 'py-2 px-3', radius: 'rounded', text: 'text-sm leading-5', icon: 20 },
    lg: { containerPad: 'px-1 py-0.5', itemPad: 'py-2.5 px-3', radius: 'rounded', text: 'text-base leading-6', icon: 24 },
  }[size];

  const handleSelect = (optValue) => {
    if (multiple) {
      const newVal = selected.includes(optValue)
        ? selected.filter(v => v !== optValue)
        : [...selected, optValue];
      onChange?.(newVal);
    } else {
      onChange?.(optValue);
    }
  };

  return (
    <div className={`flex flex-col ${sizeConfig.containerPad}`} role="listbox">
      {options.map((opt) => {
        const isSelected = selected.includes(opt.value);
        const isDisabled = opt.disabled;
        return (
          <div key={opt.value} className={sizeConfig.containerPad ? '' : ''}>
            <div
              role="option"
              aria-selected={isSelected}
              className={`
                inline-flex items-center justify-between w-full gap-2 cursor-pointer
                font-medium transition-colors duration-150 ease-out
                ${sizeConfig.itemPad} ${sizeConfig.radius} ${sizeConfig.text}
                ${isDisabled ? 'pointer-events-none' : ''}
                ${!isDisabled && !isSelected ? 'hover:bg-[var(--bg-secondary)]' : ''}
                ${!isDisabled && isSelected ? 'bg-[var(--bg-secondary)] hover:bg-[var(--bg-secondary_hover)]' : ''}
              `}
              style={{
                color: isDisabled ? 'var(--text-disabled)' : opt.variant === 'danger' ? 'var(--text-error-primary)' : 'var(--text-primary)',
                fontFamily: 'Inter, Arial, sans-serif',
              }}
              onClick={() => !isDisabled && handleSelect(opt.value)}
            >
              <div className="flex items-center gap-2 flex-1 min-w-0">
                {/* Multi-select checkbox (from checkbox.md) */}
                {multiple && (
                  isSelected ? (
                    <div className="w-4 h-4 rounded shrink-0 flex items-center justify-center"
                      style={{ backgroundColor: 'var(--bg-brand-secondary-solid)', border: '1px solid var(--bg-brand-secondary-solid)' }}>
                      <svg width="10" height="8" viewBox="0 0 10 8" fill="none">
                        <path d="M1 4.11111L3.42857 6.5L9 1" stroke="white" strokeWidth="1.5" strokeLinecap="round" strokeLinejoin="round" />
                      </svg>
                    </div>
                  ) : (
                    <div className="w-4 h-4 rounded shrink-0" style={{ border: '1px solid var(--text-disabled)', backgroundColor: 'var(--bg-primary)' }} />
                  )
                )}

                {/* Leading icon */}
                {opt.icon && <span style={{ fontSize: sizeConfig.icon, color: 'inherit' }}>{opt.icon}</span>}

                {/* Text */}
                <div className="flex flex-col flex-1">
                  <p className="break-words">{opt.label}</p>
                  {opt.subText && (
                    <span className="text-xs leading-[18px] font-normal" style={{ color: 'var(--text-tertiary)' }}>
                      {opt.subText}
                    </span>
                  )}
                </div>
              </div>

              {/* Single-select checkmark */}
              {!multiple && isSelected && (
                <svg width={sizeConfig.icon} height={sizeConfig.icon} viewBox="0 0 24 24" fill="none" style={{ flexShrink: 0 }}>
                  <path d="M20 6L9 17L4 12" stroke="var(--fg-brand-secondary)" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" />
                </svg>
              )}
            </div>
          </div>
        );
      })}
    </div>
  );
}
```

---

## CRITICAL Rules

1. **Single-select checkmark is `var(--fg-brand-secondary)` (#098486)** — appears on the right side. Do NOT use a checkbox in single-select mode.
2. **Multi-select uses the custom checkbox from `checkbox.md`** — 16x16px styled div, NOT a native HTML checkbox. Checked = `var(--bg-brand-secondary-solid)` (#098486) bg with white SVG checkmark.
3. **SM size has no container padding and 8px border-radius** — different from MD/LG which have 2px 4px container padding and 4px border-radius.
4. **SM and MD both use 14px/20px/500 text** — they differ in padding and border-radius, NOT typography.
5. **Danger variant text is `var(--text-error-primary)` (#F02C00)** and hover bg is `#FFEBE6` (light red) — NOT the standard `var(--bg-secondary)` (#F1F4F7) hover.
6. **Selected + Hover uses darker `var(--bg-secondary_hover)` (#E8ECF1)** — NOT the same `var(--bg-secondary)` (#F1F4F7) as selected-only.
7. **Disabled + Selected keeps `var(--bg-secondary)` (#F1F4F7) background** — the selection is still visible but text is muted `var(--text-disabled)` (#8EA1AF).
8. **Background transitions are 150ms ease-out** — NOT instant.
9. **Font is Inter** — all text uses `Inter, Arial, sans-serif`.
10. **Item gap is 8px** — between checkbox/icon and text, and between text and checkmark.
