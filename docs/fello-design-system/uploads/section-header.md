# Section Header Component — Strict Specification

> **⚠️ This is the PAGE TITLE component** (white background, inside content cards). It is NOT the app shell top-header (dark plum/teal background). For the app shell header bar, see `top-header.md`.

> **TOKEN RULE:** Every color in this spec is expressed as a CSS variable token. In Tailwind classes use `bg-[var(--token)]` / `text-[var(--token)]` / `border-[var(--token)]`. In inline styles use `'var(--token)'`. In prose and tables, show `var(--token)` (#hex) for reference.

### ⛔ USAGE RULE — Page Title Only

**Use this component ONLY for the single main page title** — the biggest heading at the top of the page (e.g., "Settings", "Dashboard", "Contacts"). There should be only ONE Section Header per page.

**Do NOT use this component for:**
- ❌ Sub-section headings within a page (e.g., "Email Address", "Change Password", "Regional Preferences")
- ❌ Card titles or form group titles
- ❌ Any heading that is not the main page title

**All other headings are free text** and must follow global guidelines: max 18px (`text-lg`), section headings 16px (`text-base`), sub-sections 14px (`text-sm`).

---

## Section Header Anatomy

A horizontal bar with a title and optional badge on the left, and action buttons on the right:

```
┌─────────────────────────────────────────────────────────┐
│  Section Title  [Badge]              [Edit ▾] [Actions ▾]│
└─────────────────────────────────────────────────────────┘
  ─────────────────────────────────────────────────────────  ← optional border-bottom
```

- Title is always required (heading-md typography)
- Badge sits next to the title (optional, medium size only)
- Action buttons align to the right (optional, medium size only)
- Optional bottom border separates from content below

---

## Container

| Property | Value | Tailwind |
|----------|-------|----------|
| Display | Block | `block` |
| Color | `var(--text-primary)` (#353E5A) | `text-[var(--text-primary)]` |

### With Border (Default: `showBorder = true`)

| Property | Value | Tailwind |
|----------|-------|----------|
| Padding bottom | 16px (spacing-4) | `pb-4` |
| Border bottom | 1px solid `var(--border-tertiary)` (#E8ECF1) | `border-b border-[var(--border-tertiary)]` |

### Without Border

| Property | Value | Tailwind |
|----------|-------|----------|
| Padding bottom | 0 | — |
| Border bottom | None | — |

---

## Content Layout

| Property | Value | Tailwind |
|----------|-------|----------|
| Display | Flex row | `flex` |
| Gap | 12px | `gap-3` |
| Justify | Space-between | `justify-between` |
| Width | 100% | `w-full` |

---

## Left Section (Title + Badge)

| Property | Value | Tailwind |
|----------|-------|----------|
| Display | Flex row, centered | `flex items-center` |
| Gap | 12px | `gap-3` |

### Title

| Property | Value | Tailwind |
|----------|-------|----------|
| Typography | 24px / 32px / weight 600 (heading-md) | `text-2xl leading-8` + `style={{ fontWeight: 600 }}` |
| Color | `var(--text-primary)` (#353E5A) | `text-[var(--text-primary)]` |

### Badge
- Only **medium size** badges are allowed
- Refer to the Badge component specification for MD badge styling

---

## Right Section (Action Buttons)

| Property | Value | Tailwind |
|----------|-------|----------|
| Display | Flex row, centered | `flex items-center` |
| Gap | 16px | `gap-4` |

### Buttons
- Only **medium (MD) size** buttons are allowed
- All button variants can be used (primary, secondary, hyperlink, etc.)
- Refer to the Button component specification for MD button styling

---

## Options

| Property | Default | Description |
|----------|---------|-------------|
| sectionTitle | — (required) | The heading text |
| showBorder | true | Show bottom border separator |

---

## Implementation Pattern for Lovable

```jsx
{/* === Section Header — With border, badge, and action buttons === */}
<div className="block text-[var(--text-primary)] pb-4 border-b border-[var(--border-tertiary)]">
  <div className="flex gap-3 justify-between w-full">
    {/* Left: Title + Badge */}
    <div className="flex items-center gap-3">
      <h2
        className="text-2xl leading-8"
        style={{ fontWeight: 600 }}
      >
        Section Title
      </h2>
      {/* Badge (MD size) — refer to Badge spec */}
      <span className="
        inline-flex items-center
        h-6 px-2 rounded
        bg-[var(--bg-success-primary)] text-[var(--text-success-primary)]
        text-xs leading-[18px]
      "
        style={{ fontWeight: 500 }}
      >
        Active
      </span>
    </div>

    {/* Right: Action buttons */}
    <div className="flex items-center gap-4">
      <button className="
        inline-flex items-center gap-1
        h-9 py-2.5 px-3.5 rounded-lg
        bg-transparent text-[var(--text-brand-secondary)]
        text-sm leading-5
        hover:bg-[var(--bg-brand-secondary)]
        transition-all duration-150 ease-out
      "
        style={{ fontWeight: 600 }}
      >
        Edit
        <Pencil className="w-5 h-5" />
      </button>
      <button className="
        inline-flex items-center gap-1
        h-9 py-2.5 px-3.5 rounded-lg
        bg-transparent text-[var(--text-brand-secondary)]
        text-sm leading-5
        hover:bg-[var(--bg-brand-secondary)]
        transition-all duration-150 ease-out
      "
        style={{ fontWeight: 600 }}
      >
        Actions
        <ChevronDown className="w-5 h-5" />
      </button>
    </div>
  </div>
</div>


{/* === Section Header — Without border, no badge === */}
<div className="block text-[var(--text-primary)]">
  <div className="flex gap-3 justify-between w-full">
    <div className="flex items-center gap-3">
      <h2
        className="text-2xl leading-8"
        style={{ fontWeight: 600 }}
      >
        Dashboard
      </h2>
    </div>

    <div className="flex items-center gap-4">
      <button className="
        h-9 py-2.5 px-3.5 rounded-lg
        bg-[var(--bg-brand-solid)] text-white
        text-sm leading-5
        hover:bg-[var(--bg-brand-solid_hover)]
        transition-all duration-150 ease-out
      "
        style={{ fontWeight: 600 }}
      >
        Add New
      </button>
    </div>
  </div>
</div>


{/* === Section Header — Title only, with border === */}
<div className="block text-[var(--text-primary)] pb-4 border-b border-[var(--border-tertiary)]">
  <div className="flex gap-3 justify-between w-full">
    <div className="flex items-center gap-3">
      <h2
        className="text-2xl leading-8"
        style={{ fontWeight: 600 }}
      >
        Settings
      </h2>
    </div>
  </div>
</div>
```

### Key Implementation Rules

1. **Title uses heading-md** — 24px / 32px / 600 weight, uses `text-2xl leading-8` with inline `style={{ fontWeight: 600 }}`
2. **Bottom border is `var(--border-tertiary)` (#E8ECF1)** — only shown when `showBorder` is true (default)
3. **Bottom padding is 16px** (`pb-4`) — only present when border is shown
4. **Left section gap is 12px** (`gap-3`) between title and badge
5. **Right section gap is 16px** (`gap-4`) between action buttons
6. **Only MD size buttons are allowed** — 36px height, 8px border-radius
7. **Only MD size badges are allowed** — refer to Badge component spec
8. **Overall layout uses `justify-between`** — title left, actions right
9. **Text color inherits `var(--text-primary)` (#353E5A)** for the entire section
10. **No background** — section header is transparent, sits on page background
11. **All button variants work** — primary, secondary, hyperlink, outline, etc.
12. **Content projection**: badge appears next to title, buttons appear in right section
