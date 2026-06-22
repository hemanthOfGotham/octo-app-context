# Capsule Component — Strict Specification

When building any capsule, chip, toggle pill, or selectable tag, follow these specifications exactly. Do NOT use default Tailwind chip styles, shadcn toggles, or any other chip/pill system.

> **TOKEN RULE — mandatory for every colour value in this file.**
> Never use a raw hex colour in code output. Always use the design-system CSS variable via `var(--token)`.
> In **tables** write: `var(--token)` (#HEX). In **Tailwind classes** write: `bg-[var(--bg-token)]`, `text-[var(--text-token)]`, etc.
> In **inline styles** write: `style={{ color: 'var(--text-primary)' }}`. In **prose** write: `var(--token)` (#HEX).

---

## Capsule Anatomy

A capsule is an inline pill-shaped selectable element:

```
[ Label Text ]
```

- Layout: `inline-flex`, `align-items: center`
- Shape: Fully rounded (`border-radius: 9999px` / `rounded-full`)
- Cursor: Pointer when enabled, default when disabled
- Hidden `<input>` inside for form integration (checkbox or radio)
- Content projected via `<ng-content>` — just text or any inline content

---

## Sizes

There are 2 capsule sizes:

### Small
| Property | Value | Tailwind |
|----------|-------|----------|
| Typography | 12px / 18px / weight 500 | `text-xs leading-[18px] font-medium` |
| Padding | 4px vertical, 16px horizontal | `py-1 px-4` |

### Medium (Default)
| Property | Value | Tailwind |
|----------|-------|----------|
| Typography | 14px / 20px / weight 500 | `text-sm leading-5 font-medium` |
| Padding | 8px vertical, 16px horizontal | `py-2 px-4` |

---

## Types

A capsule can behave as either:

| Type | Behavior |
|------|----------|
| **checkbox** (default) | Toggles independently — multiple capsules can be selected |
| **radio** | Only one in a named group can be selected at a time |

Both types look identical visually. The difference is only in selection behavior.

---

## States — Complete Color Spec

### Default Unchecked
| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `var(--bg-primary)` (#FFFFFF) | `bg-[var(--bg-primary)]` |
| Border | 1px solid `var(--border-secondary)` (#CCD6DC) | `border border-[var(--border-secondary)]` |
| Text color | `var(--text-primary)` (#353E5A) | `text-[var(--text-primary)]` |
| Shadow | `0 1px 2px 0 rgba(34,42,63,0.08)` (shadow-xs) | `shadow-sm` |

### Unchecked Hover
| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `var(--bg-secondary)` (#F1F4F7) | `bg-[var(--bg-secondary)]` |
| Border | 1px solid `var(--border-secondary)` (#CCD6DC) | unchanged |
| Text color | `var(--text-primary)` (#353E5A) | unchanged |
| Shadow | shadow-xs | unchanged |

### Unchecked Focus-Visible
| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `var(--bg-secondary)` (#F1F4F7) | `bg-[var(--bg-secondary)]` |
| Border | 1px solid `var(--border-secondary)` (#CCD6DC) | unchanged |
| Text color | `var(--text-primary)` (#353E5A) | unchanged |
| Shadow | `0 0 0 4px rgba(34,42,63,0.08), 0 1px 2px 0 rgba(17,23,41,0.16)` (focus-gray-xs) | `ring-4 ring-[rgba(34,42,63,0.08)]` |

### Checked (Selected)
| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `var(--bg-brand-secondary-solid)` (#098486) | `bg-[var(--bg-brand-secondary-solid)]` |
| Border | 1px solid `var(--bg-brand-secondary-solid)` (#098486) (same as bg) | `border-[var(--bg-brand-secondary-solid)]` |
| Text color | `var(--text-white)` (#FFFFFF) | `text-[var(--text-white)]` |
| Shadow | shadow-xs | unchanged |

### Checked Hover
| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `#0BA5A7` (secondary-600) | `bg-[var(--bg-brand-secondary-solid_hover)]` |
| Border | 1px solid `var(--border-brand-secondary-solid)` (#0BA5A7) | `border-[var(--border-brand-secondary-solid)]` |
| Text color | `var(--text-white)` (#FFFFFF) | unchanged |
| Shadow | shadow-xs | unchanged |

### Checked Focus-Visible
| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `#0BA5A7` (secondary-600) | `bg-[var(--bg-brand-secondary-solid_hover)]` |
| Border | 1px solid `var(--border-brand-secondary-solid)` (#0BA5A7) | unchanged |
| Text color | `var(--text-white)` (#FFFFFF) | unchanged |
| Shadow | `0 0 0 4px rgba(11,165,167,0.24), 0 1px 2px 0 rgba(17,23,41,0.16)` | `ring-4 ring-[rgba(11,165,167,0.24)]` |

### Disabled Unchecked
| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `var(--bg-tertiary)` (#E8ECF1) | `bg-[var(--bg-tertiary)]` |
| Border | 1px solid `var(--border-secondary)` (#CCD6DC) | `border-[var(--border-secondary)]` |
| Text color | `var(--text-disabled)` (#8EA1AF) | `text-[var(--text-disabled)]` |
| Cursor | Default (no pointer) | `cursor-default` |

### Disabled Checked
| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `var(--bg-brand-secondary-solid)` (#098486) at reduced opacity | `bg-[var(--bg-brand-secondary-solid)]/50` |
| Border | Same as background | — |
| Text color | `var(--text-white)` (#FFFFFF) at reduced opacity | `text-[var(--text-white)]/50` |
| Cursor | Default (no pointer) | `cursor-default` |

---

## Transitions

- Smooth transitions on `background-color` and `box-shadow`: `transition: background-color, box-shadow 150ms ease-out`

---

## Accessibility

| Property | Value |
|----------|-------|
| Role | Implicit via `<label>` + hidden `<input>` |
| `aria-checked` | Reflects checked state |
| `aria-disabled` | Reflects disabled state |
| `tabindex` | 0 when enabled, -1 when disabled |
| Keyboard | Space and Enter toggle selection |

---

## Implementation Pattern for Lovable

```jsx
{/* === Capsule Group — Checkbox behavior === */}
<div className="flex flex-wrap gap-2">
  {/* Unchecked capsule — Medium */}
  <button
    role="checkbox"
    aria-checked="false"
    tabIndex={0}
    className="
      inline-flex items-center
      py-2 px-4
      rounded-full
      bg-[var(--bg-primary)] text-[var(--text-primary)]
      border border-[var(--border-secondary)]
      shadow-sm
      text-sm leading-5 font-medium
      cursor-pointer outline-none
      hover:bg-[var(--bg-secondary)]
      focus-visible:bg-[var(--bg-secondary)] focus-visible:ring-4 focus-visible:ring-[rgba(34,42,63,0.08)]
      transition-[background-color,box-shadow] duration-150 ease-out
    "
  >
    Option A
  </button>

  {/* Checked capsule — Medium */}
  <button
    role="checkbox"
    aria-checked="true"
    tabIndex={0}
    className="
      inline-flex items-center
      py-2 px-4
      rounded-full
      bg-[var(--bg-brand-secondary-solid)] text-[var(--text-white)]
      border border-[var(--bg-brand-secondary-solid)]
      shadow-sm
      text-sm leading-5 font-medium
      cursor-pointer outline-none
      hover:bg-[var(--bg-brand-secondary-solid_hover)] hover:border-[var(--border-brand-secondary-solid)]
      focus-visible:bg-[var(--bg-brand-secondary-solid_hover)] focus-visible:border-[#0BA5A7] focus-visible:ring-4 focus-visible:ring-[rgba(11,165,167,0.24)]
      transition-[background-color,box-shadow] duration-150 ease-out
    "
  >
    Option B
  </button>

  {/* Disabled capsule — Medium */}
  <button
    role="checkbox"
    aria-checked="false"
    aria-disabled="true"
    tabIndex={-1}
    className="
      inline-flex items-center
      py-2 px-4
      rounded-full
      bg-[var(--bg-tertiary)] text-[var(--text-disabled)]
      border border-[var(--border-secondary)]
      shadow-sm
      text-sm leading-5 font-medium
      cursor-default outline-none
    "
  >
    Disabled
  </button>
</div>


{/* === Small capsules === */}
<div className="flex flex-wrap gap-2">
  <button
    role="checkbox"
    aria-checked="false"
    tabIndex={0}
    className="
      inline-flex items-center
      py-1 px-4
      rounded-full
      bg-[var(--bg-primary)] text-[var(--text-primary)]
      border border-[var(--border-secondary)]
      shadow-sm
      text-xs leading-[18px] font-medium
      cursor-pointer outline-none
      hover:bg-[var(--bg-secondary)]
      focus-visible:bg-[var(--bg-secondary)] focus-visible:ring-4 focus-visible:ring-[rgba(34,42,63,0.08)]
      transition-[background-color,box-shadow] duration-150 ease-out
    "
  >
    Small Tag
  </button>

  <button
    role="checkbox"
    aria-checked="true"
    tabIndex={0}
    className="
      inline-flex items-center
      py-1 px-4
      rounded-full
      bg-[var(--bg-brand-secondary-solid)] text-[var(--text-white)]
      border border-[var(--bg-brand-secondary-solid)]
      shadow-sm
      text-xs leading-[18px] font-medium
      cursor-pointer outline-none
      hover:bg-[var(--bg-brand-secondary-solid_hover)] hover:border-[var(--border-brand-secondary-solid)]
      transition-[background-color,box-shadow] duration-150 ease-out
    "
  >
    Selected
  </button>
</div>
```

### Key Implementation Rules

1. **Capsules are ALWAYS pill-shaped** — `rounded-full`, no exceptions
2. **Unchecked = `var(--bg-primary)` (#FFFFFF) background** with `var(--border-secondary)` (#CCD6DC) border and `var(--text-primary)` (#353E5A) text
3. **Checked = `var(--bg-brand-secondary-solid)` (#098486) teal background** with matching border and `var(--text-white)` (#FFFFFF) text
4. **Hover on checked changes to `var(--bg-brand-secondary-solid_hover)` (#0BA5A7)** (lighter teal, no token available)
5. **Shadow is always present** (`shadow-sm`) in all non-focus states
6. **Focus ring color differs**: gray ring for unchecked, teal ring for checked
7. **Horizontal padding is always 16px** (`px-4`) regardless of size
8. **Font weight is always 500** (`font-medium`)
9. **Disabled unchecked uses `var(--bg-tertiary)` (#E8ECF1) background** with `var(--text-disabled)` (#8EA1AF) text
10. **Disabled capsules have `cursor-default`** and `tabindex="-1"`
11. **Use `role="checkbox"` or `role="radio"`** with proper `aria-checked` for accessibility
12. **Space and Enter keys** should toggle the capsule when focused
