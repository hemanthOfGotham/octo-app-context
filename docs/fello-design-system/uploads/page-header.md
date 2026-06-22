# Page Header Component — Strict Specification

> **⚠️ This is a CONTENT-AREA header** (white background, inside the content card). It is NOT the app shell top-header (dark plum/teal background). For the app shell header bar, see `top-header.md`.

When building any page header, section header with title and actions, or top-of-page navigation bar, follow these specifications exactly. Do NOT use default Tailwind headers or any other page header system.

> **TOKEN RULE:** Every color in this file is expressed as a CSS variable token. In code use `bg-[var(--bg-token)]` / `text-[var(--text-token)]` / `border-[var(--border-token)]` classes. In inline styles use `'var(--token)'`. The parenthetical hex is for reference only — never use raw hex in production code.

---

## Page Header Anatomy

A horizontal bar at the top of a page/section with a title on the left and action buttons on the right, optionally with a tab row below:

```
┌─────────────────────────────────────────────────────────────────┐
│  [Title ▾]                              [Action1] [Action2]     │  ← Row 1: title + actions
├─────────────────────────────────────────────────────────────────┤
│     Tab 1    Tab 2    Tab 3                                     │  ← Row 2 (optional): tabs
└─────────────────────────────────────────────────────────────────┘
```

- Title can be a plain text label or a clickable dropdown button
- Action buttons sit on the right side (small size only)
- Optional bottom row for tabs (primary type only)
- Bottom border separates the header from page content

---

## Container

