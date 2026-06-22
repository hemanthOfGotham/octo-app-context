# Kanban Board Component — Strict Specification

When building any kanban board, task board, column-based drag-and-drop layout, or project board, follow these specifications exactly. Do NOT use default Tailwind board layouts, shadcn kanban, or any other kanban system.

> **TOKEN RULE:** Every color in this spec is expressed as a CSS variable token. In Tailwind classes use `bg-[var(--token)]` / `text-[var(--token)]`. In inline styles use `'var(--token)'`. In prose and tables, show `var(--token)` (#hex) for reference.

---

## Kanban Board Anatomy

A horizontal row of columns, each containing a header and a scrollable list of cards:

```
┌──────────────────────────────────────────────────────────────────────┐
│  ┌──────────────┐ ┌──────────────┐ ┌──────────────┐                │
│  │ To Do    (4) │ │ In Progress  │ │ Done     (8) │  ← Scrollable →│
│  ├──────────────┤ ├──────────────┤ ├──────────────┤                │
│  │  ┌────────┐  │ │  ┌────────┐  │ │  ┌────────┐  │                │
│  │  │ Card 1 │  │ │  │ Card 1 │  │ │  │ Card 1 │  │                │
│  │  └────────┘  │ │  └────────┘  │ │  └────────┘  │                │
│  │  ┌────────┐  │ │  ┌────────┐  │ │  ┌────────┐  │                │
│  │  │ Card 2 │  │ │  │ Card 2 │  │ │  │ Card 2 │  │                │
│  │  └────────┘  │ │  └────────┘  │ │  └────────┘  │                │
│  │     ↕ scroll │ │              │ │     ↕ scroll │                │
│  └──────────────┘ └──────────────┘ └──────────────┘                │
└──────────────────────────────────────────────────────────────────────┘
```

- Board: Horizontal flex container with horizontal scroll
- Columns: Vertical flex containers with vertical scroll on content
- Cards: Content inside columns, spaced with gaps

---

## Board (Container) Specs

| Property | Value | Tailwind |
|----------|-------|----------|
| Display | Flex (row) | `flex` |
| Gap | 16px | `gap-4` |
| Width | 100% | `w-full` |
| Overflow X | Auto (horizontal scroll) | `overflow-x-auto` |

---

## Column Specs

| Property | Value | Tailwind |
|----------|-------|----------|
| Display | Flex column | `flex flex-col` |
| Background | `var(--bg-secondary)` (#F1F4F7) | `bg-[var(--bg-secondary)]` |
| Border | 1px solid `var(--border-secondary)` (#CCD6DC) | `border border-[var(--border-secondary)]` |
| Border radius | 8px | `rounded-lg` |
| Padding | 8px top/bottom, 0 left/right | `py-2 px-0` |
| Min width | Set a practical minimum (e.g., 280px) | `min-w-[280px]` |

---

## Column Header

| Property | Value | Tailwind |
|----------|-------|----------|
| Typography | 16px / 24px / weight 500 (caption-lg) | `text-base leading-6 font-medium` |
| Color | `var(--text-primary)` (#353E5A) | `text-[var(--text-primary)]` |
| Padding | 8px vertical, 20px horizontal | `py-2 px-5` |

---

## Column Content (Card Area)

| Property | Value | Tailwind |
|----------|-------|----------|
| Display | Flex column | `flex flex-col` |
| Gap | 6px | `gap-1.5` |
| Flex | 1 (fills remaining vertical space) | `flex-1` |
| Overflow Y | Auto (vertical scroll) | `overflow-y-auto` |
| Padding | 0 vertical, 8px horizontal | `px-2` |

---

## Card Specs

Cards inside columns follow the standard card system:

| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `var(--bg-primary)` (#FFFFFF) | `bg-[var(--bg-primary)]` |
| Border | 1px solid `var(--border-secondary)` (#CCD6DC) | `border border-[var(--border-secondary)]` |
| Border radius | 8px | `rounded-lg` |
| Padding | 20px (card default) or custom | `p-5` |
| Cursor | Grab (when draggable) | `cursor-grab` |

---

## Drag & Drop Behavior

### Drag Placeholder
When a card is being dragged, a placeholder appears where it will drop:

| Property | Value | Tailwind |
|----------|-------|----------|
| Min width | 150px | `min-w-[150px]` |
| Min height | 100px | `min-h-[100px]` |
| Background | `var(--bg-tertiary)` (#E8ECF1) | `bg-[var(--bg-tertiary)]` |
| Border | 1px solid `var(--border-tertiary)` (#E8ECF1) | `border border-[var(--border-tertiary)]` |
| Border radius | 8px | `rounded-lg` |

### Drag Animation
| Property | Value |
|----------|-------|
| Sorting transition | `transform 250ms cubic-bezier(0, 0, 0.2, 1)` |
| Drop animation | `transform 300ms cubic-bezier(0, 0, 0.2, 1)` |

### Drag Operations
- **Within same column**: Cards reorder vertically
- **Between columns**: Cards transfer from one column to another
- **Dragging cursor**: `cursor-grabbing` while actively dragging

---

## Scroll Behavior

### Board Level (Horizontal)
- Overflow X: Auto
- Columns scroll horizontally when they exceed the board width
- Board takes 100% of its container's width

### Column Level (Vertical)
- Overflow Y: Auto
- Each column scrolls independently when cards exceed the available height
- Column content area uses `flex-1` to fill available space

---

## Colors Summary

| Element | Token | Hex |
|---------|-------|-----|
| Board background | Transparent (inherits) | — |
| Column background | `var(--bg-secondary)` | (#F1F4F7) |
| Column border | `var(--border-secondary)` | (#CCD6DC) |
| Column header text | `var(--text-primary)` | (#353E5A) |
| Card background | `var(--bg-primary)` | (#FFFFFF) |
| Card border | `var(--border-secondary)` | (#CCD6DC) |
| Drag placeholder bg | `var(--bg-tertiary)` | (#E8ECF1) |
| Drag placeholder border | `var(--border-tertiary)` | (#E8ECF1) |

---

## Implementation Pattern for Lovable

```jsx
{/* === Kanban Board === */}
<div className="flex gap-4 w-full overflow-x-auto pb-2">

  {/* Column 1 — To Do */}
  <div className="
    flex flex-col
    min-w-[280px] max-h-[600px]
    bg-[var(--bg-secondary)]
    border border-[var(--border-secondary)]
    rounded-lg
    py-2
  ">
    {/* Column header */}
    <div className="py-2 px-5">
      <h3 className="text-base leading-6 font-medium text-[var(--text-primary)]">
        To Do
      </h3>
    </div>

    {/* Column content — scrollable card area */}
    <div className="flex flex-col gap-1.5 flex-1 overflow-y-auto px-2">

      {/* Card */}
      <div className="
        bg-white
        border border-[var(--border-secondary)]
        rounded-lg
        p-5
        cursor-grab active:cursor-grabbing
      ">
        <h4
          className="text-sm leading-5 text-[var(--text-primary)]"
          style={{ fontWeight: 600 }}
        >
          Design homepage mockup
        </h4>
        <p className="mt-1 text-xs leading-[18px] font-normal text-[var(--text-secondary)]">
          Create wireframes for the new landing page
        </p>
      </div>

      {/* Card */}
      <div className="
        bg-white border border-[var(--border-secondary)] rounded-lg p-5
        cursor-grab active:cursor-grabbing
      ">
        <h4
          className="text-sm leading-5 text-[var(--text-primary)]"
          style={{ fontWeight: 600 }}
        >
          Set up CI/CD pipeline
        </h4>
        <p className="mt-1 text-xs leading-[18px] font-normal text-[var(--text-secondary)]">
          Configure automated builds and deployments
        </p>
      </div>

      {/* Drag placeholder (shown during drag) */}
      {/* <div className="min-w-[150px] min-h-[100px] bg-[var(--bg-tertiary)] border border-[var(--border-tertiary)] rounded-lg" /> */}

    </div>
  </div>

  {/* Column 2 — In Progress */}
  <div className="
    flex flex-col
    min-w-[280px] max-h-[600px]
    bg-[var(--bg-secondary)]
    border border-[var(--border-secondary)]
    rounded-lg
    py-2
  ">
    <div className="py-2 px-5">
      <h3 className="text-base leading-6 font-medium text-[var(--text-primary)]">
        In Progress
      </h3>
    </div>

    <div className="flex flex-col gap-1.5 flex-1 overflow-y-auto px-2">
      <div className="
        bg-white border border-[var(--border-secondary)] rounded-lg p-5
        cursor-grab active:cursor-grabbing
      ">
        <h4
          className="text-sm leading-5 text-[var(--text-primary)]"
          style={{ fontWeight: 600 }}
        >
          Implement authentication
        </h4>
        <p className="mt-1 text-xs leading-[18px] font-normal text-[var(--text-secondary)]">
          Add JWT-based login flow
        </p>
      </div>
    </div>
  </div>

  {/* Column 3 — Done */}
  <div className="
    flex flex-col
    min-w-[280px] max-h-[600px]
    bg-[var(--bg-secondary)]
    border border-[var(--border-secondary)]
    rounded-lg
    py-2
  ">
    <div className="py-2 px-5">
      <h3 className="text-base leading-6 font-medium text-[var(--text-primary)]">
        Done
      </h3>
    </div>

    <div className="flex flex-col gap-1.5 flex-1 overflow-y-auto px-2">
      <div className="
        bg-white border border-[var(--border-secondary)] rounded-lg p-5
        cursor-grab active:cursor-grabbing
      ">
        <h4
          className="text-sm leading-5 text-[var(--text-primary)]"
          style={{ fontWeight: 600 }}
        >
          Project kickoff meeting
        </h4>
        <p className="mt-1 text-xs leading-[18px] font-normal text-[var(--text-secondary)]">
          Completed initial planning session
        </p>
      </div>
    </div>
  </div>

</div>
```

### Key Implementation Rules

1. **Board is horizontal flex** with `gap-4` (16px) between columns and `overflow-x-auto`
2. **Column background is `var(--bg-secondary)` (#F1F4F7)** with `var(--border-secondary)` (#CCD6DC) border and 8px radius
3. **Column padding is 8px vertical only** (`py-2 px-0`) — header and content have their own horizontal padding
4. **Column header uses 16px/24px/500 typography** with 20px horizontal padding (`px-5`)
5. **Card area uses 8px horizontal padding** (`px-2`) with 6px gap between cards
6. **Cards have white background** with `var(--border-secondary)` (#CCD6DC) border, 8px radius, 20px padding
7. **Cards use `cursor-grab`** and switch to `cursor-grabbing` while actively dragging
8. **Drag placeholder** uses `var(--bg-tertiary)` (#E8ECF1) bg and `var(--border-tertiary)` (#E8ECF1) border, minimum 150x100px
9. **Column content scrolls vertically** (`overflow-y-auto` + `flex-1`)
10. **Board scrolls horizontally** when columns exceed container width
11. **Set `min-width` on columns** (e.g., 280px) to prevent them from collapsing
12. **Set `max-height` on columns** to constrain the board height and enable vertical scroll
13. **Sorting animation is 250ms**, drop animation is 300ms — both use `cubic-bezier(0, 0, 0.2, 1)`
