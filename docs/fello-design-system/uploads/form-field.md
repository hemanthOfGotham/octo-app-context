# Form Field & Input Wrapper — Strict Specification

When building any form input layout that includes a label, hint text, error message, or a bordered input container, follow these specifications exactly. Do NOT use default shadcn form fields, Tailwind form groups, or any other form wrapper system.

> **TOKEN RULE:** Never use raw hex color values. Always use the CSS variable tokens defined in this spec (e.g., `var(--text-primary)`, `var(--bg-primary)`, `var(--border-secondary)`). In Tailwind classes, use arbitrary value syntax: `text-[var(--text-primary)]`, `bg-[var(--bg-primary)]`, `border-[var(--border-secondary)]`. In inline styles, use `color: 'var(--text-primary)'`, etc.

### ⛔ Default Icon Rule

**Leading icon and trailing icon slots are EMPTY by default.** Do not populate them with icons unless the user explicitly asks. No email icon for email fields, no phone icon for phone fields, no user icon for name fields, etc. The only icons that appear automatically are: password toggle (eye), clear button (×), error icon (alert triangle on validation error), and dropdown arrow (select inputs).

---

## Architecture Overview

Every form control in this design system uses a two-layer wrapper:

1. **Form Field** (outer) — provides label, required asterisk, info icon, error/hint text
2. **Input Wrapper** (inner) — provides the bordered container with states, icons, and adornments

```
Form Field
├── Label row: label text + required asterisk + info icon
├── Input Wrapper (bordered container)
│   ├── Leading icon
│   ├── Leading adornment (e.g. "$")
│   ├── [FORM CONTROL — projected from text-inputs or select-inputs spec]
│   ├── Trailing adornment (e.g. ".00")
│   ├── Error icon OR Trailing icon
│   └── Additional trailing content
└── Error message OR Hint message (never both)
```

---

## Form Field (Outer Wrapper)

### Structure

```html
<div className="flex flex-col gap-1">
  {/* Label row */}
  {label && (
    <div className="flex justify-between gap-1" style={{ color: 'var(--text-primary)' }}>
      <span className={labelTypographyClass}>
        {label}
        {required && <span style={{ color: 'var(--text-error-primary)' }}> *</span>}
      </span>
      {infoIcon && (
        <span style={{ color: 'var(--fg-quaternary)', fontSize: '16px' }}>{/* info icon */}</span>
      )}
    </div>
  )}

  {/* Input Wrapper — see next section */}
  {children}

  {/* Error OR Hint — errors take priority */}
  {error ? (
    <span className={hintTypographyClass} style={{ color: 'var(--text-error-primary)' }}>{error}</span>
  ) : hint ? (
    <span className={hintTypographyClass} style={{ color: 'var(--text-secondary)' }}>{hint}</span>
  ) : null}
</div>
```

### Label & Hint Typography by Size

| Size | Label Class | Label Size | Label Weight | Control Text | Error/Hint Class | Error/Hint Size |
|------|------------|------------|--------------|-------------|-----------------|-----------------|
| **Small** | caption-sm | 12px / 18px | 500 | 12px / 18px / 400 | paragraph-xs | 10px / 16px / 400 |
| **Medium** | caption-md | 14px / 20px | 500 | 14px / 20px / 400 | paragraph-sm | 12px / 18px / 400 |
| **Large** | caption-md | 14px / 20px | 500 | 16px / 24px / 400 | paragraph-md | 14px / 20px / 400 |

### Typography Implementation

Use content-fonts for all label/hint/control text. Always use inline `style={{ fontWeight }}` for precision:

**Label (caption family, weight 500):**
```jsx
// Small
<span style={{ fontFamily: 'Inter, sans-serif', fontSize: '12px', lineHeight: '18px', fontWeight: 500 }}>

// Medium / Large
<span style={{ fontFamily: 'Inter, sans-serif', fontSize: '14px', lineHeight: '20px', fontWeight: 500 }}>
```

