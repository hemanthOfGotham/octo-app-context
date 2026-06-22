# Navigation List Component — Strict Specification

When building any side navigation, menu list, nav tree, or navigation drawer menu, follow these specifications exactly. Do NOT use default Tailwind nav menus, shadcn navigation, or any other nav list system.

> **TOKEN RULE — MANDATORY**
> Never use raw hex color values in implementation code. Every color must reference a CSS variable token (`var(--token-name)`) via Tailwind arbitrary-value syntax (e.g., `text-[var(--text-primary)]`, `bg-[var(--bg-brand-primary)]`). Hex codes appear in this spec only as reference annotations in parentheses. See `design-tokens.md` for the full token list.

---

## Navigation List Anatomy

A vertical list of navigation items, optionally grouped and nestable:

```
┌──────────────────────────────────┐
│  Group Title                     │  ← Optional group label
│                                  │
│  🏠  Dashboard                   │  ← Active item (brand bg)
│  📊  Analytics                   │  ← Default item
│  ▸  Settings                     │  ← Expandable with children
│     │── General                  │  ← Nested child with branch line
│     └── Security                 │  ← Last child
│  👤  Profile                     │
│                                  │
└──────────────────────────────────┘
```

- Items have optional leading icons, text, and trailing elements
- Active item gets a brand-primary background
- Expandable items show a chevron that rotates on expand
- Nested items are indented 32px with L-shaped branch lines

---

## Sizes

### Extra Small (XS)
| Property | Value | Tailwind |
|----------|-------|----------|
| Wrapper padding | 0 | `p-0` |
| Item padding | 6px vertical, 12px horizontal | `py-1.5 px-3` |
| Typography | 14px / 20px / weight 500 | `text-sm leading-5 font-medium` |
| Icon size | 20px | `w-5 h-5` |
| Border radius | 4px | `rounded` |

### Small (SM)
| Property | Value | Tailwind |
|----------|-------|----------|
| Wrapper padding | 0 vertical, 4px horizontal | `px-1` |
| Item padding | 6px vertical, 12px horizontal | `py-1.5 px-3` |
| Typography | 12px / 18px / weight 500 | `text-xs leading-[18px] font-medium` |
| Icon size | 20px | `w-5 h-5` |
| Border radius | 4px | `rounded` |

### Medium (MD) — Default
| Property | Value | Tailwind |
|----------|-------|----------|
| Wrapper padding | 2px vertical, 4px horizontal | `py-0.5 px-1` |
| Item padding | 8px vertical, 12px horizontal | `py-2 px-3` |
| Typography | 14px / 20px / weight 500 | `text-sm leading-5 font-medium` |
| Icon size | 24px | `w-6 h-6` |
| Border radius | 4px | `rounded` |

**⚠️ Exception — Inside side navigation:** When nav items are rendered inside a side navigation container, there is NO wrapper `<div>` at all — no `py-0.5`, no `px-1`. The `<button>` sits directly inside the nav container. The container's `12px` padding + button's `py-2 px-3` provide all spacing. Items must sit flush with zero gap between them. See `side-navigation.md` for details.

---

## Colors

