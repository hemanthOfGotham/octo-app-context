# Page Layout Shell

> ⚠️ **TOKEN RULE**: This spec shows hex values for reference. When implementing, always use CSS variables from `design-tokens.md` instead of raw hex. Example: use `var(--bg-primary-solid)` not `#353E5A`, use `var(--bg-secondary_subtle)` not `#F7F9FA`. See the Hex-to-Token Translation Table in the design tokens doc for all mappings.

The page layout shell is the outermost wrapper for every screen in the app. It provides a **dark colored background** and a **light content card** — nothing else. The header and side navigation are **separate components** that render inside the shell via slots; the shell itself does not contain them.

---

## Structure

The shell has **two modes** depending on whether the page has a side navigation (and therefore a top header) or not.

### Mode A — With Global Side Navigation + Top Header (most pages)

The header and global side nav are present. Shell padding is `0 8px 8px` (0 on top because the header sits there).

**⛔ The side nav at shell level is ALWAYS the global app navigation in collapsed/icons-only mode (64px wide).** It is NOT a page-specific navigation. See `side-navigation.md` collapsed state.

```
┌──────────────────────────────────────────────────────────────┐
│  DARK SHELL (var(--bg-primary-solid)) (#353E5A)  padding: 0 8px 8px   │
│  ┌────────────────────────────────────────────────────────┐  │
│  │  HEADER SLOT (44px) — transparent bg                   │  │
│  └────────────────────────────────────────────────────────┘  │
│                                                              │
│  ╭────────────────────────────────────────────────────────╮  │
│  │ CONTENT CARD (var(--bg-secondary_subtle)) (#F7F9FA, radius: 12px)│  │
│  │ ┌────┐┌──────────────────────────────────────────────┐│  │
│  │ │ICON││ ░ left shadow                                ││  │
│  │ │ONLY││                                              ││  │
│  │ │NAV ││       MAIN CONTENT (var(--bg-primary)) (#FFFFFF)││  │
│  │ │64px││                                              ││  │
│  │ │    ││       Page content renders here               ││  │
│  │ │    ││       (may include page-level side nav)       ││  │
│  │ └────┘└──────────────────────────────────────────────┘│  │
│  ╰────────────────────────────────────────────────────────╯  │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```

The dark background is visible as a thin border around the content card — 0px on top (header sits there), 8px on the left, 8px on the right, and 8px on the bottom.

**Inside the content card**, the global icons-only side nav and main content sit side by side. The main content area has a **left shadow** (`-4px 0 8px -2px rgba(73,88,131,0.08)`) that creates a subtle visual separation from the side navigation. This is NOT a border — it is a shadow on the left edge.

### Two levels of navigation (do NOT confuse them)

| Level | Where it sits | What it is | Width |
|-------|--------------|------------|-------|
| **Global Side Nav** | On shell bg, inside content card, left side | App-wide navigation (Home, Leads, Settings, etc.) — **always collapsed / icons-only** | 64px |
| **Page-Level Nav** (optional) | Inside the white main content area | Page-specific navigation (e.g., Settings sub-menu: Profile, Notifications, Security) | varies |

**Do NOT place page-specific navigation on the shell background.** Page-specific navigation always lives inside the white main content area as part of the page's children.

### Mode B — Without Side Navigation (no header)

When the page has **NO side navigation**, the top header is also NOT present. In this mode there is **NO 44px header slot**. The shell padding becomes `8px` on ALL four sides, and the content card fills the entire shell area.

```
┌──────────────────────────────────────────────────────────────┐
│  DARK SHELL (var(--bg-primary-solid)) (#353E5A)  padding: 8px (all sides)│
│                                                              │
│  ╭────────────────────────────────────────────────────────╮  │
│  │ CONTENT CARD (var(--bg-secondary_subtle)) (#F7F9FA, radius: 12px)│  │
│  │                                                        │  │
│  │       MAIN CONTENT (var(--bg-primary)) (#FFFFFF) or (var(--bg-secondary_subtle)) (#F7F9FA)│  │
│  │                                                        │  │
│  │       Page content renders here                        │  │
│  │       (no side nav, no header)                         │  │
│  │                                                        │  │
│  ╰────────────────────────────────────────────────────────╯  │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```

The dark plum background is visible as an **8px border on ALL four sides** — top, left, right, and bottom.

### When to use which mode

| Condition | Mode | Header | Global Side Nav | Shell Padding |
|-----------|------|--------|-----------------|---------------|
| Page has global side navigation | **Mode A** | ✅ 44px header slot | ✅ Icons-only (64px) | `0 8px 8px` |
| Page has NO global side navigation | **Mode B** | ❌ No header slot | ❌ Not present | `8px` (all sides) |

---

## Specs

### Shell (outer container)

