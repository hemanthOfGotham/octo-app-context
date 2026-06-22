# Progress Bar Component — Strict Specification

When building any progress bar, progress indicator, loading bar, or completion meter, follow these specifications exactly. Do NOT use default Tailwind progress bars, shadcn progress, or any other progress system.

> **TOKEN RULE:** Every colour in this file is expressed as a CSS custom-property token — `var(--token)` — with the raw hex in parentheses only as a comment. When writing Tailwind classes use the `[var(--token)]` syntax (e.g. `bg-[var(--bg-secondary)]`). Never hard-code a hex value.

---

## Progress Bar Anatomy

A horizontal track with a colored fill representing progress, with optional text labels:

```
Top-aligned text:
  Start Label                    End Label
  ┌──────────────────────────────────────┐
  │████████████████░░░░░░░░░░░░░░░░░░░░░│  ← Track with fill
  └──────────────────────────────────────┘

Center-aligned text:
  Start Label  ┌████████████░░░░░░░░░┐  End Label
               └─────────────────────┘

Bottom-aligned text:
  ┌──────────────────────────────────────┐
  │████████████████░░░░░░░░░░░░░░░░░░░░░│
  └──────────────────────────────────────┘
  Start Label                    End Label
```

- Track is the full-width background bar
- Fill is the colored portion representing current progress
- Text labels (start-text, end-text) can appear above, below, or beside the bar
- Fill width = `(value / max) * 100%`, clamped at 100%

---

## Sizes (Track Height)

| Size | Height | Tailwind |
|------|--------|----------|
| XS | 4px | `h-1` |
| SM | 6px | `h-1.5` |
| MD (Default) | 8px | `h-2` |

---

## Track

| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `var(--bg-tertiary)` (#E8ECF1) | `bg-[var(--bg-tertiary)]` |
| Border radius | 8px | `rounded-lg` |
| Overflow | Hidden | `overflow-hidden` |
| Width | 100% | `w-full` |

---

## Fill

| Property | Value | Tailwind |
|----------|-------|----------|
| Height | 100% | `h-full` |
| Border radius | 8px | `rounded-lg` |
| Width | `(value / max) * 100%` | `style={{ width: '${percentage}%' }}` |
| Max width | 100% | `max-w-full` |
| Transition | width 400ms ease-out, 100ms delay | `transition-all duration-[400ms] ease-out delay-100` |

---

## Color Variants

| Variant | Fill Color | Tailwind |
|---------|-----------|----------|
| Primary (Default) | `var(--bg-brand-solid)` (#FF725C) | `bg-[var(--bg-brand-solid)]` |
| Danger | `var(--bg-error-solid)` (#F02C00) | `bg-[var(--bg-error-solid)]` |
| Secondary | `var(--chart-series-1)` (#0DC1C4) | `bg-[var(--chart-series-1)]` |
| AI | Gradient (purple to teal) | See below |

### AI Gradient

```
background: var(--gradient-ai-progress)
```

---

## Text Labels

| Property | Value | Tailwind |
|----------|-------|----------|
| Typography | 12px / 18px / weight 600 (title-sm) | `text-xs leading-[18px]` + `style={{ fontWeight: 600 }}` |
| Color | `var(--text-primary)` (#353E5A) | `text-[var(--text-primary)]` |

### Start Text
Positioned at the left side of the bar — typically shows a label like "Progress" or "Uploading...".

### End Text
Positioned at the right side of the bar — typically shows the percentage or a value like "75%" or "3/4".

---

## Text Alignment

### Top Alignment
| Property | Value | Tailwind |
|----------|-------|----------|
| Layout | Flex column | `flex flex-col` |
| Text row | Above bar | — |
| Gap | 8px between text and bar | `gap-2` |
| Text layout | Flex, space-between | `flex justify-between` |

### Bottom Alignment
| Property | Value | Tailwind |
|----------|-------|----------|
| Layout | Flex column-reverse | `flex flex-col-reverse` |
| Text row | Below bar | — |
| Gap | 8px between bar and text | `gap-2` |
| Text layout | Flex, space-between | `flex justify-between` |

### Center Alignment
| Property | Value | Tailwind |
|----------|-------|----------|
| Layout | Flex row, centered | `flex items-center` |
| Gap | 12px between elements | `gap-3` |
| Bar | flex-1 (fills remaining space) | `flex-1` |
| Text | No-wrap | `whitespace-nowrap` |

---

## Accessibility

| Property | Value |
|----------|-------|
| Role | `progressbar` |
| aria-valuemin | `0` |
| aria-valuemax | max value |
| aria-valuenow | current value |

---

## Implementation Pattern for Lovable

```jsx
{/* === Progress Bar — Primary, MD, Top-aligned text === */}
<div className="flex flex-col gap-2 w-full">
  {/* Text row */}
  <div className="flex justify-between">
    <span
      className="text-xs leading-[18px] text-[var(--text-primary)]"
      style={{ fontWeight: 600 }}
    >
      Uploading...
    </span>
    <span
      className="text-xs leading-[18px] text-[var(--text-primary)]"
      style={{ fontWeight: 600 }}
    >
      75%
    </span>
  </div>
  {/* Track */}
  <div className="w-full h-2 bg-[var(--bg-tertiary)] rounded-lg overflow-hidden">
    <div
      className="h-full bg-[var(--bg-brand-solid)] rounded-lg transition-all duration-[400ms] ease-out delay-100"
      style={{ width: '75%' }}
      role="progressbar"
      aria-valuemin={0}
      aria-valuemax={100}
      aria-valuenow={75}
    />
  </div>
</div>


{/* === Progress Bar — Danger, SM, Bottom-aligned text === */}
<div className="flex flex-col-reverse gap-2 w-full">
  {/* Text row (rendered below due to flex-col-reverse) */}
  <div className="flex justify-between">
    <span
      className="text-xs leading-[18px] text-[var(--text-primary)]"
      style={{ fontWeight: 600 }}
    >
      Storage Used
    </span>
    <span
      className="text-xs leading-[18px] text-[var(--text-primary)]"
      style={{ fontWeight: 600 }}
    >
      92%
    </span>
  </div>
  {/* Track */}
  <div className="w-full h-1.5 bg-[var(--bg-tertiary)] rounded-lg overflow-hidden">
    <div
      className="h-full bg-[var(--bg-error-solid)] rounded-lg transition-all duration-[400ms] ease-out delay-100"
      style={{ width: '92%' }}
      role="progressbar"
      aria-valuemin={0}
      aria-valuemax={100}
      aria-valuenow={92}
    />
  </div>
</div>


{/* === Progress Bar — Secondary, XS, Center-aligned text === */}
<div className="flex items-center gap-3 w-full">
  {/* Start text */}
  <span
    className="text-xs leading-[18px] text-[var(--text-primary)] whitespace-nowrap"
    style={{ fontWeight: 600 }}
  >
    Progress
  </span>
  {/* Track */}
  <div className="flex-1 h-1 bg-[var(--bg-tertiary)] rounded-lg overflow-hidden">
    <div
      className="h-full bg-[var(--chart-series-1)] rounded-lg transition-all duration-[400ms] ease-out delay-100"
      style={{ width: '60%' }}
      role="progressbar"
      aria-valuemin={0}
      aria-valuemax={100}
      aria-valuenow={60}
    />
  </div>
  {/* End text */}
  <span
    className="text-xs leading-[18px] text-[var(--text-primary)] whitespace-nowrap"
    style={{ fontWeight: 600 }}
  >
    60%
  </span>
</div>


{/* === Progress Bar — AI gradient, MD, Top-aligned text === */}
<div className="flex flex-col gap-2 w-full">
  <div className="flex justify-between">
    <span
      className="text-xs leading-[18px] text-[var(--text-primary)]"
      style={{ fontWeight: 600 }}
    >
      AI Processing
    </span>
    <span
      className="text-xs leading-[18px] text-[var(--text-primary)]"
      style={{ fontWeight: 600 }}
    >
      45%
    </span>
  </div>
  <div className="w-full h-2 bg-[var(--bg-tertiary)] rounded-lg overflow-hidden">
    <div
      className="h-full rounded-lg transition-all duration-[400ms] ease-out delay-100"
      style={{
        width: '45%',
        background: 'var(--gradient-ai-progress)'
      }}
      role="progressbar"
      aria-valuemin={0}
      aria-valuemax={100}
      aria-valuenow={45}
    />
  </div>
</div>


{/* === Progress Bar — No text, MD, Primary === */}
<div className="w-full h-2 bg-[var(--bg-tertiary)] rounded-lg overflow-hidden">
  <div
    className="h-full bg-[var(--bg-brand-solid)] rounded-lg transition-all duration-[400ms] ease-out delay-100"
    style={{ width: '50%' }}
    role="progressbar"
    aria-valuemin={0}
    aria-valuemax={100}
    aria-valuenow={50}
  />
</div>


{/* === Progress Bar — 0% (empty) === */}
<div className="w-full h-2 bg-[var(--bg-tertiary)] rounded-lg overflow-hidden">
  <div
    className="h-full bg-[var(--bg-brand-solid)] rounded-lg transition-all duration-[400ms] ease-out delay-100"
    style={{ width: '0%' }}
    role="progressbar"
    aria-valuemin={0}
    aria-valuemax={100}
    aria-valuenow={0}
  />
</div>


{/* === Progress Bar — 100% (complete) === */}
<div className="w-full h-2 bg-[var(--bg-tertiary)] rounded-lg overflow-hidden">
  <div
    className="h-full bg-[var(--bg-brand-solid)] rounded-lg transition-all duration-[400ms] ease-out delay-100"
    style={{ width: '100%' }}
    role="progressbar"
    aria-valuemin={0}
    aria-valuemax={100}
    aria-valuenow={100}
  />
</div>
```

### Key Implementation Rules

1. **Track background is `var(--bg-tertiary)` (#E8ECF1)** with 8px border-radius (`rounded-lg`)
2. **Track has `overflow-hidden`** so the fill doesn't bleed past rounded corners
3. **Fill border-radius matches track** — both use `rounded-lg` (8px)
4. **Fill width is set via inline style** — `style={{ width: '${percentage}%' }}`
5. **Fill animation is 400ms ease-out with 100ms delay** — smooth progression effect
6. **Default size is MD (8px)**, XS is 4px, SM is 6px
7. **Primary color is `var(--bg-brand-solid)` (#FF725C)**, Danger is `var(--bg-error-solid)` (#F02C00), Secondary is `#0DC1C4`
8. **AI variant uses a gradient** — purple to teal (`var(--gradient-ai-progress)`) (left to right)
9. **Text labels use 12px/18px/600** (title-sm) in `var(--text-primary)` (#353E5A)
10. **Top alignment**: `flex flex-col gap-2`, text row above bar
11. **Bottom alignment**: `flex flex-col-reverse gap-2`, text row below bar
12. **Center alignment**: `flex items-center gap-3`, bar is `flex-1`, text is `whitespace-nowrap`
13. **Always include ARIA attributes**: `role="progressbar"`, `aria-valuemin`, `aria-valuemax`, `aria-valuenow`
14. **Fill clamped at 100%** — never exceeds track width
15. **Text is optional** — the bar can be used standalone without labels