### Default State
| Property | Value | Tailwind |
|----------|-------|----------|
| Background | Transparent | `bg-transparent` |
| Text | `var(--text-primary)` (#353E5A) | `text-[var(--text-primary)]` |
| Hover bg | `var(--bg-secondary)` (#F1F4F7) | `hover:bg-[var(--bg-secondary)]` |
| Focus bg | `var(--bg-secondary)` (#F1F4F7) | `focus-visible:bg-[var(--bg-secondary)]` |

### Active State
| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `var(--bg-brand-primary)` (#FDF1EF) | `bg-[var(--bg-brand-primary)]` |
| Text | `var(--text-primary)` (#353E5A) | `text-[var(--text-primary)]` |
| Icon color | `var(--fg-brand-primary)` (#FF725C) | `text-[var(--fg-brand-primary)]` |
| Hover bg | `var(--bg-brand-primary_alt)` (#FDE7E3) | `hover:bg-[var(--bg-brand-primary_alt)]` |
| Focus bg | `var(--bg-brand-primary_alt)` (#FDE7E3) | `focus-visible:bg-[var(--bg-brand-primary_alt)]` |

---

## Item Content Layout

| Property | Value | Tailwind |
|----------|-------|----------|
| Display | Flex, centered | `flex items-center` |
| Gap | 8px | `gap-2` |
| Width | 100% | `w-full` |
| Cursor | Pointer | `cursor-pointer` |
| Border radius | 4px | `rounded` |

---

## Supporting Text

| Property | Value | Tailwind |
|----------|-------|----------|
| Typography | 12px / 18px / weight 400 (paragraph-sm) | `text-xs leading-[18px] font-normal` |
| Color | Inherits | — |

---

## Group Title

| Property | Value | Tailwind |
|----------|-------|----------|
| Typography | 14px / 20px / weight 600 (title-md) | `text-sm leading-5` + `style={{ fontWeight: 600 }}` |
| Color | `var(--text-primary)` (#353E5A) | `text-[var(--text-primary)]` |
| Padding | 6px vertical, 12px horizontal | `py-1.5 px-3` |

---

## Trailing Elements

Items can have a trailing icon, trailing badge, or trailing button:

### Trailing Icon
| Property | Value |
|----------|-------|
| Size | 16px |
| Alignment | Right side of the item |

### Trailing Button
| Property | Value |
|----------|-------|
| Visibility | Always or on hover only (configurable) |
| Position | Right side of the item |

---

## Expandable Items (Navigation Title)

Items with children show a chevron icon that rotates:

| Property | Value | Tailwind |
|----------|-------|----------|
| Chevron | Right-pointing arrow | `ChevronRight` |
| Collapsed | rotate(0deg) | `rotate-0` |
| Expanded | rotate(90deg) | `rotate-90` |
| Transition | transform 300ms ease-in | `transition-transform duration-300 ease-in` |

---

## Nested Items (Branch Lines)

When items are nested (inline expansion):

| Property | Value |
|----------|-------|
| Children indent | 32px left padding |
| Vertical parent line | 1px solid `var(--border-secondary)` (#CCD6DC), left: 22px, full height |
| Branch connector | L-shaped, 10px wide, 1px solid `var(--border-secondary)` (#CCD6DC), 4px bottom-left radius |
| Branch position | -10px from item left edge, 50% height |
| Last child | Covers parent line below with white background |

### Expand/Collapse Animation
| Property | Value |
|----------|-------|
| Collapsed | height: 0, overflow: hidden |
| Expanded | height: auto |
| Transition | 300ms ease-in |

---

## Popover List Items (openingType = "popover")

When an item uses `openingType="popover"`, clicking it opens a **floating popover panel to the RIGHT** instead of expanding children inline. This is used in the side navigation for items like Home, Leads, Records, Marketing — they open a fly-out menu with child items.

### Popover Container

| Property | Value | Tailwind / Style |
|----------|-------|-----------------|
| Width | 240px | `w-[240px]` |
| Background | `var(--bg-primary)` (#FFFFFF) | `bg-[var(--bg-primary)]` |
| Border | 1px solid `var(--border-secondary)` (#CCD6DC) | `border border-[var(--border-secondary)]` |
| Border radius | 8px | `rounded-lg` |
| Box shadow | `shadow-2xl` | `shadow-2xl` — `0 24px 48px -12px rgba(73,88,131,0.16)` |
| Max height | 440px, scrollable | `max-h-[440px] overflow-y-auto` |
| Padding (inner content) | 8px (gap="xs") | `p-2` |
| Position | Floats to the RIGHT of the parent item | Absolutely positioned right of trigger |
| Arrow | 10x10px rotated square, `var(--bg-primary)` (#FFFFFF) bg, 1px solid `var(--border-secondary)` (#CCD6DC) border, positioned on the LEFT edge pointing toward the trigger | See arrow styles below |
| Animation | Slides in from left | `slide-right` transition |

### Popover Title (inside popover)

| Property | Value | Tailwind / Style |
|----------|-------|-----------------|
| Typography | 16px / 24px / weight 600 (title-lg) | `text-base leading-6` + `style={{ fontWeight: 600 }}` |
| Padding | 8px | `p-2` |
| Margin | 8px | `m-2` |
| Color | `var(--text-primary)` (#353E5A) | `text-[var(--text-primary)]` |

The title displays the **parent item's label** (e.g., "Home", "Leads").

### Key Differences from List Mode

| Aspect | List Mode (`openingType="list"`) | Popover Mode (`openingType="popover"`) |
|--------|----------------------------------|---------------------------------------|
| Children render | Inline, below the parent | In a floating popover to the RIGHT |
| Branch lines | Yes — L-shaped connectors | **No** — children have no branch lines |
| Children indent | 32px left padding | None — children sit flat inside popover |
| Expand/collapse | Animated height toggle | Popover open/close |
| Title | None | Parent item label as `title-lg` header inside popover |
| Trigger | Click toggles inline expand | Click opens popover, hover supported for side nav collapsed state |
| Child `isChild` | true | **false** — children behave as top-level items inside popover |

### Implementation Pattern — Popover List

```jsx
{/* Parent item that opens a popover */}
function PopoverNavItem({ item, children, active }) {
  const [isOpen, setIsOpen] = useState(false);

  return (
    <div className="relative">
      {/* Trigger — the nav item itself */}
      <div className="py-0.5">
        <button
          className={`
            flex items-center gap-2 w-full
            py-2 px-3 rounded
            text-[var(--text-primary)] text-sm leading-5 font-medium
            cursor-pointer outline-none border-none
            transition-colors duration-150 ease-out
            ${active
              ? 'bg-[var(--bg-brand-primary)] hover:bg-[var(--bg-brand-primary_alt)]'
              : 'bg-transparent hover:bg-[var(--bg-secondary)]'
            }
          `}
          onClick={() => setIsOpen(!isOpen)}
        >
          <item.icon className={`w-6 h-6 shrink-0 ${active ? 'text-[var(--fg-brand-primary)]' : ''}`} />
          <span className="truncate">{item.label}</span>
          <ChevronRight className={`w-4 h-4 shrink-0 ml-auto transition-transform duration-300 ease-in ${isOpen ? 'rotate-90' : ''}`} />
        </button>
      </div>

      {/* Popover — floats to the RIGHT */}
      {isOpen && (
        <div
          className="
            absolute left-full top-0 ml-2
            w-[240px] max-h-[440px] overflow-y-auto
            bg-[var(--bg-primary)] border border-[var(--border-secondary)] rounded-lg
            shadow-2xl z-50
            p-2
          "
        >
          {/* Arrow pointing left toward parent */}
          <div className="
            absolute -left-[6px] top-4
            w-2.5 h-2.5
            bg-[var(--bg-primary)] border-l border-b border-[var(--border-secondary)]
            rotate-45
          " />

          {/* Popover title — parent item's label */}
          <h6
            className="text-[var(--text-primary)] p-2 m-2"
            style={{ fontSize: '16px', lineHeight: '24px', fontWeight: 600 }}
          >
            {item.label}
          </h6>

          {/* Child items — rendered as flat nav items (no branches, no indent) */}
          <div className="flex flex-col gap-0">
            {children.map(child => (
              <div key={child.id} className="py-0.5">
                <button className="
                  flex items-center gap-2 w-full
                  py-2 px-3 rounded
                  bg-transparent text-[var(--text-primary)]
                  text-sm leading-5 font-medium
                  cursor-pointer outline-none border-none
                  hover:bg-[var(--bg-secondary)]
                  transition-colors duration-150 ease-out
                ">
                  {child.icon && <child.icon className="w-6 h-6 shrink-0" />}
                  <span className="truncate">{child.label}</span>
                </button>
              </div>
            ))}
          </div>
        </div>
      )}
    </div>
  );
}
```

---

## Drag Placeholder

| Property | Value | Tailwind |
|----------|-------|----------|
| Height | 32px | `h-8` |
| Width | 100% | `w-full` |
| Background | `var(--bg-secondary_hover)` (#E8ECF1) | `bg-[var(--bg-secondary_hover)]` |
| Border radius | 4px | `rounded` |

---

## Implementation Pattern for Lovable

```jsx
{/* === Navigation List === */}
<nav className="flex flex-col gap-0">

  {/* Group Title */}
  <div className="py-1.5 px-3">
    <span
      className="text-sm leading-5 text-[var(--text-primary)]"
      style={{ fontWeight: 600 }}
    >
      Main Navigation
    </span>
  </div>

  {/* Active item */}
  <div className="py-0.5">
    <button className="
      flex items-center gap-2 w-full
      py-2 px-3 rounded
      bg-[var(--bg-brand-primary)] text-[var(--text-primary)]
      cursor-pointer outline-none
      hover:bg-[var(--bg-brand-primary_alt)]
      focus-visible:bg-[var(--bg-brand-primary_alt)]
      transition-colors duration-150 ease-out
    ">
      <Home className="w-6 h-6 text-[var(--fg-brand-primary)] shrink-0" />
      <span className="text-sm leading-5 font-medium truncate">Dashboard</span>
    </button>
  </div>

  {/* Default item */}
  <div className="py-0.5">
    <button className="
      flex items-center gap-2 w-full
      py-2 px-3 rounded
      bg-transparent text-[var(--text-primary)]
      cursor-pointer outline-none
      hover:bg-[var(--bg-secondary)]
      focus-visible:bg-[var(--bg-secondary)]
      transition-colors duration-150 ease-out
    ">
      <BarChart className="w-6 h-6 shrink-0" />
      <span className="text-sm leading-5 font-medium truncate">Analytics</span>
    </button>
  </div>

  {/* Expandable item — expanded */}
  <div className="py-0.5">
    <button
      className="
        flex items-center gap-2 w-full
        py-2 px-3 rounded
        bg-transparent text-[var(--text-primary)]
        cursor-pointer outline-none
        hover:bg-[var(--bg-secondary)]
        transition-colors duration-150 ease-out
      "
      onClick={() => toggleSettings()}
    >
      <ChevronRight className="w-6 h-6 shrink-0 rotate-90 transition-transform duration-300 ease-in" />
      <span className="text-sm leading-5 font-medium truncate">Settings</span>
    </button>

    {/* Nested children */}
    <div className="relative pl-8">
      {/* Vertical parent line */}
      <div className="absolute left-[22px] bottom-0 w-px h-full border-l border-[var(--border-secondary)]" />

      {/* Child item */}
      <div className="py-0.5 relative">
        {/* Branch connector */}
        <div className="absolute -left-2.5 top-0 w-2.5 h-1/2 border-l border-b border-[var(--border-secondary)] rounded-bl" />
        <button className="
          flex items-center gap-2 w-full
          py-2 px-3 rounded
          bg-transparent text-[var(--text-primary)]
          cursor-pointer outline-none
          hover:bg-[var(--bg-secondary)]
          transition-colors duration-150 ease-out
        ">
          <span className="text-sm leading-5 font-medium truncate">General</span>
        </button>
      </div>

      {/* Last child item */}
      <div className="py-0.5 relative">
        <div className="absolute -left-2.5 top-0 w-2.5 h-1/2 border-l border-b border-[var(--border-secondary)] rounded-bl" />
        {/* Cover parent line below last child */}
        <div className="absolute -left-3 top-0 w-1 h-full bg-[var(--bg-primary)]" />
        <button className="
          flex items-center gap-2 w-full
          py-2 px-3 rounded
          bg-transparent text-[var(--text-primary)]
          cursor-pointer outline-none
          hover:bg-[var(--bg-secondary)]
          transition-colors duration-150 ease-out
        ">
          <span className="text-sm leading-5 font-medium truncate">Security</span>
        </button>
      </div>
    </div>
  </div>

  {/* Item with supporting text */}
  <div className="py-0.5">
    <button className="
      flex items-center gap-2 w-full
      py-2 px-3 rounded
      bg-transparent text-[var(--text-primary)]
      cursor-pointer outline-none
      hover:bg-[var(--bg-secondary)]
      transition-colors duration-150 ease-out
    ">
      <User className="w-6 h-6 shrink-0" />
      <div>
        <span className="text-sm leading-5 font-medium block">Profile</span>
        <span className="text-xs leading-[18px] font-normal block">Manage your account</span>
      </div>
    </button>
  </div>
</nav>
```

### Key Implementation Rules

1. **Active item background is `var(--bg-brand-primary)`** (#FDF1EF) — NOT white or gray
2. **Active item icon turns `var(--fg-brand-primary)`** (#FF725C) — text stays `var(--text-primary)` (#353E5A)
3. **Default hover is `var(--bg-secondary)`** (#F1F4F7), active hover is `var(--bg-brand-primary_alt)`** (#FDE7E3)
4. **Item border-radius is 4px** (`rounded`) — NOT 6px or 8px
5. **MD size uses 8px/12px padding** (py-2 px-3), SM/XS use 6px/12px (py-1.5 px-3)
6. **Gap between icon and text is 8px** (`gap-2`)
7. **Wrapper padding depends on context** — standalone nav lists use `py-0.5 px-1` (MD). **Inside a side navigation: NO wrapper at all** — buttons sit directly in the container with zero gap
8. **Nested children are indented 32px** (`pl-8`) with L-shaped `var(--border-secondary)` (#CCD6DC) branch lines
9. **Expandable chevron rotates 90°** clockwise when expanded (300ms ease-in)
10. **Last nested child covers the parent vertical line** below it with a white strip
11. **Vertical parent line** runs at left: 22px for the full height of the children container
12. **Group title uses 14px/20px/600** — heavier than items (which use 500)
13. **NO extra gap between items** — The nav container uses `flex flex-col gap-0`. Do NOT add `gap-*`, `space-y-*`, or extra margins between items. Inside side navigation, there is NO wrapper and NO spacing between items at all — buttons sit flush.
17. **Inside side navigation = NO wrapper at all** — When navigation list items are used inside a side navigation container, do NOT wrap them in a `<div>`. No `py-0.5`, no `px-1`. The `<button>` is a direct child of the nav container. The container's `12px` padding + button's `py-2 px-3` provide all spacing. Items sit flush with zero gap.
14. **Three opening types** — Navigation list items support three behaviors: **(a) Default** — simple clickable item with no children, **(b) Navigation Title** (`isNavigationTitle`) — expandable parent with chevron, children expand inline with branch lines (list mode), **(c) Popover** (`openingType="popover"`) — children open in a floating 240px popover to the RIGHT with a title header, no branch lines.
15. **Popover children are flat** — Inside a popover, child items render as regular nav items with NO branch lines, NO indent, and `isChild=false`. They sit flat inside the popover panel.
16. **Popover has a title header** — The popover displays the parent item's label as a `title-lg` (16px/24px/600) header with 8px padding and 8px margin at the top.
