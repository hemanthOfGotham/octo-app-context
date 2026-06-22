# Design System Tokens — Strict Reference (Tier 1 Foundation)

You MUST use ONLY these design tokens when building UI. Do NOT invent colors, shadows, spacing, or typography values. Every visual property must map to a token defined below.

**⚠️ CRITICAL: This file is the FOUNDATION LAYER. Read this FIRST before any component spec.**

---

## ⚠️ MANDATORY — CSS Variables Setup (ONE TIME)

Add this block at the TOP of `src/index.css`, BEFORE any Tailwind directives. This creates CSS variables that ALL components MUST use instead of raw hex values.

```css
@import url('https://cdn.icomoon.io/231278/FelloIcons/style.css?ubpci2');

:root {
  /* ===== TEXT COLORS ===== */
  --text-primary: #353E5A;
  --text-primary_on-brand: #ffffff;
  --text-secondary: #495883;
  --text-secondary_hover: #353E5A;
  --text-secondary_on-brand: #FFC7BE;
  --text-tertiary: #6B748E;
  --text-tertiary_hover: #495883;
  --text-tertiary_on-brand: #FFC7BE;
  --text-quaternary: #9298A9;
  --text-quaternary_on-brand: #FFAA9D;
  --text-white: #ffffff;
  --text-white-disabled: #ffffff3d;
  --text-disabled: #8EA1AF;
  --text-disabled-subtle: #BDC8D3;
  --text-placeholder: #BDC8D3;
  --text-placeholder_subtle: #CCD6DC;
  --text-brand-primary: #FF725C;
  --text-brand-primary_alt: #FF8E7D;
  --text-brand-primary-disabled: #FF725C80;
  --text-brand-secondary: #098486;
  --text-brand-secondary-disabled: #0BA5A780;
  --text-success-primary: #31A172;
  --text-warning-primary: #F08400;
  --text-error-primary: #F02C00;
  --text-error-primary-disabled: #F02C0080;
  --text-info-primary: #3D93F5;
  --text-magic-primary: #8D6AE7;
  --text-magentapink-primary: #B4289A;

  /* ===== BORDER COLORS ===== */
  --border-primary: #BDC8D3;
  --border-primary-solid: #353E5A;
  --border-secondary: #CCD6DC;
  --border-secondary-solid: #8EA1AF;
  --border-tertiary: #E8ECF1;
  --border-quaternary: #F1F4F7;
  --border-inverted: #ffffff;
  --border-disabled: #BDC8D3;
  --border-transparent: transparent;
  --border-white: #ffffff;
  --border-disabled_subtle: #CCD6DC;
  --border-white-disabled: #ffffff80;
  --border-brand: #FFAA9D;
  --border-brand-solid: #FF725C;
  --border-brand-solid-disabled: #FF725C80;
  --border-brand_subtle: #FDF1EF;
  --border-brand-secondary: #46E6E8;
  --border-brand-secondary-solid: #0BA5A7;
  --border-brand-secondary_subtle: #E7FDFD;
  --border-brand-secondary_solid-disabled: #0BA5A780;
  --border-error: #FF7452;
  --border-error-solid: #F02C00;
  --border-error_subtle: #FFEBE6;
  --border-error-disabled: #F02C0080;
  --border-warning: #FFC400;
  --border-warning-solid: #F08400;
  --border-warning_subtle: #FFFAE6;
  --border-success: #57D9A3;
  --border-success-solid: #31A172;
  --border-success_subtle: #E3FCEF;
  --border-info: #B6D6FB;
  --border-info-solid: #3D93F5;
  --border-info_subtle: #F0F7FF;
  --border-magic: #D3C6F6;
  --border-magic-solid: #8D6AE7;
  --border-magic_subtle: #F4F3FF;
  --border-magentapink: #EB61A2;
  --border-magentapink-solid: #B4289A;
  --border-magentapink_subtle: #FFCAD4;

  /* ===== FOREGROUND / ICON COLORS ===== */
  --fg-primary: #353E5A;
  --fg-secondary: #495883;
  --fg-secondary_hover: #353E5A;
  --fg-tertiary: #6B748E;
  --fg-tertiary_hover: #495883;
  --fg-quaternary: #9298A9;
  --fg-quaternary_hover: #6B748E;
  --fg-quinary: #C5C9D7;
  --fg-quinary_hover: #9298A9;
  --fg-senary: #D3D7E1;
  --fg-white: #ffffff;
  --fg-white-disabled: #ffffff3d;
  --fg-disabled: #8EA1AF;
  --fg-disabled_subtle: #BDC8D3;
  --fg-brand-primary: #FF725C;
  --fg-brand-primary_alt: #FF8E7D;
  --fg-brand-primary-disabled: #FF725C80;
  --fg-brand-secondary: #098486;
  --fg-brand-secondary_alt: #0BA5A7;
  --fg-brand-secondary-disabled: #0BA5A780;
  --fg-warning-primary: #F08400;
  --fg-warning-secondary: #FF991F;
  --fg-error-primary: #F02C00;
  --fg-error-secondary: #FF431B;
  --fg-error-disabled: #F02C0080;
  --fg-success-primary: #31A172;
  --fg-success-secondary: #36B37E;
  --fg-info-primary: #3D93F5;
  --fg-info-secondary: #6EAEF7;
  --fg-magic-primary: #8D6AE7;
  --fg-magic-secondary: #AB91ED;
  --fg-magentapink-primary: #B4289A;
  --fg-magentapink-secondary: #C93191;

  /* ===== BACKGROUND COLORS ===== */
  --bg-primary: #ffffff;
  --bg-primary_hover: #F1F4F7;
  --bg-primary-solid: #353E5A;
  --bg-primary-solid_hover: #222A3F;
  --bg-secondary: #F1F4F7;
  --bg-secondary_hover: #E8ECF1;
  --bg-secondary_subtle: #F7F9FA;
  --bg-secondary-solid: #495883;
  --bg-secondary-solid_hover: #353E5A;
  --bg-tertiary: #E8ECF1;
  --bg-quaternary: #CCD6DC;
  --bg-active: #F1F4F7;
  --bg-disabled: #E8ECF1;
  --bg-disabled_subtle: #F1F4F7;
  --bg-transparent: transparent;
  --bg-alpha-white: #ffffff14;
  --bg-alpha-white_hover: #ffffff29;
  --bg-alpha-white-disabled: #ffffff5c;
  --bg_overlay: #4958838f;
  --bg-brand-primary: #FDF1EF;
  --bg-brand-primary_alt: #FDE7E3;
  --bg-brand-solid: #FF725C;
  --bg-brand-solid_hover: #FF8E7D;
  --bg-brand-solid-disabled: #FF725C5c;
  --bg-brand-secondary: #E7FDFD;
  --bg-brand-secondary_alt: #CAFBFB;
  --bg-brand-secondary-solid: #098486;
  --bg-brand-secondary-solid_hover: #0BA5A7;
  --bg-brand-secondary-solid_disabled: #0BA5A7f2;
  --bg-brand-section-primary: #E66753;
  --bg-brand-section-secondary: #0DC1C4;
  --bg-brand-section-primary-subtle: #FF725C;
  --bg-error-primary: #FFEBE6;
  --bg-error-secondary: #FFBDAD;
  --bg-error-solid: #F02C00;
  --bg-error-solid_hover: #FF431B;
  --bg-error-solid-disabled: #F02C005c;
  --bg-warning-primary: #FFFAE6;
  --bg-warning-secondary: #FFF0B3;
  --bg-warning-solid: #F08400;
  --bg-warning-solid_hover: #FF991F;
  --bg-success-primary: #E3FCEF;
  --bg-success-secondary: #ABF5D1;
  --bg-success-solid: #31A172;
  --bg-success-solid_hover: #36B37E;
  --bg-info-primary: #F0F7FF;
  --bg-info-secondary: #E7F1FE;
  --bg-info-solid: #3D93F5;
  --bg-info-solid_hover: #6EAEF7;
  --bg-magic-primary: #F4F3FF;
  --bg-magic-secondary: #F2EDFC;
  --bg-magic-solid: #8D6AE7;
  --bg-magic-solid_hover: #AB91ED;
  --bg-magentapink-primary: #FFCAD4;
  --bg-magentapink-secondary: #FEA2BE;
  --bg-magentapink-solid: #B4289A;
  --bg-magentapink-solid_hover: #C93191;

  /* ===== TOAST COLORS ===== */
  --toast-bg-primary: #052033;
  --toast-text-primary: #ffffff;
  --toast-fg-white: #ffffff;
  --toast-fg-info-primary: #6EAEF7;
  --toast-fg-success-primary: #36B37E;
  --toast-fg-warning-primary: #FF991F;
  --toast-fg-danger-primary: #FF431B;

  /* ===== TOOLTIP COLORS ===== */
  --tooltip-bg-primary-dark: #052033;
  --tooltip-text-primary-dark: #ffffff;
  --tooltip-bg-primary-white: #ffffff;
  --tooltip-text-primary-white: #353E5A;

  /* ===== AI COLORS ===== */
  --ai-00: #14BACB;
  --ai-01: #26A9DD;
  --ai-02: #3898F0;
  --ai-03: #5188F1;
  --ai-04: #6F79EC;
  --ai-05: #8D6AE6;
  --ai-06: #B066CD;
  --ai-07: #D363B3;
  --ai-08: #EC67A0;
  --ai-09: #F17E99;
  --ai-10: #F59593;

  /* ===== CHART COLORS ===== */
  --chart-series-0: #FF725C;
  --chart-series-1: #0DC1C4;
  --chart-series-2: #6EAEF7;
  --chart-series-3: #6B748E;
  --chart-series-4: #AB91ED;
  --chart-ai-0: #F17E99;
  --chart-ai-1: #26A9DD;
  --chart-ai-2: #5188F1;
  --chart-ai-3: #8D6AE6;
  --chart-ai-4: #D363B3;
  --chart-ai-00: #14BACB;
  --chart-ai-06: #B066CD;
  --chart-ai-10: #F59593;
  --chart-gradient-0-from: #FDE7E3;
  --chart-gradient-0-to: #FFF9F8;
  --chart-gradient-1-from: #CAFBFB;
  --chart-gradient-1-to: #F9FFFF;
  --chart-gradient-2-from: #E7F1FE;
  --chart-gradient-2-to: #F6FAFF;
  --chart-gradient-3-from: #E2E4EB;
  --chart-gradient-3-to: #F8F8FA;
  --chart-gradient-4-from: #F2EDFC;
  --chart-gradient-4-to: #FAFAFF;

  /* ===== GRADIENT TOKENS ===== */
  --gradient-ai-primary: linear-gradient(98.42deg, #14BACB 3.79%, #3898F0 24.69%, #8D6AE6 50.77%, #EC67A0 74.81%, #F59593 98.71%);
  --gradient-ai-hover: linear-gradient(98.42deg, #26A9DD 3.79%, #5188F1 24.69%, #AB91ED 50.77%, #D363B3 74.81%, #F17E99 98.71%);
  --gradient-ai-outline-bg: linear-gradient(98.42deg, #F1F4F7 3.79%, #F4F3FF 51.25%, #FDF1EF 98.71%);
  --gradient-ai-progress: linear-gradient(90deg, #14BACB, #3898F0, #8D6AE6, #EC67A0, #F59593);

  /* ===== CONSTANT COLORS ===== */
  --transparent: transparent;
  --black: #000000;
  --white: #ffffff;

  /* ===== ELEVATION SHADOWS ===== */
  --shadow-xs: 0 1px 2px 0 rgba(73, 88, 131, 0.08);
  --shadow-sm: 0 1px 2px 0 rgba(73, 88, 131, 0.04), 0 1px 3px 0 rgba(73, 88, 131, 0.16);
  --shadow-md: 0 2px 4px -2px rgba(73, 88, 131, 0.04), 0 4px 8px -2px rgba(73, 88, 131, 0.16);
  --shadow-lg: 0 4px 6px -2px rgba(73, 88, 131, 0.04), 0 12px 16px -4px rgba(73, 88, 131, 0.16);
  --shadow-xl: 0 8px 8px -4px rgba(73, 88, 131, 0.04), 0 20px 24px -4px rgba(73, 88, 131, 0.16);
  --shadow-2xl: 0 24px 48px -12px rgba(73, 88, 131, 0.16);
  --shadow-3xl: 0 32px 64px -12px rgba(73, 88, 131, 0.16);
  --shadow-4xl: 0 40px 80px -16px rgba(73, 88, 131, 0.16);
  --shadow-5xl: 0 48px 96px -16px rgba(73, 88, 131, 0.24);

  /* ===== FOCUS RINGS ===== */
  --ring-brand-primary: 0 0 0 4px rgba(255, 114, 92, 0.24);
  --ring-brand-secondary: 0 0 0 4px rgba(11, 165, 167, 0.24);
  --ring-gray-primary: 0 0 0 4px rgba(73, 88, 131, 0.08);
  --ring-gray-secondary: 0 0 0 4px rgba(9, 53, 85, 0.08);
  --ring-error-primary: 0 0 0 4px rgba(240, 44, 0, 0.24);
  --ring-ai-primary: 0 -2px 4px -2px #8D6AE633, 0 2px 8px -2px #8D6AE64D, -2px 0 4px 0 #26A9DD33, 2px 0 4px 0 #F17E9933;
  --ring-ai-secondary: -1px 0 2px 2px #14BACB12, 1px 0 2px 2px #D363B312, 0 -1px 2px 2px #6F79EC12, 0 1px 2px 2px #F5959312;

  /* ===== FOCUS COMBINATIONS ===== */
  --focus-brand-xs: 0 0 0 4px rgba(255, 114, 92, 0.24), 0 1px 2px 0 rgba(73, 88, 131, 0.04);
  --focus-brand-sm: 0 0 0 4px rgba(255, 114, 92, 0.24), 0 1px 2px 0 rgba(73, 88, 131, 0.04), 0 1px 3px 0 rgba(73, 88, 131, 0.16);
  --focus-gray-xs: 0 0 0 4px rgba(73, 88, 131, 0.08), 0 1px 2px 0 rgba(73, 88, 131, 0.04);
  --focus-gray-sm: 0 0 0 4px rgba(73, 88, 131, 0.08), 0 1px 2px 0 rgba(73, 88, 131, 0.04), 0 1px 3px 0 rgba(73, 88, 131, 0.16);
  --focus-error-xs: 0 0 0 4px rgba(240, 44, 0, 0.24), 0 1px 2px 0 rgba(73, 88, 131, 0.04);
  --focus-brand-secondary-xs: 0 0 0 4px rgba(11, 165, 167, 0.24), 0 1px 2px 0 rgba(73, 88, 131, 0.04);
  --focus-brand-secondary-sm: 0 0 0 4px rgba(11, 165, 167, 0.24), 0 1px 2px 0 rgba(73, 88, 131, 0.04), 0 1px 3px 0 rgba(73, 88, 131, 0.16);
  --focus-ai-xs: 0 1px 2px 0 rgba(73, 88, 131, 0.04), 0 -2px 4px -2px #8D6AE633, 0 2px 8px -2px #8D6AE64D, -2px 0 4px 0 #26A9DD33, 2px 0 4px 0 #F17E9933;
  --focus-ai-sm: 0 1px 3px 0 rgba(73, 88, 131, 0.16), 0 1px 2px 0 rgba(73, 88, 131, 0.04), 0 -2px 4px -2px #8D6AE633, 0 2px 8px -2px #8D6AE64D, -2px 0 4px 0 #26A9DD33, 2px 0 4px 0 #F17E9933;

  /* ===== DIRECTIONAL SHADOWS ===== */
  --shadow-left-xs: -1px 0 2px 0 rgba(73, 88, 131, 0.08);
  --shadow-left-sm: -2px 0 4px 0 rgba(73, 88, 131, 0.08);
  --shadow-left-md: -4px 0 8px 0 rgba(73, 88, 131, 0.08);
  --shadow-left-lg: -6px 0 12px 0 rgba(73, 88, 131, 0.08);
  --shadow-left-xl: -8px 0 16px 0 rgba(73, 88, 131, 0.08);
  --shadow-right-xs: 1px 0 2px 0 rgba(73, 88, 131, 0.08);
  --shadow-right-sm: 2px 0 4px 0 rgba(73, 88, 131, 0.08);
  --shadow-right-md: 4px 0 8px 0 rgba(73, 88, 131, 0.08);
  --shadow-right-lg: 6px 0 12px 0 rgba(73, 88, 131, 0.08);
  --shadow-right-xl: 8px 0 16px 0 rgba(73, 88, 131, 0.08);
  --shadow-up-xs: 0 -1px 2px 0 rgba(73, 88, 131, 0.08);
  --shadow-up-sm: 0 -2px 4px 0 rgba(73, 88, 131, 0.08);
  --shadow-up-md: 0 -4px 8px 0 rgba(73, 88, 131, 0.08);
  --shadow-up-lg: 0 -6px 12px 0 rgba(73, 88, 131, 0.08);
  --shadow-up-xl: 0 -8px 16px 0 rgba(73, 88, 131, 0.08);
  --shadow-down-xs: 0 1px 2px 0 rgba(73, 88, 131, 0.08);
  --shadow-down-sm: 0 2px 4px 0 rgba(73, 88, 131, 0.08);
  --shadow-down-md: 0 4px 8px 0 rgba(73, 88, 131, 0.08);
  --shadow-down-lg: 0 6px 12px 0 rgba(73, 88, 131, 0.08);
  --shadow-down-xl: 0 8px 16px 0 rgba(73, 88, 131, 0.08);

  /* ===== LEGACY DIRECTIONAL SHADOWS ===== */
  --shadow-up: 0 -12px 16px -4px rgba(73, 88, 131, 0.16);
  --shadow-down: 0 12px 16px -4px rgba(73, 88, 131, 0.16);
  --shadow-left: -12px 0 16px -4px rgba(73, 88, 131, 0.16);
  --shadow-right: 12px 0 16px -4px rgba(73, 88, 131, 0.16);
}
```

