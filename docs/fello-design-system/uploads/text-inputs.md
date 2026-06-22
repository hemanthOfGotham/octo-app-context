# Text Input Components — Strict Specification

This spec covers **Text Field**, **Textarea**, and **Number Field**. All three use the Form Field + Input Wrapper system defined in `form-field.md`. Read that spec first — it defines the label, error/hint, border states, sizes, and shared input styling that these controls inherit.

Do NOT use shadcn `<Input>`, `<Textarea>`, or any default Tailwind form controls. Build from scratch using these specs.

### ⛔ Default Icon Rule

**`leadingIcon` and `trailingIcon` are EMPTY by default.** Do not add any icons inside input fields unless the user explicitly asks for them. This includes "contextually meaningful" icons — no email icon in email fields, no phone icon in phone fields, no currency icon in amount fields, no calendar icon in date fields, no user icon in name fields.

The ONLY icons that appear automatically (without user asking) are:
- Password toggle (eye/eye-off) — built into `type="password"`
- Clear button (×) — when `clearable={true}`
- Error alert icon — when field has a validation error
- Dropdown arrow — for select inputs only

> **TOKEN RULE:** Every color in this spec is expressed as a CSS variable token. In Tailwind classes use `bg-[var(--token)]` / `text-[var(--token)]` / `border-[var(--token)]`. In inline styles use `'var(--token)'`. In prose and tables, show `var(--token)` (#hex) for reference.

---

## Icon & Adornment Colors (All Text Input Components)

This section applies to **all three controls** (Text Field, Textarea, Number Field). These colors are set by the Input Wrapper — not by the individual controls.

### Leading / Trailing Icon Colors by State

| State | Icon Color Token | Hex | Notes |
|-------|-----------------|-----|-------|
| **Default** | `var(--fg-primary)` | #353E5A | Dark navy — uses `fg` token for icons, NOT `text` token |
| **Hover** | `var(--fg-primary)` | #353E5A | No change on hover |
| **Focus** | `var(--fg-primary)` | #353E5A | No change on focus |
| **Error** | Trailing icon **replaced** by alert-triangle in `var(--text-error-primary)` (#F02C00). Leading icon stays `var(--fg-primary)` | — | Error icon replaces trailing icon |
| **Disabled** | `var(--text-disabled-subtle)` | #BDC8D3 | Both icons AND adornments become this color |

**IMPORTANT:** In the source code, the default icon color is inherited (not explicitly set) — it cascades from the icomoon font which uses `#353E5A`. In the Lovable/React context with Lucide icons that use `currentColor`, you MUST explicitly set the icon color to `var(--fg-primary)` (#353E5A). Icons use `fg-*` tokens, not `text-*` tokens — `text-*` is for text content only.

### Icon Color Implementation

```jsx
{/* Leading icon — dark navy in default state, uses fg-primary (NOT text-primary) */}
{leadingIcon && (
  <LeadingIcon
    className={`${iconSizeClass} ${disabled ? 'text-[var(--text-disabled-subtle)]' : 'text-[var(--fg-primary)]'}`}
  />
)}

{/* Trailing icon — replaced by error icon when in error state */}
{hasError ? (
  <AlertTriangle
    className={`${iconSizeClass} text-[var(--text-error-primary)]`}
  />
) : trailingIcon ? (
  <TrailingIcon
    className={`${iconSizeClass} ${disabled ? 'text-[var(--text-disabled-subtle)]' : 'text-[var(--fg-primary)]'}`}
  />
) : null}
```

### Icon Sizes by Field Size

| Size | Icon Size | Tailwind |
|------|-----------|----------|
| **Small** | 16px | `w-4 h-4` |
| **Medium** | 20px | `w-5 h-5` |
| **Large** | 24px | `w-6 h-6` |

### Adornment Colors by State

| State | Adornment Text Color |
|-------|---------------------|
| **Default** | Inherits from wrapper — effectively `var(--text-primary)` (#353E5A) |
| **Disabled** | `var(--text-disabled-subtle)` (#BDC8D3) |

---

## 1. Text Field

A single-line text input with optional password toggle and clear button. In the source code this is `fello-text-field-v2` (selector) / `FelloTextFieldV2Component` (class).

### Structure

```html
<!-- Uses Form Field wrapper from form-field spec -->
<FormFieldWrapper label={label} error={error} hint={hint} size={size}>
  <InputWrapper size={size} disabled={disabled} error={hasError}>
    {leadingIcon}
    {leadingAdornment}

    <input
      type={computedType}
      className="flex-1 w-full outline-none bg-transparent border-none"
      style={{ fontFamily: 'inherit', fontSize: 'inherit', lineHeight: 'inherit', fontWeight: 400, color: 'var(--text-primary)' }}
      placeholder={placeholder}
      value={value}
      disabled={disabled}
    />

    {/* Password toggle OR Clear button — mutually exclusive */}
    {type === 'password' ? (
      <PasswordToggleIcon />
    ) : clearable ? (
      <ClearIcon />
    ) : null}

    {/* Error icon OR trailing icon — error takes priority */}
    {hasError ? (
      <AlertTriangle className={`${iconSizeClass} text-[var(--text-error-primary)]`} />
    ) : (
      trailingIcon
    )}

    {trailingAdornment}
  </InputWrapper>
</FormFieldWrapper>
```

### Content Rendering Order (Left to Right)

```
[ leading-icon ] [ leading-adornment ] [ input control ] [ trailing-adornment ] [ error-icon OR trailing-icon ] [ extra trailing content ]
```

This order is defined by the Input Wrapper template. The `<input>` fills the remaining space via `flex-1`.

### Input Types

| Type | `<input type>` | Trailing Behavior |
|------|---------------|-------------------|
| `text` (default) | `text` | Shows trailing icon or clear button |
| `password` | `password` (toggles to `text`) | Shows eye/eye-remove toggle icon |

### Password Toggle

When `type="password"`:
- Default state: `type="password"` with eye icon (`eye`)
- After toggle click: `type="text"` with eye-remove icon (`eye-remove`)
- Icon has `cursor: pointer`
- Icon color: `var(--fg-primary)` (#353E5A) — same dark navy as other input icons
- The toggle icon is always in the trailing position

```jsx
<button
  type="button"
  onClick={togglePasswordVisibility}
  className="cursor-pointer bg-transparent border-none p-0 outline-none"
>
  {isPasswordVisible ? (
    <EyeOff className={`${iconSizeClass} text-[var(--fg-primary)]`} />
  ) : (
    <Eye className={`${iconSizeClass} text-[var(--fg-primary)]`} />
  )}
</button>
```

### Clear Button

When `clearable={true}` AND type is NOT `password`:
- Shows a close (×) icon in the trailing position
- Clicking clears the input value to empty string
- Icon has `cursor: pointer`
- Icon color: `var(--fg-primary)` (#353E5A)
- Only visible when there is a value

```jsx
{clearable && value && (
  <button
    type="button"
    onClick={clearInput}
    className="cursor-pointer bg-transparent border-none p-0 outline-none"
  >
    <X className={`${iconSizeClass} text-[var(--fg-primary)]`} />
  </button>
)}
```

**Priority rule:** Password toggle icon takes precedence over clearable button. They are mutually exclusive — never show both.

### Text Field Has NO Component-Specific Styles

All styling comes from the Form Field + Input Wrapper specs. The text field component itself adds zero custom SCSS — it relies entirely on the shared wrapper system.

---

## 2. Textarea

A multi-line text input with auto-resize, character counter, and optional footer.

### Structure

```html
<FormFieldWrapper label={label} error={error} hint={hint} size={size}>
  <InputWrapper size={size} disabled={disabled} error={hasError}>
    <div className="w-full flex flex-col">
      <textarea
        className="w-full outline-none bg-transparent border-none"
        style={{
          resize: 'none',
          fontFamily: 'inherit',
          fontSize: 'inherit',
          lineHeight: 'inherit',
          fontWeight: 'inherit',
          color: 'var(--text-primary)',
          maxHeight: maxHeight,
        }}
        rows={rows}
        placeholder={placeholder}
        value={value}
        disabled={disabled}
      />

      {/* Bottom row: footer and/or character counter */}
      {(hasFooter || characterLimit) && (
        <div className="flex justify-between items-center" style={bottomRowStyle}>
          <div>{footerContent}</div>
          {characterLimit !== undefined && (
            <span
              className="ml-auto"
              style={{
                fontSize: '12px',
                lineHeight: '18px',
                fontWeight: 400,
                color: value.length > characterLimit ? 'var(--text-error-primary)' : 'var(--text-primary)',
              }}
            >
              {value.length}/{characterLimit}
            </span>
          )}
        </div>
      )}
    </div>
  </InputWrapper>
</FormFieldWrapper>
```

### Textarea-Specific Inputs

| Input | Type | Default | Description |
|-------|------|---------|-------------|
| `rows` | number | `3` | Initial visible rows |
| `maxHeight` | string | `'300px'` | Maximum height before scrolling |
| `characterLimit` | number | undefined | Optional character counter |
| `disableResize` | boolean | `false` | Disable auto-resize behavior |

### Auto-Resize Behavior

By default, the textarea automatically grows to fit its content:

1. On every input change, reset `height: auto` then set `height = scrollHeight + 'px'`
2. The `maxHeight` CSS constraint limits expansion (default 300px)
3. When content exceeds maxHeight, the textarea scrolls internally
4. When `disableResize=true`, auto-resize is disabled and `overflow-y: hidden` is removed

```jsx
const autoResize = (textarea) => {
  if (disableResize) return;
  textarea.style.height = 'auto';
  textarea.style.height = textarea.scrollHeight + 'px';
};
```

**Auto-resize CSS:**
```css
textarea.auto-resize {
  overflow-y: hidden;
  min-height: auto;
}
```

### Character Counter

When `characterLimit` is set:
- Shows `currentLength/maxLength` in the bottom-right of the textarea
- Uses `paragraph-sm` typography (12px / 18px / 400 weight)
- Text color: `var(--text-primary)` (#353E5A) (normal) → `var(--text-error-primary)` (#F02C00) (when exceeded)
- Positioned with `ml-auto` to push to the right

### Bottom Row Heights

The bottom row (footer + counter) has a fixed height matching one line of textarea text:

| Size | Bottom Row Height | Padding Top |
|------|------------------|-------------|
| Small | 18px | 2px |
| Medium | 20px | 2px |
| Large | 24px | 2px |

### Footer Slot

Custom content (e.g., emoji picker, formatting buttons) can be projected into the bottom-left of the textarea:

```jsx
<div className="flex justify-between items-center" style={{ height: bottomRowHeight, paddingTop: '2px' }}>
  <div>{footerContent}</div>
  {characterCounter}
</div>
```

---

## 3. Number Field

A numeric input with automatic formatting, comma separators, and increment/decrement arrows.

### Structure

```html
<FormFieldWrapper label={label} error={error} hint={hint} size={size}>
  <InputWrapper size={size} disabled={disabled} error={hasError}>
    {leadingIcon}
    {leadingAdornment}

    <input
      className="flex-1 w-full outline-none bg-transparent border-none"
      style={{ fontFamily: 'inherit', fontSize: 'inherit', lineHeight: 'inherit', fontWeight: 400, color: 'var(--text-primary)' }}
      placeholder={placeholder}
      value={formattedDisplayValue}
      disabled={disabled}
      onInput={handleInput}
      onKeyDown={handleKeyDown}
    />

    {trailingAdornment}

    {/* Arrow step buttons */}
    {!disabled && !hideArrowStep && (
      <div className="flex flex-col" style={arrowWrapperStyle}>
        <button tabIndex={-1} onClick={() => { increment(); inputRef.focus(); }}>
          <ChevronUpIcon style={{ fontSize: arrowIconSize, color: 'var(--fg-quaternary)' }} />
        </button>
        <button tabIndex={-1} onClick={() => { decrement(); inputRef.focus(); }}>
          <ChevronDownIcon style={{ fontSize: arrowIconSize, color: 'var(--fg-quaternary)' }} />
        </button>
      </div>
    )}

    {trailingIcon}
  </InputWrapper>
</FormFieldWrapper>
```

### Number Field-Specific Inputs

| Input | Type | Default | Description |
|-------|------|---------|-------------|
| `mode` | `'integer'` \| `'decimal'` | `'decimal'` | Controls allowed characters |
| `hideCommaSeparator` | boolean | `false` | Disable thousand separators |
| `stepSize` | number | `1` | Increment/decrement step value |
| `hideArrowStep` | boolean | `false` | Hide arrow buttons |
| `decimalPlaces` | number | `2` | Max decimal places allowed |

### Input Modes

**Integer mode:**
- Allows only: digits (`0-9`) and minus sign (`-`) at start
- Formats with comma separators: `1234567` → `1,234,567`
- Regex to clean input: removes all non-digit chars except leading minus

**Decimal mode:**
- Allows: digits, period (`.`), and minus sign (`-`) at start
- Formats integer part with commas: `1234.56` → `1,234.56`
- Limits decimal places to `decimalPlaces` (default 2)
- Only one period allowed

### Comma Separator Formatting

```javascript
// Format integer with comma separators
const formatWithCommas = (numStr) => {
  return numStr.replace(/(-?\d)(?=(\d{3})+(?!\d))/g, '$1,');
};

// For decimal mode: split on period, format integer part only
const [intPart, decPart] = value.split('.');
const formatted = formatWithCommas(intPart) + (decPart !== undefined ? '.' + decPart : '');
```

When `hideCommaSeparator=true`, skip the comma formatting step.

### Arrow Step Buttons

Two vertically stacked chevron buttons on the right side of the input:

**Arrow wrapper positioning:**
```jsx
style={{
  position: 'relative',
  left: '16px',
  marginLeft: '-20px',   // negative to overlap into padding
  marginRight: '8px',
}}
```

**Arrow wrapper margins by size:**

| Size | Margin Top/Bottom | Arrow Button Height |
|------|------------------|-------------------|
| Small | -1px | 12px |
| Medium | -4px | auto |
| Large | -8px | auto |

**Arrow icon sizes by field size:**

| Size | Icon Size |
|------|-----------|
| Small | 16px |
| Medium | 20px |
| Large | 24px |

**Arrow button styling:**
- Color: `var(--fg-quaternary)` (#9298A9)
- No focus ring (`tabIndex={-1}`)
- After click, refocuses the input element
- `overflow: hidden` on each button
- Up arrow has `margin-bottom: -4px`
- Down arrow has `margin-top: -4px`

### Keyboard Step Behavior

- **Arrow Up** key: increment value by `stepSize`
- **Arrow Down** key: decrement value by `stepSize`
- Calls `event.preventDefault()` to prevent page scrolling
- Does nothing if `hideArrowStep=true`
- Rounds result to match `stepSize` decimal precision

```javascript
// Round to step precision
const getStepPrecision = (step) => {
  const parts = step.toString().split('.');
  return parts[1] ? parts[1].length : 0;
};

const roundToStepPrecision = (value, step) => {
  const precision = getStepPrecision(step);
  const factor = Math.pow(10, precision);
  return Math.round(value * factor) / factor;
};
```

### Value Flow

The number field maintains two separate values:
1. **Display value** (formatted string with commas): shown in the input
2. **Numeric value** (actual number): emitted to the parent

```
User types "1234.5"
  → Clean input (remove invalid chars)
  → Format: "1,234.5" (display)
  → Parse: 1234.5 (numeric value)
  → Emit numeric value to parent
```

---

## Complete Color Reference (All Text Input Components)

| Element | State | Token | Hex |
|---------|-------|-------|-----|
| Input text | Default | `var(--text-primary)` | #353E5A |
| Placeholder | Default | `var(--text-placeholder)` | #BDC8D3 |
| Input text | Disabled | `var(--text-tertiary)` | #6B748E |
| Leading/trailing icon | Default | `var(--fg-primary)` | #353E5A |
| Leading/trailing icon | Disabled | `var(--text-disabled-subtle)` | #BDC8D3 |
| Error icon (replaces trailing) | Error | `var(--text-error-primary)` | #F02C00 |
| Password toggle icon | Default | `var(--fg-primary)` | #353E5A |
| Clear button icon | Default | `var(--fg-primary)` | #353E5A |
| Arrow step icons | Default | `var(--fg-quaternary)` | #9298A9 |
| Adornment text | Default | inherits (effectively `var(--text-primary)`) | #353E5A |
| Adornment text | Disabled | `var(--text-disabled-subtle)` | #BDC8D3 |
| Disabled bg | Disabled | `var(--bg-disabled_subtle)` | #F1F4F7 |
| Disabled border | Disabled | `var(--border-disabled_subtle)` | #CCD6DC |

---

## CRITICAL Rules

1. **Always wrap in Form Field + Input Wrapper** — never render a bare `<input>` or `<textarea>`. Use the wrapper system from `form-field.md`.
2. **Password toggle and clear button are mutually exclusive** — if `type="password"`, show the eye toggle. If `clearable=true` and type is not password, show the clear button. Never both.
3. **Textarea resize: none** — never allow browser drag-resize handle. Auto-resize is JavaScript-driven.
4. **Character counter turns red at `var(--text-error-primary)` (#F02C00)** when the character count exceeds the limit.
5. **Number field arrow buttons are not keyboard-focusable** — `tabIndex={-1}`. They refocus the input after click.
6. **Number field always stores a numeric value internally** — the formatted display string with commas is only for visual display.
7. **Comma formatting uses the regex** `/(-?\d)(?=(\d{3})+(?!\d))/g` — this inserts commas before every 3-digit group.
8. **All three controls inherit ALL styling from the Form Field spec** — these components add minimal to zero custom CSS. The border colors, focus rings, shadows, disabled backgrounds are all defined in the form-field spec.
9. **The textarea bottom row height must match one line of text** — Small=18px, Medium=20px, Large=24px. This ensures consistent height when comparing textareas with and without footers.
10. **Decimal places are enforced on the display** — if `decimalPlaces=2`, user cannot type more than 2 digits after the period.
11. **Icon color is `var(--fg-quaternary)` (#9298A9)** — a muted gray. Do NOT use `var(--text-primary)` (dark navy), `var(--fg-secondary)` (darker gray), or black for input icons. Icons must be subtle and not compete with input text.
12. **Disabled state changes icon & adornment color to `var(--text-disabled-subtle)` (#BDC8D3)** — this is explicitly set by the input wrapper, overriding the default `var(--fg-quaternary)`.
13. **Error icon replaces trailing icon** — when an error exists, the trailing icon slot shows `alert-triangle` in `var(--text-error-primary)` (#F02C00) with a tooltip containing the error text. The leading icon is NOT affected by error state.
