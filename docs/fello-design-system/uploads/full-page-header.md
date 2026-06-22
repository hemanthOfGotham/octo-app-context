# Full Page Header Component — Strict Specification

> **⚠️ This is a FULL-PAGE/DIALOG header** (white background, used inside modals, slide-outs, or full-screen pages). It is NOT the app shell top-header (dark plum/teal background). For the app shell header bar, see `top-header.md`.

When building any full-page/modal header, dialog top bar, slide-over panel header, or full-screen page header with a close button, follow these specifications exactly. Do NOT use default Tailwind headers or any other header system.

> **TOKEN RULE — MANDATORY:** Every color in this spec is expressed as a CSS custom-property token (with the raw hex shown only for reference in parentheses). When writing code, always use the `var(--token)` form — never hard-code a raw hex value.

**⚠️ IMPORTANT — Use existing component specs for all sub-elements:**
- **Close button** → Highlighted icon-only MD button — see `dialog-close-button.md` for the highlighted variant pattern, but use **MD size (40×40px)** here, NOT SM (32×32px)
- **Action buttons** → See `button.md` for exact button styles (variants, sizes, colors)
- **Badge** → See `badge.md` for exact badge styles
- **Separator** → See `separator.md` for exact separator styles
- **Tabs** → See `tab.md` for exact tab styles

Do NOT invent your own button, badge, separator, or tab styles. Always use the specs listed above.

---

## Full Page Header Anatomy

A horizontal bar at the top of a full-page/modal view with a close button, title, and optional content slots:

```
┌──────────────────────────────────────────────────────────────────────┐
│  [X]  Page Title  [Leading btns] [Leading content]  ···  [Actions] │
├──────────────────────────────────────────────────────────────────────┤
│         [Tab Group or Stepper — indented 56px]                      │  ← Optional Row 2
└──────────────────────────────────────────────────────────────────────┘
```

- Row 1: Close button → Title → Leading buttons → Leading content (icons, separators) → Spacer → Right content (text, icons) → Badge → Action buttons / Tab group
- Row 2 (optional): Tab group or horizontal stepper, indented to align past the close button
- Width: 100%

---

## Two Background Modes

