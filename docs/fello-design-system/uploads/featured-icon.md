# Featured Icon Component — Strict Specification

When building any featured/highlighted icon, icon badge, icon circle container, or decorative icon wrapper, follow these specifications exactly. Do NOT use default Tailwind icon containers or any other icon badge system.

> **TOKEN RULE — MANDATORY**: Never use raw hex colour values for icon, background, border, or text colours. Always use the CSS variable tokens or Tailwind palette names listed in each variant table below. In Tailwind classes use the `bg-[var(--token)]` / `text-[var(--token)]` / `border-[var(--token)]` syntax, or the palette shorthand (e.g. `bg-secondary-100`) when a Tailwind palette name is specified.

---

## Featured Icon Anatomy

A circular container that wraps an icon with a colored background. Optionally has a "pulse" outer ring border.

```
Without pulse:           With pulse:
  ┌─────────┐            ╔═══════════╗
  │  [icon] │            ║ ┌───────┐ ║
  └─────────┘            ║ │ [icon]│ ║
                         ║ └───────┘ ║
                         ╚═══════════╝
```

- Shape: Fully circular (`rounded-full`)
- Layout: Inline-flex, centered
- Icon is centered within the circle
- Optional pulse adds a solid border ring around the circle

---

## Sizes

There are 5 featured icon sizes:

| Size | Container | Icon Font Size | Pulse Border Width |
|------|-----------|---------------|-------------------|
| **XS** | 24px × 24px | 18px | 2px |
| **SM** | 32px × 32px | 20px | 3px |
| **MD** (default) | 40px × 40px | 24px | 4px |
| **LG** | 48px × 48px | 28px | 5px |
| **XL** | 60px × 60px | 32px | 6px |

---

## Color Variants (9 Standard + 1 AI)