**Control text (paragraph family, weight 400):**
```jsx
// Small
style={{ fontSize: '12px', lineHeight: '18px', fontWeight: 400 }}

// Medium
style={{ fontSize: '14px', lineHeight: '20px', fontWeight: 400 }}

// Large
style={{ fontSize: '16px', lineHeight: '24px', fontWeight: 400 }}
```

### Colors

| Element | Token | Value |
|---------|-------|-------|
| Label text | `var(--text-primary)` | (#353E5A) |
| Required asterisk | `var(--text-error-primary)` | (#F02C00) |
| Info icon | `var(--fg-quaternary)` | (#9298A9) |
| Hint text | `var(--text-secondary)` | (#495883) |
| Error text | `var(--text-error-primary)` | (#F02C00) |
| Placeholder text | `var(--text-placeholder)` | (#BDC8D3) |
| Leading/trailing icon (default) | `var(--fg-quaternary)` | (#9298A9) |
| Leading/trailing icon (disabled) | `var(--text-disabled-subtle)` | (#BDC8D3) |
| Error icon (replaces trailing) | `var(--text-error-primary)` | (#F02C00) |
| Adornment text (disabled) | `var(--text-disabled-subtle)` | (#BDC8D3) |
| Disabled control text | `var(--text-tertiary)` | (#6B748E) |
| Disabled hint text | `var(--text-disabled-subtle)` | (#BDC8D3) |

### Label Spacing

When a label is present above the input wrapper, add `margin-bottom: 4px` (spacing-1) between the label and the input wrapper. The outer flex column uses `gap: 4px` (gap-1).

---

## Input Wrapper (Inner Bordered Container)

### Structure

```html
<div className={`
  flex items-center gap-2 rounded-md w-full bg-white cursor-pointer
  ${sizeClasses}
  ${stateClasses}
`}>
  {/* Leading icon */}
  {/* Leading adornment */}
  <div className="flex-1">{/* form control (input/textarea/dropdown) */}</div>
  {/* Trailing adornment */}
  {/* Error icon with tooltip OR trailing icon */}
  {/* Additional trailing content */}
</div>
```

### Size Dimensions

| Size | Min Height | Vertical Padding | Horizontal Padding | Icon Size | Compact H-Padding |
|------|-----------|-----------------|-------------------|-----------|-------------------|
| **Small** | 32px | 4px | 16px | 16px | 4px |
| **Medium** | 40px | 7px | 16px | 20px | 8px |
| **Large** | 48px | 9px | 16px | 24px | 10px |

### Tailwind Size Classes

```jsx
// Small
className="min-h-[32px] py-1 px-4"  // icons: w-4 h-4 (16px)

// Medium
className="min-h-[40px] px-4" style={{ paddingTop: '7px', paddingBottom: '7px' }}  // icons: w-5 h-5 (20px)

// Large
className="min-h-[48px] px-4" style={{ paddingTop: '9px', paddingBottom: '9px' }}  // icons: w-6 h-6 (24px)
```

### Border & Shadow States

The input wrapper transitions `border-color` and `box-shadow` with `150ms ease-out`.

**Apply this transition to the wrapper:**
```jsx
style={{ transition: 'border-color 150ms ease-out, box-shadow 150ms ease-out' }}
```

#### Default State
```jsx
style={{
  border: '1px solid var(--border-secondary)',        // (#CCD6DC)
  boxShadow: '0 1px 2px 0 var(--shadow-xs)',          // (#49588314)
}}
```

#### Hover State (not disabled, not error)
```jsx
// On hover:
style={{
  border: '1px solid var(--border-brand-secondary-solid)',  // (#0BA5A7)
  boxShadow: '0 1px 2px 0 var(--shadow-xs)',               // (#49588314)
}}
```

#### Focus State (not disabled, not error)
```jsx
// On focus-within:
style={{
  border: '1px solid var(--border-brand-secondary-solid)',                          // (#0BA5A7)
  boxShadow: '0 0 0 4px var(--ring-brand-secondary), 0 1px 2px 0 var(--shadow-xs-subtle)',  // (#0BA5A73d, #4958830a)
}}
```

#### Error State
```jsx
style={{
  border: '1px solid var(--border-error-solid)',      // (#F02C00)
  boxShadow: '0 1px 2px 0 var(--shadow-xs)',          // (#49588314)
}}
```

#### Error + Focus State
```jsx
// Error state with focus-within:
style={{
  border: '1px solid var(--border-error-solid)',                                   // (#F02C00)
  boxShadow: '0 0 0 4px var(--ring-error-primary), 0 1px 2px 0 var(--shadow-xs-subtle)',  // (#F02C003d, #4958830a)
}}
```

#### Disabled State
```jsx
style={{
  border: '1px solid var(--border-disabled_subtle)',   // (#CCD6DC)
  background: 'var(--bg-disabled_subtle)',             // (#F1F4F7)
  cursor: 'default',
}}
// Icon and adornment color: var(--text-disabled-subtle)  (#BDC8D3)
```

### Border Radius

All input wrappers use `border-radius: 6px` (radius-3 = 3 × 2px).

### Complete State Reference

| State | Border Color | Box Shadow | Background | Cursor |
|-------|-------------|-----------|------------|--------|
| Default | `var(--border-secondary)` (#CCD6DC) | `0 1px 2px 0 var(--shadow-xs)` (#49588314) | `var(--bg-primary)` (#FFFFFF) | pointer |
| Hover | `var(--border-brand-secondary-solid)` (#0BA5A7) | `0 1px 2px 0 var(--shadow-xs)` (#49588314) | `var(--bg-primary)` (#FFFFFF) | pointer |
| Focus | `var(--border-brand-secondary-solid)` (#0BA5A7) | `0 0 0 4px var(--ring-brand-secondary), 0 1px 2px 0 var(--shadow-xs-subtle)` (#0BA5A73d, #4958830a) | `var(--bg-primary)` (#FFFFFF) | pointer |
| Error | `var(--border-error-solid)` (#F02C00) | `0 1px 2px 0 var(--shadow-xs)` (#49588314) | `var(--bg-primary)` (#FFFFFF) | pointer |
| Error+Focus | `var(--border-error-solid)` (#F02C00) | `0 0 0 4px var(--ring-error-primary), 0 1px 2px 0 var(--shadow-xs-subtle)` (#F02C003d, #4958830a) | `var(--bg-primary)` (#FFFFFF) | pointer |
| Disabled | `var(--border-disabled_subtle)` (#CCD6DC) | `0 1px 2px 0 var(--shadow-xs)` (#49588314) | `var(--bg-disabled_subtle)` (#F1F4F7) | default |

---

## Error Icon Behavior

When an error exists, the input wrapper automatically replaces the trailing icon with an alert-triangle error icon:

```jsx
{hasError ? (
  <AlertTriangleIcon
    className="text-[var(--fg-error-primary)]"
    style={{ fontSize: iconSize }}
    title={errorMessage}  // tooltip with error text
  />
) : (
  trailingIcon
)}
```

The error icon shows a tooltip containing the error message text.

---

## Adornments

Adornments are text labels placed inside the input wrapper (e.g., "$", "USD", ".00"):

### Normal Adornment
```jsx
<span style={{ fontWeight: 500, fontSize: captionSize, lineHeight: captionLineHeight }}>
  {adornmentText}
</span>
```

### Highlighted Adornment
Highlighted adornments have a subtle background:
```jsx
<span
  className="rounded"
  style={{
    fontWeight: 500,
    fontSize: captionSize,
    background: 'var(--bg-secondary)',  // (#F1F4F7)
    borderRadius: '4px',     // radius-2
    padding: highlightedPadding,
  }}
>
  {adornmentText}
</span>
```

**Highlighted adornment padding by size:**
| Size | Padding |
|------|---------|
| Small | 2px 4px |
| Medium | 4px |
| Large | 4px |

### Adornment Typography by Size

| Size | Font Size | Line Height | Weight |
|------|-----------|-------------|--------|
| Small | 12px | 18px | 500 |
| Medium | 14px | 20px | 500 |
| Large | 16px | 24px | 500 |

---

## Input Control Base Styles

All form controls (`<input>`, `<textarea>`) inside the wrapper share these styles:

```jsx
<input
  className="flex-1 outline-none bg-transparent border-none w-full"
  style={{
    fontFamily: 'inherit',
    color: 'var(--text-primary)',      // (#353E5A)
    fontSize: 'inherit',
    lineHeight: 'inherit',
    fontWeight: 400,
  }}
  placeholder={placeholder}
  disabled={disabled}
/>
```

**Placeholder color:** `var(--text-placeholder)` (#BDC8D3)

```css
input::placeholder { color: var(--text-placeholder); /* (#BDC8D3) */ }
```

**Disabled input text:** `var(--text-tertiary)` (#6B748E) with `background: var(--bg-disabled_subtle)` (#F1F4F7)

---

## Compact Mode

Some controls use a compact padding mode (e.g., dropdowns with custom triggers):

| Size | Compact H-Padding |
|------|-------------------|
| Small | 4px |
| Medium | 8px |
| Large | 10px |

Vertical padding stays the same; only horizontal padding is reduced.

---

## Assembly Example

A complete medium-sized text field with label, error, and leading icon:

```jsx
{/* Form Field wrapper */}
<div className="flex flex-col gap-1">
  {/* Label */}
  <div className="flex justify-between gap-1">
    <span style={{ color: 'var(--text-primary)', fontFamily: 'Inter, sans-serif', fontSize: '14px', lineHeight: '20px', fontWeight: 500 }}>
      Email <span style={{ color: 'var(--text-error-primary)' }}>*</span>
    </span>
  </div>

  {/* Input Wrapper */}
  <div
    className="flex items-center gap-2 rounded-md w-full min-h-[40px] px-4"
    style={{
      paddingTop: '7px',
      paddingBottom: '7px',
      border: '1px solid var(--border-error-solid)',      // (#F02C00)
      boxShadow: '0 1px 2px 0 var(--shadow-xs)',          // (#49588314)
      background: 'var(--bg-primary)',                     // (#FFFFFF)
      borderRadius: '6px',
      transition: 'border-color 150ms ease-out, box-shadow 150ms ease-out',
    }}
  >
    <SearchIcon className="w-5 h-5 text-[var(--fg-quaternary)]" />
    <div className="flex-1">
      <input
        className="w-full outline-none bg-transparent border-none"
        style={{ fontFamily: 'Inter, sans-serif', fontSize: '14px', lineHeight: '20px', fontWeight: 400, color: 'var(--text-primary)' }}
        placeholder="Enter email"
      />
    </div>
    <AlertTriangleIcon className="w-5 h-5 text-[var(--text-error-primary)]" />
  </div>

  {/* Error message */}
  <span style={{ fontFamily: 'Inter, sans-serif', fontSize: '12px', lineHeight: '18px', fontWeight: 400, color: 'var(--text-error-primary)' }}>
    Please enter a valid email address
  </span>
</div>
```

---

## CRITICAL Rules

1. **NEVER** use shadcn `<FormField>`, `<FormItem>`, `<FormLabel>`, `<FormMessage>` — build from scratch using these specs.
2. **Error takes priority over hint** — if both exist, only show the error message.
3. **Error icon replaces trailing icon** — when in error state, the trailing icon slot shows the alert-triangle error icon instead of any custom trailing icon.
4. **Border radius is always 6px** — never use rounded-lg (8px) or rounded-sm (2px).
5. **The teal focus ring `var(--ring-brand-secondary)` (#0BA5A73d)** is used for ALL form controls — not the brand primary orange.
6. **Placeholder color is `var(--text-placeholder)` (#BDC8D3)** — not Tailwind's default gray placeholder.
7. **All font weights must use inline styles** — `style={{ fontWeight: 500 }}` for labels, `style={{ fontWeight: 400 }}` for body/input text.
8. **`var(--shadow-xs)` (#49588314) is always present** on the input wrapper as the base box-shadow, even in default state.
9. **Disabled background is `var(--bg-disabled_subtle)` (#F1F4F7)** (a very light blue-gray), not plain gray.
10. **This spec is the foundation** — the text-inputs spec and select-inputs spec both reference this document for their wrapper styling.
