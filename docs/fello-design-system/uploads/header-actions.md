# Header Action Buttons

A row of icon-only utility buttons displayed in the app header. The set of buttons is **customizable per project** — projects can add, remove, or modify action buttons. The default example set includes Ask AI, Recent Engagement, Notifications, and Help, but this is only a reference. The first button is visually separated from the remaining buttons by a vertical divider.

> **TOKEN RULE:** Every color in the code examples below uses CSS variable tokens or Tailwind palette names — never raw hex. When implementing, copy the token references exactly. See `design-tokens.md` for definitions.

---

## Structure

```
┌──────┐   │   ┌──────┐ ┌──────┐ ┌──────┐
│  AI  │   │   │ List │ │ Bell │ │  ?   │
└──────┘   │   └──────┘ └──────┘ └──────┘
            ↑
     vertical separator
```

From left to right:

1. **Ask AI** button — opens an AI assistant dialog
2. **Vertical separator** — thin line divider
3. **Recent Engagement** button — opens recent engagement dialog
4. **Notifications** button — opens notifications dialog (icon changes when there are unread notifications)
5. **Help** button — opens a help/resource center

---

## Button Specs (shared by all 4 buttons)

| Property | Value |
|----------|-------|
| Width | `32px` |
| Height | `32px` |
| Border radius | `6px` |
| Icon size | `16px` |
| Icon color | `var(--fg-white)` (#FFFFFF) |
| Background (default) | `transparent` |
| Padding | not applicable (icon-only, icon is centered) |
| Margin right | `4px` (between each button) |
| Cursor | `pointer` |
| Tooltip | each button has a descriptive tooltip (see Button Details below) |

---

## Button Details

| # | Tooltip | Icon description |
|---|---------|-----------------|
| 1 | "Ask AI" | Sparkle/AI brain icon |
| 2 | "Recent Engagement" | Activity list icon |
| 3 | "Notifications" | Bell icon (default) / Bell with dot (when unread notifications exist) |
| 4 | "Help" | Question mark inside a circle |

### Notification Badge

The notifications button does **not** use a separate badge element. Instead, the **entire icon switches** to a bell-with-dot variant when the unread notification count is greater than 0. The dot is baked into the icon itself.

---

## Vertical Separator

A thin vertical line placed between the "Ask AI" button and the "Recent Engagement" button.

| Property | Value |
|----------|-------|
| Width | `1px` |
| Height | stretches to fill parent height (minus vertical padding) |
| Color | `var(--border-secondary)` (#CCD6DC) (subtle gray — secondary border color) |
| Style | solid |
| Horizontal margin | `8px` on each side (left and right) |
| Vertical padding | `8px` top and bottom (so the line is shorter than the full header height) |

---

## Interactive States

### Default

| Property | Value |
|----------|-------|
| Background | `transparent` |
| Icon color | `var(--fg-white)` (#FFFFFF) |
| Box shadow | none |

### Hover

| Property | Value |
|----------|-------|
| Background | `var(--bg-secondary)` (#F1F4F7) (very light gray) |
| Box shadow | `0 1px 2px 0 rgba(73, 88, 131, 0.08)` |

### Focus

| Property | Value |
|----------|-------|
| Background | `var(--bg-secondary)` (#F1F4F7) (same as hover) |
| Box shadow | `0 0 0 4px rgba(73, 88, 131, 0.08), 0 1px 2px 0 rgba(73, 88, 131, 0.04), 0 1px 3px 0 rgba(73, 88, 131, 0.16)` |

---

## Behavior

- Each button opens a dialog or panel on click (no navigation)
- Tooltips appear on hover after a short delay
- The buttons do **not** change between primary/secondary header variants — they always use transparent background with white icons regardless of header color

---

## Implementation

```jsx
function HeaderActions({ actions }) {
  // Default actions — override with your own set of action buttons.
  // Each action has: id, tooltip, icon, onClick handler.
  const defaultActions = [
    { id: 'ai', tooltip: 'Ask AI', icon: <SparkleIcon />, onClick: () => {} },
    { id: 'recent', tooltip: 'Recent Engagement', icon: <ListIcon />, onClick: () => {} },
    { id: 'notifications', tooltip: 'Notifications', icon: <BellIcon />, onClick: () => {} },
    { id: 'help', tooltip: 'Help', icon: <HelpCircleIcon />, onClick: () => {} },
  ];

  const items = actions || defaultActions;
  const separatorIndex = 0; // Separator appears after the first button

  return (
    <div className="flex items-center" style={{ height: '100%' }}>
      {items.map((action, index) => (
        <React.Fragment key={action.id}>
          <button
            title={action.tooltip}
            onClick={action.onClick}
            className="flex items-center justify-center shrink-0"
            style={{
              width: '32px',
              height: '32px',
              borderRadius: '6px',
              background: 'transparent',
              border: 'none',
              cursor: 'pointer',
              marginRight: '4px',
              color: 'var(--fg-white)',
            }}
            onMouseEnter={(e) => {
              e.currentTarget.style.background = 'var(--bg-secondary)';
              e.currentTarget.style.boxShadow = '0 1px 2px 0 rgba(73, 88, 131, 0.08)';
            }}
            onMouseLeave={(e) => {
              e.currentTarget.style.background = 'transparent';
              e.currentTarget.style.boxShadow = 'none';
            }}
          >
            <span style={{ width: '16px', height: '16px' }}>{action.icon}</span>
          </button>

          {/* Vertical separator after the first button */}
          {index === separatorIndex && (
            <div
              style={{
                width: '1px',
                alignSelf: 'stretch',
                backgroundColor: 'var(--border-secondary)',
                margin: '8px',
              }}
            />
          )}
        </React.Fragment>
      ))}
    </div>
  );
}
```

---

## CRITICAL Rules

1. Each action button is exactly `32px x 32px` with `border-radius: 6px`.
2. Icon size is `16px`, centered within the button. Icon color is always `var(--fg-white)` (#FFFFFF).
3. Default background is `transparent`. Hover background is `var(--bg-secondary)` (#F1F4F7) with `box-shadow: 0 1px 2px 0 rgba(73, 88, 131, 0.08)`.
4. Focus background is `var(--bg-secondary)` (#F1F4F7) with `box-shadow: 0 0 0 4px rgba(73, 88, 131, 0.08), 0 1px 2px 0 rgba(73, 88, 131, 0.04), 0 1px 3px 0 rgba(73, 88, 131, 0.16)`.
5. Margin-right between buttons is `4px`.
6. The vertical separator is `1px` wide, color `var(--border-secondary)` (#CCD6DC), with `8px` margin on left and right, and `8px` vertical padding (top/bottom) so the line is shorter than the full header height.
7. The separator ALWAYS appears after the **first** button, regardless of what that button is.
8. The action buttons are **variant-agnostic** — they do NOT change between primary/secondary header variants. Always transparent background with white icons.
9. The set of action buttons is **customizable**. When restyling an existing header, keep all existing action buttons and their functionality — only change the visual treatment to match this spec.
10. Every button must have a descriptive `title` (tooltip) for accessibility.

---

## Showcase

When rendering the Header Action Buttons on the showcase page:

1. Display all 4 buttons + separator on a dark plum `var(--bg-primary-solid)` (#353E5A) strip to simulate the header
2. Show default state, hover state (on one button), and focus state (on another)
3. Show the notification bell in both states: without unread (normal bell) and with unread (bell with dot)
4. Label each button with its tooltip text below
