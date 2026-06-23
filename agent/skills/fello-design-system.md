---
name: fello-design-system
description: Fello brand + design system for any generated UI, HTML, chart, dashboard, story, or visual. Use whenever building or styling output that a person will see — apply Fello's palette, typography, and components so it looks on-brand. Triggers on build/generate/design/style a page, component, chart, dashboard, story, report, HTML, or visual; or any request to "make it on-brand / Fello-styled".
---

# Fello Design System

Apply this whenever you generate something visual (HTML, a story/dashboard layout, a chart, a report). Goal: output looks like Fello, not generic.

## Brand foundation (NON-NEGOTIABLE)
- **Light mode only.** White/very-light backgrounds. Never dark gradients, glows, neon, or orbs.
- **Palette:** plum `#353E5A` (primary text/headings), coral `#FF725C` (primary CTA / accent only), teal `#098486` (links). Surfaces: white `#FFFFFF`, subtle `#F7F7F9`, borders `#E8ECF1`.
- **Chart series order:** coral `#FF725C`, teal `#098486`, plum `#353E5A`, amber `#E8A33D`, violet `#7C6BD9`.
- **Typography:** Inter. Headings ≤ 24px (heading-md is the hard max). Body text `#353E5A`.
- **No emoji as icons.** Use the Fello icon font / component icons.
- **Coral is for the primary action only** — don't flood the UI with coral.

## Reference files (read before building)
All under `docs/fello-design-system/`:
- `colors_and_type.css` — the foundation tokens (CSS custom properties) + typography classes. Use `var(--token)`, never raw hex when a token exists.
- `fello-components.css` — the `.fello-*` component library (buttons, cards, badges, tables, forms, callouts, etc.). **Prefer these classes; don't hand-write component CSS.**
- `uploads/<component>.md` — per-component spec (exact classes, variants, sizes). Read the relevant one before building that component.
- `fello-logo.svg` / `fello-icon.svg` — the real logo. Never hand-draw a logo.

## Answer normally; only build a STORY when explicitly asked for a dashboard/report
- **Normal questions → answer in the chat as usual** (text + inline charts via the chart tools). Do NOT create a story for a plain question or follow-up.
- **Only when the user asks for a dashboard / report / saved or shareable artifact** → use `create_story`.
- When you DO build a story: put the `<grid>`/`<chart>`/`<table>` markup in the story `content` via `create_story` — do NOT paste that markup into a chat reply (those tags only render inside a story; in chat they show as raw text). A short chat line like "Built it → see the story" is enough.

## Deliver dashboards/reports as a STORY (the shareable artifact)
A dashboard, report, or multi-section HTML page should be saved as a **Story** — that's the shareable, side-panel artifact (it gets its own URL + shows in Stories). Don't just render it inline in the chat reply.

**To do this:** call `create_story` with a title and `content` that contains the Fello HTML inside a ```html fenced block (plus any `<chart>` tags). The story renders the HTML artifact. Inline-in-chat HTML is only a quick preview; the artifact is the story.

## How to build on-brand HTML
**CRITICAL — to render, the HTML MUST be inside a ```html fenced code block.** A full HTML doc pasted as raw text will NOT render (it shows as text). Always wrap it:

````
```html
<!DOCTYPE html>
<html>… your Fello-styled page …</html>
```
````

1. Read `colors_and_type.css` + `fello-components.css` (and the relevant `uploads/<component>.md`).
2. Inline the styles in a `<style>` block (the artifact renders in a sandboxed iframe — external links won't load), then build markup with `var(--token)` values + `.fello-*`-style classes.
3. Flow top-down (never vertically center a full page). Card/grid layout.
4. Headings ≤ 24px, plum text, coral only for the primary CTA, charts in the series order above.
5. Self-contained: one `<style>` block + markup. No external CSS/JS/font URLs (use `Inter, system-ui` fallback).

## How to style charts / dashboards / stories
- Use the chart series order above (coral first).
- Keep surfaces light, text plum, accents coral/teal. Match the foundation tokens.
