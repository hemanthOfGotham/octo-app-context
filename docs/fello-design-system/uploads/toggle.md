# Toggle Component — Strict Specification

When building any toggle switch, on/off control, or boolean selector, follow these specifications exactly. Do NOT use default Tailwind toggle styles, shadcn switches, or any other toggle system.

> **TOKEN RULE:** Every color in this spec is expressed as a CSS variable token. In Tailwind classes use `bg-[var(--token)]` / `text-[var(--token)]`. In inline styles use `'var(--token)'`. In prose and tables, show `var(--token)` (#hex) for reference.

---

## Toggle Anatomy

A toggle is a horizontal flex container with an optional label:

```
[switch track + slider]  [label]
                         [sub-text]
```

- Layout: `display: flex`, `align-items: flex-start`
- Gap: varies by size (see below)
- Switch is always on the left by default (can be moved to right)
- Label and sub-text stack vertically

---

## Sizes

There are 3 toggle sizes:

### Small
| Property | Value |
|----------|-------|
| Track | 28px × 16px |
| Slider (knob) | 12px × 12px |
| Checked slide | `translateX(12px)` |
| Label typography | 12px / 18px / weight 500 (`text-xs leading-[18px] font-medium`) |
| Sub-text typography | 10px / 16px / weight 400 (`text-[10px] leading-[16px] font-normal`) |
| Gap | 8px (`gap-2`) |

### Medium (Default)
| Property | Value |
|----------|-------|
| Track | 36px × 20px |
| Slider (knob) | 16px × 16px |
| Checked slide | `translateX(16px)` |
| Label typography | 14px / 20px / weight 500 (`text-sm leading-5 font-medium`) |
| Sub-text typography | 12px / 18px / weight 400 (`text-xs leading-[18px] font-normal`) |
| Gap | 8px (`gap-2`) |

### Large
| Property | Value |
|----------|-------|
| Track | 44px × 24px |
| Slider (knob) | 20px × 20px |
| Checked slide | `translateX(20px)` |
| Label typography | 16px / 24px / weight 500 (`text-base leading-6 font-medium`) |
| Sub-text typography | 14px / 20px / weight 400 (`text-sm leading-5 font-normal`) |
| Gap | 12px (`gap-3`) |

---

## Track (Container)

The track is the pill-shaped background behind the slider.

| Property | Value | Tailwind |
|----------|-------|----------|
| Shape | Fully rounded | `rounded-full` |
| Position | Relative | `relative` |
| Cursor | Pointer | `cursor-pointer` |
| Transition | `background-color`, `box-shadow` | `transition-[background-color,box-shadow] duration-150 ease-out` |

---

## Slider (Knob)

The slider is the circular handle that moves left/right.

| Property | Value | Tailwind |
|----------|-------|----------|
| Shape | Fully circular | `rounded-full` |
| Background | `var(--bg-primary)` (#FFFFFF) | `bg-[var(--bg-primary)]` |
| Shadow | `var(--shadow-sm)` | `shadow-sm` |
| Position | Absolute, top: 2px, left: 2px | `absolute top-0.5 left-0.5` |
| Transition | `transform` | `transition-transform duration-150 ease-out` |

---

## States — Complete Color Spec

### Unchecked (Off)
| Property | Value |
|----------|-------|
| Track background | `var(--bg-tertiary)` (#E8ECF1) |
| Slider | White with shadow |

### Unchecked Hover
| Property | Value |
|----------|-------|
| Track background | `var(--border-secondary)` (#CCD6DC) |

### Unchecked Focus-Visible
| Property | Value |
|----------|-------|
| Track background | `var(--border-secondary)` (#CCD6DC) |
| Box shadow | `var(--ring-gray-primary)` |

### Checked (On)
| Property | Value |
|----------|-------|
| Track background | `var(--bg-brand-secondary-solid)` (#098486) |
| Slider | White, translated right by knob width |

### Checked Hover
| Property | Value |
|----------|-------|
| Track background | `var(--bg-brand-secondary-solid_hover)` (#0BA5A7) |

### Checked Focus-Visible
| Property | Value |
|----------|-------|
| Track background | `var(--bg-brand-secondary-solid_hover)` (#0BA5A7) |
| Box shadow | `var(--ring-brand-secondary)` — `0 0 0 4px rgba(11,165,167,0.24), 0 1px 2px 0 rgba(17,23,41,0.16)` |

### Disabled Unchecked
| Property | Value |
|----------|-------|
| Track background | `var(--bg-tertiary)` (#E8ECF1) |
| Slider background | `var(--border-secondary)` (#CCD6DC) — gray knob |
| Cursor | Not allowed / default |

### Disabled Checked
| Property | Value |
|----------|-------|
| Track background | `var(--bg-brand-secondary-solid_hover)` (#0BA5A7) at 50% opacity |
| Slider background | `var(--bg-tertiary)` (#E8ECF1) — gray knob |
| Cursor | Not allowed / default |

---

## Label & Sub-text

| Property | Value |
|----------|-------|
| Label color | `var(--text-primary)` (#353E5A) — inherits from host |
| Sub-text color | `var(--text-tertiary)` (#6B748E) |
| Layout | Flex column, stacked vertically |
| Label wrapping | Flex: 1 (fills remaining space) |

---

## Layout Modifiers

| Modifier | Effect |
|----------|--------|
| **Toggle on right** | `flex-direction: row-reverse` — switch moves to right side |
| **Full width** | `width: 100%` — stretches to fill container |

---

## Accessibility

| Property | Value |
|----------|-------|
| Role | `switch` |
| `aria-checked` | Reflects checked state |
| `aria-label` | Custom label or auto-generated |
| `aria-labelledby` | Points to label text element |
| `aria-describedby` | Points to sub-text element |
| `tabindex` | 0 (focusable) |
| Keyboard | Space and Enter toggle the switch |

---

## Implementation Pattern for Lovable

```jsx
{/* === Medium Toggle — Unchecked === */}
<div
  role="switch"
  aria-checked="false"
  tabIndex={0}
  className="
    flex items-start gap-2
    cursor-pointer w-fit
    text-[var(--text-primary)]
  "
  onClick={() => setChecked(!checked)}
>
  {/* Track */}
  <div className={`
    relative w-9 h-5 rounded-full shrink-0
    transition-[background-color,box-shadow] duration-150 ease-out
    ${checked ? 'bg-[var(--bg-brand-secondary-solid)] hover:bg-[var(--bg-brand-secondary-solid_hover)]' : 'bg-[var(--bg-tertiary)] hover:bg-[var(--border-secondary)]'}
  `}>
    {/* Slider */}
    <div className={`
      absolute top-0.5 left-0.5
      w-4 h-4 rounded-full
      bg-white shadow-sm
      transition-transform duration-150 ease-out
      ${checked ? 'translate-x-4' : 'translate-x-0'}
    `} />
  </div>

  {/* Label */}
  <div className="flex flex-col flex-1">
    <span className="text-sm leading-5 font-medium">Enable notifications</span>
    <span className="text-xs leading-[18px] font-normal text-[var(--text-tertiary)]">
      Receive email updates about activity
    </span>
  </div>
</div>


{/* === Small Toggle — No label === */}
<div
  role="switch"
  aria-checked={checked}
  tabIndex={0}
  className="flex items-start gap-2 cursor-pointer w-fit"
>
  <div className={`
    relative w-7 h-4 rounded-full shrink-0
    transition-[background-color,box-shadow] duration-150 ease-out
    ${checked ? 'bg-[var(--bg-brand-secondary-solid)]' : 'bg-[var(--bg-tertiary)]'}
  `}>
    <div className={`
      absolute top-0.5 left-0.5
      w-3 h-3 rounded-full
      bg-[var(--bg-primary)] shadow-sm
      transition-transform duration-150 ease-out
      ${checked ? 'translate-x-3' : 'translate-x-0'}
    `} />
  </div>
</div>


{/* === Large Toggle — Toggle on right === */}
<div
  role="switch"
  aria-checked={checked}
  tabIndex={0}
  className="
    flex flex-row-reverse items-start gap-3
    cursor-pointer w-full
    text-[var(--text-primary)]
  "
>
  <div className={`
    relative w-11 h-6 rounded-full shrink-0
    transition-[background-color,box-shadow] duration-150 ease-out
    ${checked ? 'bg-[var(--bg-brand-secondary-solid)]' : 'bg-[var(--bg-tertiary)]'}
  `}>
    <div className={`
      absolute top-0.5 left-0.5
      w-5 h-5 rounded-full
      bg-white shadow-sm
      transition-transform duration-150 ease-out
      ${checked ? 'translate-x-5' : 'translate-x-0'}
    `} />
  </div>

  <div className="flex flex-col flex-1">
    <span className="text-base leading-6 font-medium">Dark mode</span>
    <span className="text-sm leading-5 font-normal text-[var(--text-tertiary)]">
      Switch between light and dark themes
    </span>
  </div>
</div>


{/* === Disabled Toggle === */}
<div
  role="switch"
  aria-checked="false"
  aria-disabled="true"
  className="
    flex items-start gap-2
    cursor-default opacity-100 pointer-events-none
    text-[var(--text-primary)]
  "
>
  <div className="relative w-9 h-5 rounded-full shrink-0 bg-[var(--bg-tertiary)]">
    {/* Disabled knob is gray, not white */}
    <div className="
      absolute top-0.5 left-0.5
      w-4 h-4 rounded-full
      bg-[var(--border-secondary)]
      transition-transform duration-150 ease-out
    " />
  </div>
  <span className="text-sm leading-5 font-medium">Unavailable</span>
</div>
```

### Toggle Size Quick Reference

| Size | Track W×H | Knob | Translate | Tailwind Track | Tailwind Knob |
|------|-----------|------|-----------|----------------|---------------|
| Small | 28×16 | 12px | 12px | `w-7 h-4` | `w-3 h-3 translate-x-3` |
| Medium | 36×20 | 16px | 16px | `w-9 h-5` | `w-4 h-4 translate-x-4` |
| Large | 44×24 | 20px | 20px | `w-11 h-6` | `w-5 h-5 translate-x-5` |

### Key Implementation Rules

1. **Track is always fully rounded** (`rounded-full`) — pill shape
2. **Unchecked = `var(--bg-tertiary)` (#E8ECF1) gray**, **Checked = `var(--bg-brand-secondary-solid)` (#098486) teal**
3. **Slider is always white** except in disabled state where it becomes `var(--border-secondary)` (#CCD6DC) (gray)
4. **Slider always has shadow** (`shadow-sm`) — even when disabled unchecked the knob is gray but still visible
5. **2px inset** from track edge — slider sits at `top: 2px, left: 2px`
6. **Checked slides right** by exactly the knob width (12/16/20px depending on size)
7. **Focus ring differs**: gray ring for unchecked, teal ring for checked
8. **Sub-text uses `var(--text-tertiary)` (#6B748E)**, not text-secondary
9. **Toggle on right uses `flex-row-reverse`** — label goes left, switch goes right
10. **Disabled checked state**: track uses teal at ~50% opacity, knob becomes gray
11. **Use `role="switch"`** with `aria-checked` for accessibility
12. **Space and Enter keys** should toggle the switch
