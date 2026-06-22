# Icon Toggle Button & Toggle Button — Strict Specification

> **⚠️ CROSS-REFERENCE — REQUIRED COMPONENT SPEC:**
> Both toggle components use the **Button component** from `button.md` as the rendering primitive. Do NOT create custom button styling — use the button spec's `text` button type and `secondary`/`neutral` variants.

When building a toggle button (a button that switches between checked and unchecked states), follow these specifications exactly.

> **TOKEN RULE:** Every color in this spec is expressed as a CSS variable token. In Tailwind classes use `bg-[var(--token)]` / `text-[var(--token)]`. In inline styles use `'var(--token)'`. In prose and tables, show `var(--token)` (#hex) for reference.

This spec covers **two distinct components**:

1. **Icon Toggle Button** — An icon-only button that toggles between highlighted (unchecked) and normal (checked) states.
2. **Toggle Button** — A text button that swaps its label and icon on hover when checked.

---

## 1. Icon Toggle Button

### Architecture

```
Unchecked:                    Checked:
┌──────┐                     ┌──────┐
│  ♡   │  ← Highlighted      │  ♥   │  ← Normal (white bg)
└──────┘                     └──────┘

Checked + Hover:
┌──────┐
│  ♥   │  ← Teal bg (var(--bg-brand-secondary) #E7FDFD)
└──────┘
```

### Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `checked` | `boolean` | `false` | Toggle state |
| `icon` | `string` | Required | Icon to display (e.g., `heart` / `heart-fill`) |
| `size` | `'sm' \| 'md' \| 'lg'` | `'sm'` | Button size (from button spec) |
| `disabled` | `boolean` | `false` | Disables the button |

### Behavior

| State | Button Type | Variant | Highlighted | Background |
|-------|------------|---------|-------------|------------|
| Unchecked | `text` | `secondary` | `true` | transparent (text/secondary highlighted) |
| Checked | `text` | `secondary` | `false` | `var(--bg-primary)` (#FFFFFF), no border |
| Checked + Hover | `text` | `secondary` | `false` | `var(--bg-brand-secondary)` (#E7FDFD) |
| Checked + Focus | `text` | `secondary` | `false` | `var(--bg-brand-secondary)` (#E7FDFD) |

### Configuration

The icon toggle button automatically configures the host button:
- `iconOnly = true` — renders as an icon-only button
- `variant = 'secondary'` — uses the secondary teal color scheme
- `buttonType = 'text'` — no border or solid background
- `highlighted` — toggled based on checked state (highlighted when UNchecked)

### Styles

| Property | Value | Tailwind / CSS |
|----------|-------|----------------|
| Checked background | `var(--bg-primary)` (#FFFFFF) | `bg-[var(--bg-primary)]` |
| Checked border | none | `border-none` |
| Checked hover/focus bg | `var(--bg-brand-secondary)` (#E7FDFD) | `bg-[var(--bg-brand-secondary)]` |

---

## 2. Toggle Button

### Architecture

```
Unchecked:
┌─────────────────┐
│  ✓  Check       │  ← Highlighted, neutral variant
└─────────────────┘

Checked (default):
┌─────────────────┐
│  ✓  Checked     │  ← Secondary variant (teal text)
└─────────────────┘

Checked (on hover):
┌─────────────────┐
│  ✕  Uncheck     │  ← Highlighted, shows uncheckText + uncheckIcon
└─────────────────┘
```

### Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `checked` | `boolean` | `false` | Toggle state |
| `checkText` | `string` | `''` | Label shown when unchecked |
| `checkedText` | `string` | `''` | Label shown when checked (not hovered) |
| `uncheckText` | `string` | `''` | Label shown when checked AND hovered |
| `checkIcon` | `string` | — | Icon shown in unchecked state and checked (not hovered) |
| `uncheckIcon` | `string` | — | Icon shown when checked AND hovered |
| `loading` | `boolean` | `false` | Shows loading spinner |
| `disabled` | `boolean` | `false` | Disables the button |
| `fullWidth` | `boolean` | `false` | Stretches button to full width |

### Behavior

The toggle button renders **two underlying buttons** but only shows one at a time:

| State | Visible Button | Button Type | Variant | Highlighted | Label | Icon |
|-------|---------------|-------------|---------|-------------|-------|------|
| Unchecked | Default | `text` | `neutral` | `true` | `checkText` | `checkIcon` |
| Checked (not hovered) | Default | `text` | `secondary` | `false` | `checkedText` | `checkIcon` |
| Checked + Hover | Hover | `text` | — | `true` | `uncheckText` | `uncheckIcon` |
| Loading | Default | `text` | `neutral` | `true` | `checkText` | spinner |
| Disabled | Default | `text` | `neutral` | `true` | `checkText` | `checkIcon` |

### Hover Swap Logic

The hover swap only happens when the button is **checked AND not disabled**:
1. By default, show the "default" button with `checkedText`
2. On hover, hide the "default" button and show the "hover" button with `uncheckText`
3. On mouse leave, swap back to the default button

### Styles

| Property | Value | Tailwind / CSS |
|----------|-------|----------------|
| Host display | inline-block | `inline-block` |
| Size | always `sm` | Small button from button spec |
| Neutral unchecked bg | `var(--bg-secondary)` (#F1F4F7) | Highlighted neutral text button |
| Neutral unchecked border-radius | 6px | `rounded-md` (radius(3) = 3 x 2 = 6px) |
| Neutral hover bg | `var(--bg-secondary_hover)` (#E8ECF1) | hover state |
| Neutral border | none | — |
| Neutral overflow | hidden | `overflow-hidden` |
| Transition | swap between buttons | CSS display toggle |

### Accessibility

| Attribute | Value |
|-----------|-------|
| `role` | `switch` |
| `aria-checked` | matches `checked` state |
| `aria-disabled` | matches `disabled` state |
| `aria-label` | `checkedText` when checked, `checkText` when unchecked |
| `tabIndex` | `-1` when loading, `0` otherwise |

---

## Color Reference

| Token | Hex | Usage |
|-------|-----|-------|
| `var(--bg-primary)` | (#FFFFFF) | Checked icon toggle bg |
| `var(--bg-brand-secondary)` | (#E7FDFD) | Checked icon toggle hover/focus bg |
| `var(--bg-secondary)` | (#F1F4F7) | Unchecked toggle button bg |
| `var(--bg-secondary_hover)` | (#E8ECF1) | Unchecked toggle button hover bg |
| `var(--text-brand-secondary)` | (#098486) | Checked toggle button text |
| `var(--text-primary)` | (#353E5A) | Default text |

---

## Implementation

### Icon Toggle Button

```jsx
function IconToggleButton({ checked = false, icon, size = 'sm', disabled = false, onToggle }) {
  return (
    <button
      className={`
        inline-flex items-center justify-center transition-colors duration-150
        border-none cursor-pointer
        ${size === 'sm' ? 'w-8 h-8 text-sm' : size === 'md' ? 'w-9 h-9 text-base' : 'w-10 h-10 text-lg'}
        ${checked
          ? 'bg-[var(--bg-primary)] hover:bg-[var(--bg-brand-secondary)] focus-visible:bg-[var(--bg-brand-secondary)]'
          : 'bg-transparent hover:bg-[var(--bg-brand-secondary)]'
        }
        ${disabled ? 'opacity-50 pointer-events-none' : ''}
        rounded-md
      `}
      style={{ color: 'var(--fg-brand-secondary)', fontFamily: 'Inter, Arial, sans-serif' }}
      onClick={() => !disabled && onToggle?.(!checked)}
      aria-pressed={checked}
    >
      <span style={{ fontSize: size === 'lg' ? 20 : size === 'md' ? 18 : 16 }}>
        {/* Render icon */}
      </span>
    </button>
  );
}
```

### Toggle Button

```jsx
function ToggleButton({
  checked = false,
  checkText = '',
  checkedText = '',
  uncheckText = '',
  checkIcon,
  uncheckIcon,
  loading = false,
  disabled = false,
  fullWidth = false,
  onToggle,
}) {
  const [isHovered, setIsHovered] = useState(false);
  const showHoverButton = checked && !disabled && isHovered;

  return (
    <span
      className={`inline-block ${fullWidth ? 'w-full' : ''}`}
      onMouseEnter={() => setIsHovered(true)}
      onMouseLeave={() => setIsHovered(false)}
      style={{ fontFamily: 'Inter, Arial, sans-serif' }}
    >
      {/* Default button */}
      <button
        className={`
          inline-flex items-center gap-1.5 px-3 py-1.5 text-sm font-semibold
          rounded-md overflow-hidden border-none transition-colors duration-150
          ${showHoverButton ? 'hidden' : 'inline-flex'}
          ${fullWidth ? 'w-full justify-center' : ''}
          ${checked && !loading
            ? 'bg-transparent text-[var(--text-brand-secondary)]'
            : 'bg-[var(--bg-secondary)] text-[var(--text-primary)] hover:bg-[var(--bg-tertiary)]'
          }
          ${disabled ? 'opacity-50 pointer-events-none' : 'cursor-pointer'}
        `}
        role="switch"
        aria-checked={checked}
        aria-disabled={disabled}
        aria-label={checked ? checkedText : checkText}
        tabIndex={loading ? -1 : 0}
        onClick={() => !disabled && !loading && onToggle?.(!checked)}
      >
        {loading ? (
          <span className="animate-spin w-4 h-4 border-2 border-current border-t-transparent rounded-full" />
        ) : checkIcon ? (
          <span>{/* Render checkIcon */}</span>
        ) : null}
        <span>{checked ? checkedText : checkText}</span>
      </button>

      {/* Hover button (shown only when checked + hovered) */}
      <button
        className={`
          inline-flex items-center gap-1.5 px-3 py-1.5 text-sm font-semibold
          rounded-md overflow-hidden border-none transition-colors duration-150
          ${showHoverButton ? 'inline-flex' : 'hidden'}
          ${fullWidth ? 'w-full justify-center' : ''}
          bg-[var(--bg-secondary)] text-[var(--text-primary)] hover:bg-[var(--bg-tertiary)]
          cursor-pointer
        `}
        role="switch"
        aria-checked={checked}
        aria-disabled={disabled}
        aria-label={uncheckText}
        tabIndex={loading ? -1 : 0}
        onClick={() => !disabled && !loading && onToggle?.(!checked)}
      >
        {uncheckIcon && <span>{/* Render uncheckIcon */}</span>}
        <span>{uncheckText}</span>
      </button>
    </span>
  );
}
```

---

## CRITICAL Rules

1. **Icon Toggle Button uses `text`/`secondary` button** — it is icon-only, always uses secondary (teal) variant. Do NOT use primary or neutral variant.
2. **Checked icon toggle has white bg, NO border** — `bg-[var(--bg-primary)]` border-none. Hover/focus changes to `var(--bg-brand-secondary)` (#E7FDFD).
3. **Unchecked icon toggle is highlighted** — the `highlighted` property is true when unchecked, false when checked. This is the INVERSE of the checked state.
4. **Toggle Button renders TWO buttons** — one for default state, one for hover-when-checked state. Only one is visible at a time via CSS display toggle.
5. **Hover swap ONLY happens when checked** — unchecked state always shows the same button regardless of hover. Only checked + hover triggers the swap.
6. **Toggle Button always uses size `sm`** — regardless of any other configuration.
7. **Neutral button in Toggle Button** — unchecked state uses `bg-[var(--bg-secondary)]`, 6px border-radius, overflow hidden, NO border.
8. **Checked Toggle Button text is teal `var(--text-brand-secondary)` (#098486)** — uses secondary variant. Text changes from `checkText` to `checkedText`.
9. **`role="switch"` for accessibility** — both button elements use `role="switch"` with proper `aria-checked`.
10. **Font is Inter** — all text uses `Inter, Arial, sans-serif`.
11. **Transitions are 150ms ease-out** — for background color changes.
12. **Loading state shows spinner** — disables click and uses `tabIndex={-1}`.
