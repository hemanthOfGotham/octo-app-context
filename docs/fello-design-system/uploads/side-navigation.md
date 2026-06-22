# Side Navigation Component — Strict Specification

When building any sidebar navigation, collapsible side menu, or vertical navigation panel, follow these specifications exactly. Do NOT use shadcn Sidebar, Tailwind navigation, or any other sidebar system.

> **TOKEN RULE:** Every color in the code examples below uses CSS variable tokens or Tailwind palette names — never raw hex. When implementing, copy the token references exactly. See `design-tokens.md` for definitions.

**Note:** Each navigation item uses the Navigation List Item spec (see `navigation-list.md`). Separators use the Separator spec (see `separator.md`).

---

## Architecture Overview

```
Side Navigation (outer container — white rounded card with border + shadow)
├── Toggle button (absolutely positioned at right edge, hidden until hover)
├── START section (top — Home item only)
│   └── Home NavigationListItem
├── Separator (gap="sm" → 8px margin, 1px solid var(--border-secondary) (#CCD6DC) line)
├── CENTER section (main — flex-1, scrollable overflow)
│   ├── NavigationListItem(s) — Leads, Records, Marketing, Database, etc.
│   └── "More" item (popover with hidden overflow items)
├── Separator (gap="xs" → 4px margin, 1px solid var(--border-secondary) (#CCD6DC) line)
└── END section (bottom — Settings, App Marketplace)
    └── NavigationListItem(s)
```

