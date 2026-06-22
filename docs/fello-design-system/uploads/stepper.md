# Stepper Component — Strict Specification

When building any stepper, wizard, multi-step process indicator, or progress tracker, follow these specifications exactly. Do NOT use default Tailwind stepper styles, shadcn steppers, or any other stepper system.

> **TOKEN RULE:** Every color in the code examples below uses CSS variable tokens or Tailwind palette names — never raw hex. When implementing, copy the token references exactly. See `design-tokens.md` for definitions.

---

## Stepper Anatomy

A stepper is a sequence of steps with circle indicators, labels, and connecting lines:

```
Horizontal:
  (○) ———— (○) ———— (✓) ———— (○)
  Step 1    Step 2    Step 3    Step 4

Vertical:
  (○) Step 1 — Description
  │   [Content area]
  │
  (✓) Step 2 — Description
  │   [Content area]
  │
  (○) Step 3 — Description
      [Content area]
```

- Each step has a circle indicator, label, and optional description
- Steps are connected by lines (horizontal or vertical)
- **CRITICAL: Circles are EMPTY by default — do NOT show step numbers (1, 2, 3…) inside the circles**
- Completed circles show a tick icon, error circles show an alert icon
- Default and selected circles are EMPTY (border only, no content inside)

---

## Layouts

### Horizontal
- Steps arranged in a row
- Connector: 24px wide × 1px tall line between steps
- Content panel below all step headers

### Vertical
- Steps stacked in a column
- Connector: 1px wide × full height vertical line on the left
- Content expands below each step header when selected

---

## Circle Indicator

The circle is the step icon element. **Circles do NOT contain step numbers.**

| Property | Value | Tailwind |
|----------|-------|----------|
| Size | 20px × 20px | `w-5 h-5` |
| Border width | 2.5px | `border-[2.5px]` |
| Shape | Fully circular | `rounded-full` |
| Layout | Flex, centered | `flex items-center justify-center` |
| Content | Empty (default/selected), tick icon (completed), alert icon (error) | — |

---

## Step States — Circle Colors

