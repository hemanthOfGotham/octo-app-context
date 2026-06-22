# Credits Button

A pill-shaped button group displayed in the app header that shows the user's credit balance. It can expand to show a secondary action button when the balance is low.

> **TOKEN RULE:** Every color in the code examples below uses CSS variable tokens or Tailwind palette names — never raw hex. When implementing, copy the token references exactly. See `design-tokens.md` for definitions.

---

## Structure

### Normal state (balance ≥ 50)

```
╭──────────────────────────╮
│  ⬡  1,250           ▾   │
╰──────────────────────────╯
 ↑   ↑                 ↑
 credit  formatted     chevron
 icon    balance       (dropdown arrow)
```

A single pill button containing:
- A **leading credit icon** (small coin/diamond glyph)
- The **formatted credit balance** (number with comma separators, e.g. `1,250`)
- A **trailing chevron-down icon** that indicates a dropdown popup

### Low balance state (balance ≤ 50 AND credits consumed in last 6 months)

```
╭──────────────────────────┬───────────────────╮
│  ⬡  12                   │  Out of Credits   │
╰──────────────────────────┴───────────────────╯
 ↑                          ↑
 balance button             action button
 (no chevron)               ("Out of Credits" or "Buy Credits")
```

Two buttons joined in a single pill:
- **Leading button** — credit icon + balance, no chevron
- **Trailing button** — text label, separated from the leading button by a 1px gap
- The entire pill gets a **red error border** when credits were consumed recently

### Low balance state (balance ≤ 50 AND credits NOT consumed recently)