| Mode | Background | Tailwind |
|------|-----------|----------|
| **Light** (default) | `var(--bg-primary)` (#FFFFFF) | `bg-white` |
| **Dark** | `var(--bg-secondary)` (#F1F4F7) | `bg-[var(--bg-secondary)]` |

---

## Container Specs

| Property | Value | Tailwind |
|----------|-------|----------|
| Display | Flex column | `flex flex-col` |
| Width | 100% | `w-full` |
| Padding left | 12px | `pl-3` |
| Padding right | 24px | `pr-6` |

---

## Row 1 — Main Header Row

| Property | Value | Tailwind |
|----------|-------|----------|
| Display | Flex, centered | `flex items-center` |
| Gap | 12px | `gap-3` |
| Padding vertical | 8px | `py-2` |

### Element Order (left to right):

1. **Close Button** — Highlighted icon-only MD button
   - Size: **40×40px** (MD icon-only)
   - Border radius: **8px** (`rounded-lg`)
   - Background: `var(--bg-secondary)` (#F1F4F7)
   - Hover background: `var(--bg-secondary_hover)` (#E8ECF1)
   - Icon: `X` (close), **20px**, color `var(--fg-tertiary)` (#6B748E)
   - No border
   - Shrink: No (`flex-shrink-0`)
   - **Uses the highlighted button variant** — same pattern as dialog close button but MD size

2. **Title**
   - Element: `<h2>` (or `<h1>`)
   - Typography: title-xl — 18px / 28px / weight 600
   - Color: `var(--text-primary)` (#353E5A)
   - Overflow: Ellipsis truncation (`truncate`)
   - Layout: `flex items-center overflow-hidden`
   - Contains optional leading button(s) inside it (e.g., edit icon next to title)

3. **Leading Content** (optional) — after title, before spacer
   - Separators: vertical `1px` line, `var(--border-secondary)` (#CCD6DC), height `20px` — use `separator.md`
   - Icons: standalone icons
   - Text paragraphs: `paragraph-sm` (12px/18px/400)
   - All have `flex-shrink-0`

4. **Spacer**
   - `flex-1` — pushes everything after it to the right edge

5. **Right-Side Content** (optional) — after spacer
   - Text: `paragraph-sm` (12px/18px/400)
   - Icons

6. **Badge** (optional)
   - Use exact badge spec from `badge.md`
   - `flex-shrink-0`

7. **Action Buttons** (optional) — rightmost
   - Use exact button spec from `button.md`
   - Common pattern: SM outline neutral button ("Save Draft") + SM contained primary button ("Publish")
   - Button groups also supported
   - Tab group with `tabPosition='top'` can appear here instead
   - All have `flex-shrink-0`

---

## Row 2 — Steps Section (Optional)

| Property | Value | Tailwind |
|----------|-------|----------|
| Margin left | 56px | `ml-14` |
| Content | Tab group, tab nav bar, or horizontal stepper | — |

The 56px indent aligns the step content past the close button area.

- Tabs use the exact spec from `tab.md`

---

## Typography

| Element | Size | Line Height | Weight | Color | Spec |
|---------|------|-------------|--------|-------|------|
| Title | 18px | 28px | 600 | `var(--text-primary)` (#353E5A) | title-xl |
| Additional text | 12px | 18px | 400 | `var(--text-primary)` (#353E5A) | paragraph-sm |

---

## Close Button — Detailed Spec

The close button in the full-page-header is a **highlighted icon-only MD (40×40px) button**:

```jsx
{/* Close button — highlighted variant, MD size */}
{/* See dialog-close-button.md for the highlighted pattern */}
{/* This uses MD (40×40) instead of SM (32×32) */}
<button
  className="
    flex items-center justify-center
    w-10 h-10 rounded-lg
    bg-[var(--bg-secondary)] text-[var(--fg-tertiary)]
    cursor-pointer outline-none border-none
    hover:bg-[var(--bg-secondary_hover)]
    focus-visible:bg-[var(--bg-secondary_hover)]
    transition-colors duration-150 ease-out
    shrink-0
  "
  onClick={onClose}
  aria-label="Close"
>
  <X className="w-5 h-5" />
</button>
```

| Property | Value |
|----------|-------|
| Width × Height | 40×40px (`w-10 h-10`) |
| Border radius | 8px (`rounded-lg`) |
| Background | `var(--bg-secondary)` (#F1F4F7) |
| Hover bg | `var(--bg-secondary_hover)` (#E8ECF1) |
| Icon size | 20px (`w-5 h-5`) |
| Icon color | `var(--fg-tertiary)` (#6B748E) |
| Border | none |

---

## Vertical Separator (Leading Content)

When a vertical separator appears between the title and other leading elements:

```jsx
{/* Vertical separator — see separator.md */}
<div className="w-px h-5 bg-[var(--border-secondary)] shrink-0" />
```

| Property | Value |
|----------|-------|
| Width | 1px |
| Height | 20px (`h-5`) |
| Color | `var(--border-secondary)` (#CCD6DC) |

---

## Implementation Pattern for Lovable

```jsx
{/* === Full Page Header — Basic with close + title + actions === */}
{/* All buttons MUST use button.md spec */}
<div className="flex flex-col w-full pl-3 pr-6 bg-white">
  {/* Row 1 */}
  <div className="flex items-center gap-3 py-2">
    {/* Close button — highlighted MD icon-only */}
    <button
      className="
        flex items-center justify-center
        w-10 h-10 rounded-lg
        bg-[var(--bg-secondary)] text-[var(--fg-tertiary)]
        hover:bg-[var(--bg-secondary_hover)]
        border-none outline-none cursor-pointer
        shrink-0
        transition-colors duration-150 ease-out
      "
      aria-label="Close"
    >
      <X className="w-5 h-5" />
    </button>

    {/* Title */}
    <h2
      className="flex items-center overflow-hidden text-[var(--text-primary)] truncate"
      style={{ fontSize: '18px', lineHeight: '28px', fontWeight: 600 }}
    >
      Create New Campaign
    </h2>

    {/* Spacer */}
    <div className="flex-1" />

    {/* Action buttons — use button.md spec */}
    <div className="flex items-center gap-3 shrink-0">
      {/* SM outline neutral button */}
      <button
        className="
          flex items-center justify-center gap-1.5
          h-8 px-3 rounded-md
          bg-[var(--bg-secondary)] text-[var(--text-primary)]
          border border-[var(--text-disabled)]
          text-xs leading-[18px] font-semibold
          cursor-pointer outline-none
          hover:bg-[var(--bg-secondary_hover)] hover:shadow-sm
          transition-colors duration-150 ease-out
        "
      >
        Save Draft
      </button>
      {/* SM contained primary button */}
      <button
        className="
          flex items-center justify-center gap-1.5
          h-8 px-3 rounded-md
          bg-[var(--bg-brand-solid)] text-white
          border-none
          text-xs leading-[18px] font-semibold
          cursor-pointer outline-none
          hover:bg-[var(--bg-brand-solid_hover)] hover:shadow-sm
          transition-colors duration-150 ease-out
        "
      >
        Publish
      </button>
    </div>
  </div>
</div>


{/* === Full Page Header — With leading badge, separator, and edit button === */}
{/* Badge MUST use badge.md spec */}
{/* Separator MUST use separator.md spec */}
<div className="flex flex-col w-full pl-3 pr-6 bg-white">
  <div className="flex items-center gap-3 py-2">
    {/* Close button — highlighted MD */}
    <button
      className="
        flex items-center justify-center
        w-10 h-10 rounded-lg
        bg-[var(--bg-secondary)] text-[var(--fg-tertiary)]
        hover:bg-[var(--bg-secondary_hover)]
        border-none outline-none cursor-pointer shrink-0
        transition-colors duration-150 ease-out
      "
      aria-label="Close"
    >
      <X className="w-5 h-5" />
    </button>

    {/* Title with leading edit button inside */}
    <h2
      className="flex items-center overflow-hidden text-[var(--text-primary)] truncate"
      style={{ fontSize: '18px', lineHeight: '28px', fontWeight: 600 }}
    >
      <span className="truncate">Campaign Details</span>
      {/* Leading edit button (SM text secondary icon-only) */}
      <button
        className="
          flex items-center justify-center shrink-0 ml-1
          w-8 h-8 rounded-md
          bg-transparent text-[var(--text-brand-secondary)]
          border-none cursor-pointer outline-none
          hover:bg-[var(--bg-brand-secondary)] hover:shadow-sm
          transition-colors duration-150 ease-out
        "
      >
        <Pencil className="w-4 h-4" />
      </button>
    </h2>

    {/* Vertical separator — see separator.md */}
    <div className="w-px h-5 bg-[var(--border-secondary)] shrink-0" />

    {/* Badge — see badge.md */}
    <span className="shrink-0 ...use-exact-badge-spec...">Active</span>

    {/* Spacer */}
    <div className="flex-1" />

    {/* Additional text */}
    <p className="text-xs leading-[18px] font-normal text-[var(--text-primary)] shrink-0">
      Additional Text
    </p>

    {/* Action button */}
    <button
      className="
        flex items-center justify-center gap-1.5
        h-8 px-3 rounded-md
        bg-[var(--bg-brand-solid)] text-white border-none
        text-xs leading-[18px] font-semibold
        cursor-pointer outline-none
        hover:bg-[var(--bg-brand-solid_hover)] hover:shadow-sm
        transition-colors duration-150 ease-out
      "
    >
      Save
    </button>
  </div>
</div>


{/* === Full Page Header — With tabs (Row 2) === */}
{/* Tabs MUST use tab.md spec */}
<div className="flex flex-col w-full pl-3 pr-6 bg-white">
  {/* Row 1 */}
  <div className="flex items-center gap-3 py-2">
    <button
      className="
        flex items-center justify-center
        w-10 h-10 rounded-lg
        bg-[var(--bg-secondary)] text-[var(--fg-tertiary)]
        hover:bg-[var(--bg-secondary_hover)]
        border-none cursor-pointer shrink-0
        transition-colors duration-150 ease-out
      "
      aria-label="Close"
    >
      <X className="w-5 h-5" />
    </button>

    <h2
      className="flex items-center overflow-hidden text-[var(--text-primary)] truncate"
      style={{ fontSize: '18px', lineHeight: '28px', fontWeight: 600 }}
    >
      Settings
    </h2>

    <div className="flex-1" />

    {/* Action buttons */}
    <div className="flex items-center gap-3 shrink-0">
      <button className="... outline neutral SM button from button.md ...">
        Cancel
      </button>
      <button className="... contained primary SM button from button.md ...">
        Save Changes
      </button>
    </div>
  </div>

  {/* Row 2 — Tabs indented 56px */}
  {/* Tabs MUST use tab.md spec (primary type) */}
  <div className="ml-14">
    {/* Primary tab group — see tab.md for exact tab implementation */}
    <div className="relative flex">
      {/* Gray separator line at bottom */}
      <div className="absolute bottom-0 left-0 w-full h-px bg-[var(--border-tertiary)] z-0" />
      {/* Tab items */}
      <button className="... active primary tab from tab.md ...">One</button>
      <button className="... inactive primary tab from tab.md ...">Two</button>
      <button className="... inactive primary tab from tab.md ...">Three</button>
    </div>
  </div>
</div>


{/* === Full Page Header — Dark background mode === */}
<div className="flex flex-col w-full pl-3 pr-6 bg-[var(--bg-secondary)]">
  <div className="flex items-center gap-3 py-2">
    {/* Close button — on dark bg, button bg is slightly darker */}
    <button
      className="
        flex items-center justify-center
        w-10 h-10 rounded-lg
        bg-[var(--bg-secondary_hover)] text-[var(--fg-tertiary)]
        hover:bg-[var(--border-secondary)]
        border-none cursor-pointer shrink-0
        transition-colors duration-150 ease-out
      "
      aria-label="Close"
    >
      <X className="w-5 h-5" />
    </button>

    <h2
      className="flex items-center overflow-hidden text-[var(--text-primary)] truncate"
      style={{ fontSize: '18px', lineHeight: '28px', fontWeight: 600 }}
    >
      Page Title
    </h2>

    <div className="flex-1" />

    {/* Buttons use same spec — see button.md */}
  </div>
</div>
```

---

## Key Implementation Rules

1. **Close button is always first** — highlighted icon-only MD (40×40px, `rounded-lg`, bg `var(--bg-secondary)` (#F1F4F7), icon `var(--fg-tertiary)` (#6B748E) at 20px). It is NOT a regular button — it uses the highlighted variant.
2. **Title uses 18px/28px/600** — use inline `style={{ fontWeight: 600 }}` for precision.
3. **Title truncates** with ellipsis if it overflows — use `truncate` class.
4. **All non-title elements have `shrink-0`** — they maintain their size while title truncates.
5. **Spacer (`flex-1`) separates left and right content** — pushes actions to the right edge.
6. **Gap between all elements is 12px** (`gap-3`).
7. **Vertical padding is 8px** (`py-2`) on the main row.
8. **Horizontal padding is asymmetric**: 12px left (`pl-3`), 24px right (`pr-6`).
9. **Row 2 (tabs/stepper) is indented 56px** (`ml-14`) to align past the close button.
10. **Two background modes**: Light (white) is default, Dark (`var(--bg-secondary)` (#F1F4F7)) for secondary context.
11. **Width is always 100%** — the header spans the full width of its container.

---

## ⚠️ CRITICAL — Cross-Reference Rules

12. **ALL buttons in the header MUST use `button.md`** — use the exact sizes, colors, border-radius, and typography from the button spec. Common header buttons are SM size with outline/neutral or contained/primary variants. Do NOT create custom button styles.
13. **ALL badges MUST use `badge.md`** — use the exact badge component spec for any status badges, count badges, or labels in the header.
14. **ALL separators MUST use `separator.md`** — vertical separators between header elements use a `1px` wide, `20px` tall div with `var(--border-secondary)` (#CCD6DC) color. Do NOT use `<hr>` or Tailwind divide utilities.
15. **ALL tabs MUST use `tab.md`** — if a tab group appears in Row 1 (top position) or Row 2 (below header), use the exact tab component spec. Do NOT create custom tab styles.
16. **The close button uses the highlighted variant** from `dialog-close-button.md` but at **MD size (40×40px, rounded-lg, 20px icon)** — NOT the SM size used in dialog headers.