| Property | Mode A (with side nav) | Mode B (no side nav) |
|----------|----------------------|---------------------|
| Width | `100vw` | `100vw` |
| Height | `100vh` | `100vh` |
| Display | `flex` | `flex` |
| Flex direction | `column` | `column` |
| Padding | `0 8px 8px` | **`8px`** (all sides) |
| Background (primary) | `var(--bg-primary-solid)` (#353E5A) | `var(--bg-primary-solid)` (#353E5A) |
| Background (secondary) | `var(--bg-brand-secondary-solid)` (#098486) (admin emulation) | `var(--bg-brand-secondary-solid)` (#098486) (admin emulation) |

### Header slot (Mode A only)

| Property | Value |
|----------|-------|
| Height | `44px` |
| Flex shrink | `0` (does not shrink) |
| Background | `transparent` — sits directly on the shell's dark background. **Do NOT give it its own background color.** |

> ⚠️ **The header slot is ONLY rendered when the page has a side navigation (Mode A).** If there is no side navigation, do NOT render the 44px header slot — skip it entirely.

### Content card

| Property | Value |
|----------|-------|
| Background | `var(--bg-secondary_subtle)` (#F7F9FA) (very light gray — **NOT pure white `var(--bg-primary)` (#FFFFFF)**) |
| Flex | `1` (fills all remaining vertical space) |
| Width | `100%` |
| Display | `flex` (side nav + main content sit side by side) |
| Border radius | `12px` on **ALL four corners** |
| Overflow | `hidden` (children handle their own scroll) |

### Global side navigation area (inside content card, left side)

| Property | Value |
|----------|-------|
| Flex shrink | `0` (fixed width, does not shrink) |
| Outer padding | `0` — **no padding** around the global nav wrapper |
| Background | Inherits from content card (`var(--bg-secondary_subtle)` (#F7F9FA)) |
| Navigation mode | **Always collapsed / icons-only (64px wide)** |

This is the global app navigation — Home, Leads, Records, Settings, etc. It is ALWAYS in collapsed/icons-only mode at shell level. See `side-navigation.md` collapsed state. Page-specific navigation (Settings sub-menu, etc.) goes inside the main content area, NOT here.

### Main content area (inside content card, right side)

| Property | Value |
|----------|-------|
| Background | `var(--bg-primary)` (#FFFFFF) (white — contrasts with the `var(--bg-secondary_subtle)` (#F7F9FA) content card) |
| Flex | `1` (fills remaining width) |
| Overflow | `auto` (scrollable) |
| Border radius | `12px 0 0 12px` — rounded on **top-left and bottom-left** only |
| Box shadow | `-4px 0 8px -2px rgba(73, 88, 131, 0.08)` (left shadow for separation from side nav) |

The left border-radius creates the visible curve where the white content area meets the gray side nav area — matching the outer content card's rounding. The right corners are `0` because the content card's own rounding handles those. The left shadow creates a subtle visual boundary between the side navigation and the main content. Do NOT use a border — use only this shadow.

---

## Variants

| Variant | Shell background | When used |
|---------|-----------------|-----------|
| Primary (default) | `var(--bg-primary-solid)` (#353E5A) (dark plum/navy) | Normal app usage |
| Secondary | `var(--bg-brand-secondary-solid)` (#098486) (teal) | Admin emulating another user's account |

The variant only changes the shell background color. The content card and all other properties remain the same.

---

## Implementation

**⚠️ IMPORTANT: This is ONE component with conditional logic — NOT two separate components. The `hasSideNav` prop controls whether the header slot and side navigation are rendered.**

```jsx
function PageLayoutShell({ variant = 'primary', hasSideNav = true, children }) {
  const shellBg = variant === 'secondary'
    ? 'var(--bg-brand-secondary-solid)' /* #076C6E — teal, admin emulation */
    : 'var(--bg-primary-solid)';          /* #353E5A — dark plum/navy, default */

  return (
    <div
      className="flex flex-col"
      style={{
        width: '100vw',
        height: '100vh',
        /*
         * CONDITIONAL PADDING:
         * - hasSideNav=true  → '0 8px 8px'  (0 top because header sits there)
         * - hasSideNav=false → '8px'         (8px ALL sides, no header)
         */
        padding: hasSideNav ? '0 8px 8px' : '8px',
        backgroundColor: shellBg,
      }}
    >
      {/*
       * CONDITIONAL HEADER:
       * Only render the 44px header slot when hasSideNav is true.
       * When hasSideNav is false, this entire div is SKIPPED — no 44px space.
       */}
      {hasSideNav && (
        <div
          className="shrink-0"
          style={{
            height: '44px',
            background: 'transparent',
          }}
        >
          {/* <TopHeader variant={variant} /> */}
        </div>
      )}

      {/* Content card */}
      <div
        className="flex-1 w-full flex overflow-hidden"
        style={{
          backgroundColor: 'var(--bg-secondary_subtle)', /* #F7F9FA */
          borderRadius: '12px',            /* 12px */
        }}
      >
        {/*
         * CONDITIONAL SIDE NAV:
         * Only render the side navigation area when hasSideNav is true.
         */}
        {/* Global side nav — ALWAYS collapsed / icons-only (64px), NO outer padding */}
        {hasSideNav && (
          <div className="shrink-0">
            {/* <SideNavigation collapsed={true} /> — see side-navigation.md collapsed state */}
          </div>
        )}

        {/* Main content area */}
        <div
          className="flex-1 overflow-auto"
          style={{
            backgroundColor: 'var(--bg-primary)', /* #FFFFFF */
            /*
             * CONDITIONAL LEFT BORDER-RADIUS + SHADOW:
             * Only apply when side nav is present.
             * borderRadius: top-left 12px, top-right 0, bottom-right 0, bottom-left 12px
             */
            ...(hasSideNav
              ? {
                  borderRadius: '12px 0 0 12px',
                  boxShadow: '-4px 0 8px -2px rgba(73, 88, 131, 0.08)',
                }
              : {}),
          }}
        >
          {children}
        </div>
      </div>
    </div>
  );
}
```

### Usage Examples

```jsx
{/* Simple page WITH global side nav → icons-only nav + header */}
<PageLayoutShell hasSideNav={true}>
  <DashboardPage />
</PageLayoutShell>

{/* Settings page — global icons-only nav + page-level settings nav inside content */}
<PageLayoutShell hasSideNav={true}>
  <div className="flex h-full">
    {/* Page-level settings nav — inside the white content area */}
    <div className="shrink-0 w-[240px] border-r border-[var(--border-tertiary)] p-4">
      <NavigationList items={settingsItems} />
    </div>
    {/* Settings content */}
    <div className="flex-1 p-6 overflow-auto">
      <SettingsContent />
    </div>
  </div>
</PageLayoutShell>

{/* Page WITHOUT side navigation → NO header, NO side nav, 8px all sides */}
<PageLayoutShell hasSideNav={false}>
  <LoginPage />
</PageLayoutShell>
```

---

## CRITICAL Rules

1. Shell background is `var(--bg-primary-solid)` (#353E5A) (primary/default) or `var(--bg-brand-secondary-solid)` (#098486) (secondary/admin emulation). NEVER use any other color.
2. **Shell padding depends on mode:**
   - **Mode A (with side nav + header):** `0 8px 8px` — 0 on top (header sits there), 8px on left, right, and bottom.
   - **Mode B (no side nav, no header):** `8px` on ALL four sides — the dark background is visible as an equal 8px border around the entire content card.
3. **Header slot exists ONLY in Mode A** — exactly `44px` tall, `flex-shrink: 0`, `transparent` background. **If there is NO side navigation, do NOT render the header slot at all.**
4. **Side navigation and top header are linked** — if the page has a side nav, it has a header (Mode A). If it has no side nav, it has no header (Mode B). They always appear together or not at all.
5. Content card background is `var(--bg-secondary_subtle)` (#F7F9FA) (very light gray) — NOT pure white `var(--bg-primary)` (#FFFFFF).
6. Content card border-radius is `12px` on ALL FOUR corners.
7. Content card has `display: flex`, `overflow: hidden`, and `flex: 1` to fill remaining space.
8. The shell is `100vw × 100vh` with `display: flex; flex-direction: column`.
9. The variant ONLY changes the shell background color. The content card and all other properties remain identical.
10. **Inside the content card (Mode A)**: Global Side Navigation (collapsed/icons-only, 64px) sits on the left (`shrink-0`, **no outer padding on the wrapper**), Main Content sits on the right (`flex-1`, `overflow: auto`).
11. **The side nav at shell level is ALWAYS the global app navigation in collapsed/icons-only mode.** It is NOT a page-specific navigation. Page-specific navigation (e.g., Settings sub-menu) goes inside the white main content area as part of the page's children.
12. **Main content background is `var(--bg-primary)` (#FFFFFF)** (white) — this contrasts with the `var(--bg-secondary_subtle)` (#F7F9FA) content card behind the side nav.
13. **Main content has border-radius on the left side (Mode A only)**: `border-radius: 12px 0 0 12px` — rounded on top-left and bottom-left, straight on the right. This creates the visible curve where the white content meets the gray side nav area.
14. **Main content has a left shadow (Mode A only)**: `box-shadow: -4px 0 8px -2px rgba(73, 88, 131, 0.08)`. This creates a subtle separation from the side nav. Do NOT use a border for this — use only this shadow. In Mode B, there is no left shadow or border-radius since there is no side nav.
15. **Every page MUST use the dark plum shell** — whether it has a side nav or not. The only difference is padding and whether the header slot is present.

---

## Layout Showcase

When rendering the **Layout** showcase page, display the shell visually:

- Full-viewport dark plum (`var(--bg-primary-solid)` (#353E5A)) background
- A 44px tall header slot at the top shown as a dashed outline with the label: **"Header slot — separate component renders here"**
- A large light gray (`var(--bg-secondary_subtle)` (#F7F9FA)) rounded card filling the remaining space with the label: **"Content Area"** and subtitle **"Page content renders here"**
- The 8px padding visible around the content card on left, right, and bottom edges
- Annotation labels showing: `44px header slot`, `var(--bg-primary-solid) (#353E5A) (shell background) — padding: 0 8px 8px`, `border-radius: 12px (all corners)`, `bg: var(--bg-secondary_subtle) (#F7F9FA)`, `8px padding`
