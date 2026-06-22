# Search Bar

A text input used as the global search bar in the app header. It sits on the dark header background and blends into it visually.

> **TOKEN RULE:** Every color in the code examples below uses CSS variable tokens or Tailwind palette names — never raw hex. When implementing, copy the token references exactly. See `design-tokens.md` for definitions.

---

## Structure

```
┌──────────────────────────────────────────────────────┐
│  🔍  Search                                     ✕   │
└──────────────────────────────────────────────────────┘
 ↑      ↑                                         ↑
 leading placeholder                          clear button
 icon   text                                  (visible when
                                               text is entered)
```

The search bar is a single-line text input with:
- A **leading search icon** (magnifying glass) on the left
- **Placeholder text** "Search" when empty
- A **trailing clear button** (✕ icon) that appears only when there is text

---

## Dimensions

| Property | Value |
|----------|-------|
| Width | `300px` |
| Height | `32px` |
| Padding | `4px 16px` (vertical, horizontal) |
| Border radius | `6px` |
| Border | `1px solid` |
| Leading icon size | `16px` |
| Font size | small (14px) |

---

## States — Primary (default, on dark plum `var(--bg-primary-solid)` (#353E5A) header)

This is the standard appearance when the search bar sits on the dark plum header background.

### Default

| Property | Value |
|----------|-------|
| Background | `primary-text-600` (#495883) (slightly lighter plum — blends into the dark header) |
| Border color | `transparent` |
| Box shadow | none |
| Text color | `var(--text-white)` (#FFFFFF) |
| Placeholder color | `var(--text-white)` (#FFFFFF) |
| Icon color | `var(--fg-white)` (#FFFFFF) |

### Hover

| Property | Value |
|----------|-------|
| Background | `primary-text-600` (#495883) (unchanged) |
| Border color | `var(--border-secondary-solid)` (#8EA1AF) (muted gray-blue) |
| Box shadow | none |
| Text color | `var(--text-white)` (#FFFFFF) |

### Focus

| Property | Value |
|----------|-------|
| Background | `primary-text-600` (#495883) (unchanged) |
| Border color | `var(--border-secondary-solid)` (#8EA1AF) (muted gray-blue) |
| Box shadow | `0 0 0 4px rgba(73, 88, 131, 0.08), 0 1px 2px rgba(73, 88, 131, 0.04), 0 1px 3px rgba(73, 88, 131, 0.16)` |
| Text color | `var(--text-white)` (#FFFFFF) |

### Disabled

| Property | Value |
|----------|-------|
| Background | disabled subtle (light gray) |
| Border color | disabled subtle border |
| Text color | muted/tertiary |

---

## States — Secondary (emulated mode, on teal `var(--bg-brand-secondary-solid)` (#098486) header)

This variant appears when an admin is emulating another user's account. The header background changes to teal, and the search bar adapts.

### Default

| Property | Value |
|----------|-------|
| Background | `var(--bg-brand-secondary-solid)` (#098486) (dark teal — blends into the teal header) |
| Border color | `transparent` |
| Box shadow | none |
| Text color | `var(--text-white)` (#FFFFFF) |
| Placeholder color | `var(--text-white)` (#FFFFFF) |

### Hover

| Property | Value |
|----------|-------|
| Background | `var(--bg-brand-secondary-solid)` (#098486) (unchanged) |
| Border color | `var(--border-brand-secondary-solid)` (#0BA5A7) (lighter teal) |
| Box shadow | none |

### Focus

| Property | Value |
|----------|-------|
| Background | `var(--bg-brand-secondary-solid)` (#098486) (unchanged) |
| Border color | `var(--border-brand-secondary-solid)` (#0BA5A7) (lighter teal) |
| Box shadow | `0 0 0 4px rgba(11, 165, 167, 0.24), 0 1px 2px rgba(73, 88, 131, 0.04)` |

---

## Clear Button (trailing)

Appears inside the input on the right side only when there is text entered.

| Property | Value |
|----------|-------|
| Type | Icon-only button |
| Icon | close (✕) |
| Size | extra small |
| Background | transparent |
| Icon color | `var(--fg-white)` (#FFFFFF) |
| Behavior | Clears the search text on click |

---

## Behavior

- Typing triggers a search after a **200ms debounce**
- A **minimum character count** is required before search executes
- If the minimum is not met, a floating error appears below: "Enter X more character(s)"
- Results appear in an **autocomplete dropdown** below the input showing contact suggestions

---

## Implementation

```jsx
function SearchBar({ variant = 'primary', value, onChange, onClear }) {
  const [isFocused, setIsFocused] = React.useState(false);
  const [isHovered, setIsHovered] = React.useState(false);

  const colors = {
    primary: {
      bg: 'var(--fg-secondary)', /* Tailwind: bg-primary-text-600 (#495883) — no semantic --bg-* var exists */
      borderHover: 'var(--border-secondary-solid)',
      focusShadow: '0 0 0 4px rgba(73, 88, 131, 0.08), 0 1px 2px rgba(73, 88, 131, 0.04), 0 1px 3px rgba(73, 88, 131, 0.16)',
    },
    secondary: {
      bg: 'var(--bg-brand-secondary-solid)',
      borderHover: 'var(--border-brand-secondary-solid)',
      focusShadow: '0 0 0 4px rgba(11, 165, 167, 0.24), 0 1px 2px rgba(73, 88, 131, 0.04)',
    },
  };

  const c = colors[variant];

  return (
    <div
      className="flex items-center relative"
      style={{
        width: '300px',
        height: '32px',
        borderRadius: '6px',
        padding: '4px 16px',
        backgroundColor: c.bg,
        border: `1px solid ${isHovered || isFocused ? c.borderHover : 'transparent'}`,
        boxShadow: isFocused ? c.focusShadow : 'none',
        cursor: 'text',
      }}
      onMouseEnter={() => setIsHovered(true)}
      onMouseLeave={() => setIsHovered(false)}
    >
      {/* Leading search icon */}
      <svg
        width="16" height="16" viewBox="0 0 16 16" fill="none"
        style={{ flexShrink: 0, marginRight: '8px' }}
      >
        <circle cx="7" cy="7" r="5.5" stroke="var(--fg-white)" strokeWidth="1.5" />
        <line x1="11" y1="11" x2="14" y2="14" stroke="var(--fg-white)" strokeWidth="1.5" strokeLinecap="round" />
      </svg>

      {/* Input */}
      <input
        type="text"
        placeholder="Search"
        value={value}
        onChange={onChange}
        onFocus={() => setIsFocused(true)}
        onBlur={() => setIsFocused(false)}
        className="flex-1"
        style={{
          background: 'transparent',
          border: 'none',
          outline: 'none',
          color: 'var(--text-white)',
          fontSize: '14px',
          width: '100%',
        }}
      />

      {/* Clear button — only visible when text is entered */}
      {value && (
        <button
          onClick={onClear}
          className="flex items-center justify-center"
          style={{
            background: 'transparent',
            border: 'none',
            cursor: 'pointer',
            color: 'var(--fg-white)',
            padding: '0',
            marginLeft: '4px',
          }}
        >
          <svg width="14" height="14" viewBox="0 0 14 14" fill="none">
            <line x1="3" y1="3" x2="11" y2="11" stroke="var(--fg-white)" strokeWidth="1.5" strokeLinecap="round" />
            <line x1="11" y1="3" x2="3" y2="11" stroke="var(--fg-white)" strokeWidth="1.5" strokeLinecap="round" />
          </svg>
        </button>
      )}
    </div>
  );
}
```

---

## CRITICAL Rules

1. Search bar width is exactly `300px`, height is `32px`, border-radius is `6px`.
2. Primary background is `primary-text-600` (#495883) — NOT transparent, NOT white. It MUST blend into the dark plum header.
3. Secondary background is `var(--bg-brand-secondary-solid)` (#098486) — it MUST blend into the teal header.
4. All text, placeholder text, and icon colors are `var(--text-white)` (#FFFFFF) / `var(--fg-white)` (#FFFFFF). Never use dark text.
5. Default border is `transparent`. Hover/focus border is `var(--border-secondary-solid)` (#8EA1AF) (primary) or `var(--border-brand-secondary-solid)` (#0BA5A7) (secondary).
6. Focus box shadow for primary: `0 0 0 4px rgba(73, 88, 131, 0.08), 0 1px 2px rgba(73, 88, 131, 0.04), 0 1px 3px rgba(73, 88, 131, 0.16)`.
7. Focus box shadow for secondary: `0 0 0 4px rgba(11, 165, 167, 0.24), 0 1px 2px rgba(73, 88, 131, 0.04)`.
8. The clear button (✕) ONLY appears when the input has text. It is hidden when the input is empty.
9. The search bar does NOT have its own background that stands out. It is designed to blend into the header — the colored bg creates a subtle, slightly lighter area within the dark header.
10. Placeholder text is `::placeholder { color: var(--text-white); }` (#FFFFFF) — white to match the header aesthetic.

---

## Showcase

When rendering the Search Bar on the showcase page, display both variants side by side:

1. **Primary variant** — on a dark plum (`var(--bg-primary-solid)` (#353E5A)) background strip, showing default, hover, and focus states
2. **Secondary variant** — on a teal (`var(--bg-brand-secondary-solid)` (#098486)) background strip, showing default, hover, and focus states

For each state, show the search bar with its exact background, border, and shadow. Label each state clearly (Default, Hover, Focus).
