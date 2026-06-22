# Timeline Component — Strict Specification

When building any timeline, event history, or milestone list, follow these specifications exactly. Do NOT use default Tailwind timeline styles or any other timeline system.

> **TOKEN RULE:** Every colour in this spec is expressed as a CSS variable token. When writing Tailwind classes use the `bg-[var(--token)]` / `text-[var(--token)]` / `border-[var(--token)]` syntax. In inline styles use `'var(--token)'`. In prose and tables the original hex is kept only as a parenthetical annotation, e.g. `var(--bg-primary)` (#FFFFFF).

---

## Timeline Anatomy

A timeline is a vertical list of items, each with a dot indicator and a dashed connecting line:

```
● ─ ─ ─  [Content for item 1]
│
● ─ ─ ─  [Content for item 2]
│
●        [Content for item 3 — last item, no line]
```

- Container: `display: block` wrapper
- Each item: `display: flex` row with leading indicator + content
- Last item has no connecting line

---

## Timeline Item Structure

Each timeline item is a horizontal flex row:

```
[leading wrapper]  [content area]
```

### Leading Wrapper (left column)
Contains the circle dot and the dashed line below it.

| Property | Value |
|----------|-------|
| Position | Relative (for absolute line positioning) |

### Circle Dot
| Property | Value | Tailwind |
|----------|-------|----------|
| Size | 10px × 10px | `w-2.5 h-2.5` |
| Background | `var(--bg-primary)` (#FFFFFF) | `bg-[var(--bg-primary)]` |
| Border | 2px solid `var(--border-primary-solid)` (#353E5A) | `border-2 border-[var(--border-primary-solid)]` |
| Shape | Fully circular | `rounded-full` |
| Margin top | 16px (`mt-4`) — aligns with first line of content | `mt-4` |

### Dashed Connecting Line
| Property | Value |
|----------|-------|
| Type | SVG `<line>` element |
| Width | 10px container (line centered at x=5) |
| Stroke color | `var(--border-secondary)` (#CCD6DC) |
| Stroke width | 2px |
| Dash pattern | 6px dash, 6px gap (`stroke-dasharray: 6 6`) |
| Line caps | Round (`stroke-linecap: round`) |
| Position | Absolute, top: 26px (below the circle) |
| Height | 100% of remaining item height |
| Visibility | Hidden on last item |

### Content Area
| Property | Value | Tailwind |
|----------|-------|----------|
| Padding | 12px vertical (`py-3`) | `py-3` |
| Width | 100% (fills remaining space) | `w-full` |
| Content | Any projected content (headings, text, etc.) | — |

---

## Container Spec

| Property | Value | Tailwind |
|----------|-------|----------|
| Gap between dot column and content | 16px (`gap-4`) | `gap-4` |
| Transition | Background color (smooth) | `transition-colors duration-150 ease-out` |

---

## Implementation Pattern for Lovable

```jsx
{/* === Timeline Container === */}
<div className="block">

  {/* Timeline Item 1 — with connecting line */}
  <div className="flex gap-4">
    {/* Leading wrapper */}
    <div className="relative">
      {/* Circle dot */}
      <div className="mt-4 w-2.5 h-2.5 rounded-full bg-[var(--bg-primary)] border-2 border-[var(--border-primary-solid)]" />
      {/* Dashed line */}
      <div className="absolute top-[26px] h-[calc(100%-26px)]">
        <svg className="w-2.5 h-full">
          <line
            x1="5" x2="5" y1="0" y2="100%"
            stroke="var(--border-secondary)"
            strokeWidth="2"
            strokeDasharray="6 6"
            strokeLinecap="round"
          />
        </svg>
      </div>
    </div>

    {/* Content */}
    <div className="py-3 w-full">
      <h3 className="text-sm leading-5 text-[var(--text-brand-secondary)]" style={{ fontWeight: 600 }}>
        Project Kickoff
      </h3>
      <p className="text-sm leading-5 font-medium text-[var(--text-secondary)]">January 15, 2025</p>
      <p className="text-sm leading-5 font-normal text-[var(--text-tertiary)]">
        The project officially begins with goals and objectives defined.
      </p>
    </div>
  </div>

  {/* Timeline Item 2 — with connecting line */}
  <div className="flex gap-4">
    <div className="relative">
      <div className="mt-4 w-2.5 h-2.5 rounded-full bg-[var(--bg-primary)] border-2 border-[var(--border-primary-solid)]" />
      <div className="absolute top-[26px] h-[calc(100%-26px)]">
        <svg className="w-2.5 h-full">
          <line
            x1="5" x2="5" y1="0" y2="100%"
            stroke="var(--border-secondary)"
            strokeWidth="2"
            strokeDasharray="6 6"
            strokeLinecap="round"
          />
        </svg>
      </div>
    </div>

    <div className="py-3 w-full">
      <h3 className="text-sm leading-5 text-[var(--text-brand-secondary)]" style={{ fontWeight: 600 }}>
        Design Phase
      </h3>
      <p className="text-sm leading-5 font-medium text-[var(--text-secondary)]">February 20, 2025</p>
      <p className="text-sm leading-5 font-normal text-[var(--text-tertiary)]">
        Wireframes and design prototypes are created for review.
      </p>
    </div>
  </div>

  {/* Timeline Item 3 — LAST item, NO connecting line */}
  <div className="flex gap-4">
    <div className="relative">
      <div className="mt-4 w-2.5 h-2.5 rounded-full bg-[var(--bg-primary)] border-2 border-[var(--border-primary-solid)]" />
      {/* No dashed line on last item */}
    </div>

    <div className="py-3 w-full">
      <h3 className="text-sm leading-5 text-[var(--text-brand-secondary)]" style={{ fontWeight: 600 }}>
        Development Starts
      </h3>
      <p className="text-sm leading-5 font-medium text-[var(--text-secondary)]">March 5, 2025</p>
      <p className="text-sm leading-5 font-normal text-[var(--text-tertiary)]">
        Core features and functionalities are built.
      </p>
    </div>
  </div>

</div>
```

### Key Implementation Rules

1. **Circle dot is always 10px × 10px** with 2px solid `var(--border-primary-solid)` (#353E5A) border and `var(--bg-primary)` (#FFFFFF) background
2. **Connecting line is dashed** — 2px stroke, `var(--border-secondary)` (#CCD6DC) color, 6px dash with 6px gap
3. **Last item has NO connecting line** — only the circle dot
4. **Gap between dot column and content is 16px** (`gap-4`)
5. **Content area has 12px vertical padding** (`py-3`)
6. **Circle has `mt-4`** (16px top margin) to vertically align with the first line of content text
7. **Line starts at 26px from top** of the item (below the circle) and stretches to 100% height
8. **Line uses SVG** with round line caps for smooth dash appearance
9. **Content is fully flexible** — can contain any headings, paragraphs, badges, etc.
10. **Timeline is purely presentational** — no built-in states, selection, or interactivity