Same layout as above, but:
- Trailing button label is **"Buy Credits"** instead of "Out of Credits"
- Trailing button text color is **coral** `var(--text-brand-primary)` (#FF725C) on primary variant
- **No red error border** around the pill

---

## Dimensions

| Property | Value |
|----------|-------|
| Pill height | `32px` |
| Border radius | fully rounded (pill shape — very large radius, e.g. `9999px`) |
| Overflow | `hidden` |
| Gap between buttons | `1px` (visible as a thin line between leading and trailing buttons) |
| Button padding | `0 12px` (horizontal only) |
| Button height | `100%` (fills the 32px pill) |
| Font size | `14px` (small, bold/semibold) |
| Leading icon size | `14px` |
| Trailing chevron size | `14px` |
| Icon-to-text gap | `6px` |
| Margin right | `8px` (space between credits button and next header element) |

---

## States — Primary (default, on dark plum `var(--bg-primary-solid)` (#353E5A) header)

### Default

| Property | Value |
|----------|-------|
| Button background | `primary-text-600` (#495883) (medium blue-gray — blends into header) |
| Text color | `var(--text-white)` (#FFFFFF) |
| Icon color | `var(--fg-white)` (#FFFFFF) |
| Border | none |
| Box shadow | none |

### Hover

| Property | Value |
|----------|-------|
| Button background | `var(--bg-primary-solid)` (#353E5A) (darkens to match shell background) |
| Text color | `var(--text-white)` (#FFFFFF) |
| Transition | `background-color 150ms ease-out` |

### Focus

| Property | Value |
|----------|-------|
| Button background | `var(--bg-primary-solid)` (#353E5A) (same as hover) |
| Box shadow (pill) | `0 0 0 4px rgba(73, 88, 131, 0.08), 0 1px 2px rgba(73, 88, 131, 0.04), 0 1px 3px rgba(73, 88, 131, 0.16)` |

### Danger (low credits + consumed recently)

| Property | Value |
|----------|-------|
| Pill border | `1px solid var(--border-error-solid)` (#F02C00) (error red) |
| Focus box shadow | `0 0 0 4px rgba(240, 44, 0, 0.24), 0 1px 2px rgba(73, 88, 131, 0.04)` |

---

## States — Secondary (emulated mode, on teal `var(--bg-brand-secondary-solid)` (#098486) header)

### Default

| Property | Value |
|----------|-------|
| Button background | `var(--bg-brand-secondary-solid)` (#098486) (dark teal — blends into teal header) |
| Text color | `var(--text-white)` (#FFFFFF) |

### Hover

| Property | Value |
|----------|-------|
| Button background | `secondary-900` (#055051) (very dark teal) |
| Transition | `background-color 150ms ease-out` |

### Focus

| Property | Value |
|----------|-------|
| Button background | `secondary-900` (#055051) (same as hover) |
| Box shadow (pill) | `0 0 0 4px rgba(11, 165, 167, 0.24), 0 1px 2px rgba(73, 88, 131, 0.04)` |

### Danger (low credits + consumed recently)

| Property | Value |
|----------|-------|
| Pill border | `1px solid var(--border-error-solid)` (#F02C00) (error red) |
| Focus box shadow | `0 0 0 4px rgba(240, 44, 0, 0.24), 0 1px 2px rgba(73, 88, 131, 0.04)` |

---

## Trailing Action Button (visible only when balance ≤ 50)

The trailing button appears to the right of the balance button, separated by a 1px gap. It shares the same pill container and background color.

### "Out of Credits" (consumed recently)

| Property | Value |
|----------|-------|
| Label | `Out of Credits` |
| Text color | `var(--text-white)` (#FFFFFF) |
| Background | same as leading button (inherited from pill) |
| Behavior | Opens a manage credits dialog |

### "Buy Credits" (NOT consumed recently, primary variant)

| Property | Value |
|----------|-------|
| Label | `Buy Credits` |
| Text color | `var(--text-brand-primary)` (#FF725C) (coral) |
| Background | same as leading button |
| Behavior | Opens a manage credits dialog |

### "Buy Credits" (NOT consumed recently, secondary variant)

| Property | Value |
|----------|-------|
| Label | `Buy Credits` |
| Text color | `var(--text-white)` (#FFFFFF) |
| Background | same as leading button |

---

## Behavior

- Clicking the **balance button** (leading) opens a **credit balance popup** below the button (desktop only)
- Clicking the **trailing action button** opens a manage credits dialog
- The chevron-down icon only appears when balance is **above 50** (normal state) and on **desktop**
- On **mobile**, the chevron and popup are hidden — only the balance button is shown, no trailing button
- The balance number is **formatted with commas** (e.g. `1,250` not `1250`)
- Both buttons within the pill change background together on hover/focus (the hover applies to the entire pill group, not individual buttons)

---

## Implementation

```jsx
function CreditsButton({ variant = 'primary', balance = 1250, consumedRecently = false }) {
  const [isHovered, setIsHovered] = React.useState(false);
  const isLow = balance <= 50;

  const colors = {
    primary: {
      bg: 'var(--bg-secondary-solid)',
      hoverBg: 'var(--bg-primary-solid)',
      focusShadow: '0 0 0 4px rgba(73, 88, 131, 0.08), 0 1px 2px rgba(73, 88, 131, 0.04), 0 1px 3px rgba(73, 88, 131, 0.16)',
    },
    secondary: {
      bg: 'var(--bg-brand-secondary-solid)',
      hoverBg: 'var(--bg-brand-secondary-solid_hover)',
      focusShadow: '0 0 0 4px rgba(11, 165, 167, 0.24), 0 1px 2px rgba(73, 88, 131, 0.04)',
    },
  };

  const c = colors[variant];
  const dangerBorder = isLow && consumedRecently ? '1px solid var(--border-error-solid)' : 'none';
  const formattedBalance = balance.toLocaleString();

  return (
    <div
      className="flex items-center shrink-0"
      style={{
        height: '32px',
        borderRadius: '9999px',
        overflow: 'hidden',
        border: dangerBorder,
        marginRight: '8px',
      }}
      onMouseEnter={() => setIsHovered(true)}
      onMouseLeave={() => setIsHovered(false)}
    >
      {/* Leading balance button */}
      <button
        className="flex items-center justify-center"
        style={{
          height: '100%',
          padding: '0 12px',
          backgroundColor: isHovered ? c.hoverBg : c.bg,
          border: 'none',
          cursor: 'pointer',
          color: 'var(--text-white)',
          fontSize: '14px',
          fontWeight: 600,
          gap: '6px',
          transition: 'background-color 150ms ease-out',
        }}
      >
        {/* Credit icon */}
        <svg width="14" height="14" viewBox="0 0 14 14" fill="none">
          <polygon points="7,1 12,4 12,10 7,13 2,10 2,4" stroke="var(--fg-white)" strokeWidth="1.2" fill="none" />
        </svg>
        <span>{formattedBalance}</span>
        {/* Chevron — only when balance > 50 */}
        {!isLow && (
          <svg width="14" height="14" viewBox="0 0 14 14" fill="none">
            <polyline points="4,5 7,8 10,5" stroke="var(--fg-white)" strokeWidth="1.5" strokeLinecap="round" strokeLinejoin="round" />
          </svg>
        )}
      </button>

      {/* Trailing action button — only visible when balance ≤ 50 */}
      {isLow && (
        <>
          <div style={{ width: '1px', height: '100%', backgroundColor: isHovered ? c.hoverBg : c.bg, opacity: 0.5 }} />
          <button
            className="flex items-center justify-center"
            style={{
              height: '100%',
              padding: '0 12px',
              backgroundColor: isHovered ? c.hoverBg : c.bg,
              border: 'none',
              cursor: 'pointer',
              fontSize: '14px',
              fontWeight: 600,
              whiteSpace: 'nowrap',
              transition: 'background-color 150ms ease-out',
              color: consumedRecently
                ? 'var(--text-white)'
                : variant === 'primary'
                  ? 'var(--text-brand-primary)'
                  : 'var(--text-white)',
            }}
          >
            {consumedRecently ? 'Out of Credits' : 'Buy Credits'}
          </button>
        </>
      )}
    </div>
  );
}
```

---

## CRITICAL Rules

1. Pill height is exactly `32px` with `border-radius: 9999px` (fully rounded pill shape).
2. Primary background is `primary-text-600` (#495883). Primary hover is `var(--bg-primary-solid)` (#353E5A). It MUST blend into the dark plum header.
3. Secondary background is `var(--bg-brand-secondary-solid)` (#098486). Secondary hover is `secondary-900` (#055051). It MUST blend into the teal header.
4. All text and icon colors are `var(--text-white)` (#FFFFFF) — except "Buy Credits" text on primary variant which is `var(--text-brand-primary)` (#FF725C) (coral).
5. The chevron-down icon only appears when balance is **above 50** (normal state).
6. The trailing action button only appears when balance is **≤ 50**. It shows "Out of Credits" (white text) when credits were consumed recently, or "Buy Credits" (coral/white text) when not consumed recently.
7. When low balance + consumed recently, the entire pill gets `border: 1px solid var(--border-error-solid)` (#F02C00) (error red).
8. Transition on background-color is `150ms ease-out`.
9. Margin-right on the pill is `8px` (space between credits button and the next header element, the Menu Trigger).
10. The gap between leading/trailing buttons is `1px` (thin visible line).
11. Both buttons within the pill change background together on hover — hover applies to the entire pill group, not individual buttons.
12. On mobile, the chevron and popup are hidden — only the balance button is shown.

---

## Showcase

When rendering the Credits Button on the showcase page, display:

1. **Primary — normal balance** on dark plum `var(--bg-primary-solid)` (#353E5A) strip: pill with "⬡ 1,250 ▾"
2. **Primary — low balance (out of credits)** on dark plum strip: pill with "⬡ 12 | Out of Credits" and red border
3. **Primary — low balance (buy credits)** on dark plum strip: pill with "⬡ 12 | Buy Credits" (coral text), no red border
4. **Secondary — normal balance** on teal `var(--bg-brand-secondary-solid)` (#098486) strip: pill with "⬡ 1,250 ▾"
5. **Secondary — low balance** on teal strip: pill with "⬡ 12 | Out of Credits" and red border

Show hover state for at least one variant to demonstrate the background darkening effect.
