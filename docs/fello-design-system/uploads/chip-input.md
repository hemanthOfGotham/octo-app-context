# Chip Input Component — Strict Specification

When building any tag input, chip input, multi-value text field, or token input, follow these specifications exactly. Do NOT use shadcn TagInput or any third-party chip library.

> **TOKEN RULE:** Every color in this file is expressed as a CSS variable token. In code use `bg-[var(--bg-token)]` / `text-[var(--text-token)]` / `border-[var(--border-token)]` classes. In inline styles use `'var(--token)'`. The parenthetical hex is for reference only — never use raw hex in production code.

**Note:** This component uses the Form Field wrapper for its outer shell (see `form-field.md`). The chip input replaces the inner `<input>` with a flex-wrap container of Tag chips + an inline text input.

---

## Architecture

```
Form Field (outer wrapper — label, hint, error)
└── Input Wrapper (bordered container — see form-field spec)
    └── Chips Wrapper (flex-wrap container)
        ├── Tag chip(s) — with close button
        ├── Inline text input — grows to fill remaining space
        └── Autocomplete Dropdown (floating panel)
            ├── Loading spinner
            ├── Suggestion options
            └── "Press enter to add" fallback option
```

---

## Chips Wrapper Structure

```jsx
<div
  className="flex items-center flex-wrap w-full"
  style={{ gap: chipGap }}
  tabIndex={0}
>
  {/* Rendered chips */}
  {chips.map(chip => (
    <Tag
      key={chip}
      text={chip}
      size={tagSize}
      trailingType="close-button"
      onClose={() => removeChip(chip)}
    />
  ))}

  {/* Inline text input (hidden when max reached) */}
  {!isMaxReached && (
    <input
      className="flex-1"
      style={{ minWidth: '90px' }}
      role="combobox"
      aria-haspopup="listbox"
      placeholder={chips.length === 0 ? placeholder : ''}
      value={textValue}
      onChange={handleTextChange}
      onKeyDown={handleKeyDown}
      onBlur={handleBlur}
    />
  )}
</div>
```

---

## Chip Gap by Field Size

| Field Size | Chip Gap |
|-----------|----------|
| Small | 4px (spacing-1) |
| Medium | 6px (spacing-1.5) |
| Large | 8px (spacing-2) |

---

## Tag (Chip) Component

Each chip renders as a Tag with a close button. Tag size maps 1:1 from field size.

### Tag Dimensions

| Size | Height | Padding | Typography | Close Icon Size |
|------|--------|---------|------------|----------------|
| Small | 20px | 2px 8px | caption-sm: 12px/18px/500 | 10px |
| Medium | 24px | 3px 8px | caption-md: 14px/20px/500 | 12px |
| Large | 28px | 4px 8px | caption-md: 14px/20px/500 | 14px |

When close button is visible, right padding reduces to 4px (spacing-1).

### Tag Styling

```jsx
style={{
  display: 'inline-flex',
  alignItems: 'center',
  gap: '6px',                       // spacing-1.5
  height: tagHeight,
  padding: tagPadding,
  borderRadius: '6px',              // radius-3
  border: '1px solid var(--border-secondary)',  /* #CCD6DC */
  background: 'var(--bg-primary)',             /* #FFFFFF */
  color: 'var(--text-primary)',                /* #353E5A */
  cursor: 'pointer',
  transition: 'border-color 150ms ease-out',
}}
```

### Tag States

