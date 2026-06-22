# File Picker Component — Strict Specification

When building any file upload area, drag-and-drop zone, file input, or upload component, follow these specifications exactly. Do NOT use default Tailwind file inputs, shadcn upload zones, or any other file upload system.

> **TOKEN RULE — never use raw hex colors.**
> Every colour MUST be expressed as a CSS variable token (`var(--token)`).
> In tables: `var(--token)` (#HEX). In Tailwind classes: `bg-[var(--bg-*)]`, `text-[var(--text-*)]`, `border-[var(--border-*)]`.
> In inline styles: `style={{ color: 'var(--text-*)' }}`.
> See the design-system token map for the full palette.

---

## File Picker Anatomy

Two layout variants exist:

### Standard File Picker
A compact square drop zone:
```
┌─────────────────────┐
│                     │
│   [Upload Button]   │  ← Hyperlink-style button
│                     │
│  Max size: 10 MB    │  ← File info text
│  png, jpeg, jpg     │
│                     │
└─ ─ ─ ─ ─ ─ ─ ─ ─ ─┘  ← Dashed border
```

### Wide File Picker
A horizontal drop zone:
```
┌─────────────────────────────────────────────────────┐
│                                                     │
│  [Icon]  Drag & drop your file here                 │
│          [Upload Button]                            │
│          Max file size: 10 MB | png, jpeg, jpg      │
│                                                     │
└─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─┘
```

---

## Standard File Picker Specs

| Property | Value | Tailwind |
|----------|-------|----------|
| Width | 140px | `w-[140px]` |
| Height | 120px | `h-[120px]` |
| Border | 1px dashed `var(--border-secondary)` (#CCD6DC) | `border border-dashed border-[var(--border-secondary)]` |
| Border radius | 8px | `rounded-lg` |
| Padding | 16px | `p-4` |
| Background | `var(--bg-primary)` (#FFFFFF) | `bg-[var(--bg-primary)]` |
| Display | Flex column, centered | `flex flex-col items-center justify-center` |
| Gap | 4px | `gap-1` |
| Text align | Center | `text-center` |

### File Info Text (Standard)
| Property | Value | Tailwind |
|----------|-------|----------|
| Typography | 10px / 16px / weight 400 (paragraph-xs) | `text-[10px] leading-4 font-normal` |
| Color | `var(--text-tertiary)` (#6B748E) | `text-[var(--text-tertiary)]` |
| Format | "Max file size: {size} MB \| {types}" | — |

---

## Wide File Picker Specs

| Property | Value | Tailwind |
|----------|-------|----------|
| Width | 300px | `w-[300px]` |
| Height | Auto | `h-auto` |
| Border | 1px dashed `var(--border-secondary)` (#CCD6DC) | `border border-dashed border-[var(--border-secondary)]` |
| Border radius | 8px | `rounded-lg` |
| Padding | 16px vertical, 20px horizontal | `py-4 px-5` |
| Background | `var(--bg-primary)` (#FFFFFF) | `bg-[var(--bg-primary)]` |
| Display | Flex, centered | `flex items-center justify-center` |
| Gap | 12px | `gap-3` |

### Message Text (Wide)
| Property | Value | Tailwind |
|----------|-------|----------|
| Typography | 14px / 20px / weight 500 (caption-md) | `text-sm leading-5 font-medium` |
| Color | `var(--text-primary)` (#353E5A) | `text-[var(--text-primary)]` |
| Margin bottom | 2px | `mb-0.5` |

### File Info Text (Wide)
| Property | Value | Tailwind |
|----------|-------|----------|
| Typography | 12px / 18px / weight 400 (paragraph-sm) | `text-xs leading-[18px] font-normal` |
| Color | `var(--text-tertiary)` (#6B748E) | `text-[var(--text-tertiary)]` |
| Margin top | 8px | `mt-2` |

---

## Fluid Mode

Both variants support full-width/full-height fluid mode:

| Variant | Standard Fluid | Wide Fluid |
|---------|---------------|-----------|
| Width | 100% | 100% |
| Height | 120px (unchanged) | 100% |
| Tailwind | `w-full` | `w-full h-full` |

---

## States

### Default
| Property | Value |
|----------|-------|
| Border | 1px dashed `var(--border-secondary)` (#CCD6DC) |
| Background | `var(--bg-primary)` (#FFFFFF) |

### Hover
| Property | Value | Tailwind |
|----------|-------|----------|
| Border | 1px dashed `var(--border-brand-secondary-solid)` (#0BA5A7) | `hover:border-[var(--border-brand-secondary-solid)]` |
| Cursor | Pointer | `cursor-pointer` |

### Focus-Within
| Property | Value | Tailwind |
|----------|-------|----------|
| Border | 1px dashed `var(--border-brand-secondary-solid)` (#0BA5A7) | `focus-within:border-[var(--border-brand-secondary-solid)]` |
| Box shadow | Teal focus ring | `focus-within:shadow-[0_0_0_4px_var(--ring-brand-secondary),0_1px_2px_0_rgba(17,23,41,0.16)]` |

### Drag Over
| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `var(--bg-secondary)` (#F1F4F7) | `bg-[var(--bg-secondary)]` |

### Error
| Property | Value | Tailwind |
|----------|-------|----------|
| Border | 1px dashed `var(--border-error-solid)` (#F02C00) | `border-[var(--border-error-solid)]` |

### Error + Focus
| Property | Value |
|----------|-------|
| Border | 1px dashed `var(--border-error-solid)` (#F02C00) |
| Box shadow | Error focus ring |

### Disabled
| Property | Value | Tailwind |
|----------|-------|----------|
| Pointer events | None | `pointer-events-none` |
| Text color | `var(--text-disabled)` (#8EA1AF) | `text-[var(--text-disabled)]` |

---

## Transition

| Property | Value | Tailwind |
|----------|-------|----------|
| Properties | border-color, box-shadow | `transition-all` |
| Duration | 150ms | `duration-150` |
| Timing | ease-out | `ease-out` |

---

## Validation Rules

| Rule | Default | Error Condition |
|------|---------|----------------|
| File type | `image/*` | File MIME type doesn't match |
| File size | 10 MB max | File exceeds max size |
| File name length | 64 chars max | Filename too long |

---

## File Preview (After Upload)

### Standard File Preview
| Property | Value | Tailwind |
|----------|-------|----------|
| Width | 140px | `w-[140px]` |
| Height | 120px | `h-[120px]` |
| Border | 1px solid `var(--border-secondary)` (#CCD6DC) | `border border-[var(--border-secondary)]` |
| Border radius | 8px | `rounded-lg` |
| Padding | 6px | `p-1.5` |
| Background | `var(--bg-primary)` (#FFFFFF) | `bg-[var(--bg-primary)]` |
| Display | Flex, centered, relative | `relative flex justify-center` |

**Image display:**
- For images: Shows `<img>` with `object-contain` or `object-cover`
- For files: Shows a Featured Icon (file-checkmark, success variant)

**Hover overlay (actions):**
| Property | Value |
|----------|-------|
| Position | Absolute, covers entire preview |
| Background | Semi-transparent overlay |
| Opacity | 0 → 1 on hover |
| Transition | Opacity 150ms ease-out |
| Contains | Action buttons (delete, replace, etc.) |

### Wide File Preview
| Property | Value | Tailwind |
|----------|-------|----------|
| Width | 300px | `w-[300px]` |
| Padding | 16px | `p-4` |
| Image height | 80px (sm) / 100px (md) / 120px (lg) | `h-20` / `h-[100px]` / `h-[120px]` |
| Image border | 1px solid `var(--border-secondary)` (#CCD6DC) | `border border-[var(--border-secondary)]` |
| Image border radius | 4px | `rounded` |
| Actions gap | 12px | `gap-3` |
| Content gap | 8px | `gap-2` |

---

## Implementation Pattern for Lovable

```jsx
{/* === Standard File Picker (no file selected) === */}
<div
  className={`
    flex flex-col items-center justify-center gap-1
    w-[140px] h-[120px] p-4
    rounded-lg
    bg-[var(--bg-primary)]
    border border-dashed border-[var(--border-secondary)]
    cursor-pointer
    hover:border-[var(--border-brand-secondary-solid)]
    focus-within:border-[var(--border-brand-secondary-solid)]
    focus-within:shadow-[0_0_0_4px_var(--ring-brand-secondary),0_1px_2px_0_rgba(17,23,41,0.16)]
    transition-all duration-150 ease-out
    ${isDragOver ? 'bg-[var(--bg-secondary)]' : ''}
    ${hasError ? 'border-[var(--border-error-solid)]' : ''}
  `}
  onDragOver={handleDragOver}
  onDragLeave={handleDragLeave}
  onDrop={handleDrop}
>
  <input
    type="file"
    accept="image/*"
    className="hidden"
    id="file-input"
    onChange={handleFileChange}
  />
  {/* Upload button — use hyperlink-style or secondary button */}
  <label
    htmlFor="file-input"
    className="text-sm leading-5 font-medium text-[var(--text-brand-secondary)] cursor-pointer underline"
  >
    Upload
  </label>
  <p className="text-[10px] leading-4 font-normal text-[var(--text-tertiary)] text-center">
    Max file size: 10 MB | png, jpeg, jpg
  </p>
</div>


{/* === Wide File Picker (no file selected) === */}
<div
  className={`
    flex items-center justify-center gap-3
    w-[300px] py-4 px-5
    rounded-lg
    bg-[var(--bg-primary)]
    border border-dashed border-[var(--border-secondary)]
    cursor-pointer
    hover:border-[var(--border-brand-secondary-solid)]
    focus-within:border-[var(--border-brand-secondary-solid)]
    focus-within:shadow-[0_0_0_4px_var(--ring-brand-secondary),0_1px_2px_0_rgba(17,23,41,0.16)]
    transition-all duration-150 ease-out
    ${isDragOver ? 'bg-[var(--bg-secondary)]' : ''}
  `}
  onDragOver={handleDragOver}
  onDragLeave={handleDragLeave}
  onDrop={handleDrop}
>
  {/* Upload icon */}
  <div className="
    inline-flex items-center justify-center
    w-10 h-10 rounded-full
    bg-[var(--bg-tertiary)]
  ">
    <Upload className="w-6 h-6 text-[var(--fg-primary)]" />
  </div>

  <div>
    <p className="text-sm leading-5 font-medium text-[var(--text-primary)] mb-0.5">
      Drag & drop your file here
    </p>
    <label
      htmlFor="file-input-wide"
      className="text-sm leading-5 font-medium text-[var(--text-brand-secondary)] cursor-pointer underline"
    >
      Upload
    </label>
    <p className="mt-2 text-xs leading-[18px] font-normal text-[var(--text-tertiary)]">
      Max file size: 10 MB | png, jpeg, jpg
    </p>
  </div>
  <input type="file" accept="image/*" className="hidden" id="file-input-wide" />
</div>


{/* === Standard File Preview (after upload) === */}
<div className="
  relative flex justify-center
  w-[140px] h-[120px] p-1.5
  rounded-lg
  bg-[var(--bg-primary)]
  border border-[var(--border-secondary)]
  group
">
  <img
    src={previewUrl}
    alt="Uploaded file"
    className="max-h-full w-auto object-contain rounded"
  />

  {/* Hover overlay with actions */}
  <div className="
    absolute inset-0
    flex items-center justify-center gap-2
    bg-black/40
    rounded-lg
    opacity-0 group-hover:opacity-100
    transition-opacity duration-150 ease-out
  ">
    <button className="w-8 h-8 rounded bg-white/90 flex items-center justify-center">
      <Trash2 className="w-4 h-4 text-[var(--fg-primary)]" />
    </button>
    <button className="w-8 h-8 rounded bg-white/90 flex items-center justify-center">
      <RefreshCw className="w-4 h-4 text-[var(--fg-primary)]" />
    </button>
  </div>
</div>


{/* === Fluid Wide File Picker === */}
<div className="
  flex items-center justify-center gap-3
  w-full h-full py-4 px-5
  rounded-lg
  bg-[var(--bg-primary)]
  border border-dashed border-[var(--border-secondary)]
  cursor-pointer
  hover:border-[var(--border-brand-secondary-solid)]
  transition-all duration-150 ease-out
">
  {/* Same content as wide variant */}
</div>


{/* === Disabled File Picker === */}
<div className="
  flex flex-col items-center justify-center gap-1
  w-[140px] h-[120px] p-4
  rounded-lg bg-[var(--bg-primary)]
  border border-dashed border-[var(--border-secondary)]
  pointer-events-none
">
  <span className="text-sm leading-5 font-medium text-[var(--text-disabled)]">Upload</span>
  <p className="text-[10px] leading-4 font-normal text-[var(--text-disabled)] text-center">
    Max file size: 10 MB | png, jpeg, jpg
  </p>
</div>


{/* === Error State File Picker === */}
<div className="
  flex flex-col items-center justify-center gap-1
  w-[140px] h-[120px] p-4
  rounded-lg bg-[var(--bg-primary)]
  border border-dashed border-[var(--border-error-solid)]
  cursor-pointer
  transition-all duration-150 ease-out
">
  <span className="text-sm leading-5 font-medium text-[var(--text-brand-secondary)] underline cursor-pointer">
    Upload
  </span>
  <p className="text-[10px] leading-4 font-normal text-[var(--text-tertiary)] text-center">
    Max file size: 10 MB | png, jpeg, jpg
  </p>
</div>
```

### Key Implementation Rules

1. **Border is DASHED** (`border-dashed`) for the upload zone — NOT solid
2. **Standard variant is 140x120px**, wide variant is 300px wide with auto height
3. **Hover changes border to teal** `var(--border-brand-secondary-solid)` (#0BA5A7)
4. **Drag-over changes background** to `var(--bg-secondary)` (#F1F4F7) for visual feedback
5. **Error state uses red dashed border** `var(--border-error-solid)` (#F02C00) instead of the default gray
6. **File preview replaces the picker** after upload — same dimensions but solid border
7. **Preview hover overlay** shows action buttons (delete/replace) with semi-transparent background
8. **Disabled state** uses `pointer-events-none` and `var(--text-disabled)` (#8EA1AF) text color
9. **Fluid mode** sets width to 100% — use when the picker should fill its container
10. **Single file only** — the picker accepts one file at a time
11. **Upload button uses hyperlink style** — teal text with underline
12. **Focus ring is teal** — `var(--ring-brand-secondary)` matching the design system