**⚠️ KEY STRUCTURAL RULES:**
1. The container is a **single white card** — there is NO gray header section, NO colored top bar. Everything sits on the same `var(--bg-primary)` (#FFFFFF) background.
2. The 3 sections (START, CENTER, END) are separated by thin `var(--border-secondary)` (#CCD6DC) separator lines — nothing else.
3. The CENTER section gets `flex: 1` to fill remaining vertical space.
4. Each navigation item is a simple **flex row** with icon + text — NOT a card or button with heavy styling.
5. **Navigation items have NO subtext / supporting text** — each item is ONLY an icon + a single-line label. Do NOT add descriptions, secondary text, or helper text under any nav item.
6. **The END section (bottom) MUST always be present** — it is pinned to the bottom of the sidebar. Common items: Settings, App Marketplace. These MUST always render even if the center section is scrollable.

---

## Default Navigation Items (Story / Demo)

When building a side navigation without specific items provided, use these default items:

| Section | Icon | Label | Notes |
|---------|------|-------|-------|
| **START** | `Home` | Home | Top section, alone above the first separator. Often the active item. |
| — | — | — | **── Separator ──** |
| **CENTER** | `Heart` | Leads | |
| **CENTER** | `Users` | Records | |
| **CENTER** | `Megaphone` | Marketing | |
| **CENTER** | `Database` | Database | May have nested children (popover) |
| — | — | — | **── Separator ──** |
| **END** | `Settings` | Settings | Bottom section, pinned below last separator |
| **END** | `LayoutGrid` | App Marketplace | Bottom section, pinned below last separator |

**⚠️ Home is the ONLY item in the START section** — it sits alone at the top, with a separator line below it before the CENTER items begin.

**⚠️ The END items (Settings, App Marketplace) are MANDATORY** — they must always appear pinned at the bottom, below the CENTER→END separator. The CENTER section scrolls independently; the END section never scrolls.

**⚠️ Items are ONLY icon + label** — no subtext, no description, no secondary line of text. Each nav item is a single row: `[icon] [label]`.

---

## Container

### Expanded State

```jsx
style={{
  display: 'flex',
  flexDirection: 'column',
  position: 'relative',
  padding: '12px',                       // spacing-3
  border: '1px solid var(--border-primary)',   // #BDC8D3
  borderRadius: '12px',                       // radius-6
  width: '300px',
  height: '100%',
  background: 'var(--bg-primary)',            // #FFFFFF
  boxShadow: '0 24px 48px -12px var(--ring-gray-primary)',  // shadow-2xl
  transition: 'width 150ms ease-in-out',
}}
```

### Collapsed State

```jsx
style={{
  display: 'flex',
  flexDirection: 'column',
  alignItems: 'flex-start',
  position: 'relative',
  padding: '12px',
  width: '64px',
  height: '100%',
  border: 'none',
  boxShadow: 'none',
  background: 'transparent',
  transition: 'width 150ms ease-in-out',
}}
```

### Key Dimensions

| State | Width | Border | Shadow | Border Radius | Background |
|-------|-------|--------|--------|---------------|------------|
| Expanded | 300px | 1px solid `var(--border-primary)` (#BDC8D3) | shadow-2xl | 12px | `var(--bg-primary)` (#FFFFFF) |
| Collapsed | 64px | none | none | — | transparent |

---

## Toggle Button

A small button that appears on hover at the right edge of the sidebar:

```jsx
// Hidden by default, shown on sidebar hover
style={{
  display: isHovered ? 'block' : 'none',
  position: 'absolute',
  zIndex: 1,
  right: '-12px',
  top: '52px',
}}

// Use: XS outline neutral button with icon-only
<Button size="xs" variant="neutral" buttonType="outline" iconOnly>
  {expanded ? <ChevronLeftIcon /> : <ChevronRightIcon />}
</Button>
```

---

## Separators Between Sections

Between START → CENTER sections:
```jsx
{/* Separator with gap="sm" → 8px margin top/bottom */}
<div className="flex items-center w-full" style={{ margin: '8px 0' }}>
  <div className="flex-1" style={{ borderTop: '1px solid var(--border-secondary)' }} />
</div>
```

Between CENTER → END sections:
```jsx
{/* Separator with gap="xs" → 4px margin top/bottom */}
<div className="flex items-center w-full" style={{ margin: '4px 0' }}>
  <div className="flex-1" style={{ borderTop: '1px solid var(--border-secondary)' }} />
</div>
```

---

## Navigation Item (inside the sidebar)

Each item is a simple flex row — **icon + single-line label only**. Do NOT add supporting text, descriptions, or subtitles to any navigation item. See `navigation-list.md` for the full item spec. Here is the summary used in the sidebar context:

### Default Item

```jsx
<button className="
  flex items-center gap-2 w-full
  py-2 px-3 rounded
    bg-transparent text-[var(--text-primary)]
    cursor-pointer outline-none border-none
    text-sm leading-5 font-medium
    hover:bg-[var(--bg-secondary)]
    focus-visible:bg-[var(--bg-secondary)]
    transition-colors duration-150 ease-out
  ">
  <IconComponent className="w-6 h-6 shrink-0" />
  <span className="truncate">Item Label</span>
</button>
```

### Active Item

```jsx
<button className="
  flex items-center gap-2 w-full
  py-2 px-3 rounded
    bg-[var(--bg-brand-primary)] text-[var(--text-primary)]
    cursor-pointer outline-none border-none
    text-sm leading-5 font-medium
    hover:bg-[var(--bg-brand-primary_alt)]
    focus-visible:bg-[var(--bg-brand-primary_alt)]
    transition-colors duration-150 ease-out
  ">
  <IconComponent className="w-6 h-6 shrink-0 text-[var(--fg-brand-primary)]" />
  <span className="truncate">Active Item</span>
</button>
```

### Icon-Only Mode (Collapsed Sidebar)

When the sidebar is collapsed, each nav item shows only an icon. **On hover, a dark tooltip appears to the RIGHT of the item showing the item's label.** This uses the design system Tooltip (see `tooltip.md`) in dark mode, positioned to the right.

```jsx
<div className="relative group">
  <button className="
    flex items-center justify-center
    p-2 rounded-lg
    bg-transparent text-[var(--text-primary)]
    cursor-pointer outline-none border-none
    hover:bg-[var(--bg-secondary)]
    transition-colors duration-150 ease-out
  ">
    <IconComponent className="w-6 h-6 shrink-0" />
  </button>

  {/* Tooltip — appears to the RIGHT on hover (dark, no title, description = item label) */}
  <div
    role="tooltip"
    className="
      absolute left-full top-1/2 -translate-y-1/2 ml-2.5
      py-2 px-3
      rounded-lg
      bg-[var(--tooltip-bg-primary-dark)] text-white
      text-xs leading-[18px] font-normal
      max-w-[200px] w-fit
      shadow-lg
      whitespace-nowrap
      z-50
      pointer-events-none

      invisible group-hover:visible
      opacity-0 group-hover:opacity-100
      translate-x-1 group-hover:translate-x-0
      transition-all duration-150 ease-out
    "
  >
    {/* Arrow pointing left toward the icon */}
    <div className="
      absolute -left-1 top-1/2 -translate-y-1/2
      w-3 h-3
      bg-[var(--tooltip-bg-primary-dark)]
      rotate-45
      rounded-[1px]
    " />
    <span>Item Label</span>
  </div>
</div>
```

**Active item in collapsed mode with tooltip:**

```jsx
<div className="relative group">
  <button className="
    flex items-center justify-center
    p-2 rounded-lg
    bg-[var(--bg-brand-primary)] text-[var(--text-primary)]
    cursor-pointer outline-none border-none
    hover:bg-[var(--bg-brand-primary_alt)]
    transition-colors duration-150 ease-out
  ">
    <IconComponent className="w-6 h-6 shrink-0 text-[var(--fg-brand-primary)]" />
  </button>

  {/* Same tooltip — always dark regardless of active state */}
  <div
    role="tooltip"
    className="
      absolute left-full top-1/2 -translate-y-1/2 ml-2.5
      py-2 px-3
      rounded-lg
      bg-[var(--tooltip-bg-primary-dark)] text-white
      text-xs leading-[18px] font-normal
      max-w-[200px] w-fit
      shadow-lg
      whitespace-nowrap
      z-50
      pointer-events-none

      invisible group-hover:visible
      opacity-0 group-hover:opacity-100
      translate-x-1 group-hover:translate-x-0
      transition-all duration-150 ease-out
    "
  >
    <div className="
      absolute -left-1 top-1/2 -translate-y-1/2
      w-3 h-3
      bg-[var(--tooltip-bg-primary-dark)]
      rotate-45
      rounded-[1px]
    " />
    <span>Active Item</span>
  </div>
</div>
```

---

## Color Reference

| Element | Token | Hex |
|---------|-------|-----|
| Default text | `var(--text-primary)` | (#353E5A) |
| Default bg | transparent | — |
| Hover bg | `var(--bg-secondary)` | (#F1F4F7) |
| Active bg | `var(--bg-brand-primary)` | (#FDF1EF) |
| Active hover bg | `var(--bg-brand-primary_alt)` | (#FDE7E3) |
| Active icon | `var(--fg-brand-primary)` | (#FF725C) |
| Default icon | inherits `var(--text-primary)` | (#353E5A) |
| Separator line | `var(--border-secondary)` | (#CCD6DC) |
| Container border | `var(--border-primary)` | (#BDC8D3) |
| Container shadow | shadow-2xl | `0 24px 48px -12px var(--ring-gray-primary)` |
| Container bg | `var(--bg-primary)` | (#FFFFFF) |

---

## Nested Children (List Mode)

When `openingType="list"`, children expand inline:

```jsx
// Children wrapper
<div style={{
  position: 'relative',
  paddingLeft: '32px',                   // spacing-8
  overflow: 'hidden',
}}>
  {/* Vertical connector line */}
  <div style={{
    position: 'absolute',
    bottom: 0,
    left: '22px',
    height: '100%',
    borderLeft: '1px solid var(--border-secondary)',  // #CCD6DC
  }} />

  {/* Child items */}
  {children}
</div>
```

**Branch connector on each child item:**
```jsx
<span style={{
  position: 'absolute',
  top: 0,
  left: '-10px',
  width: '10px',
  height: '50%',
  borderLeft: '1px solid var(--border-secondary)',
  borderBottom: '1px solid var(--border-secondary)',
  borderBottomLeftRadius: '4px',         // radius-2
  zIndex: 1,
}} />
```

---

## Nested Children (Popover Mode)

When `openingType="popover"`, children open in a floating menu:

```jsx
// Popover content
<div style={{ width: '240px' }}>
  <h6 style={{ fontFamily: 'Inter, sans-serif', fontSize: '16px', lineHeight: '24px', fontWeight: 600, padding: '8px', margin: '8px' }}>
    {parentText}
  </h6>
  {children}
</div>
```

---

## "More" Button

When the center section overflows, hidden items move into a "More" popover:

- Item height constant: 44px (used for calculating visible count)
- Visible count: `Math.floor(containerHeight / 44) - 1`
- "More" item uses a horizontal dots icon and `openingType="popover"`

---

## Complete Implementation Pattern

```jsx
function SideNavigation({ items, activeId, expanded, onToggle }) {
  const [isHovered, setIsHovered] = useState(false);

  // Split items into sections
  const startItems = items.filter(i => i.section === 'start');
  const centerItems = items.filter(i => i.section === 'center');
  const endItems = items.filter(i => i.section === 'end');

  return (
    <nav
      className="flex flex-col gap-0 relative"
      style={{
        padding: '12px',
        width: expanded ? '300px' : '64px',
        height: '100%',
        background: expanded ? 'var(--bg-primary)' : 'transparent',
        border: expanded ? '1px solid var(--border-primary)' : 'none',
        borderRadius: expanded ? '12px' : '0',
        boxShadow: expanded ? '0 24px 48px -12px var(--ring-gray-primary)' : 'none',
        transition: 'width 150ms ease-in-out',
      }}
      onMouseEnter={() => setIsHovered(true)}
      onMouseLeave={() => setIsHovered(false)}
    >
      {/* Toggle button — appears on hover */}
      {isHovered && (
        <button
          className="
            absolute z-10 flex items-center justify-center
            w-6 h-6 rounded-md
            bg-white border border-[var(--border-secondary)]
            shadow-sm cursor-pointer
            hover:bg-[var(--bg-secondary)]
          "
          style={{ right: '-12px', top: '52px' }}
          onClick={onToggle}
        >
          {expanded
            ? <ChevronLeft className="w-4 h-4 text-[var(--fg-primary)]" />
            : <ChevronRight className="w-4 h-4 text-[var(--fg-primary)]" />
          }
        </button>
      )}

      {/* === START section === */}
      {startItems.map(item => (
        <NavItem key={item.id} item={item} active={item.id === activeId} iconOnly={!expanded} />
      ))}

      {/* Separator: START → CENTER (gap="sm" = 8px margin) */}
      {startItems.length > 0 && (
        <div className="flex items-center w-full" style={{ margin: '8px 0' }}>
          <div className="flex-1" style={{ borderTop: '1px solid var(--border-secondary)' }} />
        </div>
      )}

      {/* === CENTER section === (flex-1 to fill remaining space) */}
      <div className="flex-1 flex flex-col gap-0 overflow-y-auto">
        {centerItems.map(item => (
          <NavItem key={item.id} item={item} active={item.id === activeId} iconOnly={!expanded} />
        ))}
      </div>

      {/* Separator: CENTER → END (gap="xs" = 4px margin) */}
      {endItems.length > 0 && (
        <div className="flex items-center w-full" style={{ margin: '4px 0' }}>
          <div className="flex-1" style={{ borderTop: '1px solid var(--border-secondary)' }} />
        </div>
      )}

      {/* === END section === */}
      {endItems.map(item => (
        <NavItem key={item.id} item={item} active={item.id === activeId} iconOnly={!expanded} />
      ))}
    </nav>
  );
}


{/* === NavItem component === */}
function NavItem({ item, active, iconOnly }) {
  if (iconOnly) {
    return (
      <div className="relative group">
        <button
          className={`
            flex items-center justify-center
            p-2 rounded-lg
            cursor-pointer outline-none border-none
            transition-colors duration-150 ease-out
            ${active
              ? 'bg-[var(--bg-brand-primary)] hover:bg-[var(--bg-brand-primary_alt)]'
              : 'bg-transparent hover:bg-[var(--bg-secondary)]'
            }
          `}
        >
          <item.icon className={`w-6 h-6 shrink-0 ${active ? 'text-[var(--fg-brand-primary)]' : 'text-[var(--fg-primary)]'}`} />
        </button>

        {/* Dark tooltip — appears to the RIGHT showing item label */}
        <div
          role="tooltip"
          className="
            absolute left-full top-1/2 -translate-y-1/2 ml-2.5
            py-2 px-3 rounded-lg
            bg-[var(--tooltip-bg-primary-dark)] text-white
            text-xs leading-[18px] font-normal
            w-fit shadow-lg whitespace-nowrap z-50
            pointer-events-none
            invisible group-hover:visible
            opacity-0 group-hover:opacity-100
            translate-x-1 group-hover:translate-x-0
            transition-all duration-150 ease-out
          "
        >
          <div className="absolute -left-1 top-1/2 -translate-y-1/2 w-3 h-3 bg-[var(--tooltip-bg-primary-dark)] rotate-45 rounded-[1px]" />
          <span>{item.label}</span>
        </div>
      </div>
    );
  }

  return (
    <button
      className={`
        flex items-center gap-2 w-full
        py-2 px-3 rounded
        text-[var(--text-primary)] text-sm leading-5 font-medium
        cursor-pointer outline-none border-none
        transition-colors duration-150 ease-out
        ${active
          ? 'bg-[var(--bg-brand-primary)] hover:bg-[var(--bg-brand-primary_alt)] focus-visible:bg-[var(--bg-brand-primary_alt)]'
          : 'bg-transparent hover:bg-[var(--bg-secondary)] focus-visible:bg-[var(--bg-secondary)]'
        }
      `}
    >
      <item.icon className={`w-6 h-6 shrink-0 ${active ? 'text-[var(--fg-brand-primary)]' : ''}`} />
      <span className="truncate">{item.label}</span>
    </button>
  );
}
```

---

## Animations

| Animation | Duration | Easing |
|-----------|----------|--------|
| Sidebar width (expand/collapse) | 150ms | ease-in-out |
| Children expand/collapse (list mode) | 300ms | ease-in |
| Chevron icon rotation | 300ms | ease-in |

---

## CRITICAL Rules

1. **The sidebar is a SINGLE white card** — `var(--bg-primary)` (#FFFFFF) background, `1px solid var(--border-primary)` (#BDC8D3) border, `12px` border-radius, `shadow-2xl`. There is NO separate gray header area, NO colored top bar, NO card-within-a-card. Everything sits on the same flat white surface.
2. **Expanded width is exactly 300px**, collapsed is 64px. Do NOT use percentage widths.
3. **Container padding is 12px** on all sides (`p-3`). Items sit directly inside this padded area.
4. **3 sections (START, CENTER, END)** are separated ONLY by thin 1px `var(--border-secondary)` (#CCD6DC) horizontal lines. START→CENTER uses `8px` margin (gap="sm"), CENTER→END uses `4px` margin (gap="xs").
5. **CENTER section gets `flex: 1`** to fill all remaining vertical space. It is the only scrollable section.
6. **Active item background is `var(--bg-brand-primary)`** (#FDF1EF, light peach / PRIMARY_50) — NOT white, NOT gray, NOT teal.
7. **Active item icon color is `var(--fg-brand-primary)`** (#FF725C, primary orange). Text stays `var(--text-primary)` (#353E5A). Default (non-active) icons inherit `var(--text-primary)` (#353E5A).
8. **Item border-radius is 4px** (`rounded`) normally, 8px (`rounded-lg`) in icon-only / collapsed mode.
9. **Nav items have NO wrapper div and NO wrapper padding** — the `<button>` sits directly inside the nav container. Do NOT wrap items in a `<div>` with `py-0.5`, `px-1`, or any padding. The button's own `py-2 px-3` provides all the spacing. Items must sit flush against each other with zero gap.
10. **Toggle button is hidden by default** — only appears on sidebar hover. Positioned at `right: -12px, top: 52px` (sticks out from the right edge).
11. **Collapsed state has NO border and NO shadow** — background becomes transparent. Only expanded state has the white card appearance.
12. **Navigation items are simple flex rows** — icon + text label. They are NOT cards, NOT tiles, NOT buttons with heavy borders or shadows. The only visual distinction is the subtle background color on hover/active.
13. **Typography is caption-md** (14px/20px/500) for all items. Icon size is 24px (`w-6 h-6`). Gap between icon and text is 8px (`gap-2`).
14. **Item content is left-aligned** (`items-center` vertically, text flows naturally to the right of the icon). NEVER center the text.
15. **NO supporting text / subtext on any nav item** — each item is ONLY `[icon] [label]` in a single line. Do NOT add descriptions, helper text, or secondary labels below the item name.
16. **END section (bottom items) is MANDATORY** — Settings and App Marketplace (or similar items) MUST always appear pinned at the bottom of the sidebar, separated from CENTER by the `var(--border-secondary)` (#CCD6DC) line. The END section must never scroll away — only the CENTER section scrolls.
17. **Collapsed nav items MUST show a dark tooltip on hover** — When the sidebar is collapsed (icon-only mode), hovering on any nav item shows a dark (`var(--tooltip-bg-primary-dark)` (#052033)) tooltip to the RIGHT of the icon displaying the item's label. The tooltip uses the design system tooltip pattern (see `tooltip.md`): `bg-[var(--tooltip-bg-primary-dark)]`, white text, `12px` arrow pointing left, `10px` gap (`ml-2.5`), `rounded-lg`, `shadow-lg`. Do NOT rely on the native `title` attribute — use the proper tooltip component with CSS hover (`group`/`group-hover`) visibility.
18. **ZERO gap between nav items** — The nav container uses `flex flex-col gap-0`. Items are `<button>` elements directly inside the container — NO wrapper `<div>`, NO `py-0.5`, NO `px-1`, NO `gap-*`, NO `space-y-*`, NO margins. The button's own `py-2 px-3` is the only padding. Items sit flush with zero space between them.
