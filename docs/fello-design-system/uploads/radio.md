# Radio Component — Strict Specification

When building any radio button, radio input, or single-select option control, follow these specifications exactly. Do NOT use default Tailwind radio buttons, shadcn radio groups, or any other radio system.

> **TOKEN RULE — every raw hex color in this spec has a CSS-variable token.**
> In **tables** write the token first: `var(--token)` (#HEX).
> In **Tailwind classes** use the bracket syntax: `bg-[var(--bg-token)]`, `text-[var(--text-token)]`, `border-[var(--border-token)]`.
> In **inline styles** use: `style={{ color: 'var(--text-token)' }}`.
> In **prose** write: `var(--token)` (#HEX).
> Never use a bare hex — always pair it with its token.

---

## Radio Anatomy

A circular control with an optional label and sub-text:

```
  (○) Label Text
      Sub-text description

  (●) Selected Option        ← filled teal circle
      Description here
```

- Circular control on the left
- Label text to the right (optional)
- Sub-text below the label (optional, lighter color)
- Checked state shows a filled inner circle with white border

---

## Sizes

| Size | Control Diameter | Inner Circle | Gap (control → label) |
|------|-----------------|-------------|----------------------|
| Small | 16px | 14px | 8px |
| Medium (Default) | 16px | 14px | 8px |
| Large | 20px | 18px | 12px |

---

## Control Styling

### Unchecked (Default)

| Property | Value | Tailwind |
|----------|-------|----------|
| Width / Height (SM/MD) | 16px | `w-4 h-4` |
| Width / Height (LG) | 20px | `w-5 h-5` |
| Border | 1px solid `var(--border-primary)` (#BDC8D3) | `border border-[var(--border-primary)]` |
| Background | `var(--bg-primary)` (#FFFFFF) | `bg-[var(--bg-primary)]` |
| Border radius | Full circle | `rounded-full` |
| Cursor | Pointer | `cursor-pointer` |
| Flex shrink | 0 | `shrink-0` |
| Transition | border-color, box-shadow 150ms ease-out | `transition-all duration-150 ease-out` |

### Checked

| Property | Value | Tailwind |
|----------|-------|----------|
| Border | 1px solid `var(--border-brand-secondary-solid)` (#0BA5A7) | `border-[var(--border-brand-secondary-solid)]` |
| Inner circle bg | `var(--bg-brand-secondary-solid)` (#098486) | — |
| Inner circle border | 2px solid `var(--bg-primary)` (#FFFFFF) | — |
| Inner circle size (SM/MD) | 14px | — |
| Inner circle size (LG) | 18px | — |
| Inner circle position | Centered (absolute + translate) | — |

### Hover (Not Disabled)

| Property | Value | Tailwind |
|----------|-------|----------|
| Border color | `var(--border-brand-secondary-solid)` (#0BA5A7) | `hover:border-[var(--border-brand-secondary-solid)]` |
| Checked inner fill | `var(--bg-brand-secondary-solid_hover)` (#0BA5A7) | — |

### Focus (Not Disabled)

| Property | Value | Tailwind |
|----------|-------|----------|
| Box shadow | `0 0 0 4px var(--ring-brand-secondary)` (rgba(9,132,134,0.24)) | `focus-visible:shadow-[0_0_0_4px_var(--ring-brand-secondary)]` |
| Border color | `var(--border-brand-secondary-solid)` (#0BA5A7) | `focus-visible:border-[var(--border-brand-secondary-solid)]` |
| Outline | None | `outline-none` |

### Disabled (Unchecked)

| Property | Value | Tailwind |
|----------|-------|----------|
| Border | 1px solid `var(--border-primary)` (#BDC8D3) | `border-[var(--border-primary)]` |
| Background | `var(--bg-disabled)` (#E8ECF1) | `bg-[var(--bg-disabled)]` |
| Cursor | Not-allowed | `cursor-not-allowed` |
| Pointer events | None | `pointer-events-none` |

### Disabled (Checked)

| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `var(--bg-brand-secondary-solid_disabled)` (#0BA5A7f2) | `bg-[var(--bg-brand-secondary-solid_disabled)]` |
| Border | Transparent | `border-transparent` |
| Inner circle | `var(--bg-brand-secondary-solid_disabled)` (#0BA5A7f2) with 2px `var(--bg-primary)` (#FFFFFF) border | — |

---

## Typography

### Label Text (Caption weight 500)

| Size | Font Size | Line Height | Tailwind |
|------|-----------|-------------|----------|
| Small | 12px | 18px | `text-xs leading-[18px]` + `style={{ fontWeight: 500 }}` |
| Medium | 14px | 20px | `text-sm leading-5` + `style={{ fontWeight: 500 }}` |
| Large | 16px | 24px | `text-base leading-6` + `style={{ fontWeight: 500 }}` |

### Sub-text (Paragraph weight 400)

| Size | Font Size | Line Height | Tailwind |
|------|-----------|-------------|----------|
| Small | 10px | 16px | `text-[10px] leading-4` |
| Medium | 12px | 18px | `text-xs leading-[18px]` |
| Large | 14px | 20px | `text-sm leading-5` |

### Text Colors

| Element | Color | Hex | Tailwind |
|---------|-------|-----|----------|
| Label | text-primary | `var(--text-primary)` (#353E5A) | `text-[var(--text-primary)]` |
| Sub-text | text-tertiary | `var(--text-tertiary)` (#6B748E) | `text-[var(--text-tertiary)]` |
| Disabled text | text-disabled | `var(--text-disabled)` (#8EA1AF) | `text-[var(--text-disabled)]` |
| Disabled sub-text | text-disabled | `var(--text-disabled)` (#8EA1AF) | `text-[var(--text-disabled)]` |

---

## Implementation Pattern for Lovable

```jsx
{/* === Radio — Medium, with label and sub-text === */}
<label className="inline-flex items-start gap-2 cursor-pointer text-[var(--text-primary)]">
  {/* Radio control */}
  <div className="relative w-4 h-4 shrink-0 mt-0.5">
    <input
      type="radio"
      name="group1"
      value="option1"
      className="
        appearance-none w-4 h-4
        border border-[var(--border-primary)] bg-[var(--bg-primary)] rounded-full
        shrink-0 cursor-pointer
        transition-all duration-150 ease-out
        hover:border-[var(--border-brand-secondary-solid)]
        focus-visible:shadow-[0_0_0_4px_var(--ring-brand-secondary)]
        focus-visible:border-[var(--border-brand-secondary-solid)]
        checked:border-[var(--border-brand-secondary-solid)]
        outline-none
      "
    />
    {/* Inner circle (shown when checked — use peer selector or state) */}
  </div>
  {/* Label wrapper */}
  <span className="inline-flex flex-col gap-0.5">
    <span
      className="text-sm leading-5"
      style={{ fontWeight: 500 }}
    >
      Enroll All Contacts
    </span>
    <span className="text-xs leading-[18px] text-[var(--text-tertiary)]">
      This will enroll all the contacts
    </span>
  </span>
</label>


{/* === Radio Group — Medium size, vertical stack === */}
<div className="flex flex-col gap-3" role="radiogroup">
  {/* Option 1 — selected */}
  <label className="inline-flex items-start gap-2 cursor-pointer text-[var(--text-primary)]">
    <div className="relative w-4 h-4 shrink-0 mt-0.5">
      <input
        type="radio"
        name="preferences"
        value="all"
        defaultChecked
        className="
          peer appearance-none w-4 h-4
          border border-[var(--border-primary)] bg-[var(--bg-primary)] rounded-full
          cursor-pointer transition-all duration-150 ease-out
          hover:border-[var(--border-brand-secondary-solid)]
          focus-visible:shadow-[0_0_0_4px_var(--ring-brand-secondary)]
          focus-visible:border-[var(--border-brand-secondary-solid)]
          checked:border-[var(--border-brand-secondary-solid)]
          outline-none
        "
      />
      {/* Checked inner circle */}
      <div
        className="
          absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2
          w-3.5 h-3.5 rounded-full
          bg-[var(--bg-brand-secondary-solid)] border-2 border-[var(--bg-primary)]
          pointer-events-none
          hidden peer-checked:block
        "
      />
    </div>
    <span className="inline-flex flex-col gap-0.5">
      <span className="text-sm leading-5" style={{ fontWeight: 500 }}>
        All Contacts
      </span>
      <span className="text-xs leading-[18px] text-[var(--text-tertiary)]">
        Includes every contact in the system
      </span>
    </span>
  </label>

  {/* Option 2 — unselected */}
  <label className="inline-flex items-start gap-2 cursor-pointer text-[var(--text-primary)]">
    <div className="relative w-4 h-4 shrink-0 mt-0.5">
      <input
        type="radio"
        name="preferences"
        value="active"
        className="
          peer appearance-none w-4 h-4
          border border-[var(--border-primary)] bg-[var(--bg-primary)] rounded-full
          cursor-pointer transition-all duration-150 ease-out
          hover:border-[var(--border-brand-secondary-solid)]
          focus-visible:shadow-[0_0_0_4px_var(--ring-brand-secondary)]
          focus-visible:border-[var(--border-brand-secondary-solid)]
          checked:border-[var(--border-brand-secondary-solid)]
          outline-none
        "
      />
      <div
        className="
          absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2
          w-3.5 h-3.5 rounded-full
          bg-[var(--bg-brand-secondary-solid)] border-2 border-[var(--bg-primary)]
          pointer-events-none
          hidden peer-checked:block
        "
      />
    </div>
    <span className="inline-flex flex-col gap-0.5">
      <span className="text-sm leading-5" style={{ fontWeight: 500 }}>
        Active Only
      </span>
      <span className="text-xs leading-[18px] text-[var(--text-tertiary)]">
        Only currently active contacts
      </span>
    </span>
  </label>

  {/* Option 3 — disabled */}
  <label className="inline-flex items-start gap-2 text-[var(--text-disabled)] pointer-events-none cursor-not-allowed">
    <div className="relative w-4 h-4 shrink-0 mt-0.5">
      <input
        type="radio"
        name="preferences"
        value="archived"
        disabled
        className="
          appearance-none w-4 h-4
          border border-[var(--border-primary)] bg-[var(--bg-disabled)] rounded-full
          cursor-not-allowed
        "
      />
    </div>
    <span className="inline-flex flex-col gap-0.5">
      <span className="text-sm leading-5" style={{ fontWeight: 500 }}>
        Archived
      </span>
      <span className="text-xs leading-[18px] text-[var(--text-disabled)]">
        This option is unavailable
      </span>
    </span>
  </label>
</div>


{/* === Radio — Large size, no sub-text === */}
<label className="inline-flex items-start gap-3 cursor-pointer text-[var(--text-primary)]">
  <div className="relative w-5 h-5 shrink-0 mt-0.5">
    <input
      type="radio"
      name="size-example"
      value="large"
      className="
        peer appearance-none w-5 h-5
        border border-[var(--border-primary)] bg-[var(--bg-primary)] rounded-full
        cursor-pointer transition-all duration-150 ease-out
        hover:border-[var(--border-brand-secondary-solid)]
        checked:border-[var(--border-brand-secondary-solid)]
        outline-none
      "
    />
    <div
      className="
        absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2
        w-[18px] h-[18px] rounded-full
        bg-[var(--bg-brand-secondary-solid)] border-2 border-[var(--bg-primary)]
        pointer-events-none
        hidden peer-checked:block
      "
    />
  </div>
  <span className="text-base leading-6" style={{ fontWeight: 500 }}>
    Large Radio Option
  </span>
</label>
```

### Key Implementation Rules

1. **Radio control is teal** (`var(--bg-brand-secondary-solid)` (#098486)) when checked — NOT the brand primary orange
2. **Hover changes to lighter teal** `var(--bg-brand-secondary-solid_hover)` (#0BA5A7) — applies to border and inner circle
3. **Focus ring is teal** — `0 0 0 4px var(--ring-brand-secondary)` (rgba(9,132,134,0.24)) with `var(--border-brand-secondary-solid)` (#0BA5A7) border
4. **Inner circle uses `border: 2px solid var(--bg-primary)` (#FFFFFF)** — creates the distinctive donut effect
5. **Inner circle is 14px (SM/MD)** or **18px (LG)** — 2px smaller than the outer control
6. **Use `peer` / `peer-checked`** pattern in Tailwind to show/hide inner circle
7. **Label uses caption weights (500)** — NOT title (600) or paragraph (400)
8. **Sub-text uses paragraph weight (400)** in `var(--text-tertiary)` (#6B748E)
9. **Gap between control and label is 8px (SM/MD)** or **12px (LG)**
10. **Gap between label and sub-text is 2px** (`gap-0.5`)
11. **Disabled unchecked: gray bg `var(--bg-disabled)` (#E8ECF1)** with gray border `var(--border-primary)` (#BDC8D3)
12. **Disabled checked: semi-transparent teal** `var(--bg-brand-secondary-solid_disabled)` (#0BA5A7f2) — NOT fully opaque
13. **All disabled text becomes `var(--text-disabled)` (#8EA1AF)** including sub-text
14. **Control aligns to flex-start** (top) with a small top margin when label/sub-text present
15. **Transition is 150ms ease-out** on border-color and box-shadow
