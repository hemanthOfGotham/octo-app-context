# Checkbox Group Component — Strict Specification

When building a group of checkboxes that share a value (array of selected values) and need coordinated disabled state, follow these specifications exactly.

**Note:** The Checkbox Group is a **behavioral wrapper only** — it has NO visual styling, NO template, and NO layout. It manages multi-value state (an array of selected values) and propagates disabled state to child checkboxes. All visual rendering is done by the individual Checkbox components inside it (see `checkbox.md`).

> **TOKEN RULE:** Every color in this spec is expressed as a CSS variable token. In Tailwind classes use `bg-[var(--token)]` / `text-[var(--token)]` / `border-[var(--token)]`. In inline styles use `'var(--token)'`. In prose and tables, show `var(--token)` (#hex) for reference.

---

## Architecture

```
Checkbox Group (behavioral — manages value array + disabled state)
├── Checkbox 1 (visual — from checkbox spec)
├── Checkbox 2 (visual — from checkbox spec)
├── Checkbox 3 (visual — from checkbox spec)
└── ... any number of checkboxes
```

---

## Behavior

### Value Management
- The group value is an **array** of selected values: `unknown[]` / `any[]`
- Each child checkbox contributes its value to this array when checked
- Checking a checkbox **adds** its value to the array
- Unchecking a checkbox **removes** its value from the array
- The group implements form control behavior (like React controlled component)

### Disabled Propagation
- When the group is `disabled`, ALL child checkboxes become disabled
- Individual child checkboxes can also be independently disabled
- A child is disabled if either the group OR the individual checkbox is disabled

### No Layout
- The Checkbox Group provides **NO layout** — no flex, no grid, no gap
- The consumer decides how to lay out the checkboxes (vertical stack, horizontal row, grid, etc.)

---

## Implementation Pattern for Lovable

```jsx
{/* === Vertical Checkbox Group === */}
function CheckboxGroup({ options, value, onChange, disabled }) {
  const toggleValue = (optionValue) => {
    if (disabled) return;
    const newValue = value.includes(optionValue)
      ? value.filter(v => v !== optionValue)
      : [...value, optionValue];
    onChange(newValue);
  };

  return (
    <div className="flex flex-col gap-3" role="group" aria-label="Options">
      {options.map(option => (
        <label
          key={option.value}
          className={`inline-flex items-start gap-2 ${
            disabled || option.disabled ? 'cursor-default' : 'cursor-pointer'
          }`}
          style={{ color: disabled || option.disabled ? 'var(--text-disabled)' : 'var(--text-primary)' }}
        >
          {/* Checkbox box — from checkbox.md */}
          {/* mt-0.5 aligns the 16px box with the label text — see checkbox spec rule #14 */}
          <div
            className={`
              flex items-center justify-center shrink-0
              mt-0.5
              w-4 h-4 rounded
              border transition-all duration-150 ease-out
              ${disabled || option.disabled
                ? value.includes(option.value)
                  ? 'bg-[var(--bg-brand-secondary-solid_hover)]/50 border-[var(--border-brand-secondary-solid)]/50'
                  : 'bg-[var(--bg-tertiary)] border-[var(--border-secondary)]'
                : value.includes(option.value)
                  ? 'bg-[var(--bg-brand-secondary-solid)] border-[var(--border-brand-secondary-solid)] hover:bg-[var(--bg-brand-secondary-solid_hover)] hover:border-[var(--border-brand-secondary-solid)] cursor-pointer'
                  : 'bg-white border-[var(--border-primary)] hover:border-[var(--border-brand-secondary-solid)] cursor-pointer'
              }
            `}
            tabIndex={disabled || option.disabled ? -1 : 0}
            role="checkbox"
            aria-checked={value.includes(option.value)}
            aria-disabled={disabled || option.disabled}
            onClick={(e) => { e.preventDefault(); toggleValue(option.value); }}
          >
            {value.includes(option.value) && (
              <svg width="10" height="8" viewBox="0 0 10 8" fill="none">
                <path d="M9 1L3.5 6.5L1 4" stroke="white" strokeWidth="1.5"
                  strokeLinecap="round" strokeLinejoin="round" />
              </svg>
            )}
          </div>

          {/* Label */}
          <div className="flex flex-col">
            <span className="text-sm leading-5 font-medium">{option.label}</span>
            {option.subText && (
              <span className="text-sm leading-5 font-normal"
                style={{ color: disabled || option.disabled ? 'var(--text-disabled)' : 'var(--text-tertiary)' }}>
                {option.subText}
              </span>
            )}
          </div>
        </label>
      ))}
    </div>
  );
}


{/* === Horizontal Checkbox Group === */}
<div className="flex flex-row flex-wrap gap-4" role="group" aria-label="Features">
  {features.map(feature => (
    <label key={feature.id} className="inline-flex items-start gap-2 cursor-pointer"
      style={{ color: 'var(--text-primary)' }}>
      {/* mt-0.5 aligns the 16px box with the label text */}
      <div
        className={`
          flex items-center justify-center shrink-0
          mt-0.5
          w-4 h-4 rounded border cursor-pointer
          transition-all duration-150 ease-out
          ${selectedFeatures.includes(feature.id)
            ? 'bg-[var(--bg-brand-secondary-solid)] border-[var(--border-brand-secondary-solid)] hover:bg-[var(--bg-brand-secondary-solid_hover)] hover:border-[var(--border-brand-secondary-solid)]'
            : 'bg-white border-[var(--border-primary)] hover:border-[var(--border-brand-secondary-solid)]'
          }
        `}
        role="checkbox"
        aria-checked={selectedFeatures.includes(feature.id)}
        onClick={() => toggleFeature(feature.id)}
      >
        {selectedFeatures.includes(feature.id) && (
          <svg width="10" height="8" viewBox="0 0 10 8" fill="none">
            <path d="M9 1L3.5 6.5L1 4" stroke="white" strokeWidth="1.5"
              strokeLinecap="round" strokeLinejoin="round" />
          </svg>
        )}
      </div>
      <span className="text-sm leading-5 font-medium">{feature.label}</span>
    </label>
  ))}
</div>


{/* === Usage with form state === */}
const [selectedColors, setSelectedColors] = useState([]);

<CheckboxGroup
  options={[
    { value: 'red', label: 'Red' },
    { value: 'green', label: 'Green' },
    { value: 'blue', label: 'Blue', subText: 'Primary color' },
    { value: 'gray', label: 'Gray', disabled: true },
  ]}
  value={selectedColors}
  onChange={setSelectedColors}
  disabled={false}
/>
```

---

## CRITICAL Rules

1. **Checkbox Group has NO visual styling** — no border, no background, no padding, no gap. The consumer provides ALL layout.
2. **Value is ALWAYS an array** (`unknown[]`) — not a single value. Each checked checkbox's value is an element in the array.
3. **Group disabled propagates to ALL children** — when the group is disabled, every child checkbox is disabled regardless of individual disabled state.
4. **Individual checkboxes can be independently disabled** even when the group is enabled.
5. **Layout is the consumer's responsibility** — use `flex flex-col gap-3` for vertical, `flex flex-row gap-4` for horizontal, `grid` for grid layouts, etc.
6. **Each child checkbox MUST use the exact spec from `checkbox.md`** — same box styling, same states, same SVG icons, same colors.
7. **Use `role="group"` with `aria-label`** on the container for accessibility.
8. **Toggling a checkbox adds/removes its value from the array** — never replaces the entire array.
9. **⚠️ VERTICAL ALIGNMENT: Each checkbox box MUST have `mt-0.5` (margin-top: 2px)** when a label is present and size is Medium or Large. The label container uses `items-start` (NOT `items-center`), and `mt-0.5` nudges the box down to align with the text. See checkbox spec rule #14.
