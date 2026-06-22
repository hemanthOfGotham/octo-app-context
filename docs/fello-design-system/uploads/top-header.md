# Top Header (Assembly)

The top header is a horizontal bar that renders inside the page layout shell's **header slot**. It composes several individual components into a single row.

> **TOKEN RULE:** Every color in the code examples below uses CSS variable tokens or Tailwind palette names — never raw hex. When implementing, copy the token references exactly. See `design-tokens.md` for definitions.

---

## Structure

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [Logo]  [Search Bar]              [Actions] [Credits] [Menu Trigger]      │
│  ←─── left group ───→              ←──────── right group ────────→         │
└─────────────────────────────────────────────────────────────────────────────┘
```

The header is split into two groups pushed to opposite ends:

### Left group
1. **Fello Logo** — fixed-width logo linking to home (`fello-logo.md`)
2. **Search Bar** — global search field (`search-bar.md`)

### Right group
3. **Header Action Buttons** — A set of icon-only utility buttons (the default set includes AI, Recent, Notifications, Help, but this is customizable per project) (`header-actions.md`)
4. **Credits Button** — pill showing credit balance (`credits-button.md`)
5. **Menu Trigger** — profile avatar pill that opens a dropdown menu (`menu-trigger.md`)

---

## Layout

| Property | Value |
|----------|-------|
| Display | `flex` |
| Direction | `row` |
| Justify content | `space-between` |
| Align items | `center` |
| Height | `100%` (fills the 44px header slot) |
| Background | `transparent` (the dark background comes from the page layout shell behind it) |
| Position | `relative` |
| Z-index | `3` |

---

## Component Spacing

The left group and right group are each `display: flex` with `align-items: center`.

| Between | Spacing |
|---------|---------|
| Logo and Search Bar | determined by logo wrapper width (`64px` reserves space, search bar follows naturally) |
| Last action button and Credits Button | `4px` (margin-right on last action button) |
| Credits Button and Menu Trigger | `8px` (margin-right on credits button) |

Within the right group, individual button margins handle spacing (each action button has `4px` right margin, credits button has `8px` right margin).

---

## Component Reference

Each component below has its own specification file with full details on dimensions, colors, states, and behavior:

| Component | File |
|-----------|------|
| Fello Logo | `fello-logo.md` |
| Search Bar | `search-bar.md` |
| Header Action Buttons | `header-actions.md` |
| Credits Button | `credits-button.md` |
| Menu Trigger (Profile Button) | `menu-trigger.md` |
| Page Layout Shell (parent) | `layout-guidelines.md` |

---

## Variants

### Primary (default)

The standard header on the dark plum (`var(--bg-primary-solid)` (#353E5A)) shell background. All components use their **primary** variant.

### Secondary (admin emulation)

When in admin emulation mode, the shell background changes to teal (`var(--bg-brand-secondary-solid)` (#098486)). The following components switch to their **secondary** variant:
- Search Bar
- Credits Button
- Menu Trigger

The Fello Logo and Header Action Buttons remain unchanged (they are variant-agnostic).

---

## Implementation

```jsx
function TopHeader({ variant = 'primary' }) {
  return (
    <header
      className="flex items-center justify-between relative z-[3]"
      style={{ height: '44px', background: 'transparent' }}
    >
      {/* Left group */}
      <div className="flex items-center">
        {/* Logo — see fello-logo.md */}
        <FelloLogo />
        {/* Search — see search-bar.md */}
        <SearchBar variant={variant} />
      </div>

      {/* Right group */}
      <div className="flex items-center">
        {/* Action buttons — see header-actions.md (customizable) */}
        <HeaderActions />
        {/* Credits — see credits-button.md */}
        <CreditsButton variant={variant} />
        {/* Profile — see menu-trigger.md */}
        <MenuTrigger variant={variant} />
      </div>
    </header>
  );
}
```

---

## CRITICAL Rules

1. The header background is ALWAYS transparent — it sits on the dark shell background from layout-guidelines.md.
2. The header height is exactly 44px — never change this.
3. Left group contains logo + search. Right group contains actions + credits + profile. This order is fixed.
4. Action buttons in the right group are CUSTOMIZABLE — projects can add, remove, or modify action icons. The default set (AI, Recent, Notifications, Help) is just an example.
5. All sub-components (logo, search, credits, actions, profile) have their own spec files. Always reference them for styling details.
6. The search bar and credits button change appearance based on the variant (primary vs secondary). Action buttons and logo do NOT change.
7. Do NOT give the header its own background color. The dark plum or teal comes from the parent layout shell.
8. When restyling an existing header, keep all existing action buttons and their functionality. Only change the visual treatment to match this spec.

---

## Showcase

When rendering the Top Header on the showcase page:

1. Display the **full assembled header** on a dark plum (`var(--bg-primary-solid)` (#353E5A)) background, 44px tall, showing all components in their correct positions
2. Display the **secondary variant** on a teal (`var(--bg-brand-secondary-solid)` (#098486)) background for comparison
3. Label each component region
