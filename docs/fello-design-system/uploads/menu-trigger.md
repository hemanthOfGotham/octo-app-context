# Menu Trigger (Profile Button)

A small pill-shaped button in the far right of the app header that shows the user's avatar and a chevron. Clicking it opens a dropdown menu with account options.

> **TOKEN RULE:** Every color in the code examples below uses CSS variable tokens or Tailwind palette names — never raw hex. When implementing, copy the token references exactly. See `design-tokens.md` for definitions.

---

## Structure

```
╭───────────────╮
│  (👤)  ▾      │
╰───────────────╯
  ↑       ↑
  avatar  chevron-down
  (24px)  (12px)
```

The trigger is a single button containing:
- A **circular avatar** (user's profile photo, or a default user icon if no photo)
- A **chevron-down icon** to the right of the avatar indicating a dropdown

---

## Dimensions

| Property | Value |
|----------|-------|
| Avatar size | `24px × 24px` (circular) |
| Chevron icon size | `12px` |
| Chevron color | `var(--fg-white)` (#FFFFFF) |
| Chevron left margin | `2px` |
| Button padding | `4px` (all sides) |
| Border radius | fully rounded (pill — `9999px`) |
| Overflow | `hidden` (on avatar) |

The overall button width is determined by content: approximately `24px avatar + 2px gap + 12px chevron + 8px padding = ~46px`.

---

## Avatar

The avatar is a small circular image or icon placeholder.

| Property | Value |
|----------|-------|
| Size | `24px × 24px` |
| Shape | circle (`border-radius: 50%`) |
| Content (with photo) | User's profile photo, scaled to cover |
| Content (no photo) | A generic user icon |

---

## States — Primary (default, on dark plum `var(--bg-primary-solid)` (#353E5A) header)

### Default

| Property | Value |
|----------|-------|
| Background | `primary-text-600` (#495883) (medium blue-gray — same family as header) |
| Box shadow | `0 1px 2px 0 rgba(73, 88, 131, 0.04), 0 1px 3px 0 rgba(73, 88, 131, 0.16)` |
| Chevron color | `var(--fg-white)` (#FFFFFF) |

### Hover

| Property | Value |
|----------|-------|
| Background | `var(--bg-primary-solid)` (#353E5A) (darkens to match shell background) |
| Box shadow | unchanged from default |

### Menu Open (Active)

| Property | Value |
|----------|-------|
| Background | `var(--bg-primary-solid)` (#353E5A) (same as hover) |
| Box shadow | `0 0 0 4px rgba(73, 88, 131, 0.08), 0 1px 2px 0 rgba(73, 88, 131, 0.04)` (focus ring) |

---

## States — Secondary (emulated mode, on teal `var(--bg-brand-secondary-solid)` (#098486) header)

### Default

| Property | Value |
|----------|-------|
| Background | `var(--bg-brand-secondary-solid)` (#098486) (dark teal) |
| Box shadow | `0 1px 2px 0 rgba(73, 88, 131, 0.04), 0 1px 3px 0 rgba(73, 88, 131, 0.16)` |

### Hover

| Property | Value |
|----------|-------|
| Background | `secondary-900` (#055051) (very dark teal) |

### Menu Open (Active)

| Property | Value |
|----------|-------|
| Background | `secondary-900` (#055051) (same as hover) |
| Box shadow | `0 0 0 4px rgba(11, 165, 167, 0.24), 0 1px 2px 0 rgba(73, 88, 131, 0.04)` (teal focus ring) |

---

## Dropdown Menu

The dropdown appears below (or above if no space) the trigger, aligned to the right edge.

### Menu Panel

| Property | Value |
|----------|-------|
| Width | `256px` |
| Background | `var(--bg-primary)` (#FFFFFF) |
| Border | `1px solid` a subtle secondary border color |
| Border radius | `8px` |
| Box shadow | large elevation shadow |
| Padding | `2px 0` (top/bottom) |
| Offset from trigger | `4px` below |
| Animation | slide-down (when opening below) or slide-up (when opening above) |

### Menu Position

The menu prefers to open **bottom-right** (aligned to the right edge of the trigger, expanding downward). If there is not enough space, it falls back to top-right, bottom-left, or top-left.

### Menu Content

The dropdown contains:

1. **User info section** — avatar, user name, agency name, email (non-clickable, informational)
2. **Separator** — thin horizontal line
3. **Menu items** — list of clickable items, each with a leading icon and text label (e.g. "Switch Accounts", "Settings")
4. **Separator** — thin horizontal line
5. **Logout item** — a menu item at the bottom to sign out

---

## Behavior

- Clicking the trigger **toggles** the dropdown (open/close)
- Clicking outside the dropdown **closes** it
- The trigger visually enters a **"menu open"** state when the dropdown is visible (darker background + focus ring)
- The trigger returns to default state when the dropdown closes
- On hover, the background darkens smoothly (transition on background-color)

---

## Implementation

```jsx
function MenuTrigger({ variant = 'primary', avatarUrl, userName, agencyName, email }) {
  const [isOpen, setIsOpen] = React.useState(false);
  const [isHovered, setIsHovered] = React.useState(false);

  const colors = {
    primary: {
      bg: 'var(--fg-secondary)', /* Tailwind: bg-primary-text-600 (#495883) — no semantic --bg-* var exists */
      hoverBg: 'var(--bg-primary-solid)',
      activeBg: 'var(--bg-primary-solid)',
      defaultShadow: '0 1px 2px 0 rgba(73, 88, 131, 0.04), 0 1px 3px 0 rgba(73, 88, 131, 0.16)',
      focusShadow: '0 0 0 4px rgba(73, 88, 131, 0.08), 0 1px 2px 0 rgba(73, 88, 131, 0.04)',
    },
    secondary: {
      bg: 'var(--bg-brand-secondary-solid)',
      hoverBg: 'var(--bg-brand-secondary-solid)', /* Tailwind: bg-secondary-900 (#055051) — no semantic --bg-* var exists */
      activeBg: 'var(--bg-brand-secondary-solid)', /* Tailwind: bg-secondary-900 (#055051) — no semantic --bg-* var exists */
      defaultShadow: '0 1px 2px 0 rgba(73, 88, 131, 0.04), 0 1px 3px 0 rgba(73, 88, 131, 0.16)',
      focusShadow: '0 0 0 4px rgba(11, 165, 167, 0.24), 0 1px 2px 0 rgba(73, 88, 131, 0.04)',
    },
  };

  const c = colors[variant];
  const currentBg = isOpen ? c.activeBg : isHovered ? c.hoverBg : c.bg;
  const currentShadow = isOpen ? c.focusShadow : c.defaultShadow;

  return (
    <div className="relative">
      {/* Trigger button */}
      <button
        onClick={() => setIsOpen(!isOpen)}
        className="flex items-center"
        style={{
          padding: '4px',
          borderRadius: '9999px',
          backgroundColor: currentBg,
          border: 'none',
          cursor: 'pointer',
          boxShadow: currentShadow,
          transition: 'background-color 150ms ease-out',
        }}
        onMouseEnter={() => setIsHovered(true)}
        onMouseLeave={() => setIsHovered(false)}
      >
        {/* Avatar */}
        <div
          style={{
            width: '24px',
            height: '24px',
            borderRadius: '50%',
            overflow: 'hidden',
            flexShrink: 0,
          }}
        >
          {avatarUrl ? (
            <img src={avatarUrl} alt="Profile" style={{ width: '100%', height: '100%', objectFit: 'cover' }} />
          ) : (
            <div className="flex items-center justify-center" style={{ width: '100%', height: '100%', backgroundColor: 'var(--fg-tertiary)' /* nearest match for #6B7280 */ }}>
              <svg width="14" height="14" viewBox="0 0 14 14" fill="none">
                <circle cx="7" cy="5" r="3" fill="var(--fg-white)" />
                <path d="M2 13c0-2.8 2.2-5 5-5s5 2.2 5 5" fill="var(--fg-white)" />
              </svg>
            </div>
          )}
        </div>

        {/* Chevron */}
        <svg
          width="12" height="12" viewBox="0 0 12 12" fill="none"
          style={{ marginLeft: '2px', flexShrink: 0 }}
        >
          <polyline points="3,4.5 6,7.5 9,4.5" stroke="var(--fg-white)" strokeWidth="1.5" strokeLinecap="round" strokeLinejoin="round" />
        </svg>
      </button>

      {/* Dropdown menu */}
      {isOpen && (
        <div
          style={{
            position: 'absolute',
            top: 'calc(100% + 4px)',
            right: '0',
            width: '256px',
            backgroundColor: 'var(--bg-primary)',
            border: '1px solid var(--border-tertiary)', /* nearest match for #E2E8F0 ≈ --border-tertiary (#E8ECF1) */
            borderRadius: '8px',
            boxShadow: '0 10px 15px -3px rgba(0,0,0,0.1), 0 4px 6px -2px rgba(0,0,0,0.05)',
            padding: '2px 0',
            zIndex: 50,
          }}
        >
          {/* User info section */}
          <div style={{ padding: '12px 16px' }}>
            <div style={{ fontWeight: 600, fontSize: '14px', color: 'var(--text-primary)' }}>{userName}</div>
            <div style={{ fontSize: '12px', color: 'var(--text-tertiary)' /* nearest match for #6B7280 ≈ --text-tertiary (#6B748E) */ }}>{agencyName}</div>
            <div style={{ fontSize: '12px', color: 'var(--text-tertiary)' /* nearest match for #6B7280 ≈ --text-tertiary (#6B748E) */ }}>{email}</div>
          </div>
          <div style={{ height: '1px', backgroundColor: 'var(--border-tertiary)' /* nearest match for #E2E8F0 ≈ --border-tertiary (#E8ECF1) */ }} />
          {/* Menu items rendered here — each with leading icon + text label */}
          <div style={{ padding: '4px 0' }}>
            {/* Example menu item */}
            <button
              className="flex items-center w-full"
              style={{
                padding: '8px 16px',
                background: 'transparent',
                border: 'none',
                cursor: 'pointer',
                fontSize: '14px',
                color: 'var(--text-primary)',
                gap: '8px',
              }}
            >
              <span style={{ width: '16px', height: '16px', color: 'var(--fg-tertiary)' /* nearest match for #6B7280 ≈ --fg-tertiary (#6B748E) */ }}>⚙</span>
              <span>Settings</span>
            </button>
          </div>
          <div style={{ height: '1px', backgroundColor: 'var(--border-tertiary)' /* nearest match for #E2E8F0 ≈ --border-tertiary (#E8ECF1) */ }} />
          {/* Logout */}
          <div style={{ padding: '4px 0' }}>
            <button
              className="flex items-center w-full"
              style={{
                padding: '8px 16px',
                background: 'transparent',
                border: 'none',
                cursor: 'pointer',
                fontSize: '14px',
                color: 'var(--text-primary)',
                gap: '8px',
              }}
            >
              <span style={{ width: '16px', height: '16px', color: 'var(--fg-tertiary)' /* nearest match for #6B7280 ≈ --fg-tertiary (#6B748E) */ }}>↩</span>
              <span>Logout</span>
            </button>
          </div>
        </div>
      )}
    </div>
  );
}
```

---

## CRITICAL Rules

1. Trigger button has `border-radius: 9999px` (fully rounded pill) with `padding: 4px` on all sides.
2. Avatar is exactly `24px × 24px`, circular (`border-radius: 50%`), with `overflow: hidden`.
3. Chevron icon is `12px`, `marginLeft: 2px`, color `var(--fg-white)` (#FFFFFF).
4. Primary default background is `primary-text-600` (#495883). Hover/active is `var(--bg-primary-solid)` (#353E5A). It MUST blend into the dark plum header.
5. Secondary default background is `var(--bg-brand-secondary-solid)` (#098486). Hover/active is `secondary-900` (#055051). It MUST blend into the teal header.
6. Default box shadow: `0 1px 2px 0 rgba(73, 88, 131, 0.04), 0 1px 3px 0 rgba(73, 88, 131, 0.16)`.
7. Primary focus/active shadow: `0 0 0 4px rgba(73, 88, 131, 0.08), 0 1px 2px 0 rgba(73, 88, 131, 0.04)`.
8. Secondary focus/active shadow: `0 0 0 4px rgba(11, 165, 167, 0.24), 0 1px 2px 0 rgba(73, 88, 131, 0.04)`.
9. Dropdown panel is `256px` wide, `background: var(--bg-primary)` (#FFFFFF), `border-radius: 8px`, `padding: 2px 0`, positioned `4px` below the trigger, right-aligned.
10. Clicking the trigger toggles the dropdown. Clicking outside closes it. The trigger enters "menu open" state (darker bg + focus ring) when dropdown is visible.

---

## Showcase

When rendering the Menu Trigger on the showcase page, display:

1. **Primary — default** on dark plum (`var(--bg-primary-solid)` (#353E5A)) strip: pill with avatar + chevron
2. **Primary — hover** on dark plum strip: darker background
3. **Primary — menu open** on dark plum strip: focus ring visible
4. **Secondary — default** on teal (`var(--bg-brand-secondary-solid)` (#098486)) strip
5. **Secondary — menu open** on teal strip: teal focus ring visible

For each state, show the button at actual size with its exact background, shadow, and border radius.
