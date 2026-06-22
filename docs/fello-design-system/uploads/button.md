# Button Component — Strict Specification

When building any button in the UI, follow these specifications exactly. Do NOT use default Tailwind button styles, shadcn buttons, or any other button system. Every button must match this spec.

> **TOKEN RULE:** Every color in the code examples below uses CSS variable tokens or Tailwind palette names — never raw hex. When implementing, copy the token references exactly. See `design-tokens.md` for definitions.

---

## ⚠️ MANDATORY — Only 5 Button Styles Are Allowed

> **STOP. Read this section FIRST before building ANY button.**
> The design system has many color variants documented below for reference, but in practice **only 5 button combinations are permitted**. Do NOT create a reusable `<Button>` component with variant props like `ghost`, `outline-ghost`, `text-secondary`, etc. Instead, apply the correct Tailwind classes directly based on the 5 roles below.

### The 5 Standard Buttons

| # | Role | How to Build It | Example Labels | Look |
|---|------|----------------|----------------|------|
| 1 | **Primary CTA** | `contained` + `primary` | Save, Submit, Create, Continue | Filled coral `var(--bg-brand-solid)` (#FF725C), white text |
| 2 | **Secondary action** | `outline` + `primary` | Edit, Duplicate, Export | Coral border `var(--border-brand-solid)` (#FF725C), coral text, white bg |
| 3 | **Tertiary / Cancel** | `outline` + `neutral` | Cancel, Back, Close, Discard | Gray bg `var(--bg-secondary)` (#F1F4F7), border `var(--text-disabled)` (#8EA1AF), dark text `var(--text-primary)` (#353E5A) |
| 4 | **Subtle link** | `hyperlink` + `secondary` | Learn more, View all | Teal text `var(--text-brand-secondary)` (#098486), underline on hover, no border/bg |
| 5 | **Destructive** | `contained` + `danger` | Delete, Remove, Revoke | Filled red `var(--bg-error-solid)` (#F02C00), white text |

### 🚫 FORBIDDEN Button Choices (NEVER use these)

These variants exist in the color reference below but **MUST NOT be used** unless the user explicitly types "use ghost button" or "use AI button":

- ❌ `contained` + `ghost` — NEVER for Cancel, Back, Close, or any normal button
- ❌ `contained` + `neutral` — NEVER for Cancel, Back, Close
- ❌ `contained` + `secondary` (filled teal) — NEVER unless explicitly asked
- ❌ `contained` + `white` — NEVER unless on dark backgrounds and explicitly asked
- ❌ `contained` + `ai` (gradient) — NEVER unless explicitly asked
- ❌ `outline` + `ghost` — NEVER for normal UI
- ❌ `outline` + `secondary` (teal outline) — NEVER unless explicitly asked
- ❌ `outline` + `danger` (red outline) — NEVER unless explicitly asked
- ❌ `text` + any variant — NEVER for Cancel/Back/Close (use outline+neutral instead)
- ❌ `hyperlink` + `primary` (coral link) — NEVER unless explicitly asked
- ❌ `hyperlink` + `neutral` — NEVER unless explicitly asked

### ✅ Default for Non-Primary Actions — MANDATORY Rule

**Any button that is NOT the main CTA defaults to `outline` + `neutral` (Tertiary):**
- Background: `var(--bg-secondary)` (#F1F4F7)
- Border: `1px solid var(--text-disabled)` (#8EA1AF)
- Text color: `var(--text-primary)` (#353E5A)
- Hover bg: `var(--bg-secondary_hover)` (#E8ECF1)

**NEVER use ghost, text-only, borderless, or dark filled buttons for non-primary actions.**

### Common Patterns (follow these exactly)

```
Form actions:    [Cancel (outline/neutral)]  [Save (contained/primary)]
Dialog actions:  [Cancel (outline/neutral)]  [Confirm (contained/primary)]
Delete confirm:  [Cancel (outline/neutral)]  [Delete (contained/danger)]
Page header:     [Export (outline/neutral)]   [Add Item (contained/primary)]
Settings:        [Cancel (outline/neutral)]  [Save Changes (contained/primary)]
Read-only mode:  [Edit (outline/primary)]
```

**Button order:** Lowest emphasis on left → Highest emphasis on right. Primary CTA is always rightmost.

---

## The 5 Standard Buttons — Complete JSX (Copy-Paste Ready)

### Button 1: Primary CTA (contained/primary) — MD

```jsx
<button className="
  inline-flex items-center justify-center gap-1.5
  h-10 px-4 py-2.5
  rounded-lg
  bg-[var(--bg-brand-solid)] text-[var(--text-white)]
  text-sm leading-5 font-semibold
  hover:bg-[var(--bg-brand-solid_hover)] hover:shadow-sm
  focus-visible:bg-[var(--bg-brand-solid_hover)] focus-visible:ring-4 focus-visible:ring-[var(--ring-brand-primary)]
  disabled:bg-[var(--bg-brand-solid)]/50 disabled:pointer-events-none
  transition-[background-color,box-shadow] duration-150 ease-out
  cursor-pointer border-none outline-none
">
  <span className="truncate">Save</span>
</button>
```

### Button 2: Secondary Action (outline/primary) — MD

```jsx
<button className="
  inline-flex items-center justify-center gap-1.5
  h-10 px-4 py-2.5
  rounded-lg
  bg-[var(--bg-primary)] text-[var(--text-brand-primary)]
  border border-[var(--border-brand-solid)]
  text-sm leading-5 font-semibold
  hover:bg-[var(--bg-brand-primary)] hover:shadow-sm
  focus-visible:bg-[var(--bg-brand-primary)] focus-visible:ring-4 focus-visible:ring-[var(--ring-brand-primary)]
  disabled:opacity-50 disabled:pointer-events-none
  transition-[background-color,box-shadow] duration-150 ease-out
  cursor-pointer outline-none
">
  <span className="truncate">Edit</span>
</button>
```

### Button 3: Tertiary / Cancel (outline/neutral) — MD

```jsx
{/* USE THIS FOR ALL Cancel, Back, Close, Discard buttons */}
<button className="
  inline-flex items-center justify-center gap-1.5
  h-10 px-4 py-2.5
  rounded-lg
  bg-[var(--bg-secondary)] text-[var(--text-primary)]
  border border-[var(--text-disabled)]
  text-sm leading-5 font-semibold
  hover:bg-[var(--bg-secondary_hover)] hover:shadow-sm
  focus-visible:bg-[var(--bg-secondary_hover)] focus-visible:ring-4 focus-visible:ring-[var(--ring-gray-primary)]
  disabled:bg-[var(--bg-secondary)] disabled:text-[var(--text-disabled)] disabled:border-[var(--border-primary)] disabled:pointer-events-none
  transition-[background-color,box-shadow] duration-150 ease-out
  cursor-pointer outline-none
">
  <span className="truncate">Cancel</span>
</button>
```

### Button 4: Subtle Link (hyperlink/secondary)

```jsx
<button className="
  inline-flex items-center
  bg-transparent text-[var(--text-brand-secondary)]
  text-sm leading-5 font-semibold
  underline-offset-2 decoration-transparent
  hover:decoration-[var(--text-brand-secondary)]
  cursor-pointer border-none outline-none p-0
">
  <span>View All</span>
</button>
```

### Button 5: Destructive (contained/danger) — MD

```jsx
<button className="
  inline-flex items-center justify-center gap-1.5
  h-10 px-4 py-2.5 rounded-lg
  bg-[var(--bg-error-solid)] text-[var(--text-white)]
  text-sm leading-5 font-semibold
  hover:bg-[var(--bg-error-solid_hover)] hover:shadow-sm
  focus-visible:bg-[var(--bg-error-solid_hover)] focus-visible:ring-4 focus-visible:ring-[var(--ring-error-primary)]
  disabled:bg-[var(--bg-error-solid)]/50 disabled:pointer-events-none
  transition-[background-color,box-shadow] duration-150 ease-out
  cursor-pointer border-none outline-none
">
  <span className="truncate">Delete</span>
</button>
```

### Icon-Only Button — SM

```jsx
<button className="
  inline-flex items-center justify-center
  w-8 h-8
  rounded-md
  bg-transparent text-[var(--text-primary)]
  text-xs font-normal
  hover:bg-[var(--bg-secondary_hover)] hover:shadow-sm
  transition-[background-color,box-shadow] duration-150 ease-out
  cursor-pointer border-none outline-none
" aria-label="Edit">
  <IconComponent className="w-4 h-4" />
</button>
```

---

## Button Anatomy

Every button is structured as:

```
[leading-icon] [label] [trailing-icon]
```

- Icons and label are in a horizontal flex row with `items-center justify-center gap-1.5` (6px gap)
- The label is a `<span>` with text truncation (single line, ellipsis on overflow)
- Leading and trailing icons are optional
- In **icon-only** mode, only the leading icon is shown (no label, no trailing icon)

---

## Sizes

There are 4 button sizes. Use these EXACT dimensions.

### LG (Large)
- Height: 48px
- Padding: 12px vertical, 24px horizontal (`py-3 px-6`)
- Border radius: 10px (`rounded-[10px]`)
- Typography: 16px / 24px / font-weight 600 (`text-base leading-6 font-semibold`)
- Icon size: 22px
- Icon-only width: 48px

### MD (Medium) — Default
- Height: 40px
- Padding: 10px vertical, 16px horizontal (`py-2.5 px-4`)
- Border radius: 8px (`rounded-lg`)
- Typography: 14px / 20px / font-weight 600 (`text-sm leading-5 font-semibold`)
- Icon size: 20px
- Icon-only width: 40px

### SM (Small)
- Height: 32px
- Padding: 8px vertical, 12px horizontal (`py-2 px-3`)
- Border radius: 6px (`rounded-md`)
- Typography: 12px / 18px / font-weight 600 (`text-xs leading-[18px] font-semibold`)
- Icon size: 16px
- Icon-only width: 32px

### XS (Extra Small)
- Height: 24px
- Padding: 4px all sides (`p-1`)
- Border radius: 4px (`rounded`)
- Typography: 12px / 18px / font-weight 600 (`text-xs leading-[18px] font-semibold`)
- Icon size: 16px
- Icon-only width: 24px

### Icon color inside buttons

Icons inside buttons ALWAYS inherit the button's text color via `currentColor`. Do not set a separate icon color.

| Button Type | Icon Color | Token |
|---|---|---|
| Primary (contained/primary) | White | `var(--text-white)` (#ffffff) |
| Secondary (outline/primary) | Coral | `var(--text-brand-primary)` (#FF725C) |
| Tertiary (outline/neutral) | Dark text | `var(--text-primary)` (#353E5A) |
| Quaternary (hyperlink/secondary) | Teal | `var(--text-brand-secondary)` (#098486) |
| Danger (contained/danger) | White | `var(--text-white)` (#ffffff) |

When using Fello icons (`<i className="fello-icon-*">`), the icon inherits color from the parent. When using SVG icons, ensure `fill="currentColor"` or `stroke="currentColor"`.

---

## Common Button Behaviors

### Hover Effect
All buttons (except hyperlink) gain `box-shadow: var(--shadow-xs)` on hover.

### Focus Ring
Focus-visible state adds a 4px ring around the button. Ring color depends on variant:
- **Primary:** `var(--ring-brand-primary)` (coral ring)
- **Secondary:** `rgba(11,165,167,0.24)` (teal ring)
- **Neutral/Ghost/White:** `var(--ring-gray-primary)` (gray ring)
- **Danger:** `var(--ring-error-primary)` (red ring)

### Disabled State
- `pointer-events: none` (no interaction)
- Contained: background becomes faded/semi-transparent version
- Outline: border becomes faded, text becomes faded

### Loading State
- Button maintains its size and color
- Content becomes `visibility: hidden` (invisible but occupies space)
- A dot loader is centered absolutely over the button
- `pointer-events: none` prevents interaction

### Full Width
- `width: 100%` — button stretches to fill container

### Transitions
All buttons have `transition: background-color, box-shadow` with 150ms ease-out timing.

---

## Button Group

A button group is a horizontal row of connected buttons sharing a single outer border.

### Structure
```html
<div role="group" class="flex items-center rounded-lg border overflow-hidden w-fit">
  <button>Option 1</button>
  <button>Option 2</button>
  <button>Option 3</button>
</div>
```

### Button Group Sizes

| Size | Height | Padding | Typography | Icon Size |
|------|--------|---------|------------|-----------|
| MD | 40px | 10px × 16px | title-md (14px/600) | 20px |
| SM | 32px | 8px × 12px | title-sm (12px/600) | 16px |

### Button Group Variants

#### Neutral (Default)
- Container border: 1px solid `var(--text-disabled)` (#8EA1AF)
- Button default: bg `var(--bg-secondary)` (#F1F4F7), text `var(--text-primary)` (#353E5A)
- Button hover/focus: bg `var(--bg-secondary_hover)` (#E8ECF1)
- Button disabled: bg `var(--bg-secondary_hover)` (#E8ECF1), text `var(--text-disabled)` (#8EA1AF)
- Divider between buttons: 1px solid `var(--text-disabled)` (#8EA1AF) (same as container border)

#### Primary
- Container border: 1px solid `var(--border-brand-solid)` (#FF725C)
- Button default: bg `var(--bg-brand-solid)` (#FF725C), text `var(--text-white)` (#FFFFFF)
- Button hover/focus: `var(--bg-brand-solid_hover)` (#FF8E7D)
- Button disabled: bg primary faded
- Divider between buttons: 1px solid `var(--border-brand)` (#FFAA9D)

### Button Group Rules
- Container has `border-radius: 8px` and `overflow: hidden`
- Each button (except last) has a right border as divider
- Last button has no right border
- Supports icon-only buttons, text buttons, and icon+text buttons
- Arrow key navigation between buttons (left/right)

---

## Key Implementation Rules

1. **ALWAYS use `inline-flex items-center justify-center gap-1.5`** for button layout
2. **ALWAYS use CSS variable tokens** from this spec — never raw hex or Tailwind color classes like `bg-red-500`
3. **ALWAYS include transitions** — `transition-[background-color,box-shadow] duration-150 ease-out`
4. **ALWAYS add hover shadow** — `hover:shadow-sm`
5. **ALWAYS add focus-visible ring** — appropriate 4px ring color for the variant
6. **ALWAYS set `cursor-pointer border-none outline-none`** as base styles
7. **Icon-only buttons use width = height** (square) with centered icon
8. **Use `truncate`** on the label span for text overflow
9. **Disabled buttons always get `pointer-events-none`**
10. **Hyperlink buttons have no padding, no height, no width constraints**
11. **NEVER use ghost, AI, white, or text variants** unless the user explicitly requests them by name. If you find yourself reaching for any variant outside the 5 standard buttons — STOP and use one of the 5 instead.
12. **When in doubt, use `outline` + `neutral` (Tertiary)** — any button that is NOT the main CTA should default to Tertiary. This is the safe default for all non-primary actions.
13. **Do NOT build a reusable Button component with variant props** — just apply Tailwind classes directly from the 5 standard buttons above

---

## Appendix: Full Color Reference (For Special Requests Only)

> **⚠️ WARNING: Do NOT use these variants in normal UI. They exist here ONLY as a reference if the user explicitly asks for a specific variant by name (e.g., "use a ghost button here" or "add an AI gradient button").**

### Button Types

| Type | Description |
|------|-------------|
| **contained** | Solid filled background — primary actions |
| **outline** | Border with transparent/light background — secondary actions |
| **text** | No background, no border — tertiary/inline actions |
| **hyperlink** | Link-styled, no padding, underline on hover — navigation actions |

### CONTAINED Buttons

#### Primary Contained (Default CTA)
| State | Background | Text Color |
|-------|-----------|------------|
| Default | `var(--bg-brand-solid)` (#FF725C) | `var(--text-white)` (#FFFFFF) |
| Hover | `var(--bg-brand-solid_hover)` (#FF8E7D) | `var(--text-white)` (#FFFFFF) |
| Focus | `var(--bg-brand-solid_hover)` (#FF8E7D) + focus ring | `var(--text-white)` (#FFFFFF) |
| Disabled | `var(--bg-brand-solid)` (#FF725C) at 56% opacity | `var(--text-white)` (#FFFFFF) |

Focus ring: `0 0 0 4px var(--ring-brand-primary), 0 1px 2px 0 var(--shadow-xs)`

#### Secondary Contained
| State | Background | Text Color |
|-------|-----------|------------|
| Default | `var(--bg-brand-secondary-solid)` (#098486) | `var(--text-white)` (#FFFFFF) |
| Hover | `var(--bg-brand-secondary-solid_hover)` (#0BA5A7) | `var(--text-white)` (#FFFFFF) |
| Focus | `var(--bg-brand-secondary-solid_hover)` (#0BA5A7) + focus ring | `var(--text-white)` (#FFFFFF) |
| Disabled | `var(--bg-brand-secondary-solid)` (#098486) at 56% opacity | `var(--text-white)` (#FFFFFF) |

Focus ring: `0 0 0 4px rgba(11,165,167,0.24), 0 1px 2px 0 var(--shadow-xs)`

#### Neutral Contained
| State | Background | Text Color |
|-------|-----------|------------|
| Default | `var(--bg-primary-solid)` (#353E5A) | `var(--text-white)` (#FFFFFF) |
| Hover | primary-text-800 (#222A3F) | `var(--text-white)` (#FFFFFF) |
| Focus | primary-text-800 (#222A3F) + focus ring | `var(--text-white)` (#FFFFFF) |
| Disabled | `var(--bg-tertiary)` (#E8ECF1) | `var(--text-white)` (#FFFFFF) |

Focus ring: `0 0 0 4px var(--ring-gray-primary), 0 1px 2px 0 var(--shadow-xs)`

#### Danger Contained
| State | Background | Text Color |
|-------|-----------|------------|
| Default | `var(--bg-error-solid)` (#F02C00) | `var(--text-white)` (#FFFFFF) |
| Hover | `var(--bg-error-solid_hover)` (#FF431B) | `var(--text-white)` (#FFFFFF) |
| Focus | `var(--bg-error-solid_hover)` (#FF431B) + focus ring | `var(--text-white)` (#FFFFFF) |
| Disabled | `var(--bg-error-solid)` (#F02C00) at 56% opacity | `var(--text-white)` (#FFFFFF) |

Focus ring: `0 0 0 4px var(--ring-error-primary), 0 1px 2px 0 var(--shadow-xs)`

#### AI Buttons — Required CSS Setup

> **CRITICAL: Before using ANY AI button variant, you MUST add the following CSS to the project's `src/index.css` file (inside the `@layer utilities` block or at the end of the file). Without these classes, AI buttons will NOT render correctly.**

**Add this CSS to `src/index.css`:**
```css
/* === AI Button Gradient Utilities === */
.ai-gradient-bg {
  background: var(--gradient-ai-primary);
}
.ai-gradient-bg:hover {
  background: var(--gradient-ai-hover);
}

.ai-gradient-text {
  background: var(--gradient-ai-primary);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.ai-gradient-border {
  background: linear-gradient(white, white) padding-box, var(--gradient-ai-primary) border-box;
  border: 1px solid transparent;
}
.ai-gradient-border:hover {
  background: var(--gradient-ai-outline-bg) padding-box, var(--gradient-ai-primary) border-box;
}

.ai-gradient-underline {
  position: relative;
}
.ai-gradient-underline::after {
  content: '';
  position: absolute;
  bottom: 3px;
  left: 0;
  width: 100%;
  height: 1px;
  background: var(--gradient-ai-primary);
  opacity: 0;
  transition: opacity 150ms ease-out;
}
button:hover .ai-gradient-underline::after,
button:focus-visible .ai-gradient-underline::after {
  opacity: 1;
}
```

---

#### AI Contained

| State | Background | Text Color |
|-------|-----------|------------|
| Default | AI gradient (cyan → blue → purple → pink → rose) | `var(--text-white)` (#FFFFFF) |
| Hover | AI gradient (shifts to purple/magenta mid-tones) | `var(--text-white)` (#FFFFFF) |
| Disabled | Same gradient at 30% opacity | `var(--text-white)` (#FFFFFF) |

**AI Contained JSX (MD):**
```jsx
<button className="
  ai-gradient-bg
  inline-flex items-center justify-center gap-1.5
  h-10 px-4 py-2.5 rounded-lg
  text-[var(--text-white)] text-sm leading-5 font-semibold
  hover:shadow-sm
  disabled:opacity-30 disabled:pointer-events-none
  transition-[background,box-shadow] duration-150 ease-out
  cursor-pointer border-none outline-none
">
  <span className="truncate">Ask AI</span>
</button>
```

#### AI Outline

| State | Background | Text Color | Border |
|-------|-----------|------------|--------|
| Default | White | Gradient text | Gradient border |
| Hover | Pastel gradient (teal → purple → pink) | Gradient text | Gradient border |
| Disabled | Same at 30% opacity | Gradient text | Gradient border |

**AI Outline JSX (MD):**
```jsx
<button className="
  ai-gradient-border
  inline-flex items-center justify-center gap-1.5
  h-10 px-4 py-2.5 rounded-lg
  text-sm leading-5 font-semibold
  hover:shadow-sm
  disabled:opacity-30 disabled:pointer-events-none
  transition-[background,box-shadow] duration-150 ease-out
  cursor-pointer outline-none
">
  <span className="ai-gradient-text truncate">Ask AI</span>
</button>
```

#### AI Text

| State | Background | Text Color |
|-------|-----------|------------|
| Default | Transparent | Gradient text |
| Hover | Pastel gradient (teal → purple → pink) | Gradient text |
| Disabled | 30% opacity | Gradient text |

**AI Text JSX (MD):**
```jsx
<button className="
  inline-flex items-center justify-center gap-1.5
  h-10 px-4 py-2.5 rounded-lg
  bg-transparent
  text-sm leading-5 font-semibold
  hover:shadow-sm
  disabled:opacity-30 disabled:pointer-events-none
  transition-[background,box-shadow] duration-150 ease-out
  cursor-pointer border-none outline-none
">
  <span className="ai-gradient-text truncate">Ask AI</span>
</button>
```

#### AI Hyperlink

| State | Text Color | Hover Behavior |
|-------|------------|----------------|
| Default | Gradient text | Gradient underline hidden |
| Hover/Focus | Gradient text | 1px gradient underline visible |
| Disabled | 30% opacity | No underline |

**AI Hyperlink JSX:**
```jsx
<button className="
  inline-flex items-center
  bg-transparent text-sm leading-5 font-semibold
  disabled:opacity-30 disabled:pointer-events-none
  cursor-pointer border-none outline-none p-0
">
  <span className="ai-gradient-text ai-gradient-underline">Ask AI</span>
</button>
```

#### White Contained
| State | Background | Text Color |
|-------|-----------|------------|
| Default | `var(--bg-primary)` (#FFFFFF) | `var(--text-primary)` (#353E5A) |
| Hover | `var(--bg-secondary)` (#F1F4F7) | `var(--text-primary)` (#353E5A) |
| Focus | `var(--bg-secondary)` (#F1F4F7) + gray focus ring | `var(--text-primary)` (#353E5A) |
| Disabled | white at reduced opacity | `var(--text-primary)` (#353E5A) |

#### Ghost Contained
| State | Background | Text Color |
|-------|-----------|------------|
| Default | `var(--fg-secondary)` (#495883) | `var(--text-white)` (#FFFFFF) |
| Hover | `var(--bg-primary-solid)` (#353E5A) | `var(--text-white)` (#FFFFFF) |
| Focus | `var(--bg-primary-solid)` (#353E5A) + gray focus ring | `var(--text-white)` (#FFFFFF) |
| Disabled | `var(--bg-tertiary)` (#E8ECF1) | `var(--text-white)` (#FFFFFF) |

### OUTLINE Buttons

#### Primary Outline
| State | Background | Text Color | Border |
|-------|-----------|------------|--------|
| Default | `var(--bg-primary)` (#FFFFFF) | `var(--text-brand-primary)` (#FF725C) | 1px solid `var(--border-brand-solid)` (#FF725C) |
| Hover | `var(--bg-brand-primary)` (#FDF1EF) | `var(--text-brand-primary)` (#FF725C) | 1px solid `var(--border-brand-solid)` (#FF725C) |
| Focus | `var(--bg-brand-primary)` (#FDF1EF) + brand focus ring | `var(--text-brand-primary)` (#FF725C) | 1px solid `var(--border-brand-solid)` (#FF725C) |
| Disabled | `var(--bg-primary)` (#FFFFFF) | primary-500 faded | 1px solid primary-500 at 50% opacity |

#### Secondary Outline
| State | Background | Text Color | Border |
|-------|-----------|------------|--------|
| Default | `var(--bg-primary)` (#FFFFFF) | `var(--text-brand-secondary)` (#098486) | 1px solid `var(--bg-brand-secondary-solid_hover)` (#0BA5A7) |
| Hover | `var(--bg-brand-secondary)` (#E7FDFD) | `var(--text-brand-secondary)` (#098486) | 1px solid `var(--bg-brand-secondary-solid_hover)` (#0BA5A7) |
| Focus | `var(--bg-brand-secondary)` (#E7FDFD) + secondary focus ring | `var(--text-brand-secondary)` (#098486) | 1px solid `var(--bg-brand-secondary-solid_hover)` (#0BA5A7) |
| Disabled | `var(--bg-primary)` (#FFFFFF) | secondary faded | 1px solid secondary faded |

#### Neutral Outline
| State | Background | Text Color | Border |
|-------|-----------|------------|--------|
| Default | `var(--bg-secondary)` (#F1F4F7) | `var(--text-primary)` (#353E5A) | 1px solid `var(--text-disabled)` (#8EA1AF) |
| Hover | `var(--bg-secondary_hover)` (#E8ECF1) | `var(--text-primary)` (#353E5A) | 1px solid `var(--text-disabled)` (#8EA1AF) |
| Focus | `var(--bg-secondary_hover)` (#E8ECF1) + gray focus ring | `var(--text-primary)` (#353E5A) | 1px solid `var(--text-disabled)` (#8EA1AF) |
| Disabled | `var(--bg-secondary)` (#F1F4F7) | `var(--text-disabled)` (#8EA1AF) | 1px solid `var(--border-primary)` (#BDC8D3) |

#### Danger Outline
| State | Background | Text Color | Border |
|-------|-----------|------------|--------|
| Default | `var(--bg-primary)` (#FFFFFF) | `var(--text-error-primary)` (#F02C00) | 1px solid `var(--border-error-solid)` (#F02C00) |
| Hover | `var(--bg-error-primary)` (#FFEBE6) | `var(--text-error-primary)` (#F02C00) | 1px solid `var(--border-error-solid)` (#F02C00) |
| Focus | `var(--bg-error-primary)` (#FFEBE6) + error focus ring | `var(--text-error-primary)` (#F02C00) | 1px solid `var(--border-error-solid)` (#F02C00) |
| Disabled | `var(--bg-primary)` (#FFFFFF) | error faded | 1px solid error faded |

#### Ghost Outline
| State | Background | Text Color | Border |
|-------|-----------|------------|--------|
| Default | `var(--bg-primary)` (#FFFFFF) | `var(--text-secondary)` (#495883) | 1px solid `var(--border-tertiary)` (#E8ECF1) |
| Hover | `var(--bg-secondary_subtle)` (#F7F9FA) | `var(--text-secondary)` (#495883) | 1px solid `var(--border-tertiary)` (#E8ECF1) |
| Focus | `var(--bg-secondary_subtle)` (#F7F9FA) + gray focus ring | `var(--text-secondary)` (#495883) | 1px solid `var(--border-tertiary)` (#E8ECF1) |
| Disabled | `var(--bg-secondary)` (#F1F4F7) | `var(--text-disabled)` (#8EA1AF) | 1px solid `var(--border-primary)` (#BDC8D3) |

### TEXT Buttons (no background, no border)

| Variant | Default Text | Hover BG | Disabled Text |
|---------|-------------|----------|---------------|
| primary | `var(--text-brand-primary)` (#FF725C) | `var(--bg-brand-primary)` (#FDF1EF) | primary faded |
| secondary | `var(--text-brand-secondary)` (#098486) | `var(--bg-brand-secondary)` (#E7FDFD) | secondary faded |
| neutral | `var(--text-primary)` (#353E5A) | `var(--bg-secondary_hover)` (#E8ECF1) | `var(--text-disabled)` (#8EA1AF) |
| danger | `var(--text-error-primary)` (#F02C00) | `var(--bg-error-primary)` (#FFEBE6) | error faded |
| ghost | `var(--text-secondary)` (#495883) | `var(--bg-secondary_subtle)` (#F7F9FA) | `var(--text-disabled)` (#8EA1AF) |
| ai | Apply `ai-gradient-text` class to text span (see AI Text JSX above) | Pastel gradient hover bg (see AI CSS setup) | 30% opacity |
| white | `var(--text-white)` (#FFFFFF) | white at 10% opacity | white faded |

All text buttons start with `background: transparent`. On hover they gain `box-shadow: var(--shadow-xs)` plus the hover background.

### HYPERLINK Buttons

Hyperlink buttons have **no padding, no fixed height, no fixed width**. They look like text links.

| Variant | Text Color | Hover Behavior |
|---------|-----------|----------------|
| primary | `var(--text-brand-primary)` (#FF725C) | underline appears |
| secondary | `var(--text-brand-secondary)` (#098486) | underline appears |
| neutral | `var(--text-primary)` (#353E5A) | underline appears |
| danger | `var(--text-error-primary)` (#F02C00) | underline appears |
| ghost | `var(--text-secondary)` (#495883) | underline appears |
| ai | Apply `ai-gradient-text ai-gradient-underline` classes (see AI Hyperlink JSX above) | gradient underline appears on hover |
| white | `var(--text-white)` (#FFFFFF) | underline appears |

- Default state: underline is present but `transparent` (invisible)
- Hover/focus: underline color becomes `inherit` (visible)
- Text is left-aligned with single-line ellipsis + `break-all`