---

## How to Use Tokens — ALWAYS Use CSS Variables

### ✅ CORRECT — Using CSS variables
```jsx
<p style={{ color: 'var(--text-primary)' }}>Body text</p>
<div style={{ backgroundColor: 'var(--bg-secondary)' }}>...</div>
<div style={{ border: '1px solid var(--border-primary)' }}>...</div>
<div style={{ boxShadow: 'var(--shadow-lg)' }}>...</div>
<div style={{ borderRadius: '12px' }}>...</div>
```

### ❌ WRONG — Hardcoded hex values
```jsx
<p style={{ color: '#353E5A' }}>NEVER do this</p>
<div style={{ backgroundColor: '#F1F4F7' }}>NEVER do this</div>
```

### Tailwind with CSS variables
```jsx
<p className="text-[var(--text-primary)]">Text</p>
<div className="bg-[var(--bg-secondary)] border border-[var(--border-primary)]">...</div>
<div className="shadow-[var(--shadow-lg)] rounded-xl">...</div>
```

---

## Hex-to-Token Translation Table

**When any component spec shows a raw hex value, use this table to find the correct CSS variable:**

| Hex Value | CSS Variable | Context |
|-----------|-------------|---------|
| `#353E5A` | `var(--text-primary)` or `var(--bg-primary-solid)` | Text/headings/labels OR dark shell background |
| `#495883` | `var(--text-secondary)` or `var(--bg-secondary-solid)` | Supporting text OR header button bg |
| `#6B748E` | `var(--text-tertiary)` | Subtle text, metadata |
| `#9298A9` | `var(--text-quaternary)` | Very subtle text |
| `#8EA1AF` | `var(--text-disabled)` | Disabled text, disabled icons |
| `#BDC8D3` | `var(--text-placeholder)` or `var(--border-primary)` | Placeholder text OR card/input borders |
| `#CCD6DC` | `var(--border-secondary)` or `var(--bg-quaternary)` | Separator lines, dividers |
| `#FF725C` | `var(--fg-brand-primary)` or `var(--bg-brand-solid)` | Brand icon color OR primary button bg |
| `#FF8E7D` | `var(--bg-brand-solid_hover)` | Primary button hover bg |
| `#FFFFFF` | `var(--bg-primary)` | White backgrounds, cards |
| `#F7F9FA` | `var(--bg-secondary_subtle)` | Subtle content card background |
| `#F1F4F7` | `var(--bg-secondary)` | Neutral button bg, hover states |
| `#E8ECF1` | `var(--bg-tertiary)` | Neutral button hover, disabled bg |
| `#FDF1EF` | `var(--bg-brand-primary)` | Active nav item bg, selected rows |
| `#FDE7E3` | `var(--bg-brand-primary_alt)` | Active nav item hover bg |
| `#052033` | `var(--tooltip-bg-primary-dark)` | Tooltip background, toast bg |
| `#F02C00` | `var(--bg-error-solid)` | Danger button bg, error text |
| `#FFEBE6` | `var(--bg-error-primary)` | Error banner bg |
| `#31A172` | `var(--text-success-primary)` | Success text |
| `#E3FCEF` | `var(--bg-success-primary)` | Success banner bg |
| `#F08400` | `var(--text-warning-primary)` | Warning text |
| `#FFFAE6` | `var(--bg-warning-primary)` | Warning banner bg |
| `#3D93F5` | `var(--text-info-primary)` | Info text |
| `#F0F7FF` | `var(--bg-info-primary)` | Info banner bg |
| `#8D6AE7` | `var(--text-magic-primary)` | AI/magic text |
| `#F4F3FF` | `var(--bg-magic-primary)` | AI/magic banner bg |
| `#098486` | `var(--text-brand-secondary)` | Secondary brand text |
| `#E7FDFD` | `var(--bg-brand-secondary)` | Secondary brand bg |
| `#0BA5A7` | `var(--bg-brand-secondary-solid_hover)` | Secondary brand solid hover |
| `#B4289A` | `var(--text-magentapink-primary)` | Magenta pink text |