| State | Border Color |
|-------|-------------|
| Default | `var(--border-secondary)` (#CCD6DC) |
| Hover | `var(--fg-disabled)` (#8EA1AF) |
| Focus-visible | `var(--bg-brand-secondary-solid_hover)` (#0BA5A7) |
| Disabled | background `var(--bg-secondary)` (#F1F4F7), color `var(--text-disabled)` (#8EA1AF), pointer-events: none |

### Tag Close Button

```jsx
style={{
  display: 'inline-flex',
  alignItems: 'center',
  justifyContent: 'center',
  background: 'var(--bg-secondary)',             /* #F1F4F7 */
  borderRadius: '4px',              // radius-2
  padding: '2px',                   // spacing-0.5
  transition: 'background-color 150ms ease-out',
}}
// Hover: background 'var(--bg-secondary_hover)' (#E8ECF1)
```

---

## Input Wrapper Padding

The Input Wrapper switches to **compact padding** when chips are present and no leading/trailing decorations exist:

| Size | Standard Padding | Compact Padding (with chips) |
|------|-----------------|------------------------------|
| Small | 4px 16px | 4px 4px |
| Medium | 7px 16px | 7px 8px |
| Large | 9px 16px | 9px 10px |

---

## Inputs

| Input | Type | Default | Description |
|-------|------|---------|-------------|
| `placeholder` | string | `''` | Placeholder for text input |
| `maxAllowedChips` | number \| undefined | undefined | Max chips; hides input when reached |
| `suggestionDataSource` | `(text: string) => Promise<string[]>` | undefined | Async function returning filtered suggestions |
| `inputValidator` | `(text: string) => string \| null` | undefined | Custom validator; return error string or null |
| `inputModifier` | `(text: string) => string` | undefined | Transforms input text on every keystroke |
| `chipValueTransformer` | `(text: string) => string` | undefined | Transforms chip value before adding |
| `size` | `'small'` \| `'medium'` \| `'large'` | `'medium'` | Field and chip size |
| `label` | string | — | Label above the field |
| `hint` | string | — | Hint text below the field |
| `disabled` | boolean | `false` | Disables entire component |
| `required` | boolean | `false` | Shows asterisk on label |

---

## Keyboard Interactions

### On the Text Input

| Key | Action |
|-----|--------|
| **Enter** | If text is valid, not duplicate, and max not reached: add as chip. If autocomplete is showing a highlighted suggestion, select that instead. |
| **Backspace** (when text is empty) | Focus the last chip in the list |
| **Escape** | Hide autocomplete popup, clear text |
| **Arrow Up / Down** | Navigate autocomplete suggestions |
| **Tab** | Prevented when autocomplete is open |

### On a Focused Chip

| Key | Action |
|-----|--------|
| **Backspace** | Remove the focused chip |
| **Left / Right Arrow** | Navigate between chips (wraps around) |

---

## Duplicate Detection

- Chips are stored in a Set-like structure; duplicates are detected in real-time.
- When a duplicate is entered:
  - Autocomplete shows suggestions (if `suggestionDataSource` provided) but Enter won't add the duplicate.
  - Hint message: `"You cannot enter duplicate values here."`

---

## Max Chips Behavior

- When `maxAllowedChips` is reached:
  - The text input is completely removed from the DOM.
  - Hint message: `"You cannot enter more than {n} values here."`
  - Autocomplete popup will not show.

---

## Custom Validation

- `inputValidator` is called on every text change.
- Returns `null`/empty for valid, or an error message string.
- When invalid: autocomplete hidden, Enter won't add chip.
- Hint priority: max reached > duplicate > custom error > default hint.

---

## Autocomplete Dropdown

Uses the same floating panel as Dropdown V2 (see `select-inputs.md`):

```jsx
style={{
  background: 'var(--bg-primary)',             /* #FFFFFF */
  border: '1px solid var(--border-secondary)', /* #CCD6DC */
  borderRadius: '8px',
  padding: '2px 0',
  minWidth: '160px',
  maxHeight: '400px',
  overflow: 'auto',
  boxShadow: 'var(--shadow-lg)',              /* 0 4px 6px -2px #4958830a, 0 12px 16px -4px #49588329 */
}}
```

### Popup Content

```jsx
{isLoading ? (
  <Option disabled><Spinner size="sm" /></Option>
) : (
  suggestions.length > 0 ? (
    suggestions.map(s => <Option key={s} value={s}>{s}</Option>)
  ) : (
    <Option value={textValue}>
      Press enter to add "{textValue}"
    </Option>
  )
)}
```

### Positioning
- Default: below the input wrapper, start-aligned, 4px offset
- Fallback: above if no space below
- Width syncs to input wrapper width
- Clicking outside closes the popup

---

## Chip Removal Focus Management

When a chip is removed:
1. If a chip exists at the same index → focus it.
2. Else if a chip exists at index - 1 → focus that.
3. Else → focus the text input.

---

## Blur Behavior

On input blur:
- Text field is cleared.
- Duplicate flag is reset.
- Custom validation error is cleared.

---

## Color Reference

| Token | Usage |
|-------|-------|
| `var(--text-primary)` (#353E5A) | Chip text, input text |
| `var(--text-secondary)` (#495883) | Hint text |
| `var(--text-disabled)` (#8EA1AF) | Disabled chip text |
| `var(--text-placeholder)` (#BDC8D3) | Input placeholder |
| `var(--text-error-primary)` (#F02C00) | Error hint text |
| `var(--bg-primary)` (#FFFFFF) | Chip background, input background |
| `var(--bg-secondary)` (#F1F4F7) | Close button background |
| `var(--bg-secondary_hover)` (#E8ECF1) | Close button hover |
| `var(--bg-secondary)` (#F1F4F7) | Disabled field/chip background |
| `var(--border-secondary)` (#CCD6DC) | Default chip and field border |
| `var(--fg-disabled)` (#8EA1AF) | Chip hover border |
| `var(--bg-brand-secondary-solid_hover)` (#0BA5A7) | Field hover/focus, chip focus |
| `var(--border-error-solid)` (#F02C00) | Error state border |
| `var(--ring-brand-primary)` | Field focus ring |
| `var(--ring-gray-primary)` | Error focus ring |
| `var(--shadow-xs)` | Default field shadow |
| `var(--shadow-lg)` | Dropdown shadow |

---

## CRITICAL Rules

1. **Tag border-radius is 6px** (radius-3) — NOT 4px or 8px.
2. **Tag height is 20px/24px/28px** for small/medium/large — fixed heights, not min-heights.
3. **Tag close button uses `var(--bg-secondary)` (#F1F4F7) background** with 4px radius and 2px padding.
4. **The inline input has `minWidth: 90px`** and `flex: 1` — it grows to fill remaining space on the current line.
5. **Chips wrapper uses `flex-wrap: wrap`** — chips flow to the next line when the container is full.
6. **Input wrapper switches to compact padding** when chips are present (narrower left/right padding).
7. **Max chips completely removes the input** from DOM — don't just disable it.
8. **Duplicate detection is real-time** — checked on every keystroke, not just on Enter.
9. **Autocomplete fallback shows "Press enter to add"** when no suggestions exist and input is valid.
10. **On blur, the text field is cleared** — partial text is NOT auto-added as a chip.