### Primary
| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `var(--bg-brand-primary_alt)` (#FDE7E3) | `bg-primary-100` |
| Icon color | `var(--fg-brand-primary)` (#FF725C) | `text-[var(--fg-brand-primary)]` |
| Pulse border | `var(--bg-brand-primary)` (#FDF1EF) | `border-[var(--bg-brand-primary)]` |

### Secondary
| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `var(--bg-brand-secondary_alt)` (#CAFBFB) | `bg-secondary-100` |
| Icon color | `var(--fg-brand-secondary_alt)` (#0BA5A7) | `text-secondary-600` |
| Pulse border | `var(--bg-brand-secondary)` (#E7FDFD) | `border-[var(--bg-brand-secondary)]` |

### Neutral
| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `var(--bg-tertiary)` (#E8ECF1) | `bg-[var(--bg-tertiary)]` |
| Icon color | `var(--fg-primary)` (#353E5A) | `text-[var(--fg-primary)]` |
| Pulse border | `var(--bg-secondary)` (#F1F4F7) | `border-[var(--bg-secondary)]` |

### White
| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `var(--bg-primary)` (#FFFFFF) | `bg-white` |
| Border | 1px solid `var(--border-primary)` (#BDC8D3) | `border border-[var(--border-primary)]` |
| Icon color | `var(--fg-primary)` (#353E5A) | `text-[var(--fg-primary)]` |
| Pulse | Box-shadow ring instead of border | `shadow-[0_0_0_4px_rgba(34,42,63,0.08),0_1px_2px_0_rgba(17,23,41,0.16)]` |

### Success
| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `var(--bg-success-secondary)` (#ABF5D1) | `bg-success-100` |
| Icon color | `var(--fg-success-primary)` (#31A172) | `text-[var(--fg-success-primary)]` |
| Pulse border | `var(--bg-success-primary)` (#E3FCEF) | `border-[var(--bg-success-primary)]` |

### Danger
| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `var(--bg-error-secondary)` (#FFBDAD) | `bg-error-100` |
| Icon color | `var(--fg-error-primary)` (#F02C00) | `text-[var(--fg-error-primary)]` |
| Pulse border | `var(--bg-error-primary)` (#FFEBE6) | `border-[var(--bg-error-primary)]` |

### Warning
| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `var(--bg-warning-secondary)` (#FFF0B3) | `bg-warning-100` |
| Icon color | `var(--fg-warning-primary)` (#F08400) | `text-[var(--fg-warning-primary)]` |
| Pulse border | `var(--bg-warning-primary)` (#FFFAE6) | `border-[var(--bg-warning-primary)]` |

### Info
| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `var(--bg-info-secondary)` (#E7F1FE) | `bg-blue-100` |
| Icon color | `var(--fg-info-primary)` (#3D93F5) | `text-[var(--fg-info-primary)]` |
| Pulse border | `var(--bg-info-primary)` (#F0F7FF) | `border-[var(--bg-info-primary)]` |

### Magic
| Property | Value | Tailwind |
|----------|-------|----------|
| Background | `var(--bg-magic-secondary)` (#F2EDFC) | `bg-purple-100` |
| Icon color | `var(--fg-magic-primary)` (#8D6AE7) | `text-[var(--fg-magic-primary)]` |
| Pulse border | `var(--bg-magic-primary)` (#F4F3FF) | `border-[var(--bg-magic-primary)]` |

### AI

The AI variant uses **gradient backgrounds** and **gradient icon text** — unlike the other 9 variants which use solid colors.

| Property | Value | Implementation |
|----------|-------|----------------|
| Background | Gradient: `linear-gradient(98.42deg, var(--bg-secondary) 3.79%, var(--bg-magic-secondary) 51.25%, var(--bg-brand-primary_alt) 98.71%)` | `style={{ background: 'linear-gradient(98.42deg, #F1F4F7 3.79%, #F2EDFC 51.25%, #FDE7E3 98.71%)' }}` |
| Icon color | Gradient text: `linear-gradient(43.33deg, var(--ai-00) 0%, var(--ai-02) 25%, var(--ai-05) 50%, var(--ai-08) 75%, var(--ai-10) 100%)` | See gradient text implementation below |
| Pulse ring | Gradient pseudo-element (NOT a solid border) — see below | Uses `::after` pseudo-element with gradient background |

**Background gradient color stops:**
- 3.79%: `var(--bg-secondary)` (#F1F4F7) — light gray
- 51.25%: `var(--bg-magic-secondary)` (#F2EDFC) — light purple
- 98.71%: `var(--bg-brand-primary_alt)` (#FDE7E3) — light coral/pink

**Icon gradient color stops (43.33deg):**
- 0%: `var(--ai-00)` (#14BACB) — cyan
- 25%: `var(--ai-02)` (#3898F0) — blue
- 50%: `var(--ai-05)` (#8D6AE6) — purple
- 75%: `var(--ai-08)` (#EC67A0) — pink
- 100%: `var(--ai-10)` (#F59593) — coral

**Gradient icon text implementation:**
```jsx
{/* The icon must use gradient text via background-clip */}
<span
  className="inline-flex items-center justify-center"
  style={{
    background: 'linear-gradient(43.33deg, #14BACB 0%, #3898F0 25%, #8D6AE6 50%, #EC67A0 75%, #F59593 100%)',
    WebkitBackgroundClip: 'text',
    backgroundClip: 'text',
    color: 'transparent',
  }}
>
  <Sparkles className="w-6 h-6" />
</span>
```

**AI pulse ring — gradient pseudo-element:**

Unlike other variants that use a solid `border`, the AI pulse uses a gradient `::after` pseudo-element behind the circle to create a gradient ring effect. The ring width matches the standard pulse border widths per size (2px for XS, 3px for SM, etc.).

Pulse ring gradient: `linear-gradient(98.42deg, var(--bg-brand-secondary_alt) 3.79%, var(--bg-magic-secondary) 51.25%, var(--bg-brand-primary_alt) 98.71%)`
- 3.79%: `var(--bg-brand-secondary_alt)` (#CAFBFB) — light cyan
- 51.25%: `var(--bg-magic-secondary)` (#F2EDFC) — light purple
- 98.71%: `var(--bg-brand-primary_alt)` (#FDE7E3) — light coral/pink

```jsx
{/* === Featured Icon — AI, Medium, With pulse === */}
<div className="relative inline-flex items-center justify-center">
  {/* Gradient pulse ring — positioned behind the circle */}
  <div
    className="absolute rounded-full"
    style={{
      width: 'calc(100% + 8px)',   /* 2 × 4px for MD */
      height: 'calc(100% + 8px)',
      background: 'linear-gradient(98.42deg, #CAFBFB 3.79%, #F2EDFC 51.25%, #FDE7E3 98.71%)',
      zIndex: 0,
    }}
  />
  {/* Circle with gradient background */}
  <div
    className="relative z-10 inline-flex items-center justify-center w-10 h-10 rounded-full"
    style={{
      background: 'linear-gradient(98.42deg, #F1F4F7 3.79%, #F2EDFC 51.25%, #FDE7E3 98.71%)',
    }}
  >
    {/* Gradient icon */}
    <span style={{
      background: 'linear-gradient(43.33deg, #14BACB 0%, #3898F0 25%, #8D6AE6 50%, #EC67A0 75%, #F59593 100%)',
      WebkitBackgroundClip: 'text',
      backgroundClip: 'text',
      color: 'transparent',
      display: 'inline-flex',
    }}>
      <Sparkles className="w-6 h-6" />
    </span>
  </div>
</div>
```

**AI pulse ring widths per size:**

| Size | Ring Extends By | Total Ring Size = Container + 2 × Extension |
|------|----------------|---------------------------------------------|
| XS | 2px | `calc(100% + 4px)` |
| SM | 3px | `calc(100% + 6px)` |
| MD | 4px | `calc(100% + 8px)` |
| LG | 5px | `calc(100% + 10px)` |
| XL | 6px | `calc(100% + 12px)` |

---

## Pulse State

When pulse is enabled, a solid colored border wraps the circle. The border uses `box-sizing: content-box`, so the total size increases:

**Total size with pulse** = container size + (2 × pulse border width)

| Size | Container | Pulse Border | Total with Pulse |
|------|-----------|-------------|-----------------|
| XS | 24px | 2px | 28px |
| SM | 32px | 3px | 38px |
| MD | 40px | 4px | 48px |
| LG | 48px | 5px | 58px |
| XL | 60px | 6px | 72px |

**White variant exception**: Uses `box-shadow` ring instead of border (4px margin to simulate the ring).

---

## Implementation Pattern for Lovable

```jsx
{/* === Featured Icon — Primary, Medium, No pulse === */}
<div className="
  inline-flex items-center justify-center
  w-10 h-10 rounded-full
  bg-primary-100
">
  <AlertCircle className="w-6 h-6 text-[var(--fg-brand-primary)]" />
</div>


{/* === Featured Icon — Success, Large, With pulse === */}
<div className="
  inline-flex items-center justify-center
  w-12 h-12 rounded-full
  bg-success-100
  border-[5px] border-[var(--bg-success-primary)]
  box-content
">
  <CheckCircle className="w-7 h-7 text-[var(--fg-success-primary)]" />
</div>


{/* === Featured Icon — Danger, Small, With pulse === */}
<div className="
  inline-flex items-center justify-center
  w-8 h-8 rounded-full
  bg-error-100
  border-[3px] border-[var(--bg-error-primary)]
  box-content
">
  <XCircle className="w-5 h-5 text-[var(--fg-error-primary)]" />
</div>


{/* === Featured Icon — White, Medium, With pulse === */}
<div className="
  inline-flex items-center justify-center
  w-10 h-10 rounded-full
  bg-white
  border border-[var(--border-primary)]
  shadow-[0_0_0_4px_rgba(34,42,63,0.08),0_1px_2px_0_rgba(17,23,41,0.16)]
  m-1
">
  <Settings className="w-6 h-6 text-[var(--fg-primary)]" />
</div>


{/* === Featured Icon — Info, XS, No pulse === */}
<div className="
  inline-flex items-center justify-center
  w-6 h-6 rounded-full
  bg-blue-100
">
  <Info className="w-[18px] h-[18px] text-[var(--fg-info-primary)]" />
</div>


{/* === Featured Icon — Secondary, XL, With pulse === */}
<div className="
  inline-flex items-center justify-center
  w-[60px] h-[60px] rounded-full
  bg-secondary-100
  border-[6px] border-[var(--bg-brand-secondary)]
  box-content
">
  <Zap className="w-8 h-8 text-secondary-600" />
</div>


{/* === Featured Icon — Warning, Medium, No pulse === */}
<div className="
  inline-flex items-center justify-center
  w-10 h-10 rounded-full
  bg-warning-100
">
  <AlertTriangle className="w-6 h-6 text-[var(--fg-warning-primary)]" />
</div>


{/* === Featured Icon — Magic, Medium, No pulse === */}
<div className="
  inline-flex items-center justify-center
  w-10 h-10 rounded-full
  bg-purple-100
">
  <Sparkles className="w-6 h-6 text-[var(--fg-magic-primary)]" />
</div>


{/* === Featured Icon — Neutral, Large, With pulse === */}
<div className="
  inline-flex items-center justify-center
  w-12 h-12 rounded-full
  bg-[var(--bg-tertiary)]
  border-[5px] border-[var(--bg-secondary)]
  box-content
">
  <FileText className="w-7 h-7 text-[var(--fg-primary)]" />
</div>


{/* === Featured Icon — AI, Medium, No pulse === */}
<div
  className="inline-flex items-center justify-center w-10 h-10 rounded-full"
  style={{
    background: 'linear-gradient(98.42deg, #F1F4F7 3.79%, #F2EDFC 51.25%, #FDE7E3 98.71%)',
  }}
>
  <span style={{
    background: 'linear-gradient(43.33deg, #14BACB 0%, #3898F0 25%, #8D6AE6 50%, #EC67A0 75%, #F59593 100%)',
    WebkitBackgroundClip: 'text',
    backgroundClip: 'text',
    color: 'transparent',
    display: 'inline-flex',
  }}>
    <Sparkles className="w-6 h-6" />
  </span>
</div>


{/* === Featured Icon — AI, Medium, With pulse === */}
<div className="relative inline-flex items-center justify-center">
  <div className="absolute rounded-full" style={{
    width: 'calc(100% + 8px)', height: 'calc(100% + 8px)',
    background: 'linear-gradient(98.42deg, #CAFBFB 3.79%, #F2EDFC 51.25%, #FDE7E3 98.71%)',
    zIndex: 0,
  }} />
  <div className="relative z-10 inline-flex items-center justify-center w-10 h-10 rounded-full"
    style={{ background: 'linear-gradient(98.42deg, #F1F4F7 3.79%, #F2EDFC 51.25%, #FDE7E3 98.71%)' }}>
    <span style={{
      background: 'linear-gradient(43.33deg, #14BACB 0%, #3898F0 25%, #8D6AE6 50%, #EC67A0 75%, #F59593 100%)',
      WebkitBackgroundClip: 'text', backgroundClip: 'text',
      color: 'transparent', display: 'inline-flex',
    }}>
      <Sparkles className="w-6 h-6" />
    </span>
  </div>
</div>
```

### Variant Quick Reference

| Variant | Background | Icon Color | Pulse Border |
|---------|-----------|------------|-------------|
| Primary | `var(--bg-brand-primary_alt)` (#FDE7E3) | `var(--fg-brand-primary)` (#FF725C) | `var(--bg-brand-primary)` (#FDF1EF) |
| Secondary | `var(--bg-brand-secondary_alt)` (#CAFBFB) | `var(--fg-brand-secondary_alt)` (#0BA5A7) | `var(--bg-brand-secondary)` (#E7FDFD) |
| Neutral | `var(--bg-tertiary)` (#E8ECF1) | `var(--fg-primary)` (#353E5A) | `var(--bg-secondary)` (#F1F4F7) |
| White | `var(--bg-primary)` (#FFFFFF) + border | `var(--fg-primary)` (#353E5A) | Box-shadow ring |
| Success | `var(--bg-success-secondary)` (#ABF5D1) | `var(--fg-success-primary)` (#31A172) | `var(--bg-success-primary)` (#E3FCEF) |
| Danger | `var(--bg-error-secondary)` (#FFBDAD) | `var(--fg-error-primary)` (#F02C00) | `var(--bg-error-primary)` (#FFEBE6) |
| Warning | `var(--bg-warning-secondary)` (#FFF0B3) | `var(--fg-warning-primary)` (#F08400) | `var(--bg-warning-primary)` (#FFFAE6) |
| Info | `var(--bg-info-secondary)` (#E7F1FE) | `var(--fg-info-primary)` (#3D93F5) | `var(--bg-info-primary)` (#F0F7FF) |
| Magic | `var(--bg-magic-secondary)` (#F2EDFC) | `var(--fg-magic-primary)` (#8D6AE7) | `var(--bg-magic-primary)` (#F4F3FF) |
| **AI** | **Gradient**: gray → purple → coral | **Gradient text**: cyan → blue → purple → pink → coral | **Gradient ring**: cyan → purple → coral (pseudo-element, NOT solid border) |

### Size Quick Reference

| Size | Container | Icon Size | Pulse Width | Tailwind Container |
|------|-----------|-----------|-------------|-------------------|
| XS | 24px | 18px | 2px | `w-6 h-6` |
| SM | 32px | 20px | 3px | `w-8 h-8` |
| MD | 40px | 24px | 4px | `w-10 h-10` |
| LG | 48px | 28px | 5px | `w-12 h-12` |
| XL | 60px | 32px | 6px | `w-[60px] h-[60px]` |

### Key Implementation Rules

1. **Shape is always fully circular** — `rounded-full` on all sizes and variants
2. **Icon is always centered** — `inline-flex items-center justify-center`
3. **Pulse uses `box-sizing: content-box`** — add `box-content` class so the border adds to the total size
4. **White variant is special** — always has a 1px `var(--border-primary)` (#BDC8D3) border, pulse uses box-shadow instead of border
5. **Pulse border color is the lightest shade** (50-level) of the variant's color family
6. **Background is the 100-level shade**, icon color is the 600-level shade
7. **Icon sizes scale with container**: 18→20→24→28→32px across XS→XL
8. **Do NOT use Tailwind's built-in border widths** for pulse borders larger than 4px — use `border-[5px]` or `border-[6px]` custom values
9. **White variant pulse uses 4px margin** (`m-1`) to visually compensate for box-shadow ring
10. **Featured Icon is display: inline-flex** — flows inline with surrounding text/content
11. **AI variant uses CSS gradients** — background is a gradient (NOT solid), icon uses `background-clip: text` for gradient text effect, and pulse uses a gradient `::after` pseudo-element (NOT a solid border). Use inline `style={{}}` for all gradient values since Tailwind cannot express multi-stop gradients.
12. **AI variant is the ONLY variant that requires gradient rendering** — all other 9 variants use solid token colors
