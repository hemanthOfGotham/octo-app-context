# Global Design Guidelines

> **READ THIS FILE FIRST** before building any page or component.

> **TOKEN RULE:** Every color in this file is expressed as a CSS variable token. In code use `bg-[var(--bg-token)]` / `text-[var(--text-token)]` / `border-[var(--border-token)]` classes. In inline styles use `'var(--token)'`. The parenthetical hex is for reference only — never use raw hex in production code.

---

## 0. Design System First — Always Reference Before Building

**Before writing any code, identify which design system components are needed for the task.** Then open and follow ONLY those specific component specs — do not read every file on every prompt.

1. Scan the user's request and list the UI elements involved (buttons, inputs, cards, tables, etc.).
2. Check the Component Index (Section 9) to see if specs exist for those elements.
3. Open and read only the matching spec files — use the exact values from those specs.
4. For any color, spacing, or typography value, use Design Tokens (`design-tokens.md`) — never hardcode or guess.
5. If no spec exists for a component, build it using existing design tokens for visual consistency.

**Never recreate a component from scratch** if it already exists in the design system.

---

## 1. Hard Rules (Never Break These)

**Check EVERY page against these 7 rules before finishing.**

### Rule 1: Heading sizes

The **Section Header component** (see `section-header.md`) is the ONLY component that uses 24px. It is used exclusively for the **main page title** — the single biggest heading at the top of a page (e.g., "Settings", "Dashboard"). There should be only ONE per page.

**Every other heading on the page is free text** — sub-section titles, card titles, form group titles, inline titles — and must be **18px or smaller**:

| Element | Max size | Tailwind |
|---------|----------|----------|
| Sub-section heading | 18px | `text-lg font-semibold` |
| Card / form group title | 16px | `text-base font-semibold` |
| Minor heading | 14px | `text-sm font-semibold` |

**BANNED classes for free text headings:** `text-xl` (20px), `text-2xl` (24px), `text-3xl` (30px), `text-4xl` (36px). Only the Section Header component uses `text-2xl` — never use it anywhere else.

### Rule 2: No icons inside input fields

Text inputs are plain boxes — label above, input below. Do NOT add any icons inside input fields unless the user explicitly asks for them.

**This includes "contextually meaningful" icons — they are still not allowed by default:**
- ❌ Email/Mail icon inside email field — NOT allowed
- ❌ Phone icon inside phone field — NOT allowed
- ❌ Currency icon inside amount field — NOT allowed
- ❌ User icon inside name field — NOT allowed
- ❌ Lock icon inside password field — NOT allowed
- ❌ Calendar icon inside date field — NOT allowed
- ❌ Link icon inside URL field — NOT allowed
- ❌ Any icon that "describes the field type" — NOT allowed

**The ONLY icons allowed inside inputs are functional UI controls:**
- ✅ Password toggle (eye/eye-off) — toggles visibility
- ✅ Clear button (×) — clears the input value
- ✅ Dropdown arrow — opens a select menu
- ✅ Search icon — in dedicated search bars only (see `search-bar.md`)

**The difference:** Functional icons perform an action when clicked. Contextual/descriptive icons just sit there — those are never added by default.

### Rule 3: No ghost buttons

Never use ghost button variants. If you see the word "ghost" in any button code — replace it with `outline` + `neutral` (Tertiary).

