# Fello Logo

The Fello logo sits at the very left of the app header. It is a clickable link that navigates to the home page.

> **TOKEN RULE:** Every color in the code examples below uses CSS variable tokens or Tailwind palette names — never raw hex. When implementing, copy the token references exactly. See `design-tokens.md` for definitions.

**Logo image URL:** `https://app-dev.fello.ai/assets/images/brand/logo-white.svg` — always use this exact URL as the image source.

---

## Structure

```
┌───────────────────────────────┐
│                               │
│        ┌─────────┐            │
│        │  logo   │            │
│        │  image  │            │
│        └─────────┘            │
│                               │
└───────────────────────────────┘
 ↑                               ↑
 wrapper (64px)                   logo image centered inside
```

The logo area is a fixed-width wrapper containing a centered logo image that links to the home page.

- **Wrapper** — a fixed-width container that reserves horizontal space in the header
- **Link** — the entire wrapper area is clickable, navigating to the home page
- **Logo image** — a small coral-colored abstract "F" mark, centered horizontally within the wrapper

---

## Dimensions

| Property | Value |
|----------|-------|
| Wrapper width | `64px` |
| Wrapper height | fills parent (inherits header height of `44px`) |
| Logo image max width | `28px` |
| Logo image alignment | centered horizontally and vertically within the wrapper |

---

## Appearance

| Property | Value |
|----------|-------|
| Logo color | `var(--fg-brand-primary)` (#FF725C) (coral) |
| Background | `transparent` (sits on the dark header background) |
| Cursor | `pointer` (clickable link) |

The logo is a coral vector mark — an abstract letter "F" composed of geometric shapes (a vertical bar with three small circles). Always load the logo from the exact URL: `https://app-dev.fello.ai/assets/images/brand/logo-white.svg`

---

## Behavior

- Clicking the logo navigates to the **home page**
- No hover state changes (no color shift, no underline, no scale)
- The link should have an accessible label (e.g., `aria-label="Home"` or `alt="Logo"` on the image)

---

## Implementation

```jsx
function FelloLogo() {
  return (
    <a
      href="/"
      aria-label="Home"
      className="flex items-center justify-center shrink-0"
      style={{
        width: '64px',
        height: '44px',
        background: 'transparent',
        cursor: 'pointer',
        textDecoration: 'none',
      }}
    >
      <img
        src="https://app-dev.fello.ai/assets/images/brand/logo-white.svg"
        alt="Fello Logo"
        style={{ maxWidth: '28px' }}
      />
    </a>
  );
}
```

---

## CRITICAL Rules

1. Logo wrapper is exactly 64px wide and fills the header height (44px).
2. Logo image max-width is 28px, centered horizontally and vertically within the wrapper.
3. Background is ALWAYS transparent — it sits on the dark header background.
4. ALWAYS use this exact image URL: `https://app-dev.fello.ai/assets/images/brand/logo-white.svg`
5. The logo is a clickable link to the home page ("/").
6. No hover state changes — no color shift, no underline, no scale.
7. The logo does NOT change between primary/secondary header variants. It is variant-agnostic.
8. Must have an accessible label (aria-label="Home" or alt="Fello Logo").

---

## Showcase

When rendering the Logo on the showcase page:

1. Display the logo wrapper on a dark plum (`var(--bg-primary-solid)` (#353E5A)) background strip to simulate the header
2. Show the coral logo image centered within the 64px wrapper
3. Label the wrapper width (`64px`) and the logo image max-width (`28px`)
