# Image Modal — Strict Specification

> **⚠️ CROSS-REFERENCE — REQUIRED COMPONENT SPECS:**
> The close button MUST use the **Button component** from `button.md` with `buttonType="outline"`, `variant="neutral"`, and `iconOnly`. Do NOT create a custom close button.

When building an image modal (a full-screen overlay that displays images with optional carousel navigation), follow these specifications exactly.

> **TOKEN RULE:** Every color in this spec is expressed as a CSS variable token. In Tailwind classes use `bg-[var(--token)]` / `text-[var(--token)]` / `border-[var(--token)]`. In inline styles use `'var(--token)'`. In prose and tables, show `var(--token)` (#hex) for reference.

---

## Architecture

```
┌──────────────────────────────────────────────────┐
│                                            [✕]   │  ← Close button (top-right)
│                                                  │
│          ◀   ┌──────────────────┐   ▶            │  ← Nav arrows (multi-image only)
│              │                  │                 │
│              │     IMAGE        │                 │
│              │                  │                 │
│              └──────────────────┘                 │
│                                                  │
└──────────────────────────────────────────────────┘
```

---

## Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `imageList` | `ImageModalItem[]` | Required | Array of images to display |
| `startPosition` | `number` | `0` | Initial carousel slide index |

### ImageModalItem

| Property | Type | Description |
|----------|------|-------------|
| `src` | `string` | Image URL |
| `altText` | `string` | Optional alt text for accessibility |

---

## Layout

### Overlay

| Property | Value | Tailwind / CSS |
|----------|-------|----------------|
| Position | fixed, full viewport | `fixed inset-0` |
| Background | dark overlay | `bg-black/60` |
| Z-index | top layer | `z-50` |
| Display | flex, center content | `flex items-center justify-center` |

### Close Button

| Property | Value | Tailwind / CSS |
|----------|-------|----------------|
| Position | absolute, top-right | `absolute` |
| Top | 12px | `top-3` |
| Right | 12px | `right-3` |
| Button type | outline | `buttonType="outline"` |
| Variant | neutral | `variant="neutral"` |
| Icon only | true | 40×40px icon-only button |
| Icon | ✕ (close) | Close / X icon |
| Z-index | above image | `z-10` |

### Image Container (Carousel Area)

| Property | Value | Tailwind / CSS |
|----------|-------|----------------|
| Margin top/bottom | 80px | `my-20` |
| Margin left/right (mobile) | 24px | `mx-6` |
| Margin left/right (≥768px) | 80px, auto centered | `sm:mx-auto` |
| Max height | `calc(100% - 160px)` | Reserves 80px top + 80px bottom |
| Max width (mobile) | `calc(100% - 48px)` | 24px each side |
| Max width (≥768px) | `calc(100% - 160px)` | 80px each side |

### Image

| Property | Value | Tailwind / CSS |
|----------|-------|----------------|
| Object fit | contain | `object-contain` |
| Max width | 100% | `max-w-full` |
| Max height | 100% | `max-h-full` |
| Border radius | none | — |
| User select | none | `select-none` |

---

## Carousel (Multi-Image)

When `imageList` has more than 1 image, enable carousel navigation.

### Navigation Arrows

| Property | Value | Tailwind / CSS |
|----------|-------|----------------|
| Position | absolute, vertically centered | `absolute top-1/2 -translate-y-1/2` |
| Left arrow position | left side of container | `left-2` |
| Right arrow position | right side of container | `right-2` |
| Size | 40×40px | `w-10 h-10` |
| Background | white, semi-transparent | `bg-white/80 hover:bg-white` |
| Border radius | full circle | `rounded-full` |
| Icon | chevron left / right | `<` / `>` arrows |
| Cursor | pointer | `cursor-pointer` |
| Shadow | small | `shadow-sm` |
| Transition | background 150ms | `transition-colors duration-150` |

### Single Image Mode

When `imageList` has only 1 image:
- Hide navigation arrows
- Disable drag/swipe interaction
- No carousel dots

### Slide Transition

| Property | Value |
|----------|-------|
| Type | slide (horizontal) |
| Duration | 300ms |
| Easing | ease-out |

---

## States

| State | Behavior |
|-------|----------|
| Loading | Show a centered spinner while image resolves |
| Single image | No nav arrows, no drag |
| Multi image | Show prev/next arrows, enable drag/swipe |
| Keyboard | Escape closes modal; Left/Right arrows navigate |

---

## Accessibility

| Attribute | Value |
|-----------|-------|
| `role` | `dialog` |
| `aria-modal` | `true` |
| `aria-label` | `"Image viewer"` |
| Image `alt` | From `altText` prop |
| Focus trap | Focus should be trapped within modal |
| Escape key | Closes modal |

---

## Color Reference

| Token | Hex | Usage |
|-------|-----|-------|
| Overlay | `rgba(0,0,0,0.6)` | Background overlay |
| `var(--bg-primary)` (#FFFFFF) | bg-primary | Close button bg, arrow bg |
| `var(--text-primary)` (#353E5A) | text-primary | Close icon color |

---

## Implementation

```jsx
function ImageModal({ imageList = [], startPosition = 0, onClose }) {
  const [currentIndex, setCurrentIndex] = useState(startPosition);
  const isSingle = imageList.length <= 1;

  const goNext = () => setCurrentIndex((i) => Math.min(i + 1, imageList.length - 1));
  const goPrev = () => setCurrentIndex((i) => Math.max(i - 1, 0));

  useEffect(() => {
    const handleKey = (e) => {
      if (e.key === 'Escape') onClose?.();
      if (!isSingle && e.key === 'ArrowRight') goNext();
      if (!isSingle && e.key === 'ArrowLeft') goPrev();
    };
    window.addEventListener('keydown', handleKey);
    return () => window.removeEventListener('keydown', handleKey);
  }, [isSingle]);

  return (
    <div
      className="fixed inset-0 z-50 flex items-center justify-center bg-black/60"
      role="dialog"
      aria-modal="true"
      aria-label="Image viewer"
      onClick={onClose}
    >
      {/* Close button */}
      <button
        className="absolute top-3 right-3 z-10 w-10 h-10 rounded-lg border border-[var(--border-secondary)]
          bg-white flex items-center justify-center hover:bg-[var(--bg-secondary)]
          transition-colors duration-150"
        onClick={onClose}
        aria-label="Close"
        style={{ color: 'var(--text-primary)' }}
      >
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2"
          strokeLinecap="round" strokeLinejoin="round">
          <path d="M18 6L6 18M6 6l12 12" />
        </svg>
      </button>

      {/* Image area */}
      <div
        className="relative mx-6 sm:mx-auto my-20 flex items-center justify-center"
        style={{
          maxHeight: 'calc(100% - 160px)',
          maxWidth: 'calc(100% - 48px)',
        }}
        onClick={(e) => e.stopPropagation()}
      >
        {/* Prev arrow */}
        {!isSingle && currentIndex > 0 && (
          <button
            className="absolute left-2 top-1/2 -translate-y-1/2 z-10 w-10 h-10 rounded-full
              bg-white/80 hover:bg-white shadow-sm flex items-center justify-center
              transition-colors duration-150 cursor-pointer"
            onClick={goPrev}
            aria-label="Previous image"
          >
            <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="var(--fg-primary)"
              strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">
              <path d="M15 18l-6-6 6-6" />
            </svg>
          </button>
        )}

        {/* Image */}
        <img
          src={imageList[currentIndex]?.src}
          alt={imageList[currentIndex]?.altText || ''}
          className="max-w-full max-h-full object-contain select-none"
        />

        {/* Next arrow */}
        {!isSingle && currentIndex < imageList.length - 1 && (
          <button
            className="absolute right-2 top-1/2 -translate-y-1/2 z-10 w-10 h-10 rounded-full
              bg-white/80 hover:bg-white shadow-sm flex items-center justify-center
              transition-colors duration-150 cursor-pointer"
            onClick={goNext}
            aria-label="Next image"
          >
            <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="var(--fg-primary)"
              strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">
              <path d="M9 18l6-6-6-6" />
            </svg>
          </button>
        )}
      </div>
    </div>
  );
}
```

---

## CRITICAL Rules

1. **Close button uses outline/neutral button** — 40×40px icon-only with a close (X) icon. Do NOT create custom close styling.
2. **Close button is positioned at top: 12px, right: 12px** — uses `absolute top-3 right-3`, NOT centered or bottom.
3. **Image area has 80px vertical margins** — `my-20` (80px top + 80px bottom), reserving 160px total height for padding.
4. **Responsive horizontal margins** — 24px on mobile (`mx-6`), 80px on ≥768px. The image area is centered.
5. **Single image mode disables all navigation** — no arrows, no drag, no dots. Only the close button is interactive.
6. **Multi-image shows prev/next arrows** — circular white buttons at the vertical center of the image area, appearing only when there are more slides in that direction.
7. **No zoom functionality** — images display at their natural aspect ratio, constrained by `object-contain`.
8. **Overlay click closes modal** — clicking the dark overlay (but NOT the image itself) closes the modal.
9. **Keyboard navigation** — Escape closes; ArrowLeft/ArrowRight navigate between images.
10. **Font is Inter** — all text uses `Inter, Arial, sans-serif`.
11. **Transitions are 150ms ease-out** — for button hover states. Slide transitions are 300ms ease-out.
12. **startPosition defaults to 0** — the first image is shown by default unless a different index is provided.