Any non-primary button defaults to Tertiary:
- Background: `var(--bg-secondary)` (#F1F4F7)
- Border: `1px solid var(--fg-disabled)` (#8EA1AF)
- Text: `var(--text-primary)` (#353E5A)
- Hover bg: `var(--bg-secondary_hover)` (#E8ECF1)

### Rule 4: Use CSS variable tokens for all colors

Never hardcode hex values. Use CSS variables from `design-tokens.md`.

**First-time setup (once per project):**
1. Add the `:root` CSS variables block from `design-tokens.md` to `src/index.css`
2. Extend `tailwind.config.ts` with token mappings from the same file

**After generating any component:** scan your code for raw `#xxxxxx` hex values and replace with the matching `var(--token-name)`. Exception: hex values inside `:root` definitions in `index.css`.

### Rule 5: Light mode only

Always use light mode color values. Do NOT add `dark:` Tailwind prefixes or `prefers-color-scheme` media queries.

### Rule 6: Font rendering — Inter with antialiased smoothing

The `<html>` or `<body>` element MUST include:

```css
font-family: 'Inter', sans-serif;
-webkit-font-smoothing: antialiased;
-moz-osx-font-smoothing: grayscale;
```

Or in Tailwind: add `antialiased` to the `<body>` class.

**Import Inter from Google Fonts** if not already available in the project:
```html
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
```

**Why this matters:** Without `-webkit-font-smoothing: antialiased`, browsers use subpixel rendering which makes text appear visually heavier than designed. A `font-weight: 500` will look like `600`, and `600` will look almost bold. This single property is the difference between text that matches the Figma design and text that looks too thick.

### Rule 7: Use Fello icons, not Lucide

**Always use Fello icons from `icon.md`.** Lucide React is only a last-resort fallback when a specific icon does not exist in the Fello set. If you see React icon components like `<Home />`, `<Heart />`, `<Users />`, `<Settings />` in your code — replace them with Fello `<i>` tags:

```jsx
{/* ❌ WRONG — Lucide React components */}
<Home className="w-5 h-5" />
<Heart className="w-5 h-5" />

{/* ✅ CORRECT — Fello font icons */}
<i className="fello-icon-home" style={{ fontSize: '20px' }} />
<i className="fello-icon-heart" style={{ fontSize: '20px' }} />
```

Check the Quick Lookup table in the icon spec before using any Lucide icon.

---

## 2. Page Layout — Dark Shell + Content Card

**EVERY page MUST be wrapped in a dark plum shell.** No page should render without it. See `layout-guidelines.md` for the full spec.

### Two modes (ONE component, conditional logic)

| Condition | Top Header | Global Side Nav | Shell Padding |
|-----------|------------|-----------------|---------------|
| Page has global side navigation | ✅ Rendered inside 44px slot (see `top-header.md`) | ✅ Present — **icons-only (collapsed)** | `0 8px 8px` |
| Page has NO global side navigation | ❌ Not rendered, no 44px slot | ❌ Not present | `8px` all sides |

**Side navigation and top header are always paired.** When side nav is present, the top header component MUST be rendered inside the 44px slot — it is not an empty placeholder. When side nav is absent, neither the header nor the 44px slot exist.

### ⛔ Two levels of navigation — do NOT confuse them

There are two separate navigation layers:

**1. Global Side Nav (shell level)** — sits on the transparent plum shell background, OUTSIDE the content card. This is ALWAYS the **collapsed / icons-only** version of the Side Navigation component (64px wide, see `side-navigation.md` collapsed state). It is the app-wide navigation (Home, Leads, Records, Settings, etc.).

**2. Page-Level Side Nav (inside content card)** — sits INSIDE the white content card, to the left of the main content. This is an optional, page-specific navigation (e.g., Settings sub-sections like Profile, Notifications, Security). It can be a Navigation List, expanded Side Navigation, or any other nav UI relevant to the page.

```
Shell (plum var(--bg-primary-solid) / #353E5A)
├── Top Header (44px)
├── Content Card (var(--bg-secondary_subtle) / #F7F9FA, rounded)
│   ├── Global Side Nav (icons-only, 64px, on transparent bg)
│   └── Page Content (var(--bg-primary) / #FFFFFF, left shadow)
│       ├── Optional: Page-level side nav (e.g., Settings menu)
│       └── Main content area
```

**Do NOT put page-specific navigation (Settings, Profile sub-menus) on the shell background.** Page-specific navigation always lives inside the content card.

### ❌ WRONG — page-specific nav on shell background

```jsx
{/* WRONG — Settings nav sits on shell, should be inside content card */}
<Shell>
  <TopHeader />
  <ContentCard>
    <SideNavigation items={settingsItems} />  {/* ← WRONG position */}
    <MainContent />
  </ContentCard>
</Shell>
```

### ✅ CORRECT — global nav on shell, page nav inside content

```jsx
function PageLayoutShell({ hasSideNav = true, children }) {
  return (
    <div className="flex flex-col" style={{
      width: '100vw', height: '100vh',
      padding: hasSideNav ? '0 8px 8px' : '8px',
      backgroundColor: 'var(--bg-primary-solid)',  /* #353E5A */
    }}>
      {hasSideNav && (
        <div className="shrink-0" style={{ height: '44px', background: 'transparent' }}>
          <TopHeader />
        </div>
      )}
      <div className="flex-1 w-full flex overflow-hidden" style={{
        backgroundColor: 'var(--bg-secondary_subtle)',  /* #F7F9FA */
        borderRadius: '12px',  /* 12px */
      }}>
        {/* Global side nav — ALWAYS icons-only (collapsed, 64px) */}
        {hasSideNav && (
          <div className="shrink-0">
            <SideNavigation collapsed={true} />
          </div>
        )}
        {/* Page content — white bg with left border-radius + shadow */}
        <div className="flex-1 overflow-auto" style={{
          backgroundColor: 'var(--bg-primary)',  /* #FFFFFF */
          ...(hasSideNav
            ? {
                borderRadius: '12px 0 0 12px',  /* rounded top-left & bottom-left */
                boxShadow: '-4px 0 8px -2px rgba(73, 88, 131, 0.08)',
              }
            : {}),
        }}>
          {children}
          {/* Page-level nav (if needed) goes INSIDE here as part of children */}
        </div>
      </div>
    </div>
  );
}

{/* Example: Settings page with page-level side nav */}
<PageLayoutShell hasSideNav={true}>
  <div className="flex h-full">
    {/* Page-level settings nav — inside the white content area */}
    <div className="shrink-0 w-[240px] border-r border-[var(--border-tertiary)] p-4">
      <NavigationList items={settingsItems} />
    </div>
    {/* Settings content */}
    <div className="flex-1 p-6">
      <SettingsContent />
    </div>
  </div>
</PageLayoutShell>
```

**Key:** When `hasSideNav=false`, the 44px header div and global side nav are not rendered — not hidden, not zero-height, completely absent from DOM.

### Background colors

| Area | Token | Hex |
|------|-------|-----|
| Shell | `var(--bg-primary-solid)` (#353E5A) | |
| Content card | `var(--bg-secondary_subtle)` (#F7F9FA) | |
| Side nav area | Inherits content card | `var(--bg-secondary_subtle)` (#F7F9FA) |
| Main content | `var(--bg-primary)` (#FFFFFF) | |

Main content has a left shadow (`-4px 0 8px -2px rgba(73, 88, 131, 0.08)`) for separation from side nav. No shadow when no side nav.

---

## 3. Buttons — Only 5 Types Allowed

See `button.md` for full JSX with all states.

| # | Name | Spec Mapping | Look | When |
|---|------|-------------|------|------|
| 1 | **Primary** | `contained` + `primary` | Filled coral `var(--bg-brand-solid)` (#FF725C), white text | Main CTA (one per group) |
| 2 | **Secondary** | `outline` + `primary` | Coral border, coral text | Supporting action next to primary |
| 3 | **Tertiary** | `outline` + `neutral` | Gray bg `var(--bg-secondary)` (#F1F4F7), border `var(--fg-disabled)` (#8EA1AF) | **Default for all non-primary buttons** |
| 4 | **Quaternary** | `hyperlink` + `secondary` | Teal text `var(--text-brand-secondary)` (#098486), no border | Subtle links — "Learn more", "View all" |
| 5 | **Danger** | `contained` + `danger` | Filled red `var(--bg-error-solid)` (#F02C00), white text | Delete, Remove only |

### Button order (left → right): lowest emphasis → highest emphasis

```
[Cancel (Tertiary)]  [Save (Primary)]
[Cancel (Tertiary)]  [Delete (Danger)]
[Export (Tertiary)]  [Edit (Secondary)]  [Create (Primary)]
```

### Forbidden variants

Do NOT use: `contained+ghost`, `contained+neutral`, `contained+secondary`, `contained+white`, `text+any`, `outline+ghost`, or any variant with the word "ghost". All exist in the button spec appendix for special requests only — never use them by default.

---

## 4. Typography

Font: **Inter** (the only font). Import from Google Fonts.

### Free text heading sizes

| Element | Size | Weight | Tailwind |
|---------|------|--------|----------|
| Page title | 18px | 600 | `text-lg font-semibold` |
| Section heading | 16px | 600 | `text-base font-semibold` |
| Sub-section | 14px | 600 | `text-sm font-semibold` |
| Body text | 14px | 400 | `text-sm` |
| Labels | 14px | 500 | `text-sm font-medium` |
| Small labels | 12px | 500 | `text-xs font-medium` |
| Helper text | 12px | 400 | `text-xs` |

**Note:** These limits apply to free text you write. Text sizes defined inside individual component specs (tooltip, modal header, etc.) use their own spec values — do not override those.

### Text color hierarchy

| Role | Token | Hex | Use |
|------|-------|-----|-----|
| Primary | `var(--text-primary)` (#353E5A) | Headings, body, labels |
| Secondary | `var(--text-secondary)` (#495883) | Descriptions, hints |
| Tertiary | `var(--text-tertiary)` (#6B748E) | Metadata, timestamps |
| Quaternary | `var(--text-quaternary)` (#9298A9) | Icons, UI chrome |
| Placeholder | `var(--text-placeholder)` (#BDC8D3) | Input placeholders |
| Disabled | `var(--text-disabled)` (#8EA1AF) | Disabled controls |

---

## 5. Forms

### Layout

- Fields stack vertically with `gap-4` (16px)
- Two-column rows: `grid grid-cols-2 gap-4`
- Gap between sections: `gap-8` (32px)
- Action buttons: right-aligned (`flex justify-end gap-3 mt-6`)
- Button order: `[Cancel (Tertiary)]  [Save (Primary)]`

### Field sizing

| Context | Size |
|---------|------|
| Standard forms | **MD** (40px) — default |
| Compact forms (table filters) | SM (32px) |
| Prominent forms (onboarding) | LG (48px) — only if explicitly asked |

### Input fields are plain text boxes

No icons inside inputs by default — not even "contextually meaningful" ones like email or phone icons. Only functional controls (password toggle, clear button, dropdown arrow) are allowed. Icons are added only when the user explicitly asks. See Rule 2.

### Required fields

Red asterisk `*` after the label in `var(--text-error-primary)` (#F02C00). Part of the label, not a separate element.

### Validation

Error border: `var(--border-error-solid)` (#F02C00). Error text below field in `var(--text-error-primary)` (#F02C00). Validate on blur, not on keystroke.

---

## 6. Navigation Lists — Zero Gap

All navigation lists (side nav, settings menus, dropdown nav) must have `gap-0` between items.

### ❌ WRONG

```jsx
<div className="flex flex-col gap-2">
  <NavItem />
  <NavItem />
</div>
```

### ✅ CORRECT

```jsx
<div className="flex flex-col gap-0">
  <NavItem />  {/* spacing comes from item's own py-0.5 px-1 padding */}
  <NavItem />
</div>
```

Never use `gap-*`, `space-y-*`, or extra margins between navigation items.

---

## 7. Default Sizing — Medium First

All sized components default to **MD** unless explicitly told otherwise.

| Component | Default | Height |
|-----------|---------|--------|
| Button | MD | 40px |
| Input / Select | MD | 40px |
| Badge | MD | — |
| Tag | MD | 24px |
| Checkbox | SM | 16×16 |
| Radio | MD | — |
| Toggle | MD | — |
| Avatar | MD | — |

Do NOT use Large unless the user specifically asks.

---

## 8. Spacing

All spacing follows a **4px grid**. No arbitrary values (5px, 7px, 15px).

| Context | Value | Tailwind |
|---------|-------|----------|
| Page title → first content | 24px | `mb-6` |
| Between content sections | 32px | `gap-8` |
| Section header → content | 16px | `mb-4` |
| Between cards | 16px | `gap-4` |
| Internal card padding | 16–24px | `p-4` or `p-6` |

### Alignment

- Form labels: top-left
- Action buttons: right-aligned
- Card content: left-aligned
- Table text: left; table numbers: right

---

## 9. Component Index

Before creating any custom element, check if a spec exists:

| Category | Specs |
|----------|-------|
| **App Shell** | top-header, search-bar, credits-button, fello-logo, header-actions, menu-trigger, layout-guidelines |
| **Layout** | side-navigation, page-header, section-header, full-page-header, card, separator, grid, tab |
| **Navigation** | breadcrumbs, navigation-list, side-navigation, stepper |
| **Buttons** | button, icon-toggle-button, toggle |
| **Forms** | form-field, text-inputs, select-inputs, checkbox, checkbox-group, checkbox-wrapper, radio, radio-filter-group, slider, color-picker, chip-input, date-picker, file-picker |
| **Filters** | filter-date-picker, filter-dropdown |
| **Data** | table, list-item, list-box, badge, tag, capsule, status-dot, avatar, rating, progress-bar, progress-circle, timeline, indicator-icon |
| **Charts** | axis-charts, non-axis-charts |
| **Feedback** | toast, tooltip, callout, banner, spinner, empty-state |
| **Overlays** | modal, slide-out-dialog, dialog-close-button, popup, confirmation-popup, image-modal |
| **Decorative** | featured-icon, animated-border, scroll-shadow, image-card |
| **Tokens** | design-tokens |

If a spec exists — use it. Do NOT reinvent with custom Tailwind.

---

## 10. Cross-References

When building composite UIs, reference all relevant specs:

| Building... | Reference these specs |
|------------|---------------------|
| App shell | layout-guidelines, top-header, search-bar, credits-button, fello-logo, header-actions, menu-trigger, side-navigation |
| Form | form-field, text-inputs or select-inputs, button |
| Table with actions | table, button, badge, popup |
| Modal / Dialog | modal or slide-out-dialog, dialog-close-button, button |
| Settings page | side-navigation, form-field, toggle, button |
| Card with status | card, badge, status-dot, avatar |
| Filter bar | filter-dropdown, filter-date-picker, button |

---

## 11. Restyling Existing Projects

When applying this design system to an existing project:

**DO:** Change colors, typography, spacing, borders, shadows, layout structure.

**DO NOT:** Change routing, state management, API calls, event handlers, data flow, component behavior, or content.

### Matching rules

| If you see... | Apply this spec |
|--------------|----------------|
| Top bar / app header | `top-header.md` |
| Search in header | `search-bar.md` |
| Left sidebar navigation | `side-navigation.md` |
| Page title bar with actions | `page-header.md` |
| Form with labeled inputs | `form-field.md` + `text-inputs.md` |
| Data table | `table.md` |
| Modal / Dialog | `modal.md` |
| Buttons | `button.md` |

If no matching spec exists for a component — leave it as-is.

---

## Self-Check Before Finishing Any Page

Run through this checklist:

1. ☐ Page wrapped in dark plum shell (`var(--bg-primary-solid)` / #353E5A)?
2. ☐ No heading larger than 18px (except Section Header page title)?
3. ☐ No icons inside input fields?
4. ☐ No ghost buttons? (search for "ghost" in your code)
5. ☐ No raw hex values? (all colors use CSS variables)
6. ☐ No `dark:` Tailwind prefixes?
7. ☐ `body` has `font-family: 'Inter'` and `antialiased`?
8. ☐ All buttons are one of the 5 standard types?
9. ☐ Non-primary buttons use Tertiary (`outline` + `neutral`)?
10. ☐ Navigation lists have `gap-0`?
11. ☐ All components use MD size by default?