**Rule:** Same hex can map to different tokens based on context. `#BDC8D3` is `var(--border-primary)` when used as a border, but `var(--text-placeholder)` when used as text color. Always choose the **semantic** token matching the usage context.

---

## ⚠️ Self-Audit Rule

**After generating any component, scan your code for raw hex values (#xxxxxx). If you find any, replace them with the corresponding `var(--token-name)` from the table above.** The ONLY exception is inside the `:root` CSS variable definitions themselves.

---

## Color Palette

### Primary Brand (Coral/Red-Orange)

| Token | Hex |
|-------|-----|
| primary-25 | #FFF9F8 |
| primary-50 | #FDF1EF |
| primary-100 | #FDE7E3 |
| primary-200 | #FFC7BE |
| primary-300 | #FFAA9D |
| primary-400 | #FF8E7D |
| primary-500 | #FF725C |
| primary-600 | #E66753 |
| primary-700 | #CC5B4A |
| primary-800 | #B35040 |
| primary-900 | #994437 |

**Usage:** Primary actions, brand accents, CTA buttons, active states, links.

### Secondary Brand (Teal/Cyan)

| Token | Hex |
|-------|-----|
| secondary-25 | #F9FFFF |
| secondary-50 | #E7FDFD |
| secondary-100 | #CAFBFB |
| secondary-200 | #83F5F7 |
| secondary-300 | #46E6E8 |
| secondary-400 | #0ED4D7 |
| secondary-500 | #0DC1C4 |
| secondary-600 | #0BA5A7 |
| secondary-700 | #098486 |
| secondary-800 | #076C6E |
| secondary-900 | #055051 |

**Usage:** Secondary actions, highlights, badges, accent elements.

### Primary Text (Blue-Gray)

| Token | Hex |
|-------|-----|
| primary-text-25 | #F8F8FA |
| primary-text-50 | #F0F2F5 |
| primary-text-100 | #E2E4EB |
| primary-text-200 | #D3D7E1 |
| primary-text-300 | #C5C9D7 |
| primary-text-400 | #9298A9 |
| primary-text-500 | #6B748E |
| primary-text-600 | #495883 |
| primary-text-700 | #353E5A |
| primary-text-800 | #222A3F |
| primary-text-900 | #111729 |

**Usage:** Headings, body text, labels, icons. The semantic role `text-primary` (#353E5A / 700) is the default for both headings and body text. 600 for secondary text, 500 for tertiary, 300–400 for placeholders/disabled. Do NOT use 900 for headings — always refer to the semantic color roles section for correct mappings.

### Secondary Text (Cool Gray-Blue)

| Token | Hex |
|-------|-----|
| secondary-text-25 | #F7F9FA |
| secondary-text-50 | #F1F4F7 |
| secondary-text-100 | #E8ECF1 |
| secondary-text-200 | #CCD6DC |
| secondary-text-300 | #BDC8D3 |
| secondary-text-400 | #A9B9C8 |
| secondary-text-500 | #8EA1AF |
| secondary-text-600 | #677B89 |
| secondary-text-700 | #3C5A6F |
| secondary-text-800 | #093555 |
| secondary-text-900 | #052033 |

**Usage:** Descriptions, helper text, metadata, subtle labels.

### Success (Green)

| Token | Hex |
|-------|-----|
| success-25 | #F4FFF9 |
| success-50 | #E3FCEF |
| success-100 | #ABF5D1 |
| success-200 | #79F2C0 |
| success-300 | #57D9A3 |
| success-400 | #43C78F |
| success-500 | #36B37E |
| success-600 | #31A172 |
| success-700 | #2B8D64 |
| success-800 | #237653 |
| success-900 | #1A563D |

**Usage:** Success messages, positive status, confirmations, completed states.

### Warning (Amber/Orange)

| Token | Hex |
|-------|-----|
| warning-25 | #FFFDF6 |
| warning-50 | #FFFAE6 |
| warning-100 | #FFF0B3 |
| warning-200 | #FFE380 |
| warning-300 | #FFC400 |
| warning-400 | #FFAB00 |
| warning-500 | #FF991F |
| warning-600 | #F08400 |
| warning-700 | #D17300 |
| warning-800 | #B36200 |
| warning-900 | #804600 |

**Usage:** Warning alerts, pending status, caution indicators.

### Error (Red)

| Token | Hex |
|-------|-----|
| error-25 | #FFF7F5 |
| error-50 | #FFEBE6 |
| error-100 | #FFBDAD |
| error-200 | #FF8F73 |
| error-300 | #FF7452 |
| error-400 | #FF5630 |
| error-500 | #FF431B |
| error-600 | #F02C00 |
| error-700 | #D12600 |
| error-800 | #AD2000 |
| error-900 | #801700 |

**Usage:** Error messages, destructive actions, validation errors, required indicators.

### Blue (Info)

| Token | Hex |
|-------|-----|
| blue-25 | #F6FAFF |
| blue-50 | #F0F7FF |
| blue-100 | #E7F1FE |
| blue-200 | #CFE4FC |
| blue-300 | #B6D6FB |
| blue-400 | #94C4F9 |
| blue-500 | #6EAEF7 |
| blue-600 | #3D93F5 |
| blue-700 | #0C70E4 |
| blue-800 | #0A60C2 |
| blue-900 | #074388 |

**Usage:** Informational messages, links (alternative), selected states, info badges.

### Purple (Magic)

| Token | Hex |
|-------|-----|
| purple-25 | #FAFAFF |
| purple-50 | #F4F3FF |
| purple-100 | #F2EDFC |
| purple-200 | #E4DCF9 |
| purple-300 | #D3C6F6 |
| purple-400 | #BFACF2 |
| purple-500 | #AB91ED |
| purple-600 | #8D6AE7 |
| purple-700 | #5925DC |
| purple-800 | #4B1EBD |
| purple-900 | #351584 |

**Usage:** AI features, premium/pro badges, special highlights.

### Magenta Pink

| Token | Hex |
|-------|-----|
| magenta-pink-25 | #FFF2F3 |
| magenta-pink-50 | #FFCAD4 |
| magenta-pink-100 | #FEA2BE |
| magenta-pink-200 | #F77FAE |
| magenta-pink-300 | #EB61A2 |
| magenta-pink-400 | #DC4799 |
| magenta-pink-500 | #C93191 |
| magenta-pink-600 | #B4289A |
| magenta-pink-700 | #9D219D |
| magenta-pink-800 | #711A87 |
| magenta-pink-900 | #4C136F |

**Usage:** Decorative accents, special status indicators.

### AI Gradient Colors

| Token | Hex |
|-------|-----|
| ai-00 | #14BACB |
| ai-01 | #26A9DD |
| ai-02 | #3898F0 |
| ai-03 | #5188F1 |
| ai-04 | #6F79EC |
| ai-05 | #8D6AE6 |
| ai-06 | #B066CD |
| ai-07 | #D363B3 |
| ai-08 | #EC67A0 |
| ai-09 | #F17E99 |
| ai-10 | #F59593 |

**Usage:** AI-themed gradients, magical/special UI elements. Apply as multi-stop linear gradients.

### Constants

| Token | Value |
|-------|-------|
| white | #FFFFFF |
| black | #000000 |
| overlay | #1B1A27A3 |

---

## Semantic Color Roles

Use these semantic mappings instead of raw color tokens when styling UI elements. This ensures consistency.

### Text Colors
| Role | Token Source | Hex |
|------|-------------|-----|
| text-primary | primary-text-700 | `#353E5A` |
| text-primary_on-brand | white | `#FFFFFF` |
| text-secondary | primary-text-600 | `#495883` |
| text-secondary_hover | primary-text-700 | `#353E5A` |
| text-secondary_on-brand | primary-200 | `#FFC7BE` |
| text-tertiary | primary-text-500 | `#6B748E` |
| text-tertiary_hover | primary-text-600 | `#495883` |
| text-tertiary_on-brand | primary-200 | `#FFC7BE` |
| text-quaternary | primary-text-400 | `#9298A9` |
| text-quaternary_on-brand | primary-300 | `#FFAA9D` |
| text-white | white | `#FFFFFF` |
| text-white-disabled | (alpha) | `#ffffff3d` |
| text-disabled | secondary-text-500 | `#8EA1AF` |
| text-disabled-subtle | secondary-text-300 | `#BDC8D3` |
| text-placeholder | secondary-text-300 | `#BDC8D3` |
| text-placeholder_subtle | secondary-text-200 | `#CCD6DC` |
| text-brand-primary | primary-500 | `#FF725C` |
| text-brand-primary_alt | primary-400 | `#FF8E7D` |
| text-brand-primary-disabled | (alpha) | `#FF725C80` |
| text-brand-secondary | secondary-700 | `#098486` |
| text-brand-secondary-disabled | (alpha) | `#0BA5A780` |
| text-success-primary | success-600 | `#31A172` |
| text-warning-primary | warning-600 | `#F08400` |
| text-error-primary | error-600 | `#F02C00` |
| text-error-primary-disabled | (alpha) | `#F02C0080` |
| text-info-primary | blue-600 | `#3D93F5` |
| text-magic-primary | purple-600 | `#8D6AE7` |
| text-magentapink-primary | magenta-pink-600 | `#B4289A` |

### Background Colors
| Role | Token Source | Hex |
|------|-------------|-----|
| bg-primary | white | `#FFFFFF` |
| bg-primary_hover | secondary-text-50 | `#F1F4F7` |
| bg-primary-solid | primary-text-700 | `#353E5A` |
| bg-primary-solid_hover | primary-text-800 | `#222A3F` |
| bg-secondary | secondary-text-50 | `#F1F4F7` |
| bg-secondary_hover | secondary-text-100 | `#E8ECF1` |
| bg-secondary_subtle | secondary-text-25 | `#F7F9FA` |
| bg-secondary-solid | primary-text-600 | `#495883` |
| bg-secondary-solid_hover | primary-text-700 | `#353E5A` |
| bg-tertiary | secondary-text-100 | `#E8ECF1` |
| bg-quaternary | secondary-text-200 | `#CCD6DC` |
| bg-active | secondary-text-50 | `#F1F4F7` |
| bg-disabled | secondary-text-100 | `#E8ECF1` |
| bg-disabled_subtle | secondary-text-50 | `#F1F4F7` |
| bg-transparent | transparent | `transparent` |
| bg-alpha-white | (alpha) | `#ffffff14` |
| bg-alpha-white_hover | (alpha) | `#ffffff29` |
| bg-alpha-white-disabled | (alpha) | `#ffffff5c` |
| bg_overlay | (alpha) | `#4958838f` |
| bg-brand-primary | primary-50 | `#FDF1EF` |
| bg-brand-primary_alt | primary-100 | `#FDE7E3` |
| bg-brand-solid | primary-500 | `#FF725C` |
| bg-brand-solid_hover | primary-400 | `#FF8E7D` |
| bg-brand-solid-disabled | (alpha) | `#FF725C5c` |
| bg-brand-secondary | secondary-50 | `#E7FDFD` |
| bg-brand-secondary_alt | secondary-100 | `#CAFBFB` |
| bg-brand-secondary-solid | secondary-700 | `#098486` |
| bg-brand-secondary-solid_hover | secondary-600 | `#0BA5A7` |
| bg-brand-secondary-solid_disabled | (alpha) | `#0BA5A7f2` |
| bg-brand-section-primary | primary-600 | `#E66753` |
| bg-brand-section-secondary | secondary-500 | `#0DC1C4` |
| bg-brand-section-primary-subtle | primary-500 | `#FF725C` |
| bg-error-primary | error-50 | `#FFEBE6` |
| bg-error-secondary | error-100 | `#FFBDAD` |
| bg-error-solid | error-600 | `#F02C00` |
| bg-error-solid_hover | error-500 | `#FF431B` |
| bg-error-solid-disabled | (alpha) | `#F02C005c` |
| bg-warning-primary | warning-50 | `#FFFAE6` |
| bg-warning-secondary | warning-100 | `#FFF0B3` |
| bg-warning-solid | warning-600 | `#F08400` |
| bg-warning-solid_hover | warning-500 | `#FF991F` |
| bg-success-primary | success-50 | `#E3FCEF` |
| bg-success-secondary | success-100 | `#ABF5D1` |
| bg-success-solid | success-600 | `#31A172` |
| bg-success-solid_hover | success-500 | `#36B37E` |
| bg-info-primary | blue-50 | `#F0F7FF` |
| bg-info-secondary | blue-100 | `#E7F1FE` |
| bg-info-solid | blue-600 | `#3D93F5` |
| bg-info-solid_hover | blue-500 | `#6EAEF7` |
| bg-magic-primary | purple-50 | `#F4F3FF` |
| bg-magic-secondary | purple-100 | `#F2EDFC` |
| bg-magic-solid | purple-600 | `#8D6AE7` |
| bg-magic-solid_hover | purple-500 | `#AB91ED` |
| bg-magentapink-primary | magenta-pink-50 | `#FFCAD4` |
| bg-magentapink-secondary | magenta-pink-100 | `#FEA2BE` |
| bg-magentapink-solid | magenta-pink-600 | `#B4289A` |
| bg-magentapink-solid_hover | magenta-pink-500 | `#C93191` |

### Border Colors
| Role | Token Source | Hex |
|------|-------------|-----|
| border-primary | secondary-text-300 | `#BDC8D3` |
| border-primary-solid | primary-text-700 | `#353E5A` |
| border-secondary | secondary-text-200 | `#CCD6DC` |
| border-secondary-solid | secondary-text-500 | `#8EA1AF` |
| border-tertiary | secondary-text-100 | `#E8ECF1` |
| border-quaternary | secondary-text-50 | `#F1F4F7` |
| border-inverted | white | `#ffffff` |
| border-disabled | secondary-text-300 | `#BDC8D3` |
| border-transparent | transparent | `transparent` |
| border-white | white | `#ffffff` |
| border-disabled_subtle | secondary-text-200 | `#CCD6DC` |
| border-white-disabled | (alpha) | `#ffffff80` |
| border-brand | primary-300 | `#FFAA9D` |
| border-brand-solid | primary-500 | `#FF725C` |
| border-brand-solid-disabled | (alpha) | `#FF725C80` |
| border-brand_subtle | primary-50 | `#FDF1EF` |
| border-brand-secondary | secondary-300 | `#46E6E8` |
| border-brand-secondary-solid | secondary-600 | `#0BA5A7` |
| border-brand-secondary_subtle | secondary-50 | `#E7FDFD` |
| border-brand-secondary_solid-disabled | (alpha) | `#0BA5A780` |
| border-error | error-300 | `#FF7452` |
| border-error-solid | error-600 | `#F02C00` |
| border-error_subtle | error-50 | `#FFEBE6` |
| border-error-disabled | (alpha) | `#F02C0080` |
| border-warning | warning-300 | `#FFC400` |
| border-warning-solid | warning-600 | `#F08400` |
| border-warning_subtle | warning-50 | `#FFFAE6` |
| border-success | success-300 | `#57D9A3` |
| border-success-solid | success-600 | `#31A172` |
| border-success_subtle | success-50 | `#E3FCEF` |
| border-info | blue-300 | `#B6D6FB` |
| border-info-solid | blue-600 | `#3D93F5` |
| border-info_subtle | blue-50 | `#F0F7FF` |
| border-magic | purple-300 | `#D3C6F6` |
| border-magic-solid | purple-600 | `#8D6AE7` |
| border-magic_subtle | purple-50 | `#F4F3FF` |
| border-magentapink | magenta-pink-300 | `#EB61A2` |
| border-magentapink-solid | magenta-pink-600 | `#B4289A` |
| border-magentapink_subtle | magenta-pink-50 | `#FFCAD4` |

### Foreground / Icon Colors
| Role | Token Source | Hex |
|------|-------------|-----|
| fg-primary | primary-text-700 | `#353E5A` |
| fg-secondary | primary-text-600 | `#495883` |
| fg-secondary_hover | primary-text-700 | `#353E5A` |
| fg-tertiary | primary-text-500 | `#6B748E` |
| fg-tertiary_hover | primary-text-600 | `#495883` |
| fg-quaternary | primary-text-400 | `#9298A9` |
| fg-quaternary_hover | primary-text-500 | `#6B748E` |
| fg-quinary | primary-text-300 | `#C5C9D7` |
| fg-quinary_hover | primary-text-400 | `#9298A9` |
| fg-senary | primary-text-200 | `#D3D7E1` |
| fg-white | white | `#FFFFFF` |
| fg-white-disabled | (alpha) | `#ffffff3d` |
| fg-disabled | secondary-text-500 | `#8EA1AF` |
| fg-disabled_subtle | secondary-text-300 | `#BDC8D3` |
| fg-brand-primary | primary-500 | `#FF725C` |
| fg-brand-primary_alt | primary-400 | `#FF8E7D` |
| fg-brand-primary-disabled | (alpha) | `#FF725C80` |
| fg-brand-secondary | secondary-700 | `#098486` |
| fg-brand-secondary_alt | secondary-600 | `#0BA5A7` |
| fg-brand-secondary-disabled | (alpha) | `#0BA5A780` |
| fg-error-primary | error-600 | `#F02C00` |
| fg-error-secondary | error-500 | `#FF431B` |
| fg-error-disabled | (alpha) | `#F02C0080` |
| fg-warning-primary | warning-600 | `#F08400` |
| fg-warning-secondary | warning-500 | `#FF991F` |
| fg-success-primary | success-600 | `#31A172` |
| fg-success-secondary | success-500 | `#36B37E` |
| fg-info-primary | blue-600 | `#3D93F5` |
| fg-info-secondary | blue-500 | `#6EAEF7` |
| fg-magic-primary | purple-600 | `#8D6AE7` |
| fg-magic-secondary | purple-500 | `#AB91ED` |
| fg-magentapink-primary | magenta-pink-600 | `#B4289A` |
| fg-magentapink-secondary | magenta-pink-500 | `#C93191` |

### Toast Colors
| Role | Hex |
|------|-----|
| toast-bg-primary | `#052033` (secondary-text-900) |
| toast-text-primary | `#FFFFFF` |
| toast-fg-white | `#ffffff` |
| toast-fg-info-primary | `#6EAEF7` (blue-500) |
| toast-fg-success-primary | `#36B37E` (success-500) |
| toast-fg-warning-primary | `#FF991F` (warning-500) |
| toast-fg-danger-primary | `#FF431B` (error-500) |

### Tooltip Colors
| Role | Hex |
|------|-----|
| tooltip-bg-primary-dark | `#052033` (secondary-text-900) |
| tooltip-text-primary-dark | `#FFFFFF` |
| tooltip-bg-primary-white | `#FFFFFF` |
| tooltip-text-primary-white | `#353E5A` (primary-text-700) |

---

## Typography

Use ONLY these typography presets. Do NOT use arbitrary font sizes or weights.

### ⚠️ Font Family — Inter ONLY (MANDATORY)

The **entire UI uses Inter** as the sole font family. There is NO secondary or heading font — everything is Inter.

```css
/* Import from Google Fonts — MUST be included */
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@100..900&display=swap');
```

```css
/* Set on body — all components inherit */
body {
  font-family: 'Inter', sans-serif;
  font-weight: 400;
  font-style: normal;
}
```

**CRITICAL:** NEVER use `ui-sans-serif`, `system-ui`, `Arial`, `Helvetica`, the Tailwind default `font-sans`, or any other font. ALL text in the entire application must render in Inter. When configuring Tailwind, override `fontFamily.sans` so every default Tailwind `font-sans` usage also resolves to Inter:

```js
// tailwind.config.js
module.exports = {
  theme: {
    extend: {
      fontFamily: {
        sans: ['Inter', 'sans-serif'],
      },
    },
  },
};
```

There are 4 typography types. "paragraph", "caption", and "title" share the same content font size scale. "heading" uses a larger heading font size scale. All 4 types use Inter — there is no separate heading font.

### Content Font Size Scale (used by paragraph, caption, title)

| Size | Font Size | Line Height |
|------|-----------|-------------|
| xs | 10px (0.625rem) | 16px (1rem) |
| sm | 12px (0.75rem) | 18px (1.125rem) |
| md | 14px (0.875rem) | 20px (1.25rem) |
| lg | 16px (1rem) | 24px (1.5rem) |
| xl | 18px (1.125rem) | 28px (1.75rem) |

### Heading Font Size Scale (used by heading only)

| Size | Font Size | Line Height |
|------|-----------|-------------|
| sm | 20px (1.25rem) | 28px (1.75rem) |
| md | 24px (1.5rem) | 32px (2rem) |
| lg | 32px (2rem) | 40px (2.5rem) |
| xl | 40px (2.5rem) | 52px (3.25rem) |

### Typography Types

| Type | Weight | Available Sizes | Font Scale | Use For |
|------|--------|-----------------|------------|---------|
| paragraph | 400 | xs, sm, md, lg | Content | Body text, descriptions, long-form content |
| caption | 500 | xs, sm, md, lg | Content | Labels, captions, helper text, secondary emphasis |
| title | 600 | sm, md, lg, xl | Content | Inline titles, bold labels, card headers, emphasis text |
| heading | 600 | sm, md, lg, xl | Heading | Page titles, section headings, hero text |

### Quick Reference — All Presets

| Preset | Font Size | Line Height | Weight |
|--------|-----------|-------------|--------|
| paragraph-xs | 10px | 16px | 400 |
| paragraph-sm | 12px | 18px | 400 |
| paragraph-md | 14px | 20px | 400 |
| paragraph-lg | 16px | 24px | 400 |
| caption-xs | 10px | 16px | 500 |
| caption-sm | 12px | 18px | 500 |
| caption-md | 14px | 20px | 500 |
| caption-lg | 16px | 24px | 500 |
| title-sm | 12px | 18px | 600 |
| title-md | 14px | 20px | 600 |
| title-lg | 16px | 24px | 600 |
| title-xl | 18px | 28px | 600 |
| heading-sm | 20px | 28px | 600 |
| heading-md | 24px | 32px | 600 |
| heading-lg | 32px | 40px | 600 |
| heading-xl | 40px | 52px | 600 |

### Typography Rules
- Default body text: paragraph-md (14px / weight 400)
- Default labels/captions: caption-sm (12px / weight 500)
- Default bold labels/card titles: title-sm (12px / weight 600)
- Default section headers: heading-sm (20px / weight 600)
- Default page titles: heading-md (24px / weight 600)
- NEVER use font sizes outside this scale
- NEVER use font weights other than 400, 500, or 600

---

## Spacing

Base unit: **4px**. All spacing values MUST be multiples of 4px (with two half-step exceptions).

| Token | Value |
|-------|-------|
| 0 | 0px |
| 0.5 | 2px |
| 1 | 4px |
| 1.5 | 6px |
| 2 | 8px |
| 2.5 | 10px |
| 3 | 12px |
| 4 | 16px |
| 5 | 20px |
| 6 | 24px |
| 8 | 32px |
| 10 | 40px |
| 12 | 48px |
| 20 | 80px |

### Spacing Rules
- Padding inside components: typically 2 (8px), 3 (12px), or 4 (16px)
- Gap between elements: typically 2 (8px) or 3 (12px)
- Section spacing: typically 6 (24px) or 8 (32px)
- Page-level padding: typically 6 (24px) or 8 (32px)
- NEVER use arbitrary spacing values like 5px, 7px, 15px, 18px, etc.

---

## Border Radius

Base unit: **2px**. All border radius values MUST come from this scale.

| Token | Value | Use For |
|-------|-------|---------|
| 0 | 0px | Sharp corners, no rounding |
| 2 | 4px | Subtle rounding — small tags, minor elements |
| 3 | 6px | Default rounding — buttons, inputs, cards, dropdowns |
| 4 | 8px | Medium rounding — modals, dialogs, larger cards |
| 5 | 10px | Prominent rounding — featured cards, panels |
| 6 | 12px | Large rounding — hero sections, callouts |
| 10 | 20px | Extra large rounding — pills, floating panels |
| full | 9999px | Fully circular — avatars, round badges, pills |

### Border Radius Rules
- Default for buttons: SM=6px, MD=8px, LG=10px (see button.md for per-size values)
- Default for inputs: 6px (radius-3)
- Default for cards: 8px (radius-4)
- Default for modals/dialogs: 8px (radius-4)
- Avatars and circular elements: 9999px (radius-full)
- NEVER use arbitrary radius values like 3px, 5px, 7px, etc.

---

## Shadows

### Elevation Shadows (use for layering and depth)

| Token | Value | Use For |
|-------|-------|---------|
| shadow-xs | 0 1px 2px 0 rgba(73,88,131,0.08) | Subtle depth |
| shadow-sm | 0 1px 2px 0 rgba(73,88,131,0.04), 0 1px 3px 0 rgba(73,88,131,0.16) | Light elevation |
| shadow-md | 0 2px 4px -2px rgba(73,88,131,0.04), 0 4px 8px -2px rgba(73,88,131,0.16) | Medium elevation |
| shadow-lg | 0 4px 6px -2px rgba(73,88,131,0.04), 0 12px 16px -4px rgba(73,88,131,0.16) | High elevation |
| shadow-xl | 0 8px 8px -4px rgba(73,88,131,0.04), 0 20px 24px -4px rgba(73,88,131,0.16) | Very high elevation |
| shadow-2xl | 0 24px 48px -12px rgba(73,88,131,0.16) | Maximum elevation |
| shadow-3xl | 0 32px 64px -12px rgba(73,88,131,0.16) | Extra large overlay |
| shadow-4xl | 0 40px 80px -16px rgba(73,88,131,0.16) | Massive overlay |
| shadow-5xl | 0 48px 96px -16px rgba(73,88,131,0.24) | Extreme elevation |

### Focus Ring Shadows (use for focus states)

| Token | Value | Use For |
|-------|-------|---------|
| ring-brand-primary | 0 0 0 4px rgba(255,114,92,0.24) | Focus on brand elements |
| ring-brand-secondary | 0 0 0 4px rgba(11,165,167,0.24) | Focus on secondary brand |
| ring-gray-primary | 0 0 0 4px rgba(73,88,131,0.08) | Focus on neutral elements |
| ring-gray-secondary | 0 0 0 4px rgba(9,53,85,0.08) | Focus on secondary neutral |
| ring-error-primary | 0 0 0 4px rgba(240,44,0,0.24) | Focus on error elements |

### Focus Combinations (ring + shadow combined)

| Token | Value | Use For |
|-------|-------|---------|
| focus-brand-xs | ring-brand-primary + shadow-xs | Brand focus with subtle depth |
| focus-brand-sm | ring-brand-primary + shadow-sm | Brand focus with light elevation |
| focus-gray-xs | ring-gray-primary + shadow-xs | Neutral focus with subtle depth |
| focus-gray-sm | ring-gray-primary + shadow-sm | Neutral focus with light elevation |
| focus-error-xs | ring-error-primary + shadow-xs | Error focus with subtle depth |
| focus-brand-secondary-xs | ring-brand-secondary + shadow-xs | Secondary brand focus with subtle depth |
| focus-brand-secondary-sm | ring-brand-secondary + shadow-sm | Secondary brand focus with light elevation |

### Directional Shadows

| Token | Value | Use For |
|-------|-------|---------|
| shadow-left-xs | -1px 0 2px 0 rgba(73,88,131,0.08) | Subtle left shadow |
| shadow-left-sm | -2px 0 4px 0 rgba(73,88,131,0.08) | Small left shadow |
| shadow-left-md | -4px 0 8px 0 rgba(73,88,131,0.08) | Medium left shadow |
| shadow-left-lg | -6px 0 12px 0 rgba(73,88,131,0.08) | Large left shadow |
| shadow-left-xl | -8px 0 16px 0 rgba(73,88,131,0.08) | Extra large left shadow |
| shadow-right-xs | 1px 0 2px 0 rgba(73,88,131,0.08) | Subtle right shadow |
| shadow-right-sm | 2px 0 4px 0 rgba(73,88,131,0.08) | Small right shadow |
| shadow-right-md | 4px 0 8px 0 rgba(73,88,131,0.08) | Medium right shadow |
| shadow-right-lg | 6px 0 12px 0 rgba(73,88,131,0.08) | Large right shadow |
| shadow-right-xl | 8px 0 16px 0 rgba(73,88,131,0.08) | Extra large right shadow |
| shadow-up-xs | 0 -1px 2px 0 rgba(73,88,131,0.08) | Subtle top shadow |
| shadow-up-sm | 0 -2px 4px 0 rgba(73,88,131,0.08) | Small top shadow |
| shadow-up-md | 0 -4px 8px 0 rgba(73,88,131,0.08) | Medium top shadow |
| shadow-up-lg | 0 -6px 12px 0 rgba(73,88,131,0.08) | Large top shadow |
| shadow-up-xl | 0 -8px 16px 0 rgba(73,88,131,0.08) | Extra large top shadow |
| shadow-down-xs | 0 1px 2px 0 rgba(73,88,131,0.08) | Subtle bottom shadow |
| shadow-down-sm | 0 2px 4px 0 rgba(73,88,131,0.08) | Small bottom shadow |
| shadow-down-md | 0 4px 8px 0 rgba(73,88,131,0.08) | Medium bottom shadow |
| shadow-down-lg | 0 6px 12px 0 rgba(73,88,131,0.08) | Large bottom shadow |
| shadow-down-xl | 0 8px 16px 0 rgba(73,88,131,0.08) | Extra large bottom shadow |

### Shadow Rules
- Cards at rest: shadow-xs or shadow-sm
- Dropdowns/popovers: shadow-md or shadow-lg
- Modals/dialogs: shadow-lg or shadow-xl
- Focus states: Always combine ring + shadow-xs (use focus combination tokens)
- NEVER use arbitrary box-shadow values
- ALL shadows use rgb(73, 88, 131) as the base color with varying alpha levels

---

## Breakpoints

| Token | Value | Target |
|-------|-------|--------|
| xs | 480px | Mobile phones |
| sm | 768px | Tablets |
| md | 1024px | Small desktops / landscape tablets |
| lg | 1440px | Large desktops |

### Responsive Rules
- Design mobile-first (start with smallest, add overrides for larger)
- Content max-width on large screens: 1440px
- Use these exact breakpoints for all media queries

---

## Animation & Transitions

| Token | Duration | Use For |
|-------|----------|---------|
| fast | 200ms | Micro-interactions — toggles, checkboxes, color changes |
| medium | 250ms | Small transitions — hover states, button feedback |
| default | 300ms | Standard transitions — opening menus, expanding panels |
| slow | 400ms | Large transitions — modals, page transitions |

### Animation Rules
- Default easing: ease-out
- Hover state transitions: 150ms ease-out
- All interactive elements MUST have transitions (never instant state changes)
- NEVER use durations outside this scale

---

## Popup / Menu Widths

| Token | Value | Use For |
|-------|-------|---------|
| popup-0 | max-content | Auto-width menus |
| popup-xs | 192px | Narrow menus, simple dropdowns |
| popup-sm | 224px | Standard dropdowns |
| popup-md | 256px | Medium menus with descriptions |
| popup-lg | 300px | Wide menus, filter panels |
| popup-xl | 384px | Extra wide — search results, complex menus |

---

## AI Gradients

When building AI-themed UI elements, use these gradient definitions:

### Text Gradient (AI-themed text)
```css
background: linear-gradient(90deg, #0BA5A7, #3D93F5, #8D6AE7);
-webkit-background-clip: text;
-webkit-text-fill-color: transparent;
```

### Background Gradient (AI-themed surfaces)
```css
/* Light AI background */
background: linear-gradient(98.42deg, #F8F8FA 3.79%, #F4F3FF 51.25%, #FDF1EF 98.71%);

/* Solid AI gradient */
background: linear-gradient(98.42deg, #14BACB, #5188F1, #B066CD, #F17E99, #F59593);
```

---

## Utility Class Reference

Our design system provides utility classes. When building in Lovable (which uses Tailwind), replicate these exact visual patterns using Tailwind equivalents. The class names below document our naming conventions — use them as a guide for what Tailwind classes to apply.

### Spacing Utilities

**Margin classes** — pattern: `m{direction}-{value}`
- Directions: `t` (top), `b` (bottom), `l` (left), `r` (right), `x` (horizontal), `y` (vertical), or omit for all sides
- Values: `0`, `0_5` (2px), `1` (4px), `1_5` (6px), `2` (8px), `2_5` (10px), `3` (12px), `4` (16px), `5` (20px), `6` (24px), `8` (32px), `10` (40px), `12` (48px), `20` (80px)
- Examples: `.mt-2` = margin-top 8px, `.mx-4` = margin-left/right 16px, `.m-auto` = margin auto

**Padding classes** — pattern: `p{direction}-{value}`
- Same directions and values as margin
- Examples: `.p-3` = padding 12px, `.px-4` = padding-left/right 16px, `.py-2` = padding-top/bottom 8px

**Gap classes** — pattern: `gap-{value}`
- Examples: `.gap-2` = 8px gap, `.gap-3` = 12px gap

**Tailwind equivalents to use in Lovable:**
| Our Class | Tailwind Equivalent |
|-----------|-------------------|
| .m-1 (4px) | `m-1` |
| .m-2 (8px) | `m-2` |
| .m-3 (12px) | `m-3` |
| .m-4 (16px) | `m-4` |
| .m-5 (20px) | `m-5` |
| .m-6 (24px) | `m-6` |
| .m-8 (32px) | `m-8` |
| .m-10 (40px) | `m-10` |
| .m-12 (48px) | `m-12` |
| .m-20 (80px) | `m-20` |
Same pattern for padding (`p-`), gap (`gap-`), and directional variants (`mt-`, `px-`, etc.)

### Border Radius Utilities

Pattern: `radius-{value}`

| Our Class | Value | Tailwind Equivalent |
|-----------|-------|-------------------|
| .radius-0 | 0px | `rounded-none` |
| .radius-2 | 4px | `rounded` |
| .radius-3 | 6px | `rounded-md` |
| .radius-4 | 8px | `rounded-lg` |
| .radius-5 | 10px | `rounded-[10px]` |
| .radius-6 | 12px | `rounded-xl` |
| .radius-10 | 20px | `rounded-[20px]` |
| .radius-full | 9999px | `rounded-full` |

### Typography Utilities

Pattern: `{type}-{size}`

| Our Class | Font Size | Weight | Tailwind Equivalent |
|-----------|-----------|--------|-------------------|
| .paragraph-xs | 10px | 400 | `text-[10px] leading-[16px] font-normal` |
| .paragraph-sm | 12px | 400 | `text-xs leading-[18px] font-normal` |
| .paragraph-md | 14px | 400 | `text-sm leading-5 font-normal` |
| .paragraph-lg | 16px | 400 | `text-base leading-6 font-normal` |
| .caption-xs | 10px | 500 | `text-[10px] leading-[16px] font-medium` |
| .caption-sm | 12px | 500 | `text-xs leading-[18px] font-medium` |
| .caption-md | 14px | 500 | `text-sm leading-5 font-medium` |
| .caption-lg | 16px | 500 | `text-base leading-6 font-medium` |
| .title-sm | 12px | 600 | `text-xs leading-[18px] font-semibold` |
| .title-md | 14px | 600 | `text-sm leading-5 font-semibold` |
| .title-lg | 16px | 600 | `text-base leading-6 font-semibold` |
| .title-xl | 18px | 600 | `text-lg leading-7 font-semibold` |
| .heading-sm | 20px | 600 | `text-xl leading-7 font-semibold` |
| .heading-md | 24px | 600 | `text-2xl leading-8 font-semibold` |
| .heading-lg | 32px | 600 | `text-[32px] leading-10 font-semibold` |
| .heading-xl | 40px | 600 | `text-[40px] leading-[52px] font-semibold` |

### Text Formatting Utilities

| Our Class | CSS | Tailwind Equivalent |
|-----------|-----|-------------------|
| .text-capitalize | text-transform: capitalize | `capitalize` |
| .text-uppercase | text-transform: uppercase | `uppercase` |
| .text-italic | font-style: italic | `italic` |
| .text-strikethrough | text-decoration: line-through | `line-through` |
| .text-overflow-ellipsis | overflow: hidden; text-overflow: ellipsis; white-space: nowrap | `truncate` |
| .text-nowrap | white-space: nowrap | `whitespace-nowrap` |
| .text-pre-line | white-space: pre-line | `whitespace-pre-line` |
| .text-pre-wrap | white-space: pre-wrap | `whitespace-pre-wrap` |
| .break-all | word-break: break-all | `break-all` |
| .break-word | word-break: break-word | `break-words` |
| .text-center | text-align: center | `text-center` |
| .text-start | text-align: start | `text-left` |
| .text-end | text-align: end | `text-right` |

### Display & Flexbox Utilities

| Our Class | CSS | Tailwind Equivalent |
|-----------|-----|-------------------|
| .d-block | display: block | `block` |
| .d-inline | display: inline | `inline` |
| .d-inline-block | display: inline-block | `inline-block` |
| .d-inline-flex | display: inline-flex | `inline-flex` |
| .d-flex | display: flex | `flex` |
| .d-grid | display: grid | `grid` |
| .d-none | display: none | `hidden` |
| .flex-row | flex-direction: row | `flex-row` |
| .flex-column | flex-direction: column | `flex-col` |
| .flex-row-reverse | flex-direction: row-reverse | `flex-row-reverse` |
| .flex-column-reverse | flex-direction: column-reverse | `flex-col-reverse` |
| .justify-center | justify-content: center | `justify-center` |
| .justify-space-between | justify-content: space-between | `justify-between` |
| .justify-start | justify-content: start | `justify-start` |
| .justify-end | justify-content: flex-end | `justify-end` |
| .align-center | align-items: center | `items-center` |
| .align-start | align-items: start | `items-start` |
| .align-end | align-items: flex-end | `items-end` |
| .flex-1 | flex: 1 | `flex-1` |
| .flex-wrap | flex-wrap: wrap | `flex-wrap` |
| .no-shrink | flex-shrink: 0 | `shrink-0` |

### Dimension Utilities

| Our Class | CSS | Tailwind Equivalent |
|-----------|-----|-------------------|
| .w-100 | width: 100% | `w-full` |
| .w-auto | width: auto | `w-auto` |
| .w-fit-content | width: fit-content | `w-fit` |
| .h-100 | height: 100% | `h-full` |
| .h-auto | height: auto | `h-auto` |
| .overflow-auto | overflow: auto | `overflow-auto` |
| .overflow-hidden | overflow: hidden | `overflow-hidden` |
| .overflow-visible | overflow: visible | `overflow-visible` |

### Shadow Utilities

Pattern: `shadow-{size}`

| Our Class | Tailwind Equivalent |
|-----------|-------------------|
| .shadow-xs | `shadow-sm` (customize value to match our token) |
| .shadow-sm | `shadow` (customize value to match our token) |
| .shadow-md | `shadow-md` (customize value to match our token) |
| .shadow-lg | `shadow-lg` (customize value to match our token) |
| .shadow-xl | `shadow-xl` (customize value to match our token) |
| .shadow-2xl | `shadow-2xl` (customize value to match our token) |

For exact shadow values, always refer to the Shadows section in design tokens above.

### Border Utilities

Pattern: `border-{type}` or `border-{direction}-{type}`

| Our Class | CSS | Tailwind Equivalent |
|-----------|-----|-------------------|
| .border-primary | 1px solid var(--border-primary) | `border border-[var(--border-primary)]` |
| .border-secondary | 1px solid var(--border-secondary) | `border border-[var(--border-secondary)]` |
| .border-tertiary | 1px solid var(--border-tertiary) | `border border-[var(--border-tertiary)]` |
| .border-top-primary | border-top only | `border-t border-t-[var(--border-primary)]` |
| .border-bottom-primary | border-bottom only | `border-b border-b-[var(--border-primary)]` |
| .border-left-primary | border-left only | `border-l border-l-[var(--border-primary)]` |
| .border-right-primary | border-right only | `border-r border-r-[var(--border-primary)]` |

### Position Utilities

| Our Class | CSS | Tailwind Equivalent |
|-----------|-----|-------------------|
| .position-relative | position: relative | `relative` |
| .position-absolute | position: absolute | `absolute` |
| .position-sticky | position: sticky | `sticky` |
| .position-fixed | position: fixed | `fixed` |
| .absolute-center | absolute + centered via transform | `absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2` |
| .sticky-top | sticky + top: 0 | `sticky top-0` |

### Cursor & Interaction Utilities

| Our Class | CSS | Tailwind Equivalent |
|-----------|-----|-------------------|
| .cursor-pointer | cursor: pointer | `cursor-pointer` |
| .cursor-move | cursor: move | `cursor-move` |
| .pointer-events-none | pointer-events: none | `pointer-events-none` |
| .user-select-none | user-select: none | `select-none` |

### Responsive Variants

All spacing and typography utilities support responsive prefixes matching our breakpoints:
- `xs:` → 480px (Tailwind: use custom `xs:` breakpoint or `min-[480px]:`)
- `sm:` → 768px (Tailwind: `sm:` — note: Tailwind default sm is 640px, customize to 768px)
- `md:` → 1024px (Tailwind: `md:` — note: Tailwind default md is 768px, customize to 1024px)
- `lg:` → 1440px (Tailwind: `lg:` — note: Tailwind default lg is 1024px, customize to 1440px)

**IMPORTANT:** When setting up the Tailwind config, override the default breakpoints to match our design system:
```js
screens: {
  'xs': '480px',
  'sm': '768px',
  'md': '1024px',
  'lg': '1440px',
}
```

---

## Global Rules for This Design System

1. **Font family is ALWAYS Inter.** Import Inter from Google Fonts and set `font-family: 'Inter', sans-serif` on the body. Override Tailwind's `fontFamily.sans` to `['Inter', 'sans-serif']`. NEVER use `ui-sans-serif`, `system-ui`, `Arial`, `Helvetica`, or the Tailwind default sans stack.
2. **ONLY use colors from the palette above.** Never use arbitrary hex values, Tailwind defaults, or other color systems.
3. **ONLY use spacing from the 4px grid.** Every margin, padding, and gap must be a value from the spacing scale.
4. **ONLY use border-radius from the scale.** No arbitrary rounding.
5. **ONLY use shadows from the shadow scale.** No arbitrary box-shadows.
6. **ONLY use the defined typography presets.** No arbitrary font sizes or weights.
7. **Use semantic color roles** (text-primary, bg-secondary, border-primary, etc.) rather than raw palette values whenever possible.
8. **Primary brand color is coral (#FF725C)**, not blue. All CTAs and brand elements use this.
9. **Secondary brand color is teal (#0DC1C4)**.
10. **Text colors use the blue-gray scale** (primary-text and secondary-text), not pure black or gray.
11. **Default border color is #CCD6DC** (secondary-text-200, `border-secondary`), not gray-200 or similar.
12. **Background hierarchy:** white (#FFFFFF) → #F1F4F7 (bg-secondary) → #E8ECF1 (bg-tertiary) → #CCD6DC (bg-quaternary). Never use Tailwind gray-50, gray-100 etc.
13. When in doubt, refer to the semantic color roles table above.

---

## Tailwind Configuration

When creating the project, configure tailwind.config.js to match our design system. Override defaults so Tailwind classes produce the correct values:

```js
// tailwind.config.js — extend with our design tokens
module.exports = {
  theme: {
    screens: {
      xs: '480px',
      sm: '768px',
      md: '1024px',
      lg: '1440px',
    },
    extend: {
      colors: {
        // Brand
        primary: {
          25: '#FFF9F8', 50: '#FDF1EF', 100: '#FDE7E3', 200: '#FFC7BE',
          300: '#FFAA9D', 400: '#FF8E7D', 500: '#FF725C', 600: '#E66753',
          700: '#CC5B4A', 800: '#B35040', 900: '#994437',
        },
        secondary: {
          25: '#F9FFFF', 50: '#E7FDFD', 100: '#CAFBFB', 200: '#83F5F7',
          300: '#46E6E8', 400: '#0ED4D7', 500: '#0DC1C4', 600: '#0BA5A7',
          700: '#098486', 800: '#076C6E', 900: '#055051',
        },
        // Text grays
        'primary-text': {
          25: '#F8F8FA', 50: '#F0F2F5', 100: '#E2E4EB', 200: '#D3D7E1',
          300: '#C5C9D7', 400: '#9298A9', 500: '#6B748E', 600: '#495883',
          700: '#353E5A', 800: '#222A3F', 900: '#111729',
        },
        'secondary-text': {
          25: '#F7F9FA', 50: '#F1F4F7', 100: '#E8ECF1', 200: '#CCD6DC',
          300: '#BDC8D3', 400: '#A9B9C8', 500: '#8EA1AF', 600: '#677B89',
          700: '#3C5A6F', 800: '#093555', 900: '#052033',
        },
        // Semantic
        success: {
          25: '#F4FFF9', 50: '#E3FCEF', 100: '#ABF5D1', 200: '#79F2C0',
          300: '#57D9A3', 400: '#43C78F', 500: '#36B37E', 600: '#31A172',
          700: '#2B8D64', 800: '#237653', 900: '#1A563D',
        },
        warning: {
          25: '#FFFDF6', 50: '#FFFAE6', 100: '#FFF0B3', 200: '#FFE380',
          300: '#FFC400', 400: '#FFAB00', 500: '#FF991F', 600: '#F08400',
          700: '#D17300', 800: '#B36200', 900: '#804600',
        },
        error: {
          25: '#FFF7F5', 50: '#FFEBE6', 100: '#FFBDAD', 200: '#FF8F73',
          300: '#FF7452', 400: '#FF5630', 500: '#FF431B', 600: '#F02C00',
          700: '#D12600', 800: '#AD2000', 900: '#801700',
        },
        blue: {
          25: '#F6FAFF', 50: '#F0F7FF', 100: '#E7F1FE', 200: '#CFE4FC',
          300: '#B6D6FB', 400: '#94C4F9', 500: '#6EAEF7', 600: '#3D93F5',
          700: '#0C70E4', 800: '#0A60C2', 900: '#074388',
        },
        purple: {
          25: '#FAFAFF', 50: '#F4F3FF', 100: '#F2EDFC', 200: '#E4DCF9',
          300: '#D3C6F6', 400: '#BFACF2', 500: '#AB91ED', 600: '#8D6AE7',
          700: '#5925DC', 800: '#4B1EBD', 900: '#351584',
        },
        'magenta-pink': {
          25: '#FFF2F3', 50: '#FFCAD4', 100: '#FEA2BE', 200: '#F77FAE',
          300: '#EB61A2', 400: '#DC4799', 500: '#C93191', 600: '#B4289A',
          700: '#9D219D', 800: '#711A87', 900: '#4C136F',
        },
      },
      fontFamily: {
        sans: ['Inter', 'sans-serif'],
      },
      borderRadius: {
        none: '0px',
        DEFAULT: '4px',
        md: '6px',
        lg: '8px',
        xl: '12px',
        '2xl': '20px',
        full: '9999px',
      },
      boxShadow: {
        xs: '0 1px 2px 0 rgba(73,88,131,0.08)',
        sm: '0 1px 2px 0 rgba(73,88,131,0.04), 0 1px 3px 0 rgba(73,88,131,0.16)',
        md: '0 2px 4px -2px rgba(73,88,131,0.04), 0 4px 8px -2px rgba(73,88,131,0.16)',
        lg: '0 4px 6px -2px rgba(73,88,131,0.04), 0 12px 16px -4px rgba(73,88,131,0.16)',
        xl: '0 8px 8px -4px rgba(73,88,131,0.04), 0 20px 24px -4px rgba(73,88,131,0.16)',
        '2xl': '0 24px 48px -12px rgba(73,88,131,0.16)',
      },
    },
  },
};
```

IMPORTANT: Always use these custom color names (e.g., `text-primary-500`, `bg-primary-text-100`, `border-error-300`) instead of Tailwind's default color palette (no `gray-*`, `red-*`, `blue-*` from Tailwind defaults).
