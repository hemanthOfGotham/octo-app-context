# Color Picker Component — Strict Specification

When building any color picker, color swatch selector, or hex color input, follow these specifications exactly. Do NOT use default Tailwind color pickers, shadcn color pickers, or any other color picker library.

> **TOKEN RULE:** Every colour in this file is expressed as a CSS variable token.
> In **tables / prose** write `var(--token)` (#HEX).
> In **Tailwind classes** write `bg-[var(--bg-token)]`, `text-[var(--text-token)]`, `border-[var(--border-token)]`, etc.
> In **inline styles** write `'var(--token)'`.
> Do NOT use raw hex values outside the parenthetical annotation.

---

## Color Picker Anatomy

The color picker consists of two parts:

### 1. Color Swatch (Trigger)
A clickable square card that displays the selected color and opens the picker popup.

```
┌─────────────────┐
│                  │  ← Color preview (fills with selected color)
│                  │
├──────────────────┤
│    var(--fg-brand-primary) │  ← Hex label (uppercase) or placeholder
└──────────────────┘
```

### 2. Color Picker Popup
A dropdown panel with saturation/lightness box, hue slider, and hex/RGB inputs.

```
┌────────────────────────────────────┐
│  ┌──────────────────────────────┐  │
│  │   Saturation / Lightness     │  │  ← Gradient box (240px height)
│  │        ○ Selector            │  │
│  └──────────────────────────────┘  │
│                                    │
│  ──────────────○───────────────    │  ← Hue slider (12px height)
│                                    │
│  [HEX]     [R]    [G]    [B]      │  ← Input fields
└────────────────────────────────────┘
```

---

## Color Swatch Specs

| Property | Value | Tailwind |
|----------|-------|----------|
| Width | 90px | `w-[90px]` |
| Height | 90px | `h-[90px]` |
| Border | 1px solid `var(--border-secondary)` (#CCD6DC) | `border border-[var(--border-secondary)]` |
| Border radius | 8px | `rounded-lg` |
| Background | `var(--bg-primary)` (#FFFFFF) | `bg-[var(--bg-primary)]` |
| Display | Flex column | `flex flex-col` |
| Cursor | Pointer | `cursor-pointer` |
| Transition | border-color, box-shadow 150ms ease-out | `transition-all duration-150 ease-out` |

### Color Preview Area (Top)
| Property | Value | Tailwind |
|----------|-------|----------|
| Flex | 1 (fills remaining space) | `flex-1` |
| Background | Selected color as inline style | `style={{ backgroundColor: color }}` |
| Overflow | Hidden | `overflow-hidden` |

### Hex Label Area (Bottom)
| Property | Value | Tailwind |
|----------|-------|----------|
| Padding | 6px vertical, 12px horizontal | `py-1.5 px-3` |
| Typography | 12px / 18px / weight 400 | `text-xs leading-[18px] font-normal` |
| Text align | Center | `text-center` |
| Text transform | Uppercase (when color selected) | `uppercase` |
| Border top | 1px solid `var(--border-secondary)` (#CCD6DC) | `border-t border-[var(--border-secondary)]` |
| Color (with value) | Inherits `var(--text-primary)` (#353E5A) | `text-[var(--text-primary)]` |
| Color (placeholder) | `var(--text-tertiary)` (#6B748E) | `text-[var(--text-tertiary)]` |

### Delete Button Overlay
| Property | Value |
|----------|-------|
| Position | Absolute, top: 4px, right: 4px |
| Background | Semi-transparent overlay (rgba(73, 88, 131, 0.56)) |
| Border radius | 4px |
| Visibility | Hidden by default, visible on hover/focus |

---

## Swatch States

### Default
| Property | Value |
|----------|-------|
| Border | 1px solid `var(--border-secondary)` (#CCD6DC) |
| Background | White |

### Hover
| Property | Value | Tailwind |
|----------|-------|----------|
| Border | 1px solid `var(--border-brand-secondary-solid)` (#0BA5A7) | `hover:border-[var(--border-brand-secondary-solid)]` |
| Delete button | Visible | — |

### Focus
| Property | Value | Tailwind |
|----------|-------|----------|
| Border | 1px solid `var(--border-brand-secondary-solid)` (#0BA5A7) | `focus:border-[var(--border-brand-secondary-solid)]` |
| Box shadow | Focus ring (teal) | `focus:shadow-[0_0_0_4px_rgba(11,165,167,0.24),0_1px_2px_0_rgba(17,23,41,0.16)]` |
| Delete button | Visible | — |

### Disabled
| Property | Value |
|----------|-------|
| Pointer events | None |
| Opacity | Reduced (no interaction) |

---

## Color Picker Popup Specs

| Property | Value | Tailwind |
|----------|-------|----------|
| Width | 320px | `w-80` |
| Padding | 16px all sides | `p-4` |
| Background | `var(--bg-primary)` (#FFFFFF) | `bg-[var(--bg-primary)]` |
| Border | 1px solid `var(--border-secondary)` (#CCD6DC) | `border border-[var(--border-secondary)]` |
| Border radius | 8px | `rounded-lg` |
| Box shadow | Large elevation shadow | `shadow-2xl` |
| Position | Below swatch by default (10px offset) | — |

### Saturation/Lightness Box
| Property | Value |
|----------|-------|
| Height | 240px |
| Border radius | 8px |
| Background | Gradient (horizontal: white to transparent, vertical: transparent to black) overlaid on hue color |

### Saturation Selector (Donut)
| Property | Value |
|----------|-------|
| Size | 32px x 32px |
| Border | 6px solid white |
| Border radius | Full circle |
| Box shadow | Dual: outer 1px `var(--border-primary)` (#BDC8D3) + inset 1px `var(--border-primary)` (#BDC8D3) |
| Transform | translate(-50%, -50%) — centers on position |

### Hue Slider
| Property | Value |
|----------|-------|
| Height | 12px |
| Width | Full width |
| Border radius | Full |
| Background | Rainbow gradient (0 deg to 360 deg) |
| Margin top | 16px |

### Hue Slider Selector (Donut)
| Property | Value |
|----------|-------|
| Size | 32px x 32px |
| Border | 9px solid white |
| Border radius | Full circle |
| Background | Transparent |
| Box shadow | Same dual shadow as saturation selector |

### Input Fields
| Property | Value | Tailwind |
|----------|-------|----------|
| Padding | 8px vertical, 16px horizontal | `py-2 px-4` |
| Border | 1px solid `var(--border-secondary)` (#CCD6DC) | `border border-[var(--border-secondary)]` |
| Border radius | 8px | `rounded-lg` |
| Font size | 14px | `text-sm` |
| Font weight | 500 | `font-medium` |
| Hover border | `var(--border-brand-secondary-solid)` (#0BA5A7) | `hover:border-[var(--border-brand-secondary-solid)]` |
| Focus border | `var(--border-brand-secondary-solid)` (#0BA5A7) | `focus:border-[var(--border-brand-secondary-solid)]` |
| Focus shadow | Teal ring | `focus:shadow-[0_0_0_4px_rgba(11,165,167,0.24),0_1px_2px_0_rgba(17,23,41,0.16)]` |

### Input Labels
| Property | Value | Tailwind |
|----------|-------|----------|
| Typography | 14px / 20px / weight 500 | `text-sm leading-5 font-medium` |
| Color | `var(--text-primary)` (#353E5A) | `text-[var(--text-primary)]` |

### Popup Settings
- **Alpha/Opacity**: Disabled — no alpha channel input
- **Preset Colors**: None — no color palette swatches
- **Animation**: Slide-in from direction (200ms ease-out, 12px distance)

---

## Implementation Pattern for Lovable

```jsx
{/* === Color Swatch (trigger) === */}
<button
  className="
    relative flex flex-col
    w-[90px] h-[90px]
    bg-[var(--bg-primary)] rounded-lg
    border border-[var(--border-secondary)]
    cursor-pointer overflow-hidden
    hover:border-[var(--border-brand-secondary-solid)]
    focus:border-[var(--border-brand-secondary-solid)] focus:outline-none
    focus:shadow-[0_0_0_4px_rgba(11,165,167,0.24),0_1px_2px_0_rgba(17,23,41,0.16)]
    transition-all duration-150 ease-out
  "
  onClick={() => setPickerOpen(!pickerOpen)}
>
  {/* Color preview */}
  <div
    className="flex-1 w-full"
    style={{ backgroundColor: selectedColor || 'var(--bg-primary)' }}
  />
  {/* Hex label */}
  <div className="py-1.5 px-3 text-center border-t border-[var(--border-secondary)]">
    <span className={`text-xs leading-[18px] font-normal ${selectedColor ? 'text-[var(--text-primary)] uppercase' : 'text-[var(--text-tertiary)]'}`}>
      {selectedColor || 'Select'}
    </span>
  </div>

  {/* Delete button — visible on hover */}
  {selectedColor && (
    <button
      className="
        absolute top-1 right-1
        w-6 h-6 rounded
        bg-black/40
        flex items-center justify-center
        opacity-0 group-hover:opacity-100
        transition-opacity duration-150
      "
      onClick={(e) => { e.stopPropagation(); clearColor(); }}
    >
      <TrashIcon className="w-3.5 h-3.5 text-[var(--fg-white)]" />
    </button>
  )}
</button>


{/* === Color Picker Popup (simplified — Lovable should use a library) === */}
{pickerOpen && (
  <div className="
    absolute z-50 mt-2.5
    w-80 p-4
    bg-[var(--bg-primary)] rounded-lg
    border border-[var(--border-secondary)]
    shadow-2xl
    animate-in slide-in-from-top-3
  ">
    {/* Saturation/Lightness area */}
    <div
      className="w-full h-60 rounded-lg relative cursor-crosshair"
      style={{
        background: `linear-gradient(to top, #000, transparent), linear-gradient(to right, #fff, ${hueColor})`,
      }}
    >
      {/* Selector donut */}
      <div
        className="absolute w-8 h-8 rounded-full border-[6px] border-white"
        style={{
          left: `${satX}%`,
          top: `${satY}%`,
          transform: 'translate(-50%, -50%)',
          boxShadow: '0 0 0 1px var(--border-primary), inset 0 0 0 1px var(--border-primary)',
        }}
      />
    </div>

    {/* Hue slider */}
    <div className="mt-4 relative">
      <div
        className="w-full h-3 rounded-full"
        style={{
          background: 'linear-gradient(to right, #f00, #ff0, #0f0, #0ff, #00f, #f0f, #f00)',
        }}
      />
      <div
        className="absolute top-1/2 w-8 h-8 rounded-full border-[9px] border-white bg-transparent"
        style={{
          left: `${hueX}%`,
          transform: 'translate(-50%, -50%)',
          boxShadow: '0 0 0 1px var(--border-primary), inset 0 0 0 1px var(--border-primary)',
          marginTop: '-2px',
        }}
      />
    </div>

    {/* Input fields */}
    <div className="mt-4 flex gap-2">
      {/* HEX input */}
      <div className="flex flex-col-reverse max-w-[100px]">
        <label className="text-sm leading-5 font-medium text-[var(--text-primary)]">HEX</label>
        <input
          type="text"
          value={hex}
          className="
            py-2 px-4 rounded-lg
            border border-[var(--border-secondary)]
            text-sm font-medium
            hover:border-[var(--border-brand-secondary-solid)]
            focus:border-[var(--border-brand-secondary-solid)] focus:outline-none
            focus:shadow-[0_0_0_4px_rgba(11,165,167,0.24),0_1px_2px_0_rgba(17,23,41,0.16)]
            transition-all duration-150 ease-out
          "
        />
      </div>
      {/* R, G, B inputs */}
      {['R', 'G', 'B'].map((label) => (
        <div key={label} className="flex flex-col-reverse flex-1">
          <label className="text-sm leading-5 font-medium text-[var(--text-primary)]">{label}</label>
          <input
            type="number"
            min="0"
            max="255"
            className="
              py-2 px-4 rounded-lg w-full
              border border-[var(--border-secondary)]
              text-sm font-medium
              hover:border-[var(--border-brand-secondary-solid)]
              focus:border-[var(--border-brand-secondary-solid)] focus:outline-none
              focus:shadow-[0_0_0_4px_rgba(11,165,167,0.24),0_1px_2px_0_rgba(17,23,41,0.16)]
              transition-all duration-150 ease-out
            "
          />
        </div>
      ))}
    </div>
  </div>
)}
```

### Key Implementation Rules

1. **Swatch is exactly 90px x 90px** — fixed size, no variations
2. **Popup is 320px wide** with 16px padding and 8px border-radius
3. **Saturation box is 240px tall** with 8px border-radius
4. **Selector donuts are 32px x 32px** with white borders (6px for saturation, 9px for hue)
5. **Donut shadow is dual**: outer 1px `var(--border-primary)` (#BDC8D3) + inset 1px `var(--border-primary)` (#BDC8D3)
6. **Hex label is uppercase** when a color is selected, shows placeholder in `var(--text-tertiary)` (#6B748E) when empty
7. **Input fields use 8px/16px padding** with 8px border-radius
8. **All hover/focus borders are teal** `var(--border-brand-secondary-solid)` (#0BA5A7)
9. **Focus ring is teal** — `0 0 0 4px rgba(11,165,167,0.24)`
10. **No alpha channel** — opacity input is disabled
11. **No preset color palette** — only manual selection via gradient or hex/RGB inputs
12. **Popup appears 10px below** the swatch by default with slide-in animation (200ms ease-out)
13. **Delete button appears on hover** — positioned top-right of swatch with semi-transparent overlay background