### Default (Upcoming Step)
| Property | Value | Tailwind |
|----------|-------|----------|
| Border | 2.5px solid `var(--fg-disabled)` (#8EA1AF) | `border-[2.5px] border-[var(--fg-disabled)]` |
| Background | `var(--bg-primary)` (#FFFFFF) | `bg-[var(--bg-primary)]` |
| Content | **Empty — no number, no icon** | — |

### Selected (Active Step)
| Property | Value | Tailwind |
|----------|-------|----------|
| Border | 2.5px solid `var(--border-brand-solid)` (#FF725C) | `border-[2.5px] border-[var(--border-brand-solid)]` |
| Background | `var(--bg-primary)` (#FFFFFF) | `bg-[var(--bg-primary)]` |
| Box shadow | `0 0 0 4px rgba(255,114,92,0.24)` (ring-brand-primary) | `shadow-[0_0_0_4px_rgba(255,114,92,0.24)]` |
| Content | **Empty — no number, no icon** | — |

### Completed
| Property | Value | Tailwind |
|----------|-------|----------|
| Border | 2.5px solid `var(--fg-quaternary)` (#9298A9) | `border-[2.5px] border-[var(--fg-quaternary)]` |
| Background | `var(--fg-quaternary)` (#9298A9) | `bg-[var(--fg-quaternary)]` |
| Icon | ✓ Tick/checkmark, 16px, white | — |
| Content | Tick icon (replaces number) | — |

### Error
| Property | Value | Tailwind |
|----------|-------|----------|
| Border | 2.5px solid `var(--fg-error-primary)` (#F02C00) | `border-[2.5px] border-[var(--fg-error-primary)]` |
| Background | `var(--bg-error-solid)` (#F02C00) | `bg-[var(--bg-error-solid)]` |
| Icon | ⚠ Alert circle, 16px, white | — |
| Content | Alert icon (replaces number) | — |

### Disabled
| Property | Value |
|----------|-------|
| Same as default circle styling | — |
| Label becomes disabled color | — |

---

## Step Header

The step header contains the circle indicator + label + description.

| Property | Value | Tailwind |
|----------|-------|----------|
| Padding | 8px vertical, 12px horizontal | `py-2 px-3` |
| Padding-left | 44px (makes space for absolutely positioned circle) | `pl-11` |
| Border radius | 8px | `rounded-lg` |
| Cursor | Pointer | `cursor-pointer` |
| Position | Relative (for circle positioning) | `relative` |

**Circle Position within Header:**
| Property | Value |
|----------|-------|
| Position | Absolute |
| Left | 12px |
| Top | 50%, translateY(-50%) — vertically centered |

**Hover State:**
| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `var(--bg-secondary_hover)` (#E8ECF1) | `hover:bg-[var(--bg-secondary_hover)]` |
| Transition | background-color 150ms ease-out | `transition-colors duration-150 ease-out` |

**Error Hover State:**
| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `var(--bg-error-primary)` (#FFEBE6) | `hover:bg-[var(--bg-error-primary)]` |

---

## Typography

### Step Label
| Property | Value | Tailwind |
|----------|-------|----------|
| Font size | 12px | `text-xs` |
| Line height | 18px | `leading-[18px]` |
| Font weight | 500 | `font-medium` |

**Label Color by State:**
| State | Color | Tailwind |
|-------|-------|----------|
| Default / Disabled | `var(--text-disabled)` (#8EA1AF) | `text-[var(--text-disabled)]` |
| Selected (active) | `var(--text-primary)` (#353E5A) | `text-[var(--text-primary)]` |
| Completed | `var(--text-tertiary)` (#6B748E) | `text-[var(--text-tertiary)]` |
| Error | `var(--text-error-primary)` (#F02C00) | `text-[var(--text-error-primary)]` |

### Step Description
| Property | Value | Tailwind |
|----------|-------|----------|
| Font size | 12px | `text-xs` |
| Line height | 18px | `leading-[18px]` |
| Font weight | 400 | `font-normal` |

**Description Color by State:**
| State | Color | Tailwind |
|-------|-------|----------|
| Default / Disabled | `var(--text-placeholder)` (#BDC8D3) | `text-[var(--text-placeholder)]` |
| Selected / Completed | `var(--text-tertiary)` (#6B748E) | `text-[var(--text-tertiary)]` |
| Error | `var(--text-error-primary)` (#F02C00) | `text-[var(--text-error-primary)]` |

---

## Connector Lines

### Horizontal Connector
| Property | Value | Tailwind |
|----------|-------|----------|
| Width | 24px | `w-6` |
| Height | 1px | `h-px` |
| Background | `var(--border-secondary)` (#CCD6DC) | `bg-[var(--border-secondary)]` |
| Alignment | Vertically centered between step headers | — |
| Visibility | Between steps only (not after last step) | — |

### Vertical Connector
| Property | Value |
|----------|-------|
| Width | 1px |
| Height | 100% of content container |
| Background | `var(--border-secondary)` (#CCD6DC) |
| Position | Absolute, left: 22px (aligned with circle center) |
| Visibility | Between steps only (not after last step) |

---

## Accessibility

| Property | Value |
|----------|-------|
| Stepper role | `tablist` |
| Step header role | `tab` |
| Content role | `tabpanel` |
| `aria-orientation` | "horizontal" or "vertical" |
| `aria-selected` | Reflects selected state |
| `aria-disabled` | Reflects disabled state |
| Keyboard | Arrow keys navigate steps |

---

## Implementation Pattern for Lovable

```jsx
{/* === Horizontal Stepper === */}
<div>
  {/* Step Headers */}
  <div className="flex items-center" role="tablist" aria-orientation="horizontal">
    {/* Step 1 — Completed */}
    <button
      role="tab"
      aria-selected="false"
      className="
        relative pl-11 py-2 px-3
        rounded-lg cursor-pointer
        hover:bg-[var(--bg-secondary_hover)]
        transition-colors duration-150 ease-out
      "
    >
      {/* Circle — completed */}
      <div className="
        absolute left-3 top-1/2 -translate-y-1/2
        w-5 h-5 rounded-full
        flex items-center justify-center
        border-[2.5px] border-[var(--fg-quaternary)] bg-[var(--fg-quaternary)]
      ">
        <svg className="w-3 h-3 text-white" viewBox="0 0 12 12" fill="none">
          <path d="M2 6l3 3 5-5" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"/>
        </svg>
      </div>
      <div>
        <p className="text-xs leading-[18px] font-medium text-[var(--text-tertiary)]">Step 1</p>
        <p className="text-xs leading-[18px] font-normal text-[var(--text-tertiary)]">Account setup</p>
      </div>
    </button>

    {/* Connector */}
    <div className="w-6 h-px bg-[var(--border-secondary)]" />

    {/* Step 2 — Selected (active) */}
    <button
      role="tab"
      aria-selected="true"
      className="
        relative pl-11 py-2 px-3
        rounded-lg cursor-pointer
        hover:bg-[var(--bg-secondary_hover)]
        transition-colors duration-150 ease-out
      "
    >
      {/* Circle — selected (EMPTY, no number) */}
      <div className="
        absolute left-3 top-1/2 -translate-y-1/2
        w-5 h-5 rounded-full
        border-[2.5px] border-[var(--border-brand-solid)] bg-[var(--bg-primary)]
        shadow-[0_0_0_4px_rgba(255,114,92,0.24)]
      " />
      <div>
        <p className="text-xs leading-[18px] font-medium text-[var(--text-primary)]">Step 2</p>
        <p className="text-xs leading-[18px] font-normal text-[var(--text-tertiary)]">Personal info</p>
      </div>
    </button>

    {/* Connector */}
    <div className="w-6 h-px bg-[var(--border-secondary)]" />

    {/* Step 3 — Default (upcoming) */}
    <button
      role="tab"
      aria-selected="false"
      className="
        relative pl-11 py-2 px-3
        rounded-lg cursor-pointer
        hover:bg-[var(--bg-secondary_hover)]
        transition-colors duration-150 ease-out
      "
    >
      {/* Circle — default (EMPTY, no number) */}
      <div className="
        absolute left-3 top-1/2 -translate-y-1/2
        w-5 h-5 rounded-full
        border-[2.5px] border-[var(--fg-disabled)] bg-[var(--bg-primary)]
      " />
      <div>
        <p className="text-xs leading-[18px] font-medium text-[var(--text-disabled)]">Step 3</p>
        <p className="text-xs leading-[18px] font-normal text-[var(--text-placeholder)]">Confirmation</p>
      </div>
    </button>
  </div>

  {/* Active Panel */}
  <div role="tabpanel" className="py-4">
    Step 2 content goes here
  </div>
</div>


{/* === Vertical Stepper === */}
<div role="tablist" aria-orientation="vertical">

  {/* Step 1 — Completed + expanded content */}
  <div>
    <button
      role="tab"
      aria-selected="false"
      className="
        relative w-full text-left
        pl-11 py-2 px-3
        rounded-lg cursor-pointer
        hover:bg-[var(--bg-secondary_hover)]
        transition-colors duration-150 ease-out
      "
    >
      <div className="
        absolute left-3 top-1/2 -translate-y-1/2
        w-5 h-5 rounded-full
        flex items-center justify-center
        border-[2.5px] border-[var(--fg-quaternary)] bg-[var(--fg-quaternary)]
      ">
        <svg className="w-3 h-3 text-white" viewBox="0 0 12 12" fill="none">
          <path d="M2 6l3 3 5-5" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"/>
        </svg>
      </div>
      <p className="text-xs leading-[18px] font-medium text-[var(--text-tertiary)]">Account setup</p>
      <p className="text-xs leading-[18px] font-normal text-[var(--text-tertiary)]">Enter your credentials</p>
    </button>
    {/* Vertical connector + content */}
    <div className="relative pl-11">
      {/* Vertical line */}
      <div className="absolute left-[22px] top-0 w-px h-full bg-[var(--border-secondary)]" />
      <div className="py-3">Completed step content</div>
    </div>
  </div>

  {/* Step 2 — Selected */}
  <div>
    <button
      role="tab"
      aria-selected="true"
      className="
        relative w-full text-left
        pl-11 py-2 px-3
        rounded-lg cursor-pointer
        hover:bg-[var(--bg-secondary_hover)]
        transition-colors duration-150 ease-out
      "
    >
      {/* Circle — selected (EMPTY, no number) */}
      <div className="
        absolute left-3 top-1/2 -translate-y-1/2
        w-5 h-5 rounded-full
        border-[2.5px] border-[var(--border-brand-solid)] bg-[var(--bg-primary)]
        shadow-[0_0_0_4px_rgba(255,114,92,0.24)]
      " />
      <p className="text-xs leading-[18px] font-medium text-[var(--text-primary)]">Personal info</p>
      <p className="text-xs leading-[18px] font-normal text-[var(--text-tertiary)]">Add your details</p>
    </button>
    {/* Vertical connector + content */}
    <div className="relative pl-11">
      <div className="absolute left-[22px] top-0 w-px h-full bg-[var(--border-secondary)]" />
      <div className="py-3">Active step content here</div>
    </div>
  </div>

  {/* Step 3 — Default (LAST — no connector) */}
  <div>
    <button
      role="tab"
      aria-selected="false"
      className="
        relative w-full text-left
        pl-11 py-2 px-3
        rounded-lg cursor-pointer
        hover:bg-[var(--bg-secondary_hover)]
        transition-colors duration-150 ease-out
      "
    >
      {/* Circle — default (EMPTY, no number) */}
      <div className="
        absolute left-3 top-1/2 -translate-y-1/2
        w-5 h-5 rounded-full
        border-[2.5px] border-[var(--fg-disabled)] bg-[var(--bg-primary)]
      " />
      <p className="text-xs leading-[18px] font-medium text-[var(--text-disabled)]">Confirmation</p>
      <p className="text-xs leading-[18px] font-normal text-[var(--text-placeholder)]">Review and submit</p>
    </button>
    {/* No connector on last step */}
  </div>
</div>


{/* === Error State Step === */}
<button
  role="tab"
  className="
    relative pl-11 py-2 px-3
    rounded-lg cursor-pointer
    hover:bg-[var(--bg-error-primary)]
    transition-colors duration-150 ease-out
  "
>
  {/* Circle — error */}
  <div className="
    absolute left-3 top-1/2 -translate-y-1/2
    w-5 h-5 rounded-full
    flex items-center justify-center
    border-[2.5px] border-[var(--fg-error-primary)] bg-[var(--bg-error-solid)]
  ">
    <svg className="w-3 h-3 text-white" viewBox="0 0 12 12" fill="none">
      <circle cx="6" cy="6" r="5" stroke="currentColor" strokeWidth="1.5"/>
      <path d="M6 4v3M6 8.5v.01" stroke="currentColor" strokeWidth="1.5" strokeLinecap="round"/>
    </svg>
  </div>
  <p className="text-xs leading-[18px] font-medium text-[var(--text-error-primary)]">Verification</p>
  <p className="text-xs leading-[18px] font-normal text-[var(--text-error-primary)]">Identity check failed</p>
</button>
```

### Step State Quick Reference

| State | Circle Border | Circle BG | Circle Content | Label Color | Description Color |
|-------|--------------|-----------|----------------|-------------|-------------------|
| Default | `var(--fg-disabled)` (#8EA1AF) | White | **Empty** | `var(--text-disabled)` (#8EA1AF) | `var(--text-placeholder)` (#BDC8D3) |
| Selected | `var(--border-brand-solid)` (#FF725C) + ring | White | **Empty** | `var(--text-primary)` (#353E5A) | `var(--text-tertiary)` (#6B748E) |
| Completed | `var(--fg-quaternary)` (#9298A9) | `var(--fg-quaternary)` (#9298A9) | ✓ Tick (white) | `var(--text-tertiary)` (#6B748E) | `var(--text-tertiary)` (#6B748E) |
| Error | `var(--fg-error-primary)` (#F02C00) | `var(--bg-error-solid)` (#F02C00) | ⚠ Alert (white) | `var(--text-error-primary)` (#F02C00) | `var(--text-error-primary)` (#F02C00) |
| Disabled | `var(--fg-disabled)` (#8EA1AF) | White | **Empty** | `var(--text-disabled)` (#8EA1AF) | `var(--text-placeholder)` (#BDC8D3) |

### Key Implementation Rules

1. **CRITICAL: Circles do NOT show step numbers** — default and selected circles are EMPTY (border only, no text/number inside)
2. **Circle is always 20px × 20px** with 2.5px border — fully circular
3. **Selected state has an orange ring** — `0 0 0 4px rgba(255,114,92,0.24)` box-shadow
4. **Completed state fills the circle** with `var(--fg-quaternary)` (#9298A9) gray and shows a white tick icon
5. **Error state fills the circle** with `var(--bg-error-solid)` (#F02C00) red and shows a white alert icon
6. **Step header has 44px left padding** to make room for the absolutely positioned circle
7. **Circle is positioned at left: 12px**, vertically centered within the header
8. **Horizontal connector is 24px × 1px** in `var(--border-secondary)` (#CCD6DC) between step headers
9. **Vertical connector is 1px wide** at `left: 22px` (aligned with circle center) — full height of content area
10. **Last step never has a connector** — neither horizontal nor vertical
11. **Hover background is `var(--bg-secondary_hover)` (#E8ECF1)** for normal steps, `var(--bg-error-primary)` (#FFEBE6) for error steps
12. **Label uses 12px/18px/500** (caption), description uses **12px/18px/400** (paragraph)
13. **Disabled steps** have same circle as default but use disabled text colors