| Property | Value | Tailwind |
|----------|-------|----------|
| Display | Flex column | `flex flex-col` |
| Background | `var(--bg-primary)` (#FFFFFF) | `bg-[var(--bg-primary)]` |
| Border bottom | 1px solid `var(--border-tertiary)` (#E8ECF1) | `border-b border-[var(--border-tertiary)]` |
| Width | 100% | `w-full` |

---

## Row 1 — Title + Actions

| Property | Value | Tailwind |
|----------|-------|----------|
| Display | Flex row, space-between, centered | `flex items-center justify-between` |
| Padding (no tabs) | 12px top, 24px right, 12px bottom, 16px left | `pt-3 pr-6 pb-3 pl-4` |
| Padding (with tabs) | 12px top, 24px right, 6px bottom, 16px left | `pt-3 pr-6 pb-1.5 pl-4` |

### Left Side — Title Area

| Property | Value | Tailwind |
|----------|-------|----------|
| Display | Flex, centered | `flex items-center` |
| Gap | 4px | `gap-1` |

### Right Side — Actions Area

| Property | Value | Tailwind |
|----------|-------|----------|
| Display | Flex, centered | `flex items-center` |
| Gap | 12px | `gap-3` |

---

## Title (Plain Text)

| Property | Value | Tailwind |
|----------|-------|----------|
| Typography | 18px / 28px / weight 600 (title-xl) | `text-lg leading-7` + `style={{ fontWeight: 600 }}` |
| Color | `var(--text-primary)` (#353E5A) | `text-[var(--text-primary)]` |

---

## Title Button (Dropdown Variant)

When the title is clickable (opens a dropdown), it becomes a button:

| Property | Value | Tailwind |
|----------|-------|----------|
| Display | Inline-flex, centered | `inline-flex items-center` |
| Gap | 4px | `gap-1` |
| Typography | 18px / 28px / weight 600 (title-xl) | `text-lg leading-7` + `style={{ fontWeight: 600 }}` |
| Color | `var(--text-primary)` (#353E5A) | `text-[var(--text-primary)]` |
| Border radius | 6px | `rounded-md` |
| Padding | 4px 8px | `py-1 px-2` |
| Background | Transparent | `bg-transparent` |
| Cursor | Pointer | `cursor-pointer` |

### Title Button States

| State | Property | Value | Tailwind |
|-------|----------|-------|----------|
| Hover | Background | `var(--bg-secondary)` (#F1F4F7) | `hover:bg-[var(--bg-secondary)]` |
| Focus | Text decoration | Underline | `focus-visible:underline` |
| Focus | Background | `var(--bg-secondary)` (#F1F4F7) | `focus-visible:bg-[var(--bg-secondary)]` |

### Title Button Chevron

| Property | Value | Tailwind |
|----------|-------|----------|
| Icon | ChevronDown | `ChevronDown` |
| Size | 20px | `w-5 h-5` |
| Color | Inherits text color | — |

---

## Row 2 — Tab Row (Optional)

When tabs are present, they appear in a second row below the title row:

| Property | Value | Tailwind |
|----------|-------|----------|
| Padding left | 16px | `pl-4` |
| Padding right | 24px | `pr-6` |

**Tab type**: Only **primary tabs** are used inside the page header. Refer to the Tab component specification for primary tab styling (40px height, 2px bottom indicator in `var(--border-brand-solid)` (#FF725C)).

---

## Action Buttons

Only **small (SM) size** buttons are used in the page header action area. Refer to the Button component specification for SM button styling.

---

## Implementation Pattern for Lovable

```jsx
{/* === Page Header — With Title + Actions (no tabs) === */}
<div className="flex flex-col w-full bg-white border-b border-[var(--border-tertiary)]">
  {/* Row 1 */}
  <div className="flex items-center justify-between pt-3 pr-6 pb-3 pl-4">
    {/* Title */}
    <div className="flex items-center gap-1">
      <h1
        className="text-lg leading-7 text-[var(--text-primary)]"
        style={{ fontWeight: 600 }}
      >
        Page Title
      </h1>
    </div>

    {/* Actions */}
    <div className="flex items-center gap-3">
      {/* SM outline button */}
      <button className="
        h-8 py-2 px-3 rounded-md
        bg-[var(--bg-secondary)] text-[var(--text-primary)]
        border border-[var(--fg-disabled)]
        text-sm leading-5
        hover:bg-[var(--bg-secondary_hover)]
        transition-all duration-150 ease-out
      "
        style={{ fontWeight: 600 }}
      >
        Cancel
      </button>
      {/* SM primary button */}
      <button className="
        h-8 py-2 px-3 rounded-md
        bg-[var(--bg-brand-solid)] text-white
        text-sm leading-5
        hover:bg-[var(--bg-brand-solid_hover)]
        transition-all duration-150 ease-out
      "
        style={{ fontWeight: 600 }}
      >
        Save
      </button>
    </div>
  </div>
</div>


{/* === Page Header — With Dropdown Title + Tabs === */}
<div className="flex flex-col w-full bg-white border-b border-[var(--border-tertiary)]">
  {/* Row 1 — reduced bottom padding for tabs */}
  <div className="flex items-center justify-between pt-3 pr-6 pb-1.5 pl-4">
    {/* Title Button (dropdown) */}
    <div className="flex items-center gap-1">
      <button className="
        inline-flex items-center gap-1
        py-1 px-2 rounded-md
        bg-transparent text-[var(--text-primary)]
        cursor-pointer outline-none
        hover:bg-[var(--bg-secondary)]
        focus-visible:bg-[var(--bg-secondary)] focus-visible:underline
        transition-colors duration-150 ease-out
      ">
        <span
          className="text-lg leading-7"
          style={{ fontWeight: 600 }}
        >
          Project Alpha
        </span>
        <ChevronDown className="w-5 h-5" />
      </button>
    </div>

    {/* Actions */}
    <div className="flex items-center gap-3">
      <button className="
        h-8 py-2 px-3 rounded-md
        bg-[var(--bg-brand-solid)] text-white
        text-sm leading-5
        hover:bg-[var(--bg-brand-solid_hover)]
        transition-all duration-150 ease-out
      "
        style={{ fontWeight: 600 }}
      >
        New Item
      </button>
    </div>
  </div>

  {/* Row 2 — Tabs (primary type only) */}
  <div className="pl-4 pr-6">
    <div className="flex">
      {/* Active tab */}
      <button className="
        h-10 px-3
        text-sm leading-5 font-medium
        text-[var(--text-primary)]
        border-b-2 border-[var(--border-brand-solid)]
        cursor-pointer
      ">
        Overview
      </button>
      {/* Inactive tab */}
      <button className="
        h-10 px-3
        text-sm leading-5 font-medium
        text-[var(--text-tertiary)]
        border-b-2 border-transparent
        hover:text-[var(--text-primary)] hover:border-[var(--border-secondary)]
        cursor-pointer
      ">
        Settings
      </button>
    </div>
  </div>
</div>


{/* === Page Header — Plain title, no actions === */}
<div className="flex flex-col w-full bg-white border-b border-[var(--border-tertiary)]">
  <div className="flex items-center justify-between pt-3 pr-6 pb-3 pl-4">
    <div className="flex items-center gap-1">
      <h1
        className="text-lg leading-7 text-[var(--text-primary)]"
        style={{ fontWeight: 600 }}
      >
        Dashboard
      </h1>
    </div>
  </div>
</div>
```

### Key Implementation Rules

1. **Title is 18px/28px/600** — uses `text-lg leading-7` with inline `style={{ fontWeight: 600 }}`
2. **Bottom border is `var(--border-tertiary)` (#E8ECF1)** — always present to separate from content
3. **Asymmetric padding**: left is 16px (`pl-4`), right is 24px (`pr-6`)
4. **Bottom padding changes with tabs**: 12px without tabs (`pb-3`), 6px with tabs (`pb-1.5`)
5. **Actions gap is 12px** (`gap-3`) between buttons; title-area gap is 4px (`gap-1`)
6. **Only SM buttons** are used in the action area — 32px height, 6px border-radius
7. **Only primary tabs** are used in the tab row — 40px height with 2px bottom indicator
8. **Title button has 6px radius** (`rounded-md`), 4px/8px padding, hover bg `var(--bg-secondary)` (#F1F4F7)
9. **Title button focus shows underline** in addition to background change
10. **Chevron icon in title button is 20px** (`w-5 h-5`)
11. **Tab row padding matches**: same left (16px) and right (24px) as the title row
12. **Background is always white** — no dark variant in this component
