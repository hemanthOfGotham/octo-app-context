# Empty State Component — Strict Specification

When building any empty state, no-data view, zero-state illustration, or blank state placeholder, follow these specifications exactly. Do NOT use default Tailwind empty state patterns or any other empty state system.

> **TOKEN RULE:** Every color in this spec is expressed as a CSS variable token. In Tailwind classes use `bg-[var(--token)]` / `text-[var(--token)]` / `border-[var(--token)]`. In inline styles use `'var(--token)'`. In prose and tables, show `var(--token)` (#hex) for reference.

---

## Empty State Anatomy

A vertically centered card-like container with optional image, required title, optional description, and optional action buttons:

```
┌────────────────────────────────────────────┐
│                                            │
│            [Illustration / Image]          │  ← Optional
│                                            │
│              Title Goes Here               │  ← Required
│       Description text goes here           │  ← Optional
│                                            │
│         [Primary]   [Secondary]            │  ← Optional action buttons
│                                            │
└────────────────────────────────────────────┘
```

- Layout: Flex column, align-items: center
- Background: `var(--bg-primary)` (#FFFFFF)
- All text is centered
- Action buttons are centered with wrapping

---

## Sizes

There are 3 empty state sizes:

### Small (SM)
| Property | Value | Tailwind |
|----------|-------|----------|
| Padding | 24px all sides | `p-6` |
| Image height | 100px | `h-[100px]` |
| Image margin-bottom | 16px | `mb-4` |
| Title typography | 14px / 20px / weight 600 (title-md) | `text-sm leading-5` + `style={{ fontWeight: 600 }}` |
| Description typography | 12px / 18px / weight 400 (paragraph-sm) | `text-xs leading-[18px] font-normal` |
| Actions margin-top | 16px | `mt-4` |

### Medium (MD) — Default
| Property | Value | Tailwind |
|----------|-------|----------|
| Padding | 32px top/bottom, 24px left/right | `py-8 px-6` |
| Image height | 120px | `h-[120px]` |
| Image margin-bottom | 24px | `mb-6` |
| Title typography | 18px / 28px / weight 600 (title-xl) | `text-lg leading-7` + `style={{ fontWeight: 600 }}` |
| Description typography | 14px / 20px / weight 400 (paragraph-md) | `text-sm leading-5 font-normal` |
| Actions margin-top | 20px | `mt-5` |

### Large (LG)
| Property | Value | Tailwind |
|----------|-------|----------|
| Padding | 48px top/bottom, 24px left/right | `py-12 px-6` |
| Image height | 160px | `h-[160px]` |
| Image margin-bottom | 32px | `mb-8` |
| Title typography | 24px / 32px / weight 600 (heading-md) | `text-2xl leading-8` + `style={{ fontWeight: 600 }}` |
| Description typography | 14px / 20px / weight 400 (paragraph-md) | `text-sm leading-5 font-normal` |
| Actions margin-top | 24px | `mt-6` |

---

## Colors

| Element | Color | Hex | Tailwind |
|---------|-------|-----|----------|
| Background | bg-primary | `var(--bg-primary)` (#FFFFFF) | `bg-white` |
| Title | text-primary | `#353E5A` | `text-[var(--text-primary)]` |
| Description | text-secondary | `#495883` | `text-[var(--text-secondary)]` |

---

## Image / Illustration

| Property | Value | Tailwind |
|----------|-------|----------|
| Display | Flex, centered | `flex items-center justify-center` |
| Object fit | Contain | `object-contain` |
| Max height | 100% of allocated height | `max-h-full` |
| Width | Auto | `w-auto` |

---

## Action Buttons

| Property | Value | Tailwind |
|----------|-------|----------|
| Layout | Flex, centered, wrapping | `flex items-center justify-center flex-wrap` |
| Gap | 12px | `gap-3` |
| Visibility | Hidden if no buttons present | Conditional render |

---

## Description Spacing

| Property | Value | Tailwind |
|----------|-------|----------|
| Margin top from title | 4px | `mt-1` |

---

## Implementation Pattern for Lovable

```jsx
{/* === Empty State — Medium (default) === */}
<div className="flex flex-col items-center bg-white py-8 px-6 text-center">
  {/* Illustration */}
  <div className="flex items-center justify-center h-[120px] mb-6">
    <img
      src="/illustrations/empty-inbox.svg"
      alt="No data"
      className="max-h-full w-auto object-contain"
    />
  </div>

  {/* Title — REQUIRED */}
  <h3
    className="text-lg leading-7 text-[var(--text-primary)]"
    style={{ fontWeight: 600 }}
  >
    No items found
  </h3>

  {/* Description — optional */}
  <p className="mt-1 text-sm leading-5 font-normal text-[var(--text-secondary)]">
    Try adjusting your filters or adding new items to get started.
  </p>

  {/* Actions — optional */}
  <div className="mt-5 flex items-center justify-center flex-wrap gap-3">
    <button className="...primary-button-styles...">
      Add Item
    </button>
    <button className="...secondary-button-styles...">
      Learn More
    </button>
  </div>
</div>


{/* === Empty State — Small === */}
<div className="flex flex-col items-center bg-white p-6 text-center">
  <div className="flex items-center justify-center h-[100px] mb-4">
    <img
      src="/illustrations/empty-search.svg"
      alt="No results"
      className="max-h-full w-auto object-contain"
    />
  </div>

  <h3
    className="text-sm leading-5 text-[var(--text-primary)]"
    style={{ fontWeight: 600 }}
  >
    No results found
  </h3>

  <p className="mt-1 text-xs leading-[18px] font-normal text-[var(--text-secondary)]">
    Try different search terms
  </p>

  <div className="mt-4 flex items-center justify-center flex-wrap gap-3">
    <button className="...button-styles...">Clear filters</button>
  </div>
</div>


{/* === Empty State — Large === */}
<div className="flex flex-col items-center bg-white py-12 px-6 text-center">
  <div className="flex items-center justify-center h-[160px] mb-8">
    <img
      src="/illustrations/welcome.svg"
      alt="Welcome"
      className="max-h-full w-auto object-contain"
    />
  </div>

  <h3
    className="text-2xl leading-8 text-[var(--text-primary)]"
    style={{ fontWeight: 600 }}
  >
    Welcome to your dashboard
  </h3>

  <p className="mt-1 text-sm leading-5 font-normal text-[var(--text-secondary)]">
    Get started by creating your first project or exploring the templates.
  </p>

  <div className="mt-6 flex items-center justify-center flex-wrap gap-3">
    <button className="...primary-button-styles...">Create Project</button>
    <button className="...secondary-button-styles...">Browse Templates</button>
  </div>
</div>


{/* === Empty State — No image, no actions (minimal) === */}
<div className="flex flex-col items-center bg-white py-8 px-6 text-center">
  <h3
    className="text-lg leading-7 text-[var(--text-primary)]"
    style={{ fontWeight: 600 }}
  >
    Nothing here yet
  </h3>
  <p className="mt-1 text-sm leading-5 font-normal text-[var(--text-secondary)]">
    Items will appear here once they are created.
  </p>
</div>
```

### Size Quick Reference

| Size | Padding | Image Height | Image MB | Title | Description | Actions MT |
|------|---------|-------------|---------|-------|-------------|-----------|
| SM | 24px all | 100px | 16px | 14px/20px/600 | 12px/18px/400 | 16px |
| MD | 32px 24px | 120px | 24px | 18px/28px/600 | 14px/20px/400 | 20px |
| LG | 48px 24px | 160px | 32px | 24px/32px/600 | 14px/20px/400 | 24px |

### Key Implementation Rules

1. **Title is required**, image/description/actions are all optional
2. **All content is centered** — both horizontally and vertically aligned
3. **Title uses weight 600** — use inline `style={{ fontWeight: 600 }}` for precision
4. **Description color is `var(--text-secondary)` (#495883), NOT text-primary
5. **Description has 4px margin-top** from title (`mt-1`)
6. **Action buttons have 12px gap** (`gap-3`) and wrap on narrow containers
7. **Image uses `object-contain`** to preserve aspect ratio within the height constraint
8. **Actions section is hidden** if no buttons are present — do not render the wrapper
9. **Background is always white** (`var(--bg-primary)` #FFFFFF)
10. **SM title uses title-md** (14px), **MD uses title-xl** (18px), **LG uses heading-md** (24px)
